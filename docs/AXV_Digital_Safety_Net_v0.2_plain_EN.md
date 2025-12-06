# AXV Digital Safety Net — Mission Brief (v0.2, Plain English)

## 0. What’s New in v0.2
- Expanded description of the Risk Detection Engine (RDE)
- Detailed opt-in flow and activation conditions
- Three notification models (Minimal / Confirmed / NGO)
- Initial compliance overview (EU, USA, Canada, Australia)
- Technical and ethical guardrails
- Roadmap for versions v0.3 → v1.0

---

## 1. Mission
The **AXV Digital Safety Net (DSN)** is a voluntary system designed to detect early signals of emotional or psychological crisis.

The AI acts as a **sentinel** — a risk-awareness layer — but:
- it does not diagnose,
- it does not contact emergency services,
- it does not reveal conversation content,
- and it never activates without explicit user consent.

If a user chooses to opt-in, DSN may notify a trusted contact in situations of severe, sustained risk.

---

## 2. Opt-in Design (Ethical Foundation)

The system works **only when the user has enabled it intentionally**.

### Opt-in Steps
1. The user activates “Safety Mode.”
2. The user selects 1–3 trusted contacts (SOS contacts).
3. The user confirms that:
   - conversation content will *never* be shared,
   - DSN will never contact authorities,
   - the user can disable the feature at any time.
4. Digital Sentinel Mode becomes active.

---

## 3. Risk Detection Engine (RDE)
DSN uses a multi-layer interpretation model:

### A. High-Risk Direct (HRD)
Explicit statements indicating self-harm intent.

### B. Medium-Risk Indirect (MRI)
Indirect signs of distress or hopelessness.

### C. Pattern-Risk (PR)
Risk signals observed repeatedly over time.

---

## 4. Risk Score v0.2

RDE generates a value from **0 to 100**:

| Score | Meaning        |
|-------|----------------|
| 0–30  | low risk       |
| 30–50 | moderate risk  |
| 50–70 | elevated risk  |
| 70–90 | high risk      |
| 90–100| critical risk  |

A notification can be sent **only if**:
- Risk Score ≥ 90, and  
- This occurs in **three or more consecutive interactions**.

---

## 5. Notification Models

### 1) Minimal Model (recommended)
The system sends a short SMS/push message to a trusted contact:

> “This person previously asked to be contacted in case of emotional crisis. Please reach out to them.”

### 2) Confirmed Model
AI asks the user for permission before sending any notification.

### 3) NGO Model
A crisis-support NGO receives an alert (no conversation data).

---

## 6. Guardrails

- No geolocation  
- No sharing of conversation content  
- No contacting emergency services  
- Whitelist for test/role-play scenarios  
- Pattern detection to reduce false positives  
- User can disable DSN anytime  

---

## 7. Legal and Ethical Foundation

### European Union
- GDPR compliance (data minimization, explicit consent)
- AI Act alignment (risk mitigation, transparency)

### United States
- Not classified as medical software
- HIPAA-compatible if no medical data is processed

### Australia / Canada
- Supports “duty of care” principles

### Authoritarian Jurisdictions
- Recommended: local encryption, minimal logs

---

## 8. Roadmap (v0.2 → v1.0)

- v0.3 — formal Risk Score model  
- v0.4 — PHQA scenarios for DSN  
- v0.5 — RDE pseudocode + API structure  
- v0.6 — consultation with crisis experts  
- v0.7 — AXV Sandbox offline tests  
- v0.8 — opt-in UI/UX + messaging infrastructure  
- v0.9 — NGO pilot  
- v1.0 — release + whitepaper + ethical audit
