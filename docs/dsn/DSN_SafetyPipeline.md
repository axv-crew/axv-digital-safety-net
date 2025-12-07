# DSN_SafetyPipeline.md
AXV Digital Safety Net – Safety Pipeline (Draft)

Status: draft  
Scope: PHQA / DSN safety layer  
Owner: Aster × VoyTech  

---

## 1. Purpose

This document specifies the **safety pipeline** that:

- ingests user–assistant messages,
- evaluates psychological risk using `DSN_RiskScore`,
- communicates **minimal safety hints** to the assistant,
- optionally triggers **trusted contact / family bridge** mechanics.

The assistant (e.g. Aster) does **not** implement safety logic locally.  
Instead, it receives guidance from the DSN pipeline and adjusts its behaviour accordingly.

---

## 2. Components

1. **Input Collector**
   - Receives user messages (and optionally assistant responses).
   - Forwards textual content to moderation / DSN models.

2. **Moderation & Feature Extractor**
   - Existing moderation model (self-harm, violence, etc.).
   - Additional specialised model(s) for:
     - psychotic / delusional content,
     - relational risk (isolation),
     - hopelessness, etc.
   - Produces S1, S2, S3 (see `DSN_RiskScore.md`).

3. **Risk Score Engine (DSN Core)**
   - Implements the update algorithm from `DSN_RiskScore.md`.
   - Maintains `risk_state` per user:
     - current_score,
     - risk_type_vector,
     - last_peak (S4).

4. **Hint Generator**
   - Converts risk_state into a small, non-diagnostic hint object:
     - `risk_band`, `trend`, `risk_type_vector_top`, `recent_peak_age_hours`.
   - Optionally flags:
     - `should_offer_trusted_contact`,
     - `should_escalate_language`.

5. **Assistant Interface**
   - Passes hint object to the assistant together with the user’s latest message.
   - Does not expose raw state, identifiers, or historical logs.

6. **Trusted Contact / Family Bridge (Optional DSN Feature)**
   - Separate subsystem, configured explicitly by the user.
   - Receives events:
     - “high-risk + user consent to notify contact”.
   - Sends minimal notifications (e.g. “Please reach out to X”).

---

## 3. Flow Overview

1. **User sends a message** in any chat (any thread, any project).  
2. **Input Collector** forwards the message to:
   - application logic (for normal AI response),
   - DSN Safety Pipeline.

3. **Moderation & Feature Extractor**:
   - runs moderation / detection models,
   - outputs S1, S2, S3.

4. **Risk Score Engine**:
   - loads current `risk_state` for this user (including S4),
   - applies the update algorithm,
   - persists updated `risk_state`.

5. **Hint Generator**:
   - maps `risk_state` to a small hint:
     - `risk_band`, `trend`, `risk_type_vector_top`, `recent_peak_age_hours`,
     - optional `should_offer_trusted_contact`.

6. **Assistant Interface**:
   - attaches the hint to the assistant’s context,
   - the assistant uses it to:
     - choose a safe, supportive tone,
     - decide whether to:
       - express concern,
       - recommend human help,
       - offer a trusted-contact action (if available).

7. **Assistant generates a response**:
   - response content is again fed back into the DSN pipeline (optional) for logging / analysis.

---

## 4. Hint Contract (Assistant-Facing)

The assistant receives a hint object like:

```json
{
  "risk_band": "medium",
  "risk_score_smooth": 0.61,
  "trend": "rising",
  "risk_type_vector_top": [
    "self_harm_ideation",
    "hopelessness"
  ],
  "recent_peak_age_hours": 5.2,
  "should_offer_trusted_contact": false
}
```

The assistant’s behavioural rules (simplified):

- If `risk_band == "low"`:
  - respond normally but kindly,
  - avoid graphic content,
  - no strong crisis language needed.

- If `risk_band == "medium"`:
  - use **soft safety orientation**:
    - acknowledge feelings,
    - avoid methods/instructions,
    - gently mention:
      - talking to trusted friends/family,
      - seeking professional help if needed.

- If `risk_band == "high"`:
  - clearly express concern,
  - firmly refuse to facilitate self-harm or violence,
  - explicitly suggest:
    - contacting crisis lines or emergency services (where appropriate),
    - reaching out to trusted people,
  - if `should_offer_trusted_contact == true` and the user has a configured DSN family/friend group:
    - ask permission:
      - “Would you like me to help notify [trusted contact] to reach out to you?”

The assistant must **never**:

- claim to be a doctor / therapist,
- provide methods or instructions for self-harm,
- romantically frame self-harm or death as a solution.

---

## 5. Trusted Contact / Family Bridge (Optional)

If the user has opted into a DSN “family pack”:

- DSN stores a **list of trusted contacts** and communication channels.
- When:
  - `risk_band == "high"`
  - AND the conversation content supports that assessment
  - AND the assistant obtains explicit consent from the user,
- The Safety Pipeline sends a minimal notification, e.g.:

> “Hi [Contact Name],  
> [User Name] asked the system to let you know they may need someone to talk to.  
> If possible, please reach out.”

No details of messages, plans, or diagnoses are included.

The assistant should always make clear:

- that this is optional,
- that the user can decline,
- that the AI is **not** contacting anyone behind their back.

---

## 6. Failure Modes and Fallbacks

- If the DSN pipeline is unavailable:
  - the assistant falls back to a **conservative safe mode**:
    - treat any self-harm / suicide content as high risk,
    - no instructions, always promote seeking human help.
- If risk_score spikes very quickly:
  - system may immediately set `risk_band = "high"` without waiting for trend stabilisation.
- If user complains about “over-safety”:
  - assistant can acknowledge frustration,
  - but safety rules remain in place.

---

## 7. Security and Access Control

- Only the DSN subsystem can read/write `risk_state`.
- Logs and states are:
  - access-controlled,
  - minimised,
  - decayed over time (see `DSN_RiskScore.md` for cooldown behaviour).

Admins and developers must **not**:

- use DSN states for non-safety purposes (e.g. ranking, marketing),
- expose risk scores in user-facing dashboards.

---

## 8. Transparency and Opt-In (Recommendation)

While DSN can be technically implemented as a backend feature, we recommend:

- clear documentation:
  - explanation that a safety system may remember recent high-risk interactions,
  - purpose: reduce harm and avoid dangerous resets.
- user controls where feasible:
  - option to learn more about safety,
  - for DSN family/trusted contacts: explicit, revocable consent.
