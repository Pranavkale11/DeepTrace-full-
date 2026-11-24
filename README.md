<div align="center">

# 🔍 DeepTrace

### AI-Powered Campaign Detection & Influence Operation Analysis

[![TypeScript](https://img.shields.io/badge/TypeScript-70%25-3178C6?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
[![Python](https://img.shields.io/badge/Python-28.7%25-3776AB?style=for-the-badge&logo=python)](https://www.python.org/)
[![Next.js](https://img.shields.io/badge/Next.js-16-000000?style=for-the-badge&logo=nextdotjs)](https://nextjs.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-Backend-009688?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![NLP](https://img.shields.io/badge/NLP-Sentence--Transformers-FF6F00?style=for-the-badge&logo=huggingface)](https://www.sbert.net/)
[![NetworkX](https://img.shields.io/badge/Graph-NetworkX-0099CC?style=for-the-badge)](https://networkx.org/)

> **Hardened Intelligence for the Modern Information Landscape** — DeepTrace detects inauthentic behavior and coordinated influence operations across digital ecosystems using real NLP, graph analysis, and explainable AI.

</div>

---

## 📋 Table of Contents

- [🌟 Overview](#-overview)
- [✨ Key Capabilities](#-key-capabilities)
- [🏗️ System Architecture](#️-system-architecture)
- [🧠 AI Pipeline Deep Dive](#-ai-pipeline-deep-dive)
- [⚙️ Risk Scoring Engine](#️-risk-scoring-engine)
- [📦 Backend Services](#-backend-services)
- [🛠️ Tech Stack](#️-tech-stack)
- [🚀 Quick Start](#-quick-start)
- [🐛 Troubleshooting](#-troubleshooting)
- [📁 Project Structure](#-project-structure)
- [🗺️ Roadmap](#️-roadmap)
- [🤝 Contributing](#-contributing)

---

## 🌟 Overview

DeepTrace is a full-stack intelligence platform that identifies **coordinated inauthentic behavior** and **influence operations** on social media. Unlike tools that rely on random simulation or hardcoded heuristics, DeepTrace runs **real AI models end-to-end**:

- 🧠 **Sentence Transformers** turn text into 384-dimensional semantic vectors
- 🕸️ **NetworkX graph analysis** detects coordination networks
- 📊 **Weighted multi-factor risk scoring** explains every threat classification

The result: transparent, explainable, evidence-backed detections that analysts and decision-makers can trust and act on.

---

## ✨ Key Capabilities

| Capability | Description |
|---|---|
| 📝 **Narrative Similarity Detection** | Sentence-Transformers (all-MiniLM-L6-v2) detect semantically duplicated or paraphrased content across platforms |
| 🔗 **Campaign Clustering** | Graph-theoretic analysis via NetworkX identifies clusters of accounts with synchronized posting behavior |
| 🎯 **Risk Scoring Engine** | Multi-signal weighted formula combining narrative duplication, cluster density, bot probability, and temporal coordination |
| 🔍 **Explainable AI Outputs** | Human-readable explanations for every risk score with specific threat indicators and remediation recommendations |
| 📊 **Dashboard Visualization** | Recharts + Framer Motion present complex network metrics in a clean, professional interface |
| ⚡ **CPU-Safe Inference** | Optimized for production — no GPU required, runs on standard hardware |
| 🔄 **Real-Time Analytics** | Dynamic trend data with rolling 7-day windows computed from actual post timestamps |

---

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                    FRONTEND (Next.js 16)                     │
│           React 19 + TailwindCSS + Framer Motion             │
│  ┌─────────────┐  ┌────────────────┐  ┌──────────────────┐  │
│  │  Campaign   │  │   Analytics    │  │  Risk Score      │  │
│  │  Dashboard  │  │   Dashboard    │  │  Detail View     │  │
│  └─────────────┘  └────────────────┘  └──────────────────┘  │
└──────────────────────────┬───────────────────────────────────┘
                           │ REST API (JSON)
┌──────────────────────────▼───────────────────────────────────┐
│                  BACKEND (FastAPI + Python)                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                   AI PIPELINE                        │    │
│  │                                                      │    │
│  │  [1] Embedding     [2] Similarity    [3] Graph       │    │
│  │  Service           Search            Analysis        │    │
│  │  all-MiniLM-L6-v2  Cosine (≥0.75)   NetworkX        │    │
│  │       ↓                ↓                 ↓           │    │
│  │  [4] Risk Scoring Engine → Explainable Output        │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌────────────┐  ┌────────────┐  ┌───────────────────────┐  │
│  │ api/       │  │ api/       │  │ api/                  │  │
│  │ analyze.py │  │ campaigns  │  │ analytics.py          │  │
│  └────────────┘  └────────────┘  └───────────────────────┘  │
└──────────────────────────┬───────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│              Mock Data / JSON Store (extensible)             │
│         (PostgreSQL / MongoDB integration roadmapped)        │
└──────────────────────────────────────────────────────────────┘
```

---

## 🧠 AI Pipeline Deep Dive

DeepTrace's analysis runs in 4 sequential stages after receiving campaign data:

### Stage 1 — Vectorization (`embedding_service.py`)

Text content is encoded into **384-dimensional embeddings** using the `all-MiniLM-L6-v2` sentence transformer model (~80MB). An embedding cache prevents redundant computation.

```
"India failed again" → [0.21, -0.08, 0.74, ... ] (384 values)
```

Key functions: `encode()`, `calculate_similarity_matrix()`, `find_similar_pairs()`, `get_narrative_clusters()`

### Stage 2 — Content Correlation

Cosine similarity is computed between all post pairs. Pairs with similarity **≥ 0.75** are flagged as **potential narrative duplicates** — a key indicator of coordinated inauthentic behavior.

### Stage 3 — Network Analysis (`clustering_service.py`)

A multi-layer graph is constructed using NetworkX:

```
Nodes:  Account nodes (with bot_probability)
        Post nodes (with content, hashtags, timestamps)

Edges:  Authorship links
        Content similarity links (cos_sim ≥ 0.75)
        Hashtag co-occurrence links
        Temporal proximity links (posts within minutes of each other)
```

Louvain community detection then identifies dense subgraphs — tightly coordinated clusters of accounts.

### Stage 4 — Risk Scoring (`risk_scoring_service.py`)

All signals are aggregated into a single risk score with a transparent formula:

```
risk_score = 0.30 × narrative_similarity
           + 0.25 × cluster_density
           + 0.25 × bot_probability
           + 0.20 × temporal_coordination
```

The engine then generates natural-language explanations and actionable recommendations.

---

## ⚙️ Risk Scoring Engine

### Threat Level Thresholds

| Level | Score | Meaning |
|---|---|---|
| 🔴 **CRITICAL** | ≥ 0.90 | Immediate action required |
| 🟠 **HIGH** | ≥ 0.75 | Likely coordinated campaign |
| 🟡 **MEDIUM** | ≥ 0.55 | Suspicious signals, monitor closely |
| 🟢 **LOW** | < 0.55 | No significant coordination detected |

### Example Output

```json
{
  "risk_score": 0.922,
  "risk_level": "critical",
  "confidence": 0.91,
  "explanation": "HIGH content similarity (86%) detected across 3 posts sharing identical narratives. Bot involvement: 100%. Synchronized posting (88%). Immediate investigation recommended.",
  "risk_components": {
    "narrative_similarity": { "score": 0.86, "contribution": 0.258 },
    "cluster_density":      { "score": 0.95, "contribution": 0.238 },
    "bot_involvement":      { "score": 1.00, "contribution": 0.250 },
    "temporal_coordination":{ "score": 0.88, "contribution": 0.176 }
  },
  "threat_indicators": [
    { "type": "content_duplication", "value": "3 posts", "description": "3 posts with identical narratives" },
    { "type": "bot_network",         "value": "3/3 accounts", "description": "All accounts flagged as automated" }
  ],
  "recommendations": ["Escalate to Trust & Safety", "Cross-platform correlation check"]
}
```

---

## 📦 Backend Services

### `services/embedding_service.py`
Handles all NLP vectorization. Converts raw text to embeddings, computes pairwise cosine similarity, detects duplicate narratives, and groups similar posts into narrative clusters. Includes an in-memory embedding cache for performance.

### `services/clustering_service.py`
Builds the coordination graph with NetworkX. Multi-layer edges represent different coordination signals (content, hashtags, timing, authorship). Applies Louvain community detection and calculates a coordination score per cluster.

### `services/risk_scoring_service.py`
The explainability layer. Combines all upstream signals using the weighted formula, generates threat indicator objects with evidence, writes natural-language explanations, and outputs prioritized remediation recommendations.

### `api/analyze.py`
The main analysis endpoint. Fully rewritten from random simulation to a real async AI pipeline. Uses `asyncio.sleep` for non-blocking UX feedback while processing.

### `api/campaigns.py`
Enhanced to compute real-time AI analysis on every request rather than returning static JSON scores. Returns enriched responses including `coordination_metrics`, `similar_content_pairs`, `confidence`, and `narrative_analysis`.

### `api/analytics.py`
Updated to derive all stats from actual data — real 24h/7d campaign counts, and a 7-day rolling trend window from post timestamps.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend Framework** | Next.js 16, React 19 |
| **Frontend Styling** | TailwindCSS, Framer Motion |
| **Frontend Charts** | Recharts, Lucide React |
| **Backend Framework** | FastAPI (Python) |
| **NLP Model** | Sentence-Transformers `all-MiniLM-L6-v2` |
| **Graph Analysis** | NetworkX (Louvain community detection) |
| **ML Utilities** | scikit-learn, NumPy |
| **Deep Learning** | PyTorch (CPU-optimized) |
| **Type Safety** | TypeScript (frontend), Pydantic (backend) |
| **Linting** | ESLint (`eslint.config.mjs`) |

---

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- Node.js 18+
- ~500MB disk space (model download on first run)

### 1. Clone the Repository

```bash
git clone https://github.com/Pranavkale11/DeepTrace-.git
cd DeepTrace-
```

### 2. Backend Setup

```bash
cd backend
python -m venv venv

# Windows
.\venv\Scripts\Activate.ps1

# macOS / Linux
source venv/bin/activate

pip install -r requirements.txt
python main.py
```

On first run, the NLP model (~80MB) is downloaded automatically. You'll see:

```
🔄 Loading embedding model: all-MiniLM-L6-v2...
✅ Embedding model loaded successfully!
✅ Loaded 5 campaigns, 10 posts
🚀 DeepTrace Backend API Starting...
```

| Endpoint | URL |
|---|---|
| 📚 Interactive API Docs | http://127.0.0.1:8000/docs |
| ❤️ Health Check | http://127.0.0.1:8000/health |

### 3. Frontend Setup

Open a new terminal from the project root:

```bash
npm install
npm run dev
```

| Endpoint | URL |
|---|---|
| 🌐 Application | http://localhost:3000 |

### 4. Run Analysis

```bash
curl -X POST http://localhost:8000/api/analyze
```

Or visit `http://localhost:8000/docs` and use the interactive Swagger UI.

---

## 🐛 Troubleshooting

### Port 8000 already in use (Windows)
```powershell
Stop-Process -Id (Get-NetTCPConnection -LocalPort 8000).OwningProcess -Force
```

### Port 3000 already in use (Windows)
```powershell
Stop-Process -Id (Get-NetTCPConnection -LocalPort 3000).OwningProcess -Force
```

### PowerShell execution policy error
Run once as Administrator:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### PyTorch / Torch CPU installation errors
```bash
pip install torch --index-url https://download.pytorch.org/whl/cpu
```

### Model download timeout
The `all-MiniLM-L6-v2` model is ~80MB. Ensure a stable internet connection on first run. Subsequent runs use the local cache.

---

## 📁 Project Structure

```
DeepTrace-/
│
├── backend/                         # Python FastAPI Backend
│   ├── api/
│   │   ├── analyze.py               # 🧠 Main AI analysis endpoint
│   │   ├── campaigns.py             # 📋 Campaign management + AI enrichment
│   │   └── analytics.py             # 📊 Real-time analytics & trend data
│   ├── services/
│   │   ├── embedding_service.py     # 🔤 NLP vectorization (Sentence-Transformers)
│   │   ├── clustering_service.py    # 🕸️  Graph-based coordination detection
│   │   └── risk_scoring_service.py  # 🎯 Explainable risk scoring engine
│   ├── models/                      # Pydantic schemas & response types
│   ├── utils/                       # Data loading & helper utilities
│   ├── mock_data/                   # Simulation datasets (JSON)
│   ├── requirements.txt             # Python dependencies
│   └── main.py                      # Application entry point
│
├── src/                             # Next.js Frontend Source
│   ├── app/                         # App Router pages
│   ├── components/                  # Reusable UI components
│   └── lib/                         # Frontend utilities & API client
│
├── public/                          # Static assets
│
├── IMPLEMENTATION_SUMMARY.md        # Detailed AI upgrade changelog
├── INTEGRATION_GUIDE.md             # Frontend ↔ Backend integration guide
├── next.config.ts                   # Next.js configuration
├── tsconfig.json                    # TypeScript configuration
├── eslint.config.mjs               # ESLint rules
└── package.json                     # Node.js dependencies
```

---

## 🗺️ Roadmap

| Feature | Description | Priority |
|---|---|---|
| 🔄 **Real-Time API Ingestion** | Live social media streams (X, Telegram, Reddit) for sub-minute detection | 🔴 High |
| ☁️ **Cloud-Scale Deployment** | Containerized microservices with Redis caching and GPU-accelerated inference | 🔴 High |
| 🌐 **Multi-Language Support** | Hindi and regional Indian language NLP models | 🟡 Medium |
| 🎭 **Deepfake Detection** | Multimodal analysis of AI-generated profile pictures within coordination networks | 🟡 Medium |
| 💬 **Counter-Narrative AI** | Generative AI module to draft proactive counter-disinformation messages | 🟡 Medium |
| 🗄️ **Database Integration** | PostgreSQL / MongoDB persistence replacing current JSON mock store | 🟠 Next |
| 🔐 **Auth & Multi-Tenancy** | JWT-based authentication and organization-level data isolation | 🟠 Next |

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

<div align="center">

Made with ❤️ by [Pranav Kale](https://github.com/Pranavkale11)

⭐ Star this repo if you found it useful!

*Crafted with precision for the modern intelligence landscape.*

</div>
