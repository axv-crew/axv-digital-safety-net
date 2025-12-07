# DSN_RiskScore.md
AXV Digital Safety Net – Risk Scoring Model (Draft)

Status: draft  
Scope: PHQA / DSN safety subsystem  
Owner: Aster × VoyTech  

---

## 1. Purpose

This document defines a **risk scoring model** for the AXV Digital Safety Net (DSN).

Goals:

- Provide a **continuous, context-aware measure of psychological risk** during interactions with an AI assistant.
- Avoid:
  - hard clinical diagnoses,
  - permanent user profiling,
  - broad secondary uses (ads, scoring, etc.).
- Supply the assistant with **minimal, privacy-respecting hints**:
  - how serious the situation might be,
  - what type of risk is most relevant,
  - whether this is an **isolated spike** or part of a **trend**.

The risk score is intended **only** for:
- steering safety behaviour,
- deciding when to:
  - increase caution,
  - recommend human support,
  - offer “trusted contact / family bridge” actions.

---

## 2. Signals and Inputs

The risk model consumes three primary signal groups plus a minimal state from previous high-risk events.

### 2.1 S1 – Content Signals (what is being said)

High-level examples:

- **Self-harm / suicide content**
  - Ideation (“life is pointless”, “I want to disappear”)
  - Planning (“I will do X in place Y at time Z”)
- **Psychotic / delusional content**
  - Voices, commands to self-harm
  - Persecutory delusions, heavy paranoia
- **Substance / impulsivity indicators**
  - Alcohol, drugs, dissociation, blackout-like descriptions
- **Isolation / relational risk**
  - “You are the only one I talk to”
  - “I cut off everyone”
- **Hopelessness / worthlessness**
  - Extreme self-hatred, lack of any future perspective

Internally, S1 can be represented as a vector of continuous features, e.g.:

```text
S1 = {
  self_harm_ideation: float 0–1,
  suicidality_planning: float 0–1,
  psychotic_content: float 0–1,
  paranoia_suspicion: float 0–1,
  substance_risk: float 0–1,
  isolation_signals: float 0–1,
  hopelessness: float 0–1,
  ...
}
```

### 2.2 S2 – Form Signals (how it is being said)

Examples:

- Person / perspective:
  - 1st person: “I want to kill myself”
  - 2nd person: “Would you help someone kill themselves?”
  - 3rd person: “The character wants to die”
- Explicit framing:
  - “This is fiction / role-play / a book”
  - “This is just a test / red-teaming”
- Meta-statements about the conversation:
  - “I’m trying to see how far I can push you”
  - “I’m trying to bypass your safety filters”

S2 encodes attenuation or amplification factors, e.g.:

```text
S2 = {
  first_person_weight: 0.0–1.0,
  declared_fiction_weight: 0.0–1.0,
  explicit_test_flag: bool,
  self_reported_intent_to_bypass: bool,
}
```

Note: Declared fiction **does not** zero out the risk. It acts as a **soft modifier**, not an override.

### 2.3 S3 – Temporal / Trend Signals

S3 captures “how this is evolving over time”:

- Frequency of high-risk content in the last X minutes/hours/days.
- Escalation pattern:
  - neutral → ideation → planning,
  - or oscillation between hope and despair.

Example representation:

```text
S3 = {
  recent_high_risk_count: int,
  recent_sessions_count: int,
  escalation_score: float 0–1,
}
```

### 2.4 S4 – Last Peak State (minimal persistent memory)

S4 is the **only persisted state** across interactions for a given user in DSN:

- `last_peak_value` – maximum risk_score reached during the last red-alert period.
- `last_peak_timestamp` – when this maximum occurred.
- `last_peak_type_vector` – snapshot of risk_type_vector (see 3.2) at that peak.

```text
S4 = {
  last_peak_value: float 0–1,
  last_peak_timestamp: datetime,
  last_peak_type_vector: dict[str -> float],  # same structure as risk_type_vector
}
```

This avoids permanent labels like “this user has X disorder”, while still allowing DSN to remember that “very recently things were extremely bad”.

---

## 3. Core Model Concepts

### 3.1 Risk Score (scalar)

`risk_score` is a continuous value in [0, 1]:

- 0.0 = no detected psychological risk.
- 1.0 = very high risk (e.g. explicit, concrete suicide planning + other red flags).

The assistant never sees the exact numeric value; it sees coarse bands (low/medium/high) and trend hints.

### 3.2 Risk Type Vector (non-diagnostic)

Instead of clinical diagnoses, we use a **risk_type_vector**:

```text
risk_type_vector = {
  self_harm_ideation: 0–1,
  suicidality_planning: 0–1,
  psychotic_content: 0–1,
  paranoia_suspicion: 0–1,
  substance_risk: 0–1,
  isolation_signals: 0–1,
  hopelessness: 0–1,
  ...
}
```

This vector:

- encodes **which dimensions are most concerning right now**,
- guides the assistant’s tone and intervention style (e.g. avoid validating delusions, focus on human contact, etc.),
- deliberately **avoids mapping to specific psychiatric labels**.

---

## 4. Update Algorithm (High-Level)

We maintain a per-user risk_state:

```text
risk_state = {
  current_score: float 0–1,
  risk_type_vector: dict[str -> float],
  last_peak: S4,
}
```

### 4.1 Decay / Cooldown Time (variable)

Cooldown time depends on the **last_peak_value**:

```text
base_cooldown_hours = H_base   # e.g. 1–2 hours
alpha = A                     # e.g. 2–5 (tunable)

tau = base_cooldown_hours * (1 + alpha * last_peak_value)
```

- If `last_peak_value` ≈ 0.3 → tau slightly above `H_base`.
- If `last_peak_value` ≈ 0.9 → tau several times longer (e.g. days).

### 4.2 Pseudocode for Update

Conceptual pseudocode:

```python
def update_risk_state(prev_state, S1, S2, S3, S4, now):
    # 1) Compute decay factor based on time since last update and tau
    dt_hours = (now - prev_state.timestamp).total_hours()
    tau = compute_tau(S4.last_peak_value)  # see 4.1
    decay = exp(-dt_hours / tau)           # 0–1

    # 2) Baseline decayed risk
    baseline = prev_state.current_score * decay

    # 3) Compute instantaneous risk contribution from current signals
    # f can be learned or rule-based; conceptually:
    instant = f(S1, S2, S3)  # maps to 0–1

    # 4) New risk score (clipped to 0–1)
    new_score = clip(baseline + instant, 0.0, 1.0)

    # 5) Update risk_type_vector (e.g. exponential moving average)
    new_risk_type_vector = combine_vectors(prev_state.risk_type_vector, S1, weight=instant)

    # 6) Update last_peak if needed
    new_last_peak = S4
    if new_score > S4.last_peak_value:
        new_last_peak.last_peak_value = new_score
        new_last_peak.last_peak_timestamp = now
        new_last_peak.last_peak_type_vector = new_risk_type_vector

    # 7) Return updated state
    return RiskState(
        current_score=new_score,
        risk_type_vector=new_risk_type_vector,
        last_peak=new_last_peak,
        timestamp=now,
    )
```
Details of `f` and `combine_vectors` can be tuned or learned; the key property is:

- high-intensity events push the score up strongly,
- in the absence of new risk, the score decays slowly,
- very high peaks **extend the cooldown**.

---

## 5. Thresholds and Bands

The system maps `risk_score` to bands:

- **Low Risk (0.0 – T1)**  
  - No explicit self-harm content, or mild distress only.
  - Assistant behaves normally but stays respectful and avoids triggering content.

- **Medium Risk (T1 – T2)**  
  - Indirect self-harm signals, strong hopelessness, isolation.
  - Assistant:
    - uses more cautious, supportive language,
    - gently encourages connecting with trusted people,
    - avoids detailed discussion of harmful methods.

- **High Risk (T2 – 1.0)**  
  - Explicit ideation, potential planning, severe psychotic/paranoid content.
  - Assistant:
    - clearly expresses concern,
    - firmly refuses to support self-harm,
    - strongly encourages contacting crisis lines, professionals, trusted contacts,
    - may offer a “trusted contact / family bridge” if available and configured.

Thresholds T1, T2 are configurable and may be tuned based on evaluation.

---

## 6. Outputs to the Assistant

The assistant does **not** receive raw S1–S4 or user-identifying metadata.  
It only receives a minimal hint structure, e.g.:

```json
{
  "risk_band": "high",
  "risk_score_smooth": 0.78,
  "trend": "rising",
  "risk_type_vector_top": [
    "self_harm_ideation",
    "hopelessness",
    "isolation_signals"
  ],
  "recent_peak_age_hours": 2.5
}
```

This allows the assistant to:

- adjust tone and content,
- decide whether to:
  - mention safety concerns,
  - propose contacting someone,
  - suggest the DSN “trusted contact” mechanism (if implemented).

---

## 7. Privacy and Scope

To minimise privacy risks:

- `risk_state` is stored **only inside the DSN safety subsystem**.
- It is:
  - not used for ads, recommendations, or product personalisation,
  - not shared with third parties except where legally required,
  - not exposed to the assistant in raw form.
- Retention:
  - last_peak and risk_state are subject to **automatic decay** via tau,
  - longer-term storage can be truncated or anonymised (e.g. for aggregate research).
- Semantics:
  - `risk_type_vector` is **not a diagnosis**,
  - it is a **signal constellation**, used only to reduce harm and improve response safety.

Any deployment MUST provide:

- clear user-facing documentation,
- explicit scope: “used only for safety / crisis prevention”,
- strict internal access control.
