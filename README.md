# 🔷 PrismRAG — Multimodal Agentic RAG

<p align="center">
  <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black"/>
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white"/>
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white"/>
  <img src="https://img.shields.io/badge/Google_Gemini-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white"/>
  <img src="https://img.shields.io/badge/Three.js-000000?style=for-the-badge&logo=threedotjs&logoColor=white"/>
  <img src="https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white"/>
  <img src="https://img.shields.io/badge/Python_3.13-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
</p>

<p align="center">
  <a href="https://multimodal-rag-lovat.vercel.app">
    <img src="https://img.shields.io/badge/🌐_Live_Demo-multimodal--rag--lovat.vercel.app-f54e00?style=for-the-badge"/>
  </a>
  <a href="https://github.com/avinashreddy09/PrismRAG">
    <img src="https://img.shields.io/badge/GitHub-PrismRAG-181717?style=for-the-badge&logo=github&logoColor=white"/>
  </a>
</p>

> A multimodal agentic RAG system that embeds **text, URLs, PDFs, images, audio, and video** into a single Gemini vector space — retrieves evidence via cosine similarity — and uses a **Google ADK agent** to generate grounded, cited answers. Features a **real-time 3D embedding visualization** built with Three.js. No external vector database.

---

## 📌 Project Overview

PrismRAG lets you upload content from **6 different modalities** into one shared 768-dimensional vector space, then ask questions and get **grounded, cited answers** — not hallucinations. It uses **Google Gemini Embedding 2** for embeddings, **Google ADK** for agentic reasoning, and a **Three.js 3D visualization** to show how your documents cluster in embedding space.

---

## 🧠 Core Concepts

### What is RAG?
**Retrieval-Augmented Generation** — instead of asking an LLM to answer from memory (which can hallucinate), you:
1. **Retrieve** the most relevant evidence from your own sources
2. **Augment** the LLM's prompt with that evidence
3. **Generate** an answer grounded in real data

### What is "Agentic" RAG?
A traditional RAG pipeline is linear: embed → retrieve → answer. **Agentic RAG** wraps this in a **Google ADK agent** that decides *when* and *how* to use retrieval tools, inspect the workspace, and coordinate multiple steps — making it more flexible and reliable.

### What is "Multimodal"?
Most RAG systems handle text only. **PrismRAG handles 6 modalities** — all embedded into the **same 768-dimensional vector space** so they can be compared directly.

---

## 🏗️ Architecture

```
┌─────────────────┐
│  User Uploads   │  Text / URL / PDF / Image / Audio / Video
└────────┬────────┘
         │
         ▼
┌─────────────────────────────────────┐
│  Gemini Embedding 2                 │
│  • Chunks text (~170 words, 35 overlap)
│  • Inline for small files (< 18 MB)
│  • File API for large / audio / video
│  • Blends media + title/notes vectors (68% / 32%)
└────────┬────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────┐
│  In-Memory Vector Store             │
│  (MultimodalRagStore)               │
│  • Stores vectors + metadata        │
│  • PCA projection to 3D (pure Python)
└────────┬────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────┐
│  User Asks a Question               │
│  • Query embedded with task prefix  │
│  • Cosine similarity per chunk      │
│  • Best chunk per source, top-K     │
└────────┬────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────┐
│  Google ADK Agent                   │
│  • inspect_embedding_space tool     │
│  • retrieve_relevant_context tool   │
│  • Grounded answer with citations   │
└────────┬────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────┐
│  Frontend Display                   │
│  • Answer + citation panel          │
│  • Agent trace (3-step ADK path)    │
│  • 3D embedding space + query point │
└─────────────────────────────────────┘
```

---

## 🎯 What Makes This Stand Out

| Feature | Why It's Impressive |
|---|---|
| **True Multimodal** | Handles 6 modalities in one vector space — not just text |
| **Agentic, Not Linear** | Google ADK for reasoning, not a fixed pipeline |
| **No Vector DB** | In-memory store + pure-Python PCA — deploys anywhere |
| **3D Visualization** | Interactive embedding space is rare in student projects |
| **Cited Answers** | Every answer is grounded with source + similarity score |
| **Production-Ready** | Deployed on Vercel, CORS handled, SSRF protected |
| **Custom UI** | Hand-built dark theme with orange accent — not a template |

---

## 🛠️ Tech Stack

### Backend
| Layer | Technology |
|---|---|
| Web Framework | FastAPI + Uvicorn |
| Embeddings | Gemini Embedding 2 (`gemini-embedding-2`, 768 dims) |
| LLM | Gemini 3 Flash Preview (`gemini-3-flash-preview`) |
| Agent Framework | Google ADK |
| Vector Math | Pure Python (cosine similarity + PCA via power iteration) |
| HTTP Client | httpx, aiohttp |
| HTML Parsing | BeautifulSoup4 |

### Frontend
| Layer | Technology |
|---|---|
| Framework | React 18 + TypeScript |
| Build Tool | Vite |
| 3D Rendering | Three.js |
| Icons | lucide-react |
| Styling | Vanilla CSS (custom dark theme) |

### Deployment
| Layer | Service |
|---|---|
| Frontend + Backend | Vercel Services |
| Domain | `multimodal-rag-lovat.vercel.app` |

---

## 📁 Project Structure

```
multimodal_agentic_rag/
├── vercel.json                          # Vercel multi-service config
├── README.md
├── backend/
│   ├── requirements.txt
│   ├── app_state.py                     # Shared RAG_STORE
│   ├── rag_store.py                     # Core embedding + retrieval logic
│   ├── server.py                        # FastAPI app
│   └── agentic_rag_agent/
│       ├── __init__.py
│       └── agent.py                     # Google ADK agent + tools
└── frontend/
    ├── package.json
    ├── vite.config.ts
    ├── index.html
    └── src/
        ├── App.tsx                      # Main React app
        ├── main.tsx
        └── styles.css
```

---

## 🔌 API Reference

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/api/health` | Liveness check + dimensions + source count |
| `GET` | `/api/space` | Current sources, points, projection metadata |
| `POST` | `/api/sources/text` | Add a text source |
| `POST` | `/api/sources/url` | Fetch and index a public URL (SSRF-protected) |
| `POST` | `/api/sources/file` | Upload PDF, image, audio, or video |
| `DELETE` | `/api/sources/{id}` | Remove a source and its chunks |
| `POST` | `/api/ask` | Retrieve, run ADK agent, return cited answer |

---

## 🔄 User Flow

### Adding a Source
1. Select **Text / URL / File** tab in the Source Manager
2. Enter title + content (or pick a file)
3. Click **Add source**
4. Backend chunks text → calls Gemini Embedding 2 → stores vectors
5. 3D view re-renders with the new point

### Asking a Question
1. Type question → click **Ask question**
2. Backend embeds query → cosine similarity → best chunk per source → top-K
3. ADK agent inspects workspace → retrieves context → synthesizes answer
4. Frontend renders:
   - ✅ Answer with headings and bullet lists
   - 📊 Citations with similarity score bars
   - 🔍 Agent trace (3-step ADK path)
   - 🟠 Orange query point in 3D space
   - ✨ Cited sources highlighted with glowing halos

---

## ⚙️ Getting Started

### Prerequisites
- Python 3.13+
- Node.js 18+
- Google Gemini API key

### Backend

```bash
git clone https://github.com/avinashreddy09/PrismRAG.git
cd PrismRAG/backend

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Set your API key
export GEMINI_API_KEY=your_key_here

uvicorn server:app --reload
# → http://localhost:8000
```

### Frontend

```bash
cd ../frontend
npm install
npm run dev
# → http://localhost:5173
```

### Environment Variables

```env
GEMINI_API_KEY=your_gemini_api_key
ALLOWED_ORIGINS=http://localhost:5173
```

---

## 📊 Quick Reference

| Item | Value |
|---|---|
| **Live URL** | https://multimodal-rag-lovat.vercel.app |
| **GitHub** | https://github.com/avinashreddy09/PrismRAG |
| **Embedding Model** | `gemini-embedding-2` (768 dims) |
| **LLM** | `gemini-3-flash-preview` |
| **Agent Framework** | Google ADK |
| **Chunk Size** | 170 words, 35 overlap |
| **File API Limit** | 18 MB |
| **Media Blend Ratio** | 68% media / 32% text |
| **Python Version** | 3.13 |
| **Deployment** | Vercel Services |

---

## 🔮 Future Plans

- [ ] **pgvector / Qdrant** — persistent storage, scale beyond memory
- [ ] **Re-ranking** — cross-encoder between retrieval and agent
- [ ] **Streaming answers** — stream tokens from Gemini instead of waiting
- [ ] **Background ingestion** — Celery / RQ for large video uploads
- [ ] **Auth + multi-tenancy** — different users, different workspaces
- [ ] **Evals** — track citation precision and answer faithfulness over time
- [ ] **More modalities** — spreadsheets, 3D models, code files

---

## 👨‍💻 Author

**Avinash Reddy**

[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/avinashreddy09)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/avinashreddy09/)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://avinash-reddy-portfolio.vercel.app/)
[![Gmail](https://img.shields.io/badge/Gmail-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:avinashreddydonthireddy2006@gmail.com)

---

<div align="center">

**Built with ❤️ by Avinash Reddy**

⭐ Star this repo if you found it useful!

</div>
