# AXV Digital Safety Net — Architecture Overview (Fixed v0.2, No Edge Labels)

This document outlines a **high-level architecture** for the AXV Digital Safety Net (DSN).  
It is conceptual and intended for discussion and refinement.

---

## 1. High-Level Flow

```mermaid
flowchart LR
    U[User] --> AI[AI Assistant (Sentinel Mode)]
    AI --> RDE[Risk Detection Engine (RDE)]
    RDE --> DEC{Meets Crisis Threshold?}
    DEC --> END[No Notification / Standard Support]
    DEC --> NOTIF[Notification Module]
    NOTIF --> C[Trusted Contact (Family / Friend / Therapist)]
```

**Text fallback (GitHub Pages):**

User → AI Assistant (Sentinel Mode)  
AI → Risk Detection Engine  
If threshold not met → No Notification / Standard Support  
If threshold met and DSN is active → Notification Module → Trusted Contact  

---

## 2. Components

### 2.1 AI Assistant (Sentinel Mode)
- Provides normal conversational assistance.  
- Runs crisis-detection layers in parallel.  
- Produces semantic signals:
  - direct risk,
  - indirect emotional risk,
  - pattern risk over time.

### 2.2 Risk Detection Engine (RDE)
Evaluates:
- **HRD** — High-Risk Direct  
- **MRI** — Medium-Risk Indirect  
- **PRM** — Pattern Risk Modeling  

Outputs a continuous **Risk Score (0–100)**.

### 2.3 Opt-In State
Stores:
- user opt-in,  
- trusted contacts,  
- preferences.  

### 2.4 Notification Module
- Sends minimal alerts.  
- No chat content.  
- No location.  
- No automatic contact with authorities.

---

## 3. Opt-In / Opt-Out Flow

```mermaid
flowchart TD
    START[Start] --> Q1{Enable Digital Safety Net?}
    Q1 --> OUT1[Standard AI Mode (No DSN)]
    Q1 --> CONTACTS[Select 1–3 Trusted Contacts]
    CONTACTS --> RULES[Review and Accept DSN Rules]
    RULES --> ACTIVE[Digital Sentinel Mode Active]
    ACTIVE --> OUT2[DSN Deactivated]
```

**Text fallback (GitHub Pages):**

Start → decision “Enable DSN?”  
→ Standard AI Mode (if not enabled)  
→ or: Select contacts → Accept rules → DSN active  
→ DSN can be deactivated at any time.  

---

## 4. Escalation Logic

```mermaid
flowchart TD
    A[New Message] --> B[AI + Safety Analysis]
    B --> C[Update Risk Score]
    C --> D{Score ≥ Threshold?}
    D --> E[Continue Normal Support]
    D --> F[Sustained Over Time?]
    F --> G[Opt-In Active?]
    G --> H[Trigger Notification]
    H --> I[Contact Trusted Person]
```

**Text fallback (GitHub Pages):**

New message → Safety analysis → Risk score  
If below threshold → no escalation  
If above threshold and sustained and opt-in active → notify trusted contact  

---

## 5. Data & Privacy Principles

- No conversation content stored or shared.  
- Minimal notification template only.  
- Opt-in fully reversible.  
- No automatic contact with emergency services.  

---

## 6. Future Extensions

- NGO integration  
- On-device DSN  
- Federated risk model  
- UX flows for notifications  
- Clinical review + PHQA testing  

---

End of document.
