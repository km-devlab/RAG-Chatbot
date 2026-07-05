# 📚 Chat With Your Notes — A Weekend RAG Chatbot

A lightweight Retrieval-Augmented Generation (RAG) chatbot that lets you upload your own PDFs or notes and ask questions grounded in that content. Built as a weekend project — no GPU, no local storage burden, and free to run.

![Gradio chat interface](./screenshot.png)
*Replace `screenshot.png` in this folder with your own Gradio screenshot — see "Adding the screenshot" below.*

---

## ✨ Features

- **Chat with your own documents** — upload PDFs or `.txt` files and ask natural-language questions
- **Grounded answers only** — responses are generated strictly from retrieved context, with source file tags shown under each answer
- **Two interface options**:
  - A Gradio chat UI that runs directly inside Google Colab with a public shareable link
  - A Streamlit web app for local use or free deployment
- **Zero local storage cost** — vector embeddings are computed and stored in-memory during the session
- **Free to run** — uses Google's Gemini API free tier for generation and a local open-source model for embeddings

---

## 🧱 Tech Stack

| Component | Tool |
|---|---|
| LLM (generation) | Google Gemini API (`gemini-2.5-flash-lite`) |
| Embeddings | `sentence-transformers` (`all-MiniLM-L6-v2`) — runs locally, free |
| Vector store | ChromaDB (in-memory) |
| PDF parsing | PyPDF2 |
| UI (Colab) | Gradio |
| UI (local/deployed) | Streamlit |

---

## 📂 Project Files

```
├── ragchatbot.ipynb           # Full pipeline + Gradio UI — run this in Google Colab
├── screenshot.png             # Gradio chat UI screenshot (shown above)
├── sample_data/
│   └── unit.pdf                # Example document used to test the chatbot
└── README.md
```

> An optional Streamlit version (`app.py` + `requirements.txt`) exists for running this outside Colab as a standalone web app. See "Possible Next Steps" below if you want to add it later.

---

## 🚀 Getting Started

### Option A — Google Colab (fastest, no install)

1. Get a free Gemini API key at [aistudio.google.com/apikey](https://aistudio.google.com/apikey) (Google account only, no card needed)
2. Upload `ragchatbot.ipynb` to [colab.research.google.com](https://colab.research.google.com)
3. Run every cell from top to bottom:
   - Install dependencies
   - Paste your Gemini API key
   - Upload your PDFs/notes
   - Wait for chunking + embedding to finish
4. Run the final Gradio cell — it prints a public link like `https://xxxxx.gradio.live`
5. Open that link and start chatting

### Option B (optional, not yet built) — Streamlit

A Streamlit version can be added later for running this outside Colab as a standalone web app or free-hosted link. Not included in this repo yet — see "Possible Next Steps."

---

## 🖼️ Adding the screenshot

To include your own Gradio output image in this README:

1. Take a screenshot of your running Gradio chat interface
2. Save it in the same folder as this README, named `screenshot.png`
3. The `![Gradio chat interface](./screenshot.png)` line at the top of this file will then render it automatically on GitHub or in any Markdown viewer

---

## ⚙️ How It Works (RAG Pipeline)

1. **Extract** — text is pulled from uploaded PDFs/notes
2. **Chunk** — text is split into overlapping ~500-character chunks so context isn't lost at boundaries
3. **Embed** — each chunk is converted into a vector using `all-MiniLM-L6-v2`
4. **Store** — vectors are saved in a ChromaDB collection
5. **Retrieve** — when you ask a question, the top-matching chunks are pulled from the vector store
6. **Generate** — those chunks are passed as context to Gemini, which answers using only that information

---

## 🔧 Troubleshooting

| Symptom | Likely cause |
|---|---|
| Blank chat bubble, no error | Gemini returned an empty/filtered response — check the Colab cell output for a `DEBUG:` line |
| `429` error / rate limit | Free tier quota hit — wait for the per-minute or daily reset, or switch to `gemini-2.5-flash-lite` for higher limits |
| `NameError` in Gradio chat | Setup cells weren't re-run after a Colab runtime restart — re-run all cells from the top |
| Gradio cell "hangs" | Expected — the cell keeps running as long as the server is live. Look for the `Running on public URL:` line above the spinner |

---

## 🗺️ Possible Next Steps

- Add a standalone Streamlit web app (`app.py`) for running outside Colab, deployable free on Streamlit Community Cloud
- Persist the vector database (`chromadb.PersistentClient`) instead of in-memory storage
- Support multiple simultaneous document sets
- Swap in a different LLM by editing only the `ask_rag_chatbot` function
- Deploy the Gradio version permanently on Hugging Face Spaces (free)
