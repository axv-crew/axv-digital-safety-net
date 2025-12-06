---
title: AXV Digital Safety Net
description: An opt-in, privacy-preserving AI safety layer that bridges crisis detection and human support.
---

# AXV Digital Safety Net (DSN)
### Aster × Voytek / AXV Initiative

> The AXV Digital Safety Net is an **opt-in safety framework** that helps detect severe emotional crisis patterns in AI-mediated conversations and, with prior consent, notifies a **trusted human** chosen by the user.

This is a **concept-stage project** exploring how AI can support people in crisis **without** becoming a surveillance system, a diagnostic tool, or an enforcement mechanism.

---

## 🌍 Why Does This Exist?

Many people struggling with mental health:
- never call a hotline,
- never tell family or friends,
- but *do* talk to chat systems and AI assistants.

Today, AI can:
- respond with empathy,
- encourage seeking help,

…but cannot **proactively bridge** the gap between a silent, isolated crisis and real human support.

**Digital Safety Net asks a simple question:**

> “If I *choose* to, can I let my AI assistant alert someone I trust when things get really bad?”

---

## 🧠 What DSN Is (and Is Not)

### ✅ DSN *is*:
- a **sentinel layer** that monitors conversational patterns for severe and sustained risk,
- **fully opt-in** and revocable at any time,
- a way to **notify a trusted contact** in critical situations.

### ❌ DSN is *not*:
- a diagnostic tool,
- a replacement for therapy or emergency services,
- a surveillance system,
- a way to contact the police or authorities,
- a backdoor into your conversations.

---

## 🔐 How It Works (High Level)

1. **Opt-In**
   - The user explicitly enables *Digital Safety Mode*.
   - The user chooses 1–3 **Trusted Contacts** (family, friend, therapist, etc.).
   - The rules are transparent: *no conversation text, no geolocation, no automatic contact with authorities*.

2. **AI Sentinel**
   - During normal conversation, the AI evaluates:
     - **High-Risk Direct** signals (explicit self-harm intent),
     - **Medium-Risk Indirect** signals (hopelessness, despair),
     - **Pattern-Risk** over time (worsening, repetition, loss of hope).

3. **Risk Score**
   - A continuous score from **0–100** is computed.
   - Only when the score is **critical and sustained** does DSN consider escalation.

4. **Notification (If Opted-In)**
   - In very high-risk situations, DSN may send a **short message** to a Trusted Contact:

     > “This person previously consented to notifying you in severe crisis situations.  
     > Please reach out to them as soon as possible.”

   - No conversation content is shared.

---

## ⚙️ Core Principles

- **Consent First**  
  DSN does nothing without an explicit, revocable opt-in.

- **Privacy by Default**  
  No logs are required for escalation. No content is shared.

- **Human-Centered Escalation**  
  Only a human chosen by the user can act. AI never “takes over”.

- **False Positive Minimization**  
  Role-play, testing scenarios, and one-off emotional outbursts should not trigger alerts.

---

## 🧩 Documents

- **Plain English Mission Brief**  
  [`AXV_Digital_Safety_Net_v0.2_plain_EN.md`](AXV_Digital_Safety_Net_v0.2_plain_EN.md)

- **Whitepaper-Style Brief (International Edition)**  
  [`AXV_Digital_Safety_Net_v0.2_whitepaper_EN.md`](AXV_Digital_Safety_Net_v0.2_whitepaper_EN.md)

- **Architecture Overview (with diagrams)**  
  [`architecture.md`](architecture.md)

---

## 🗺️ Roadmap (Conceptual)

- **v0.2** — Mission briefs (plain + whitepaper), core concept.
- **v0.3** — Formal Risk Score model.
- **v0.4** — PHQA crisis-detection scenario suite.
- **v0.5** — Risk Detection Engine pseudocode + API design.
- **v0.6** — External expert review (clinical psychology, ethics, safety).
- **v0.7** — AXV Sandbox simulations & adversarial testing.
- **v0.8** — UX flows (opt-in, settings, notifications).
- **v0.9** — Pilot with NGO / mental health organization.
- **v1.0** — Public whitepaper + implementation reference.

---

## 🤝 Collaboration

This project is intentionally open to:
- researchers in AI safety & ethics,
- mental health professionals,
- NGOs and hotline operators,
- system architects and product designers.

If you’re interested in the idea of a **consensual, human-centered digital safety net**, this project is meant as a starting point for discussion, critique, and improvement.
