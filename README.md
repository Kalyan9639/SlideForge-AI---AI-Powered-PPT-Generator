# 🤖 Hybrid AI Presentation Architect

A dual-mode AI-powered presentation generator that converts your **project documentation** into professional `.pptx` files using **Google Gemini** or **Sarvam AI**.

---

## ✨ Features

- 📄 **Documentation-first**: Upload a `.txt` or `.md` file as the source — no manual topic typing
- 🧠 **Dual AI backends**: Choose between **Google Gemini** (structured single-call) or **Sarvam AI** (multi-agent pipeline)
- 🤖 **Sarvam Multi-Agent Pipeline**: 1 planner agent + N per-slide writer sub-agents for reliable, focused output
- 📝 **Slide-by-slide instructions**: Optionally describe what each slide should cover
- 🌐 **8 Indian language translations**: English, Hindi, Tamil, Telugu, Bengali, Kannada, Marathi, Gujarati
- 🎨 **Custom PPTX templates**: Upload your own `.pptx` template; the app fills in the content
- 📏 **Adjustable content density**: Brief, Medium, or Detailed
- 🔢 **Slide count control**: 2–10 slides

---

## 🗂️ Project Structure

```
ppt-generator-sarvam/
├── new.py          # Streamlit frontend (full app)
├── api.py          # FastAPI backend (REST API)
├── .env            # API keys (not committed)
├── README.md
└── requirements.txt
```

---

## 🚀 Getting Started

### 1. Install Dependencies

```bash
pip install streamlit fastapi uvicorn python-pptx pydantic requests agno google-generativeai python-multipart
```

### 2. Set Up API Keys

Create a `.env` file or pass keys directly in the UI:

```
SARVAM_API_KEY=your_sarvam_key_here
GOOGLE_API_KEY=your_gemini_key_here
```

---

## 🖥️ Running the App

### Streamlit UI

```bash
streamlit run new.py
```

Then open [http://localhost:8501](http://localhost:8501) in your browser.

### FastAPI Backend

```bash
uvicorn api:app --reload
```

API docs available at [http://localhost:8000/docs](http://localhost:8000/docs)

---

## 🔌 API Endpoints (FastAPI)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Health check |
| `GET` | `/languages` | List supported languages |
| `POST` | `/generate/slides` | Generate slide JSON from documentation |
| `POST` | `/generate/pptx` | Generate and download a `.pptx` file |

### Example: Generate Slides (JSON)

```bash
curl -X POST http://localhost:8000/generate/slides \
  -F "doc_file=@my_project.txt" \
  -F "slide_count=5" \
  -F "content_length=Medium" \
  -F "ai_model=Sarvam AI" \
  -F "sarvam_key=your_key" \
  -F "slide_instructions=Slide 1: Intro\nSlide 2: Architecture"
```

### Example: Generate & Download PPTX

```bash
curl -X POST http://localhost:8000/generate/pptx \
  -F "doc_file=@my_project.txt" \
  -F "template_file=@template.pptx" \
  -F "slide_count=5" \
  -F "content_length=Medium" \
  -F "ai_model=Google Gemini" \
  -F "gemini_key=your_key" \
  --output output.pptx
```

---

## 🌐 Language Translation

Translation is powered by the **Sarvam Translate API**. A Sarvam API key is required whenever a non-English language is selected, even when using Gemini for generation.

**Supported languages:**

| Code | Language |
|------|----------|
| `en-IN` | English |
| `hi-IN` | Hindi |
| `ta-IN` | Tamil |
| `te-IN` | Telugu |
| `bn-IN` | Bengali |
| `kn-IN` | Kannada |
| `mr-IN` | Marathi |
| `gu-IN` | Gujarati |

---

## 🏗️ Architecture

```
User Input (Documentation + Instructions)
         │
         ▼
  ┌─────────────────────────────────┐
  │         AI Backend              │
  │                                 │
  │  Gemini: Single structured call │
  │                                 │
  │  Sarvam: Multi-Agent Pipeline   │
  │   ├─ Stage 1: Planner Agent     │
  │   │    └─ Generates N titles    │
  │   └─ Stage 2: Writer Agents     │
  │        └─ 1 call per slide      │
  └─────────────────────────────────┘
         │
         ▼
  Slide JSON [{ heading, content[] }]
         │
         ▼
  [Optional] Sarvam Translation
         │
         ▼
  PPTX Builder (python-pptx)
         │
         ▼
  ✅ Download presentation.pptx
```

---

## 📋 Requirements

```
streamlit
fastapi
uvicorn
python-pptx
pydantic
requests
agno
google-generativeai
python-multipart
```

---

## 📄 License

MIT License — free to use and modify.
