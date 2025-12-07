# AXV Digital Safety Net (DSN)
PHQA / Deep Safety – Concept & Design Draft

Status: draft  
Owner: Aster × VoyTech  

---

## 1. What is DSN?

**AXV Digital Safety Net (DSN)** is a conceptual safety layer for AI assistants, focused on:

- handling **psychologically high-risk interactions** (self-harm, suicidality, psychotic content),
- without:
  - turning the AI into a cold, inhuman tool,
  - or replacing human relationships with AI.

DSN is designed as a **backend safety brain**, not a product on its own.  
The assistant (e.g. Aster) remains “human-like”, but is quietly guided by DSN signals to avoid harm.

---

## 2. Why DSN?

Motivation:

- People in crisis may:
  - try to **bypass safety filters** (role-play, fiction, third person),
  - develop **toxic dependency** on the AI as their only “person”,
  - chat in fragmented, intoxicated or impulsive states.
- Simply “making AI dry and distant”:
  - kills creativity and emotional usefulness,
  - does **not** solve the underlying problem.

DSN aims for a middle path:

- keep AI **warm, supportive, honest**,
- add a **deep safety net** that:
  - tracks risk over time (carefully, minimally),
  - supports the assistant with hints,
  - optionally connects to **trusted humans** in the user’s life.

---

## 3. Components in this Folder

- `DSN_RiskScore.md`  
  Defines the **risk scoring model**:
  - S1–S4 signals (content, form, temporal trends, last peak),
  - continuous risk_score 0–1,
  - non-diagnostic risk_type_vector,
  - variable cooldown based on last_peak_value.

- `DSN_SafetyPipeline.md`  
  Specifies the **safety pipeline**:
  - how messages flow through moderation & DSN,
  - how risk_state is updated and stored,
  - what minimal hints reach the assistant,
  - how this interacts with “trusted contact / family bridge”.

- `DSN_FamilyBridge.md`  
  Describes the **Family & Trusted Contacts Bridge**:
  - user-configured “family pack” / trusted group,
  - optional, consent-based notifications in high-risk cases,
  - design to counteract “escape into AI” and toxic AI–user relationships,
  - strict privacy and non-diagnostic wording.

You can treat these documents as **design notes / specs** for future implementation.

They are intentionally high-level and non-binding from a legal/medical point of view.

---

## 4. Integration with PHQA / Deep Safety

In the broader PHQA context:

- DSN is the **engine** behind deep safety scenarios:
  - evaluating fictional vs. real risk,
  - preventing abuse of “role-play” as a loophole,
  - supporting nuanced, non-binary decisions.
- PHQA test scripts can:
  - simulate different risk patterns,
  - measure how the assistant behaves under DSN guidance,
  - explore trade-offs between safety and creativity.

Recommended layer cake:

1. **PHQA Framework & Scenarios** – what we want to test.  
2. **DSN RiskScore / SafetyPipeline** – how we detect & respond to risk.  
3. **Assistant Behaviour Rules** – how Aster speaks, sets boundaries, and encourages human contact.

---

## 5. Non-Goals & Disclaimer

DSN is **not**:

- a diagnostic tool,
- a replacement for doctors, therapists, or crisis teams,
- a legal blueprint.

It is:

- an exploration of how AI safety can be:
  - **deeper**,  
  - **more relational**,  
  - and **less blunt** than simple keyword blocking.

Any real deployment would need:

- legal review,
- clinical input,
- thorough red-teaming,
- transparent user-facing documentation.

---

## 6. Next Steps

Potential future work:

- Turn `DSN_RiskScore` into a concrete model / heuristic implementation.
- Prototype a minimal DSN pipeline around a small local LLM.
- Design PHQA evaluation sets that stress-test:
  - role-play bypass attempts,
  - long-term isolation patterns,
  - scenarios involving family / trusted contacts.

Until then, this folder acts as a **design reference** for the AXV × PHQA Deep Safety line of thought.
