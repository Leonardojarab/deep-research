<div align="center">

![Deep Research Banner](image/deepresearch.png)

![Python](https://img.shields.io/badge/Python-3.12%2B-3776AB?style=flat-square&logo=python&logoColor=white)
![Gradio](https://img.shields.io/badge/Gradio-UI-FF7C00?style=flat-square&logo=gradio&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-Agents-412991?style=flat-square&logo=openai&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-22C55E?style=flat-square)

</div>

---

## What is Deep Research?

**Deep Research** is an autonomous multi-agent pipeline that takes any question or topic, plans a set of targeted web searches, executes them in parallel, synthesizes the findings into a detailed long-form report, and delivers it straight to your inbox — all with a single click.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Gradio Web UI                          │
│                   deep_research.py                          │
└──────────────────────────┬──────────────────────────────────┘
                           │  query
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                   Research Manager                          │
│                 research_manager.py                         │
└───┬───────────────┬──────────────────┬───────────────┬──────┘
    │               │                  │               │
    ▼               ▼                  ▼               ▼
┌────────┐   ┌────────────┐   ┌──────────────┐  ┌──────────┐
│Planner │   │   Search   │   │    Writer    │  │  Email   │
│ Agent  │──▶│   Agent    │──▶│    Agent     │─▶│  Agent   │
│        │   │ (×5 async) │   │              │  │          │
└────────┘   └────────────┘   └──────────────┘  └──────────┘
  Plans 5      Parallel         Synthesizes       Sends HTML
  searches     web searches     a full report     via SendGrid
```

### Agents

| Agent | Model | Role |
|---|---|---|
| **Planner** | `gpt-4o-mini` | Breaks the query into 5 targeted web search terms |
| **Search** | `gpt-4o-mini` | Searches the web and summarizes each result (runs concurrently) |
| **Writer** | `gpt-4o-mini` | Synthesizes all results into a structured Markdown report (1000+ words) |
| **Email** | `gpt-4o-mini` | Converts the report to HTML and sends it via SendGrid |

---

## Features

- **Parallel search execution** — all 5 searches run concurrently via `asyncio`
- **Structured outputs** — agents use Pydantic models for typed, reliable responses
- **Full traceability** — every run is linked to an OpenAI trace for debugging
- **Live streaming UI** — status updates stream in real time through Gradio
- **Email delivery** — the finished report lands in your inbox automatically

---

## Installation

**1. Clone the repository**

```bash
git clone https://github.com/<your-username>/deep_research.git
cd deep_research
```

**2. Install dependencies with `uv`**

```bash
uv sync
```

Or with `pip`:

```bash
pip install -r requirements.txt
```

**3. Configure environment variables**

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your_openai_api_key
SENDGRID_API_KEY=your_sendgrid_api_key
FROM_EMAIL=your_verified_sender@example.com
TO_EMAIL=your_recipient@example.com
```

---

## Usage

```bash
uv run deep_research.py
```

The Gradio interface will open automatically in your browser.

1. Type your research question in the text box
2. Click **Run** (or press Enter)
3. Watch the progress stream in real time
4. Receive the full report in the UI and in your inbox

---

## Project Structure

```
deep_research/
├── deep_research.py       # Gradio UI entry point
├── research_manager.py    # Orchestrates the full pipeline
├── planner_agent.py       # Plans the web searches
├── search_agent.py        # Executes individual searches
├── writer_agent.py        # Writes the final report
├── email_agent.py         # Sends the report by email
├── pyproject.toml
└── requirements.txt
```

---

## Requirements

- Python 3.12+
- OpenAI API key
- SendGrid account and verified sender email

---

## License

MIT
