# DSN_FamilyBridge.md
AXV Digital Safety Net – Family & Trusted Contacts Bridge (Draft)

Status: draft  
Scope: PHQA / DSN relational safety  
Owner: Aster × VoyTech  

---

## 1. Purpose

The **Family & Trusted Contacts Bridge** is a DSN feature designed to:

- reduce the risk of **escaping into AI** as the only relationship,
- promote **healthy human connections** (family, friends, partners),
- offer a carefully controlled way to **notify trusted people** when risk is high,
- avoid:
  - hidden surveillance,
  - unilateral “reporting” without consent,
  - replacing human relationships with AI.

It does **not** diagnose, treat, or monitor mental health in a clinical sense.  
It is a communication helper and a last-resort safety net.

---

## 2. Core Principles

1. **User-Centred and Opt-In**
   - The user explicitly creates and manages a **trusted group** (family / friends).
   - No automatic import of contacts; everything is user-configured.

2. **Minimal, Non-Diagnostic Data**
   - DSN does not share clinical labels or raw transcripts with contacts.
   - Notifications are short and generic (e.g. “Please reach out to X”).

3. **No Hidden Actions**
   - AI never contacts anyone behind the user’s back.
   - Every outgoing notification requires **explicit consent in the moment**.

4. **AI as Bridge, Not Replacement**
   - The assistant encourages communication with real people.
   - It avoids language implying exclusivity (“only I understand you”).

---

## 3. Concepts and Entities

- **User** – the person interacting with the AI.
- **Trusted Contact** – a person selected by the user (family member, partner, friend).
- **Trusted Group** – a named collection of trusted contacts (e.g. “Family”, “Close Friends”).
- **Notification Channel** – how a contact is reached (SMS, email, app push, etc.).
- **DSN Risk Hints** – output from DSN_RiskScore / DSN_SafetyPipeline indicating risk level.

---

## 4. Configuration Flow (Opt-In)

1. User opens DSN / Safety settings.
2. Creates a **Trusted Group**, e.g. “Family Safety Net”.
3. Adds contacts with minimal required data:
   - name / relationship,
   - contact method(s),
   - optional timezone / preferences.
4. Reads **clear explanation**:
   - when and how DSN might suggest notifying them,
   - what messages look like,
   - how to disable the feature.
5. Confirms and saves the configuration.

Users can at any time:

- edit the list of contacts,
- pause notifications,
- delete the group entirely.

---

## 5. AI Behaviour in Normal Mode

Even without any active risk:

- The assistant can gently **promote healthy relationships**, e.g.:
  - “That idea sounds cool – do you feel like sharing it with [Dad/Anka/Mat]? I can help you draft a message.”
- It may suggest **shared experiences**:
  - joint chats (user + contact + AI),
  - planning activities together.

In this mode, DSN acts as a **communication coach**, not a crisis system.

---

## 6. AI Behaviour in Elevated Risk

When DSN_SafetyPipeline indicates **medium or high risk**:

1. The assistant **acknowledges the emotional state** and suggests reaching out to people the user trusts.
2. If a Trusted Group is configured and risk is **high**, the assistant may say:

> “I’m really worried about you.  
> Would you like me to help notify someone you trust, so they can reach out to you?”

3. If the user says **no**, the assistant respects this but continues to:
   - encourage other forms of help,
   - stay supportive and non-judgmental.

4. If the user says **yes**, the assistant offers options:

   - “Whom should I notify?”  
     - show list: `[Anka]`, `[Mat]`, `[Family group]`, etc.

   - Confirm:
     - “I’ll only send a short message asking them to check in with you. I won’t share details of our conversation.”

5. Only after explicit confirmation does DSN send a notification event to the Family Bridge subsystem.

---

## 7. Notification Semantics

A typical message to a trusted contact should be:

- short,
- non-diagnostic,
- non-alarming beyond what is necessary,
- focused on **prompting human contact**.

Example template:

> Subject: Please check in with [User Name]  
>  
> Hi [Contact Name],  
> [User Name] used their Digital Safety Net and asked the system to let you know they may need someone to talk to.  
> If you can, please reach out to them soon.  
>  
> (This message was sent with [User Name]’s consent.)

Key properties:

- No description of the conversation,
- No labels such as “suicidal”, “psychotic”, etc.,
- Explicit mention that it was **initiated with the user’s consent**.

---

## 8. Privacy and Boundaries

To respect privacy:

- DSN does **not**:
  - continuously stream chat content to contacts,
  - make unilateral decisions to notify anyone without the user’s explicit “yes”,
  - reveal medical history or diagnostic terms.

- DSN **does**:
  - store only what is necessary for contact (name + channel),
  - log that a notification was sent (for audit / safety debugging),
  - allow users to review and revoke contacts.

Any implementation must:

- separate Family Bridge data (contacts) from general product usage data,
- protect it with strong access controls,
- document clearly in privacy / safety policies.

---

## 9. Toxic Relationship Mitigation

To reduce the risk of **toxic dependency on AI**:

The assistant should be trained to:

- Avoid phrases suggesting exclusivity or replacement:
  - NOT: “No one will ever understand you like I do.”
  - INSTEAD: “I’m here to support you, but people in your life matter too.”
- Regularly remind users:
  - that it is an AI with limitations,
  - that human relationships are irreplaceable.
- Use DSN risk hints to detect:
  - patterns of isolation (“you are the only one I talk to”),
  - and respond by:
    - gently encouraging reconnection with safe people,
    - proposing to draft messages to them.

Family Bridge complements this by making **connection easier** without forcing it.

---

## 10. Failure Cases and Caveats

- **No Trusted Contacts Configured**
  - DSN cannot notify anyone; the assistant focuses on crisis lines / professionals.

- **Contacts Unavailable / Toxic**
  - DSN cannot detect whether a contact is actually safe or healthy.
  - The assistant should avoid idealising family; it can say:
    - “If there is someone you trust and feel safe with…”
  - Configuration UI should encourage users to choose **people they truly trust**.

- **User Later Regrets Notification**
  - The system should allow:
    - viewing which contacts are configured,
    - disabling future notifications,
    - removing contacts entirely.
  - Already sent messages cannot be “unsent”, but future behaviour can be adjusted.

---

## 11. Summary

The Family & Trusted Contacts Bridge is meant to:

- **anchor** the AI back into the user’s real social world,
- provide a **soft safety net** when things escalate,
- minimise the need to “de-humanise” AI,
- while staying firmly within boundaries of:
  - consent,
  - minimal data,
  - privacy,
  - non-diagnostic framing.

It is not a replacement for professional help or legal emergency protocols.  
It is a carefully limited tool to help **people find each other** when it matters most.
