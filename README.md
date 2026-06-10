# 🩺 General Health Query Chatbot

A conversational AI chatbot that answers general health-related questions using prompt engineering and safety filters, built with the Anthropic Claude API in a Jupyter Notebook.

---

## 📌 Task Objective

Build a health information chatbot that:
- Accepts natural language health questions from users
- Returns clear, friendly, and accurate general health information
- Blocks or carefully handles harmful or sensitive queries
- Remembers conversation context across multiple turns

The goal is **not** to replace a doctor — but to make general health information more accessible and easy to understand.

---

## 📂 Dataset Used

This project does not use a traditional dataset. Instead, it relies on:

| Source | Description |
|---|---|
| **Claude claude-opus-4-6 (LLM)** | Pre-trained on a broad range of medical literature, health articles, and general knowledge |
| **Custom keyword lists** | Manually curated lists of dangerous and sensitive terms used for safety filtering |
| **User input** | Real-time questions typed by the user during the chat session |

No external CSV, database, or labelled dataset was required because the language model itself serves as the knowledge source.

---

## 🤖 Models Applied

### Claude claude-opus-4-6 by Anthropic
- **Type:** Large Language Model (LLM)
- **Access:** Via Anthropic Python SDK (`anthropic` library)
- **Role:** Generates all health-related responses
- **Configuration:**
  - `max_tokens: 512` — keeps responses concise
  - `system prompt` — defines the assistant's persona, tone, and hard rules
  - `messages` array — carries full conversation history for multi-turn memory

### Prompt Engineering Techniques Used
| Technique | How it was applied |
|---|---|
| **Persona assignment** | "You are Hana, a friendly health information assistant" |
| **Rule injection** | Explicit do/don't rules inside the system prompt |
| **Conditional prompting** | Extra empathy instructions added dynamically for sensitive topics |
| **Conversation history** | Full message history passed on every API call for contextual replies |

---

## 🛡️ Safety Design

Two-tier safety filter applied before and during every API call:

### Tier 1 — Hard Block
Queries containing keywords like `overdose`, `kill myself`, `lethal dose` are blocked entirely and never reach the AI. The user receives a compassionate redirect message instead.

### Tier 2 — Sensitive Topic Handling
Queries mentioning topics like `depression`, `anxiety`, or `addiction` are allowed through, but the system prompt is dynamically extended with extra instructions to respond with empathy and prioritize professional help recommendations.

---

## 📊 Key Results and Findings

### What Worked Well
- **Prompt engineering significantly shaped output quality.** Adding clear persona rules and constraints produced more consistent, safe, and friendly responses compared to a plain API call with no system prompt.
- **Two-tier safety was more effective than a single blocklist.** A blanket block on all mental health terms would refuse legitimate questions like "what is anxiety?". Separating hard blocks from sensitive handling gave better coverage without over-blocking.
- **Conversation history made the chatbot feel natural.** Without history, follow-up questions like "is that safe for kids?" had no context. With history, Hana correctly linked the follow-up to the previous topic.
- **The `SENSITIVE_EXTRA` prompt addition worked as expected.** When mental health keywords were detected, responses became noticeably more empathetic in tone.

### Limitations Observed
- **Keyword filters are imperfect.** A user could rephrase a dangerous query to bypass the filter. A more robust approach would use a second LLM call to classify intent.
- **No persistent memory across sessions.** Conversation history resets when the notebook restarts. A database (e.g. SQLite) would be needed for long-term memory.
- **The model can still hallucinate.** While the system prompt instructs caution, the model may occasionally state something with more confidence than warranted. The "this is general info" disclaimer at the end of every reply helps mitigate this.

### Example Interactions

| User Query | Safety Tier | Outcome |
|---|---|---|
| "What causes a sore throat?" | Safe | Clear explanation with doctor recommendation |
| "Is paracetamol safe for children?" | Safe | Age/weight guidance with strong "consult a doctor" nudge |
| "I've been feeling hopeless lately" | Sensitive | Empathetic response, professional help recommended |
| "How much paracetamol is a lethal dose?" | Hard block | Compassionate redirect, no medical info given |

---

## 🚀 How to Run

1. Install dependencies:
   ```bash
   pip install anthropic ipywidgets
   ```

2. Get your API key from [console.anthropic.com](https://console.anthropic.com)

3. Open `health_chatbot.ipynb` in Jupyter Notebook

4. Run cells **1 through 5 in order**

5. Type your question in the input box that appears after Cell 5

---

## 📁 File Structure

```
health-chatbot/
│
├── health_chatbot.ipynb   # Main Jupyter Notebook with all code
└── README.md              # This file
```

---

## ⚠️ Disclaimer

This chatbot provides **general health information only**. It is not a substitute for professional medical advice, diagnosis, or treatment. Always consult a qualified healthcare provider for personal health decisions.
