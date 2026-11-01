# Deep Research

An AI-powered research assistant that turns a question into a structured, multi-search investigation and a detailed Markdown report. The finished report is displayed in the Gradio UI and is also delivered through the configured channel: email or a Pushover push notification to your mobile device.

The project demonstrates practical work with Python, asynchronous workflows, agent orchestration, structured data, web research, and a custom Gradio interface.

## What it does

1. Accepts a research question in a Gradio interface.
2. Uses a planner agent to create focused web searches.
3. Runs searches concurrently and summarizes the results.
4. Uses a writer agent to produce a detailed Markdown report with a short summary and follow-up questions.
5. Displays the finished Markdown report in the UI and sends it through the configured delivery channel: email or a Pushover push notification to your mobile device.
6. Streams progress updates in the UI and records the agent run with OpenAI Agents SDK tracing, including a link to inspect the trace in the OpenAI platform.

## Skills demonstrated

- **Python application development** — modular code organized by agent responsibility.
- **Asynchronous programming** — `async`/`await` workflow coordination and concurrent searches with `asyncio.gather`.
- **Agentic AI workflows** — planning, tool-assisted web search, report writing, and delivery using the OpenAI Agents SDK.
- **Structured LLM outputs** — Pydantic schemas define the planner's search plan and the writer's report shape, giving model responses a predictable, typed structure.
- **Agent observability and tracing** — OpenAI Agents SDK traces capture the research run and provide a platform link for inspecting agent execution.
- **Web research integration** — web search through the Agents SDK `WebSearchTool`.
- **UI development** — Gradio Blocks, event handling, example prompts, custom CSS, and JavaScript.
- **External service integration** — SMTP email and Pushover notifications.
- **Configuration management** — environment-based settings loaded with `python-dotenv`.

## Tech stack

| Area | Technologies |
|---|---|
| Language | Python |
| AI and orchestration | OpenAI Agents SDK |
| Structured LLM outputs | Pydantic models |
| Agent observability | OpenAI Agents SDK tracing |
| User interface | Gradio |
| Async execution | Python `asyncio` |
| Integrations | Web search, SMTP email, Pushover |
| Configuration | `python-dotenv` |

## Project structure

```text
.
├── app.py                # Styled Gradio interface; primary app entry point
├── simple.py             # Minimal alternative Gradio interface
├── research_manager.py   # Coordinates planning, searching, writing, and delivery
├── planner_agent.py      # Creates a structured web-search plan
├── search_agent.py       # Searches the web and summarizes results
├── writer_agent.py       # Produces structured report content
├── email_agent.py        # Formats and dispatches the finished report
├── messenger.py          # SMTP and Pushover delivery helpers
├── styles.py             # UI examples, HTML, CSS, and JavaScript
└── requirements.txt      # Python dependencies
```

## Run locally

### 1. Set up Python

Python 3.10 or later is recommended.

```bash
python -m venv .venv
```

Activate the virtual environment:

```bash
# Windows PowerShell
.\.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

### 2. Configure environment variables

Create a `.env` file in the project root. Add your own credentials; do not commit this file.

```dotenv
OPENAI_API_KEY=your_openai_api_key

# Optional: defaults to gpt-5.4-mini in the agent modules
DEFAULT_MODEL_NAME=gpt-5.4-mini

# Optional: number of searches proposed by the planner (default: 5)
HOW_MANY_SEARCHES=5

# Email delivery (default: true)
USE_EMAIL=true
EMAIL_ADDRESS=you@example.com
EMAIL_SMTP_SERVER=smtp.example.com
EMAIL_APP_PASSWORD=your_email_app_password

# Used when USE_EMAIL=false
PUSHOVER_USER=your_pushover_user_key
PUSHOVER_TOKEN=your_pushover_application_token
```

Configure the SMTP server and app password for your email provider. To use Pushover instead, set `USE_EMAIL=false` and provide the Pushover credentials. The app expects the selected delivery integration to be configured.

### 3. Start the interface

Run the primary, styled interface:

```bash
python app.py
```

Or run the minimal interface:

```bash
python simple.py
```

Open the local URL printed by Gradio in your terminal, enter a research question, and submit it.

## How the workflow is organized

`ResearchManager` coordinates the end-to-end process:

```text
Question
   ↓
Planner Agent → structured search plan
   ↓
Search Agent → concurrent web searches and summaries
   ↓
Writer Agent → structured report
   ├──→ Markdown report displayed in the Gradio UI
   └──→ Email or mobile push notification (configured delivery channel)
```

The UI output is not the only destination: after the report is generated, the delivery agent sends it through the selected notification channel as well. Configure email credentials for email delivery, or set `USE_EMAIL=false` and provide Pushover credentials to receive a push notification on your mobile device.

## Structured outputs and tracing

The planner and writer use Pydantic models as their output types. The planner returns a `WebSearchPlan` containing typed search items, while the writer returns `ReportData` with a summary, Markdown report, and follow-up questions. This makes agent outputs easier for the application to consume than free-form text alone.

Each research run is wrapped in the Agents SDK `trace` context. The app emits the trace URL as a progress update, so you can open the run in the OpenAI platform and inspect its execution.

## Configuration notes

- `DEFAULT_MODEL_NAME` controls the model used by the agents; the code defaults to `gpt-5.4-mini`.
- `HOW_MANY_SEARCHES` controls the planner's requested number of search terms; it defaults to `5`.
- Email delivery is enabled by default. Set `USE_EMAIL=false` to select the Pushover path.
- The project has two UIs: `app.py` is the styled primary interface, while `simple.py` is a compact alternative. Both use the same research workflow.

## Scope and next steps

This is a portfolio project demonstrating an agent-based research workflow. Before using it for production or sensitive research, add and validate operational safeguards such as input and output checks, delivery error handling, rate limits, and automated tests.

## Author

Built as a project exploring agentic AI, multi-agent workflow, async Python, and research automation.

