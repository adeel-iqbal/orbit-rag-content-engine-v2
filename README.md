# ORBIT — RAG Content Engine v2
### *Intelligence in Motion*

> Multi-agent RAG system that ingests research documents and generates platform-ready social media content — with branded image rendering, live web intelligence, and an orbital UI.

---

## What Is This?

**ORBIT** is an AI-powered content automation system built for organizations that produce research — industry reports, surveys, policy papers, whitepapers — and need to turn that research into social media content consistently and at scale.

You feed it documents. It builds a searchable knowledge base. Specialized AI agents (called **Satellites**) retrieve the most relevant data and write platform-native content — LinkedIn posts, Instagram carousels with rendered slides, YouTube scripts, X threads — all grounded in your actual research, with no hallucinated statistics.

Two modes:

- **Launch** — Generate content from your document knowledge base on any topic, any platform
- **Flare** — Respond to breaking events by combining live web intelligence (Perplexity) with your historical research data, producing analyst-style reactive content in minutes

---

## Screenshots

<table>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.32.50 AM.png" width="380" alt="Mission Control Dashboard"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.33.50 AM.png" width="380" alt="Launch — Satellite & Platform Selection"/></td>
  </tr>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.34.04 AM.png" width="380" alt="Launch — Topic, Tone & Year Range"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.41.50 AM.png" width="380" alt="Mission Status — Orbital Loader"/></td>
  </tr>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.42.07 AM.png" width="380" alt="Mission Status — Research Findings"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.42.16 AM.png" width="380" alt="LinkedIn Post Output"/></td>
  </tr>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.43.25 AM.png" width="380" alt="X Thread Output"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.44.04 AM.png" width="380" alt="Instagram Carousel Content"/></td>
  </tr>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.44.17 AM.png" width="380" alt="Generate Slide Images Button"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.44.30 AM.png" width="380" alt="Rendered Carousel Slide Images — Grid"/></td>
  </tr>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.44.47 AM.png" width="380" alt="Carousel Slides — Data Charts"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.44.55 AM.png" width="380" alt="Carousel Slides — Insight & CTA"/></td>
  </tr>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.45.05 AM.png" width="380" alt="X Card Rendered PNG"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.45.24 AM.png" width="380" alt="YouTube Script Output"/></td>
  </tr>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.45.33 AM.png" width="380" alt="Flare — Reactive Content Form"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.46.07 AM.png" width="380" alt="Flare — Mission Status & Research Synthesis"/></td>
  </tr>
  <tr>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.46.16 AM.png" width="380" alt="Flight Log — Generation History"/></td>
    <td><img src="Screenshots/Screenshot 2026-04-01 at 11.46.25 AM.png" width="380" alt="System Status — API & Collection Health"/></td>
  </tr>
  <tr>
</table>

---

## How It Works

```
Research PDFs  →  Qdrant Vector DB  →  CrewAI Agents  →  Platform Content
                   (hybrid search)     (Claude Sonnet)    LinkedIn / Instagram
                        +                                  YouTube / X
               Cross-encoder rerank
                              ↑
                  Perplexity Web Search (Flare mode only)
                              ↓
                  PNG Image Renderer (Playwright/Chromium)
```

1. **Ingestion** — PDFs are parsed, semantically chunked, embedded with OpenAI `text-embedding-3-large` (3072-dim), and stored in Qdrant (one collection per domain)
2. **Retrieval** — Hybrid search (dense vectors + BM25 keyword matching) with cross-encoder re-ranking for citation-accurate results
3. **Generation** — CrewAI sequential agents run research → content writing → creative direction tasks per request
4. **Flare synthesis** — For reactive content, live Perplexity web intelligence is combined with per-domain RAG research, then synthesized into one unified analytical brief before content writing
5. **Image rendering** — Carousel slides, X cards, and Facebook slides rendered as branded PNGs using Playwright headless Chromium — 6 CSS chart types, LLM-chosen data visualizations, OICCI brand colors locked
6. **Output** — Structured JSON rendered in a React UI with platform cards, copy/download, lightbox image gallery, and creative direction briefs inside each visual/video card

---

## What's New in v2

| Feature | v1 | v2 |
|---|---|---|
| LLM | GPT-4.1 Mini | Claude Sonnet 4.6 |
| Web search | DuckDuckGo | Perplexity sonar-pro |
| Image generation | — | Playwright PNG renderer (carousel + X card + Facebook slides) |
| Chart types | — | 6 CSS charts: bar, column, stacked, pie, cards, list |
| Carousel slides | Text only | Text + rendered branded PNGs (1080×1350 px) |
| X card | Text only | Rendered branded PNG (1200×675 px) |
| Facebook slides | — | 4 square branded PNGs (1080×1080 px) — auto-generated alongside caption |
| Image gallery | — | Grid view, fullscreen lightbox (scroll-locked), per-image save, Save All |
| Facebook platform | — | Full platform: 3-paragraph analytical caption + 4 rendered slide images |
| Flare architecture | Single-pass | Three-phase: per-domain RAG → synthesis → unified content |
| Rate limit handling | — | Exponential backoff retry + inter-domain delays |
| Auth | — | JWT-based login (24h session) |
| Task history | — | Persistent SQLite (survives restarts), L/F badges, abort |
| UI | Basic | Full ORBIT space theme — orbital loader, deep space palette |

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Agent Framework** | CrewAI |
| **LLM** | Claude Sonnet 4.6 (Anthropic) |
| **Vector DB** | Qdrant (embedded, no Docker needed) |
| **Embeddings** | OpenAI text-embedding-3-large (3072-dim) |
| **Re-ranking** | cross-encoder/ms-marco-MiniLM-L-6-v2 |
| **Web Search** | Perplexity sonar-pro (Flare mode) |
| **Image Rendering** | Playwright + headless Chromium |
| **Backend** | FastAPI + Python 3.12 |
| **Frontend** | React 19 + Vite + Tailwind CSS 4 |
| **Database** | SQLite (task history, persistent) |
| **Auth** | JWT (PyJWT, 24h expiry) |
| **PDF Parsing** | pdfplumber + unstructured |

---

## What It Outputs

| Platform | Output |
|---|---|
| **LinkedIn** | 1,500–1,800 char thought-leadership post — bold hook, data bullets, analyst insight layer, closing question, hashtags |
| **X (Twitter)** | Lead tweet + 5–7 tweet thread + comment variations + rendered X card PNG (1200×675 px) |
| **Instagram Carousel** | 10-slide story arc — hook → data → insight → CTA. Each slide has a headline, LLM-chosen highlight phrase (orange box), and optional data chart. All 10 slides rendered as branded PNGs (1080×1350 px). |
| **Instagram Reels** | Hook, on-screen text, voiceover script, visual concept |
| **YouTube** | Full 2–3 min script (400–500 words) with [HOOK] [CONTEXT] [DATA DEEP-DIVE] [SO WHAT] [CTA] — word counts enforced per section |
| **Facebook** | 3-paragraph analytical caption (problem → analysis → recommendations) + hashtags + 4 branded slide images rendered as PNGs (1080×1080 px square) — cover, data/chart, insight, CTA |

**Creative direction briefs** (visual design and video/audio production guidance) are generated inside the relevant platform cards — not as a separate section.

**Brand voice validation** runs server-side on every output.

---

## Carousel Image Rendering

ORBIT renders carousel slides, X cards, and Facebook slides as finished PNGs using Playwright headless Chromium — no external design tools needed.

| Asset | Canvas | Count | Trigger |
|---|---|---|---|
| Instagram Carousel | 1080 × 1350 px (portrait 4:5) | 10 slides | "Generate Slide Images" button |
| X Card | 1200 × 675 px (landscape 16:9) | 1 card | "Generate X Card Image" button |
| Facebook Slides | 1080 × 1080 px (square 1:1) | 4 slides | "Generate Facebook Slides" button |

- **Charts**: Claude decides which chart type fits the data — bar, column, stacked, pie, cards, or list — all rendered in pure CSS (no JS libraries, works on Cloud Run)
- **Bar charts**: slim pill bars cycling through OICCI brand colors (navy, teal, orange, mid-blue) — different color per bar, never all the same
- **Gallery**: responsive grid, hover to Save or View Full, fullscreen lightbox with scroll lock + keyboard navigation (←/→/Escape), Save All downloads all at once
- **Highlight box**: orange box with LLM-chosen punchy phrase — always a standalone stat, never repeating the slide body text

---

## Flare — Reactive Content Engine

When a breaking event occurs, Flare runs a three-phase pipeline:

1. **Perplexity web search** — live synthesized intelligence on the event
2. **Per-domain RAG research** — each selected satellite queries its own knowledge base with web context injected; runs sequentially with rate-limit-safe delays
3. **Synthesis + content writing** — all domain findings merged into ONE unified analytical brief; content written from that synthesis — one post per platform, not one per domain

Result: analyst-style reactive content that feels like an expert hot-take on breaking news, backed by years of documented research data.

---

## Anti-Hallucination System

This is not a chatbot wrapper. ORBIT cannot invent statistics.

Every agent is restricted to data retrieved from the knowledge base — if a fact is not in the documents, the agent states "insufficient data" rather than guessing. Five layers enforce this:

1. **Strict agent instructions** — every satellite is told at the system level: "You ONLY use data retrieved from the documents. Never fabricate statistics, quotes, or data points."
2. **Citation enforcement** — every claim in the Research Findings section carries a source reference (document name, year, page number). Fully verifiable before publishing.
3. **Hybrid search + re-ranking** — dense vector similarity + BM25 keyword matching, re-ranked by a cross-encoder. Only the top 6 most relevant passages reach the agent. Low-relevance results are filtered before the LLM ever sees them.
4. **Citation separation** — academic citations appear only in Research Findings, never in platform posts. Platform content references the source naturally ("latest data shows...") without cluttering the post.
5. **Brand voice validation** — post-generation scoring checks every output against tone rules, forbidden language categories, and content quality standards before delivery.

---

## Flight Log

Every generation is saved permanently to a local SQLite database and survives server restarts and redeployments. The Flight Log page shows:

- All past tasks with status (pending / processing / completed / failed / cancelled)
- **L / F badge** — green L for Launch, amber F for Flare — instant recognition of which mode was used
- Click any completed task to re-open and review the full generated content
- Delete with confirmation — no accidental deletions
- **Abort** — stop any in-progress generation with one click from the Mission Status page

---

## No Free-Text Prompts

ORBIT uses structured input forms, not open prompt boxes. Users select from domain cards, platform cards, tone pills, and year range selectors. The only free-text field is a single short topic line (max 200 characters for Launch, 500 for Flare).

This is intentional — it eliminates prompt engineering burden from users, enforces consistent inputs, and ensures output quality is predictable every time.

---

## Who Is This For?

- **Think tanks and research institutes** publishing reports with limited content teams
- **Chambers of commerce and trade bodies** producing surveys and policy papers
- **Consulting firms** generating client-facing research
- **Financial institutions** with regular economic outlook publications
- **NGOs and development organizations** with annual reports
- **Any organization sitting on a library of PDFs** that deserves a wider audience

---

## What It Saves

- **Time** — 5-platform content from a research report in 1–3 minutes vs. hours manually
- **Consistency** — Same brand voice rules enforced on every output, every time
- **Accuracy** — Grounded in your documents. Every data point in Research Findings has a full citation (report, year, page number)
- **Reactive speed** — Respond to breaking events in minutes with a data-backed take, not a rushed opinion

---

## Built to Be Customized

ORBIT is a framework, not a fixed product:

- **Swap the document corpus** — Point it at your own PDFs, organized by any domain structure
- **Swap the LLM** — Model-agnostic. Claude Sonnet 4.6 by default; any OpenAI-compatible model works
- **Swap the web search provider** — Perplexity is isolated in one file; replace with Brave, Tavily, or any API
- **Add or remove platforms** — Platform cards and agent tasks are modular YAML configs
- **Add brand voice rules** — Reads from a plain text file you can edit
- **Add guardrails** — Content approval workflows, restricted topics, publishing permissions ready to integrate

---

## Deployment

ORBIT is production-ready and deployable to any Docker-based cloud platform.

### Docker

A `Dockerfile` is included. It builds in two stages — React frontend first, then the Python backend — and serves everything from a single container.

```dockerfile
# Build and run locally
docker build -t orbit .
docker run -p 8000:8000 --env-file .env orbit
```

Chromium is installed inside the container for image generation:

```dockerfile
RUN playwright install chromium --with-deps
```

### Google Cloud Run (recommended)

```bash
# Build and push
gcloud builds submit --tag gcr.io/YOUR_PROJECT/orbit

# Deploy
gcloud run deploy orbit \
  --image gcr.io/YOUR_PROJECT/orbit \
  --platform managed \
  --allow-unauthenticated \
  --memory 4Gi \
  --set-env-vars OPENAI_API_KEY=...,ANTHROPIC_API_KEY=...,PERPLEXITY_API_KEY=...
```

- **Memory**: 4GB recommended — sentence-transformers and Chromium both need headroom
- **Qdrant**: switch from embedded to `QDRANT_MODE=server` + `QDRANT_URL` pointing to a managed Qdrant Cloud instance for production
- **Port**: Cloud Run injects `$PORT` automatically — the app reads it at startup

### Environment Variables

| Variable | Required | Purpose |
|---|---|---|
| `OPENAI_API_KEY` | Yes | Embeddings (text-embedding-3-large) |
| `ANTHROPIC_API_KEY` | Yes | LLM — Claude Sonnet 4.6 |
| `PERPLEXITY_API_KEY` | Yes | Flare live web search |
| `AUTH_USERNAME` | Yes | Login username |
| `AUTH_PASSWORD` | Yes | Login password |
| `QDRANT_MODE` | No | `local` (default) or `server` |
| `QDRANT_URL` | No | Qdrant server URL (production) |

---

## Quick Start

```bash
# 1. Clone and set up Python environment
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# 2. Install Playwright Chromium (required for image generation)
playwright install chromium --with-deps

# 3. Add your API keys to .env
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
PERPLEXITY_API_KEY=pplx-...
AUTH_USERNAME=youruser
AUTH_PASSWORD=yourpassword

# 4. Ingest your documents (one-time setup)
python -m src.ingestion.ingest_all

# 5. Install frontend dependencies
cd frontend && npm install && cd ..

# 6. Run
./run.sh
# Frontend: http://localhost:5173
# Backend:  http://localhost:8000
```

---

## Contact & Custom Build

If you're an organization interested in deploying ORBIT for your own research library, or a developer who wants to collaborate:

- **Email** — [adeelmemon096@yahoo.com](mailto:adeelmemon096@yahoo.com)
- **LinkedIn** — [linkedin.com/in/adeeliqbalmemon](https://linkedin.com/in/adeeliqbalmemon)
- **GitHub** — [github.com/adeel-iqbal](https://github.com/adeel-iqbal)

Custom builds, white-labeling, domain-specific deployments, and integration with existing content workflows are open for discussion.

---

*Built with CrewAI · Anthropic Claude · Qdrant · Perplexity · Playwright · FastAPI · React*
