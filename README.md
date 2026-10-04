# ComicCraft — Backend API (AI Comic Story Creator)

The Python FastAPI backend powering **ComicCraft**, an AI comic generator pipeline that transforms natural-language story prompts into structured, illustrated multi-panel comic books.

## 🏗️ Architecture

```
app/
├── ai/               # Gemini & HuggingFace clients, story/character/image pipeline
├── exporters/        # PDF (ReportLab) and PNG (Pillow) comic exporters
├── generators/       # Page layout and grid calculator
├── models/           # SQLAlchemy ORM models (Project, Character, Panel, Export)
├── routes/           # REST API endpoints (health, projects, panels, export)
├── schemas/          # Pydantic v2 validation schemas
├── services/         # Orchestrator pipeline, storage, project & demo services
├── utils/            # Logger, filename sanitizer
├── config.py         # Pydantic-settings based environment config
├── database.py       # SQLAlchemy engine and session management
└── main.py           # FastAPI application entry point
prompts/              # AI prompt templates for story, character, outline, and image generation
tests/                # Pytest unit and integration tests
requirements.txt
```

## 🛠️ Tech Stack

- **Framework**: Python FastAPI
- **Database**: SQLite + SQLAlchemy 2.0 ORM
- **Validation**: Pydantic v2
- **AI SDK**: `google-genai` (Gemini text + image) + Hugging Face Inference API (FLUX image generation)
- **Exporting**: ReportLab (PDF) + Pillow (PNG composite rendering)
- **Testing**: Pytest + HTTPX

## 🚀 Getting Started

### 1. Clone & Install

```bash
git clone https://github.com/sachinofficial1612-cyber/Comic-Ai-Backend.git
cd Comic-Ai-Backend

# Create virtual environment
python -m venv .venv
.venv\Scripts\activate   # Windows
# source .venv/bin/activate  # macOS/Linux

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure Environment

```bash
cp .env.example .env
```

Edit `.env` and fill in your API keys:

```env
GEMINI_API_KEY=your_gemini_api_key_here
HF_TOKEN=your_huggingface_token_here
```

> **Note:** ComicCraft works without any API keys using a built-in canvas fallback generator.

### 3. Run the Server

```bash
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

API will be live at: `http://127.0.0.1:8000`
Interactive docs: `http://127.0.0.1:8000/docs`

### 4. Run Tests

```bash
pytest tests/
```

## 🌐 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/health` | Health check |
| POST | `/api/projects` | Create a new comic project |
| GET | `/api/projects` | List all projects |
| GET | `/api/projects/{id}` | Get project by ID |
| POST | `/api/projects/{id}/generate` | Generate comic panels with AI |
| PUT | `/api/projects/{id}/panels/{pid}` | Edit a panel |
| POST | `/api/projects/{id}/panels/{pid}/regenerate-image` | Regenerate panel artwork |
| POST | `/api/projects/{id}/export/pdf` | Export as PDF |
| POST | `/api/projects/{id}/export/png` | Export as PNG pages |

## ☁️ Deploying to Render

1. Connect this repository to [Render](https://render.com).
2. **Build Command**: `pip install -r requirements.txt`
3. **Start Command**: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
4. Add environment variables from `.env.example`.
