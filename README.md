# ✍️ AgentWriter-AI

A professional **FastAPI** web application that provides a **real-time streaming interface** for an existing **LangGraph blog-writing workflow**. It exposes the workflow’s progress via **Server-Sent Events (SSE)**, displays the plan, generated sections, planned images, and the final Markdown article, and allows downloading the result.

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-009688?style=flat&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![LangGraph](https://img.shields.io/badge/LangGraph-1.1+-1C3C3C?style=flat)](https://langchain-ai.github.io/langgraph/)
[![Uvicorn](https://img.shields.io/badge/Uvicorn-ASGI-499848?style=flat)](https://www.uvicorn.org/)
[![Jinja2](https://img.shields.io/badge/Jinja2-Templates-B41717?style=flat&logo=jinja&logoColor=white)](https://jinja.palletsprojects.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📋 Table of Contents

- [About](#-about)
- [Key Features](#-key-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Running the Application](#-running-the-application)
- [Usage](#-usage)
- [API Endpoints](#-api-endpoints)
- [Streaming Events](#-streaming-events)
- [Testing the Frontend Without a Real Backend](#-testing-the-frontend-without-a-real-backend)
- [Security & Safety](#-security--safety)
- [Contributing](#-contributing)
- [License](#-license)
- [Acknowledgements](#-acknowledgements)

---

## 📖 About

**AgentWriter-AI** is a **FastAPI frontend wrapper** for an existing LangGraph blog-writing workflow. The core LangGraph workflow lives in `backend.py` and remains completely unchanged. This project adds a real-time streaming web interface on top of it, allowing users to watch the agent’s execution stage by stage, from routing and research to section writing and final assembly. 

> **Note**: This project is a frontend wrapper. The core LangGraph workflow lives in `backend.py` and remains completely unchanged. 

---

## ✨ Key Features

- ⚡ **Real-time streaming** of the agent’s execution using SSE.
- 📊 **Stage-by-stage visibility**: routing, research, planning, section writing, image planning, and final assembly.
- 🗂️ **Structured plan display**: shows the article outline, goals, bullets, and target word counts.
- 📝 **Section preview**: each completed section appears as it is written.
- 🖼️ **Image specifications**: lists the visuals planned by the agent.
- 📄 **Final Markdown** rendered in the browser with a one-click download.
- 🎨 **Clean, responsive UI** (dark theme) built with vanilla HTML/CSS/JS.
- ❤️ **Health check endpoint** for monitoring.
- 🔒 **Safe file handling** for generated Markdown downloads. 

---

## 🏗️ Architecture

```
┌─────────────┐      ┌─────────────────────┐      ┌─────────────────┐
│   Browser   │─────▶│  FastAPI (app.py)   │─────▶│   backend.py    │
│  (SSE/UI)   │◀─────│ StreamingResponse   │◀─────│ LangGraph app   │
└─────────────┘      └─────────────────────┘      └─────────────────┘
```

- **`app.py`** – FastAPI application, request handling, SSE streaming, file serving.
- **`backend.py`** – Compiled LangGraph workflow (`app` object).
- **`templates/index.html`** – Jinja2 template for the UI.
- **`static/`** – CSS and JavaScript assets.
- **`images/`** – Generated images (served statically).
- **`outputs/`** – Saved Markdown articles per run. 

---

## 🧰 Tech Stack

| Category | Technologies |
|----------|--------------|
| **Backend Framework** | FastAPI, Uvicorn |
| **Orchestration** | LangGraph, LangChain |
| **Templating** | Jinja2 |
| **Frontend** | Vanilla HTML/CSS/JS (dark theme) |
| **Streaming** | Server-Sent Events (SSE) |
| **Data Validation** | Pydantic |
| **Language** | Python 3.10+ |

---

## 📁 Project Structure

```
.
├── app.py                  # FastAPI application & SSE streaming
├── backend.py              # LangGraph workflow (unchanged)
├── requirements.txt        # Python dependencies
├── templates/
│   └── index.html          # Main UI template
├── static/
│   ├── app.js              # Frontend logic (SSE handling, DOM updates)
│   └── style.css           # Dark theme styling
├── images/                 # Generated images (served at /images)
└── outputs/                # Saved Markdown per run (served for download)
```

> `images/` and `outputs/` are created automatically at runtime if they are missing. 

---

## 📦 Prerequisites

- **Python 3.10 or higher**
- **`pip`** and **`venv`**
- The **`backend.py`** file containing your compiled LangGraph workflow (must expose an `app` object). 

---

## 🚀 Installation

### 1. Clone the repository

```bash
git clone https://github.com/Hichamjb/AgentWriter-AI.git
cd AgentWriter-AI
```

### 2. Create and activate a virtual environment

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

Ensure `requirements.txt` includes at least:

```
fastapi
uvicorn[standard]
jinja2
pydantic
```

Plus any dependencies required by your `backend.py` (e.g., `langgraph`, `langchain`, etc.). 

### 4. Verify project structure

Make sure your project matches the structure shown in [Project Structure](#-project-structure). 

---

## ⚙️ Configuration

**No environment variables are required.** The application uses the following directories, which are created automatically if missing:

| Directory | Purpose |
|-----------|---------|
| `templates` | Jinja2 HTML templates |
| `static` | CSS, JavaScript, and other static files |
| `images` | Generated images from the backend |
| `outputs` | Saved Markdown articles per run |



---

## 🏃 Running the Application

### Option 1: Directly with Python

```bash
python app.py
```

The server starts at `http://127.0.0.1:8000` with auto-reload enabled. 

### Option 2: Using Uvicorn

```bash
uvicorn app:app --reload --host 127.0.0.1 --port 8000
```

### Production (example)

```bash
uvicorn app:app --host 0.0.0.0 --port 8000 --workers 4
```



---

## 🎮 Usage

1. Open your browser at `http://127.0.0.1:8000`.
2. Enter a technical blog topic (e.g., *“Introduction to LangGraph for stateful agents”*).
3. Click **Lancer l’agent** (or the equivalent button).
4. Watch the real-time progress:
   - Stages update as the workflow progresses.
   - The article plan appears.
   - Completed sections are shown.
   - Planned images are listed.
   - The final Markdown is displayed.
5. Click **Télécharger le Markdown** to download the generated article. 

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Serves the main UI page. |
| `GET` | `/api/health` | Health check. Returns `{"status":"ok"}`. |
| `POST` | `/api/run` | Starts a workflow run. Body: `{"topic": "..."}`. Returns an SSE stream. |
| `GET` | `/api/runs/{run_id}/download` | Downloads the generated Markdown file. |



### Example: Start a run

```bash
curl -N -X POST http://127.0.0.1:8000/api/run \
  -H "Content-Type: application/json" \
  -d '{"topic": "Building a RAG pipeline with LangChain"}'
```

The response is a stream of **Server-Sent Events**. 

---

## 📡 Streaming Events

The `/api/run` endpoint emits SSE events with a JSON payload. Event types include:

| Event type | Description |
|------------|-------------|
| `run_started` | Indicates the run has begun; includes `run_id` and `topic`. |
| `stage` | A high-level stage update (`router`, `research`, `orchestrator`, `workers`, `reducer`). |
| `substage` | A sub-step within a stage (e.g., `merge_content`, `decide_images`). |
| `routing` | The router’s decision: mode, whether research is needed, and queries. |
| `research_complete` | Number of sources and a preview of evidence. |
| `plan` | The structured article plan (tasks with titles, goals, bullets, word counts). |
| `section_complete` | A section has been written; includes `task_id`, `title`, `markdown`, and progress. |
| `images_planned` | Image specifications decided by the agent. |
| `final` | The final Markdown article and a download URL. |
| `error` | An error occurred; includes a message. |
| `done` | The run has finished (successfully or after an error). |



---

## 🧪 Testing the Frontend Without a Real Backend

If you want to test the UI without a working LangGraph workflow, you can create a minimal `backend.py` that mimics the expected interface:

```python
# backend.py – mock for UI testing
class MockWorkflow:
    def stream(self, input, config=None, stream_mode=None, subgraphs=None):
        # Yield fake updates matching your app.py expectations
        yield ((), {"router": {"mode": "closed_book", "needs_research": False}})
        yield ((), {"orchestrator": {"plan": {"title": "Test", "tasks": [
            {"id": 1, "title": "Intro", "goal": "Say hi", "bullets": [], "target_words": 100}
        ]}}})
        yield ((), {"worker": {"sections": [[1, "## Intro\nHello world."]]}})
        yield ((), {"reducer": {"final": "# Test Article\nHello world."}})

    def get_state(self, config):
        class Snapshot:
            values = {"final": "# Test Article\nHello world."}
        return Snapshot()

app = MockWorkflow()
```

Replace the real `backend.py` temporarily to verify the frontend. 

---

## 🔒 Security & Safety

- **Run ID validation**: download endpoint only accepts alphanumeric, `-`, and `_` characters.
- **No arbitrary file access**: downloads are restricted to `outputs/{run_id}/blog.md`.
- **CORS**: not enabled by default; add `CORSMiddleware` if needed.
- **Input validation**: topic length is enforced between 3 and 1000 characters.



---

## 🤝 Contributing

1. **Fork** the repository.
2. **Create a feature branch** (`git checkout -b feature/amazing-feature`).
3. **Commit your changes** (`git commit -m 'Add amazing feature'`).
4. **Push** to the branch (`git push origin feature/amazing-feature`).
5. **Open a Pull Request**.



---

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](https://github.com/Hichamjb/AgentWriter-AI/blob/main/LICENSE) file for details. 

---

## 🙏 Acknowledgements

- [FastAPI](https://fastapi.tiangolo.com/)
- [LangGraph](https://langchain-ai.github.io/langgraph/)
- [Uvicorn](https://www.uvicorn.org/)
- [Jinja2](https://jinja.palletsprojects.com/)



---

*Built with ❤️ for the LangGraph community.*
