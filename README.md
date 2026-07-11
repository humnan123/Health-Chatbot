# 🩺 Context-Aware Health Chatbot Using RAG

A conversational health chatbot that retrieves answers from a custom knowledge base using Retrieval-Augmented Generation (RAG), maintains conversation history, and is deployed as a web app via Streamlit.

---

## 📌 Objective

Build a context-aware chatbot that:
- Answers general health questions grounded in a custom document corpus
- Remembers conversation history for natural multi-turn dialogue
- Filters dangerous queries before they reach the model
- Is deployed interactively via Streamlit

---

## 🧠 Methodology & Approach

### Stack
| Component | Tool used |
|---|---|
| LLM | Gemini 2.5 Flash (Google GenAI) |
| Embeddings | `all-MiniLM-L6-v2` via sentence-transformers |
| Vector store | FAISS |
| Document loading & splitting | LangChain |
| Deployment | Streamlit + ngrok (from Google Colab) |

### Pipeline
1. **Corpus creation** — health topics written as `.txt` files covering sore throat, paracetamol, and fever
2. **Chunking** — documents split into 300-character overlapping chunks using `RecursiveCharacterTextSplitter`
3. **Embedding** — each chunk vectorised using `all-MiniLM-L6-v2`
4. **Vector store** — vectors indexed in FAISS for fast similarity search
5. **Retrieval** — top 2 most relevant chunks fetched per query
6. **Augmentation** — retrieved chunks injected into the prompt as context
7. **Generation** — Gemini generates a response grounded in retrieved context
8. **Safety** — dangerous keywords blocked before retrieval; Gemini chat object handles conversation memory internally
9. **Deployment** — Streamlit UI exposed via ngrok tunnel from Google Colab

---

## 📊 Key Results & Observations

### Retrieval
- RAG correctly retrieved relevant chunks for all topics covered in the corpus
- For out-of-corpus topics, the model gracefully fell back to general knowledge as intended

### Safety
- All tested harmful queries were blocked by the keyword filter
- Legitimate questions about sensitive topics (e.g. "what is depression?") were correctly allowed through

### Conversation Memory
- Multi-turn follow-up questions resolved correctly across 3+ turns
- Gemini's built-in chat history handled memory without manual message management

### Response Quality
- Responses consistently included the medical disclaimer
- Tone was friendly and non-technical throughout
- Doctor recommendation included wherever relevant

### Limitations
- Small corpus (3 documents) limits coverage
- Keyword filter can be bypassed with rephrased queries
- No persistent memory across sessions — restarting Colab resets history

---

## 🚀 How to Run

1. Open `task4.ipynb` in Google Colab
2. Run all cells in order from top to bottom
3. Enter your Gemini API key and ngrok token when prompted
4. Click the ngrok URL printed in the final cell to open the Streamlit app

---

⚠️ **Disclaimer:** This chatbot provides general health information only. It is not a substitute for professional medical advice, diagnosis, or treatment.
