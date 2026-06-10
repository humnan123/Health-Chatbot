# General Health Query Chatbot

A conversational AI chatbot that answers general health-related questions using prompt engineering and safety filters, built with the Anthropic Claude API in a Jupyter Notebook.

---

##  Task Objective

Build a health information chatbot that:
- Accepts natural language health questions from users
- Returns clear, friendly, and accurate general health information
- Blocks or carefully handles harmful or sensitive queries
- Remembers conversation context across multiple turns

The goal is **not** to replace a doctor — but to make general health information more accessible and easy to understand.

---

## Models Applied
Google Gemini 3.5 Flash
Type: Large Language Model (LLM)

Access: Via Google GenAI Python SDK (google-genai library)

Role: Generates all health-related responses

Configuration:

Model Name: gemini-3.5-flash

Temperature: 0.5 — balances creative reasoning with factual consistency

System Prompt: Defines the assistant's persona, health-guidance scope, and safety boundaries

Conversation History: Managed as a list of dictionaries (role and parts array) to ensure multi-turn memory and context retention

### Prompt Engineering Techniques Used
| Technique | How it was applied |
|---|---|
| **Persona assignment** | "You are Gemma, a friendly health information assistant" |
| **Rule injection** | Explicit do/don't rules inside the system prompt |
| **Conditional prompting** | Extra empathy instructions added dynamically for sensitive topics |
| **Conversation history** | Full message history passed on every API call for contextual replies |

---

## Safety Design

Safety filter applied before and during every API call:

### Hard Block
Queries containing keywords like `overdose`, `kill myself`, `lethal dose` are blocked entirely and never reach the AI. The user receives a compassionate redirect message instead.

---

## Key Results and Findings

### What Worked Well
- **Prompt engineering significantly shaped output quality.** Adding clear persona rules and constraints produced more consistent, safe, and friendly responses compared to a plain API call with no system prompt.
- **Conversation history made the chatbot feel natural.** Without history, follow-up questions like "is that safe for kids?" had no context. With history, Hana correctly linked the follow-up to the previous topic.

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

## How to run this notebook
Clone this repository.

Create a file named .env in the root directory.

Add your API key to the file: GEMINI_API_KEY=your_api_key_here

Run the notebook cells; the client will automatically detect your key.

---

## File Structure

```
health-chatbot/
│
├── health_chatbot.ipynb   # Main Jupyter Notebook with all code
└── README.md              # This file
```

---

## ⚠️ Disclaimer

This chatbot provides **general health information only**. It is not a substitute for professional medical advice, diagnosis, or treatment. Always consult a qualified healthcare provider for personal health decisions.
