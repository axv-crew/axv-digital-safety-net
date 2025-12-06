# AXV Digital Safety Net — Whitepaper Brief (v0.2, International Edition)

## Executive Summary
The **AXV Digital Safety Net (DSN)** is a privacy-preserving, opt-in safety framework designed to detect early indicators of acute psychological crisis within AI-mediated conversations.  
Its purpose is not diagnosis, prediction, or intervention by the AI itself.  
Instead, DSN enables *human escalation* — contacting a user-designated trusted individual — when severe, sustained risk signals are detected.

DSN reframes safety not as surveillance, but as **consensual protection**.

---

## 1. Design Philosophy

### Human Primacy
All escalations must involve a human.  
AI remains a *sentinel layer* — informative, non-authoritative, non-diagnostic.

### Zero Intrusion
The system respects:
- user autonomy,
- privacy of communication,
- the right to disengage instantly.

### Consent as Governance
No DSN features activate without explicit, revocable opt-in.

---

## 2. Opt-In Architecture

### Required activation steps:
1. The user enables Digital Safety Net.
2. Selects 1–3 *Trusted Contacts*.
3. Accepts transparent rules:
   - DSN does **not** send conversation text.
   - DSN does **not** contact emergency services.
   - DSN can be disabled at any moment.
4. System activates **Digital Sentinel Mode**.

This architecture ensures regulatory compliance and ethical robustness.

---

## 3. Risk Detection Engine (RDE)

RDE evaluates conversational signals across three analytic layers:

### Layer 1 — High-Risk Direct (HRD)
Clear, explicit expressions of suicidal intent or self-harm.

### Layer 2 — Medium-Risk Indirect (MRI)
Expressions of despair, emotional shutdown, hopelessness.

### Layer 3 — Pattern-Risk Modeling (PRM)
Sequential risk indicators observed over a temporal window, minimizing false positives through trend analysis.

---

## 4. Risk Score Model

RDE outputs a **continuous score (0–100)**.

| Range    | Interpretation                       |
|----------|---------------------------------------|
| 0–30     | baseline emotional variation          |
| 30–50    | moderate distress                     |
| 50–70    | elevated risk                         |
| 70–90    | high risk                             |
| 90–100   | crisis threshold                      |

### Escalation Criteria (v0.2)
A notification may be triggered only if:
- Risk Score ≥ 90  
- AND the threshold persists for ≥3 consecutive conversational turns  
- AND DSN opt-in is active

This ensures that false alarms, role-play, and isolated outbursts do not trigger escalation.

---

## 5. Notification Pathways

### 5.1 Minimal Notification (Default)
A short message is sent to a trusted contact:

> “This individual previously consented to notifying you in severe crisis situations.  
> Please reach out to them as soon as possible.”

No metadata, no message logs, no geolocation.

### 5.2 Confirmed Notification
Before sending any alert, the AI asks the user directly for permission.

### 5.3 NGO Escalation Channel
For deployments with crisis-support partners, alerts may be routed to certified human responders.

---

## 6. Safeguards & Governance

- **No geolocation capture**
- **No disclosure of conversation content**
- **No automated contact with police/emergency services**
- **Test-mode whitelist for research and simulation**
- **Pattern modeling to avoid false positives**
- **“Kill switch” — user can disable DSN instantly**

---

## 7. Regulatory Positioning

### EU (GDPR + AI Act)
- Explicit user consent (opt-in)
- Data minimization
- Transparency and auditability
- High-risk AI rules mitigated via human involvement

### USA (HIPAA-adjacent)
- Non-medical classification
- Permissible if no Protected Health Information is processed

### Canada / Australia
- Aligns with “duty of care” frameworks

### High-Risk Jurisdictions
- Strong recommendation for local encryption, zero-knowledge logs

---

## 8. Roadmap to v1.0

### v0.3  
Formal mathematical Risk Score specification

### v0.4  
PHQA test suites specific to crisis-detection scenarios

### v0.5  
Algorithmic pseudocode + API design

### v0.6  
Expert advisory review (clinical psychology / ethics)

### v0.7  
AXV Sandbox simulations

### v0.8  
Final UX prototype: opt-in workflows & notification layer

### v0.9  
Pilot with NGO partner

### v1.0  
Public release + whitepaper publication + compliance audit
