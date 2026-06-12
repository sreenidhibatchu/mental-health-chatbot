# AI-Powered Chatbot for Mental Health Support

> A conversational AI assistant designed to provide empathetic, privacy-respecting mental health support — with built-in crisis escalation protocols that connect users to real human resources when risk is detected.

---

## Motivation

Access to mental health support is deeply unequal. Long waitlists, cost barriers, and stigma mean millions of people never get help — or wait until a crisis. While AI cannot and should not replace therapists, it can serve as a first point of contact: a low-barrier, always-available space for someone to feel heard, get coping strategies, and — critically — be connected to professional help when they need it most.

This project was built with one guiding principle: **AI should expand access to human support, not replace it.**

---

## What It Does

- Engages users in supportive, empathetic conversation using a GPT-based language model (DialoGPT) fine-tuned on counseling-style dialogue
- Detects escalating distress signals in conversation (explicit crisis language, emotional intensity, expressions of hopelessness) and triggers a **crisis escalation protocol**
- On escalation: pauses the AI conversation, surfaces real crisis resources (hotlines, local services), and encourages the user to reach out to a human
- Stores session data locally only — no data transmitted externally, no personally identifiable information retained after session ends
- Provides psychoeducational resources (breathing exercises, grounding techniques, sleep hygiene tips) woven naturally into conversation

---

## Architecture

```
User Input
    │
    ▼
┌─────────────────────────┐
│   Risk Detection Layer   │  ◄── Keyword + sentiment classifier
│  (pre-response screen)   │      flags high-risk inputs before
└─────────┬───────────────┘      model responds
          │
    ┌─────┴──────┐
    │            │
 Low risk     High risk
    │            │
    ▼            ▼
DialoGPT     Crisis Escalation
Response     Protocol + Resources
    │
    ▼
Response Audit
(harmful output filter)
    │
    ▼
User sees response
```

---

## Tech Stack

| Component | Technology |
|---|---|
| Language Model | DialoGPT (Microsoft) via Hugging Face Transformers |
| Risk Detection | Rule-based classifier + fine-tuned sentiment model |
| Backend API | Flask (Python) |
| Session Storage | SQLite (local only) |
| Front-end | HTML/CSS/JavaScript (lightweight chat UI) |
| Deployment | Local / localhost (privacy-by-design) |

---

## Key Design Decisions

### 1. Privacy by Default
All session data is stored in a local SQLite database. No data is sent to external APIs or third-party servers. The database is wiped at session end unless the user opts to save a summary for themselves.

### 2. Human-in-the-Loop Escalation
The chatbot does not attempt to handle crisis situations autonomously. When the risk detection layer identifies high-risk language, the AI steps back and surfaces:
- National crisis hotlines (988 Suicide & Crisis Lifeline, Crisis Text Line)
- Local emergency services guidance
- A clear, calm message encouraging the user to speak to a human

This is intentional. High-stakes decisions about someone's safety should never be fully automated.

### 3. Response Auditing
Before every response is shown to the user, it passes through an audit layer that checks for outputs that could cause harm — including accidental provision of dangerous information, dismissive responses to distress, or language that could worsen shame or stigma.

### 4. Empathy Over Accuracy
The model is tuned to prioritize warm, validating responses over factually dense ones. In a mental health context, feeling heard matters more than receiving a clinically precise answer.

---

## Ethical Considerations

This project raised hard questions that shaped every design choice:

- **Should AI be involved in mental health at all?** We concluded yes — with strict guardrails — because the alternative (no support for people who can't access care) is worse.
- **What happens when the model gets it wrong?** The escalation layer and response audit exist precisely because the model will sometimes get it wrong. Human oversight is not optional here.
- **Who is this for?** People experiencing mild-to-moderate distress who need a low-barrier first step. Not a replacement for therapy. Not for acute psychiatric emergencies.
- **Informed consent:** Users are shown a clear disclaimer at the start of every session explaining what the chatbot is, what it isn't, and when to call a professional.

---

## Limitations & What I'd Do Differently

- **DialoGPT has significant limitations** for this use case — it was trained on Reddit data, not counseling transcripts, and can produce tonally inconsistent responses. A production version would use a model fine-tuned specifically on therapeutic dialogue (or the Anthropic Claude API, which has built-in safety properties better suited to sensitive contexts).
- **The risk detection layer is rule-heavy** and misses nuanced expressions of distress that don't use explicit keywords. A more sophisticated approach would use a purpose-built crisis detection model.
- **No clinical validation** — the chatbot has not been evaluated by mental health professionals or tested in a clinical setting. This would be essential before any real deployment.
- **English only** — mental health support is needed across languages; this is a significant equity gap in the current version.

---

## Running Locally

```bash
# Clone the repo
git clone https://github.com/sreenidhi-batchu/mental-health-chatbot
cd mental-health-chatbot

# Install dependencies
pip install -r requirements.txt

# Run the Flask app
python app.py

# Open in browser
http://localhost:5000
```

---

## Project Context

Built as a group project during my M.S. in Computer Science (2024). My primary contributions were the **crisis escalation protocol design**, the **response audit layer**, and the **backend Flask API**. This was also the project that deepened my conviction that AI built for vulnerable populations requires a fundamentally different design mindset than AI built for productivity tools.

---

## Resources & Further Reading

- [988 Suicide & Crisis Lifeline](https://988lifeline.org)
- [Crisis Text Line](https://www.crisistextline.org)
- [Hugging Face DialoGPT](https://huggingface.co/microsoft/DialoGPT-medium)
- [WHO Mental Health Action Plan](https://www.who.int/publications/i/item/9789240031029)

---

*If you or someone you know is in crisis, please contact the 988 Suicide & Crisis Lifeline by calling or texting **988**.*
