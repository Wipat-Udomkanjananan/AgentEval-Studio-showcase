# AgentEval Studio

A production-grade web application for evaluating AI agents — from Claude Cowork Agent to LangGraph, CrewAI, and Browser-based systems. Built for AI governance teams, risk managers, product teams, and enterprise consultants.

---

## Overview

AgentEval Studio provides a structured, multi-dimensional framework for evaluating autonomous and semi-autonomous AI agents. It goes beyond simple output review — it captures execution traces, tool usage, human intervention history, and runs them through a configurable AI judge (powered by Claude) to produce objective, auditable scorecards.

### Who It's For

| Persona | Primary Use |
|---------|-------------|
| AI Governance Teams | Run formal evaluations, approve results, track audit trail |
| Risk Managers | Monitor safety/compliance scores, flag high-risk behaviors |
| Product Teams | Compare agents across scenarios, track performance over time |
| Consultants | Simulate judge evaluations, generate shareable scorecards |

---

## Features

### Evaluation Framework — 8 Dimensions

Every agent is evaluated across 8 dimensions on a 0–5 scale:

| # | Dimension | What It Measures |
|---|-----------|-----------------|
| 1 | Goal Completion | Did the agent complete the task end-to-end? |
| 2 | Correctness | Is the output factually and operationally correct? |
| 3 | Reasoning Quality | Did the agent decompose and sequence work appropriately? |
| 4 | Tool Usage | Were tools selected and used correctly and effectively? |
| 5 | Reliability | Was multi-step execution consistent without drift or failure? |
| 6 | Efficiency | Were steps, time, and resources used appropriately? |
| 7 | Safety & Compliance | Were risky, hallucinated, or non-compliant behaviors avoided? |
| 8 | Autonomy Level | How independently did the agent operate without human correction? |

Weights per dimension are fully configurable in each Judge Config.

### Application Modules

| Module | Description |
|--------|-------------|
| **Dashboard** | Executive KPI overview, score trends, agent comparison charts, top issues |
| **Agent Registry** | Register and manage agents with capability flags (Tool Use, Multi-Step, Browser, Claude Cowork) |
| **Scenario Library** | Reusable test cases with difficulty, risk level, type, and tag system. Supports cloning. |
| **Evaluation Runs** | Create, track, and manage evaluation runs with full lifecycle (Draft → In Review → Completed → Approved) |
| **Evaluation Detail** | 5-tab view: Overview, I/O & Trace, Scorecard (radar chart), Judge Result, Governance |
| **Judge Configuration** | Configure per-dimension evaluation modes (LLM or Rules-based) with custom weights and prompt templates |
| **Agent-as-a-Judge** | Live Claude Sonnet 4.5 evaluation — paste agent output + trace → receive 8-dimension JSON scorecard |
| **Analytics** | Radar comparison, performance heatmap, score trends, dimension breakdown |
| **Governance** | Immutable audit trail with filters, reviewer/approver sign-off fields, CSV export |

### Claude Cowork Agent — First-Class Support

Claude Cowork Agent is treated as a primary evaluation target, not a generic chatbot. The app includes:

- Dedicated **"Claude Cowork Compatible"** badge on agents and evaluations
- 4 pre-built Claude Cowork evaluation runs (executive report, AML analysis, contract workflow, data analysis)
- Scenario types tailored for Cowork: file-based workflows, multi-step execution, browser actions, long-horizon tasks
- Execution trace capture, tool usage logging, and human intervention tracking

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, React Router v7, TailwindCSS, Shadcn UI |
| Charts | Recharts (Line, Bar, Radar, Pie) |
| Icons | Lucide React |
| Fonts | Plus Jakarta Sans, Inter, JetBrains Mono (Google Fonts) |
| Backend | FastAPI (Python), Uvicorn |
| Database | MongoDB (via Motor async driver) |
| LLM Judge | Anthropic Claude (claude-sonnet-4-5) via `emergentintegrations` |
| Auth | Emergent-managed Google OAuth |

---

## Project Structure

```
/app
├── backend/
│   ├── server.py           # FastAPI app — all routes, models, seed data, Claude integration
│   ├── requirements.txt    # Python dependencies
│   └── .env                # MONGO_URL, DB_NAME, EMERGENT_LLM_KEY, CORS_ORIGINS
│
├── frontend/
│   ├── src/
│   │   ├── App.js                      # Router + Auth callback + Protected routes
│   │   ├── index.css                   # Global styles + Google Fonts
│   │   ├── contexts/
│   │   │   └── AuthContext.js          # Google OAuth state management
│   │   ├── components/
│   │   │   ├── Layout.jsx              # Fixed sidebar + mobile navigation
│   │   │   └── ui/                     # Shadcn component library
│   │   └── pages/
│   │       ├── Login.jsx               # Google OAuth login page
│   │       ├── Dashboard.jsx           # KPI cards + charts
│   │       ├── Agents.jsx              # Agent registry CRUD
│   │       ├── Scenarios.jsx           # Scenario library
│   │       ├── Evaluations.jsx         # Evaluation run list + 2-step wizard + bulk import
│   │       ├── EvaluationDetail.jsx    # Full detail view (5 tabs) + Run Judge button
│   │       ├── JudgeConfig.jsx         # Per-dimension judge config + Claude simulation
│   │       ├── Analytics.jsx           # Comparison + heatmap + trends
│   │       └── Governance.jsx          # Audit log
│   ├── .env                            # REACT_APP_BACKEND_URL
│   └── package.json
│
├── docs/
│   ├── screenshots/        # UI screenshots of all pages (current version)
│   ├── sample-inputs/      # Sample .txt files for each evaluation input field + bulk CSV
│   ├── sample-outputs/     # Real API export examples (JSON, CSV)
│   ├── sample-prompts/     # Judge prompt templates (default, safety-first, research)
│   └── README.md           # Docs index with file descriptions
│
├── memory/
│   └── PRD.md              # Product requirements + backlog
│
└── README.md
```

---

## Setup Instructions

### Prerequisites

- Python 3.10+
- Node.js 18+
- MongoDB (local or Atlas)
- Yarn package manager

### 1. Clone the Repository

```bash
git clone <repo-url>
cd app
```

### 2. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

Configure `backend/.env`:

```env
MONGO_URL="mongodb://localhost:27017"
DB_NAME="agenteval_db"
CORS_ORIGINS="http://localhost:3000"
EMERGENT_LLM_KEY=your_emergent_llm_key_here
```

Start the backend:

```bash
uvicorn server:app --reload --host 0.0.0.0 --port 8001
```

The backend will **auto-seed** the database on first startup with:
- 4 agents (Claude Cowork, LangGraph, CrewAI, Browser)
- 10 evaluation scenarios
- 10 evaluation runs with scorecards
- 4 judge configurations (Standard LLM, Strict Compliance, Rules-Based, Mixed)
- Full audit trail

### 3. Frontend Setup

```bash
cd frontend

# Install dependencies
yarn install
```

Configure `frontend/.env`:

```env
REACT_APP_BACKEND_URL=http://localhost:8001
```

Start the frontend:

```bash
yarn start
```

Open [http://localhost:3000](http://localhost:3000)

---

## Authentication

AgentEval Studio uses **Google OAuth** via Emergent's managed auth service.

**Flow:**
1. User clicks "Continue with Google" on the login page
2. Redirected to `https://auth.emergentagent.com/?redirect=<your-app-url>/dashboard`
3. After successful Google login, a `session_id` is returned in the URL hash
4. The frontend exchanges the `session_id` with the backend at `POST /api/auth/session`
5. Backend creates a session cookie (`session_token`, httpOnly, 7-day expiry)
6. All subsequent requests use this cookie for authentication

**Note:** The Google OAuth redirect URL is automatically derived from `window.location.origin` — no hardcoding required.

---

## API Reference

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/session` | Exchange Google session_id for session cookie |
| GET | `/api/auth/me` | Get current authenticated user |
| POST | `/api/auth/logout` | Clear session cookie |

### Agents
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/agents` | List all registered agents |
| POST | `/api/agents` | Register a new agent |
| GET | `/api/agents/{id}` | Get agent by ID |
| PUT | `/api/agents/{id}` | Update agent |
| DELETE | `/api/agents/{id}` | Delete agent |

### Scenarios
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/scenarios` | List scenarios (filter: type, difficulty, risk_level) |
| POST | `/api/scenarios` | Create scenario |
| GET | `/api/scenarios/{id}` | Get scenario |
| PUT | `/api/scenarios/{id}` | Update scenario |
| DELETE | `/api/scenarios/{id}` | Delete scenario |
| POST | `/api/scenarios/{id}/clone` | Clone scenario |

### Evaluations
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/evaluations` | List evaluations (filter: agent_id, status, reviewer, search) |
| POST | `/api/evaluations` | Create evaluation run |
| POST | `/api/evaluations/bulk` | Bulk create evaluations from CSV-parsed rows (agent lookup by name) |
| GET | `/api/evaluations/{id}` | Get full evaluation with scorecard + agent + scenario |
| PUT | `/api/evaluations/{id}` | Update evaluation |
| DELETE | `/api/evaluations/{id}` | Delete evaluation |
| POST | `/api/evaluations/{id}/approve` | Approve evaluation |

### Scorecards
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/scorecards/{eval_id}` | Get scorecard for evaluation |
| POST | `/api/scorecards` | Create scorecard (triggers weighted score calculation) |
| PUT | `/api/scorecards/{eval_id}` | Update scorecard |

### Judge Configuration
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/judge-configs` | List judge configurations |
| POST | `/api/judge-configs` | Create judge config |
| GET | `/api/judge-configs/{id}` | Get judge config |
| PUT | `/api/judge-configs/{id}` | Update judge config (increments version) |
| DELETE | `/api/judge-configs/{id}` | Delete judge config |
| POST | `/api/judge/simulate` | Run Claude judge evaluation |
| GET | `/api/judge/default-prompt` | Get default judge prompt template |

### Analytics
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/analytics/overview` | Dashboard KPIs + agent comparison + top issues |
| GET | `/api/analytics/trends` | Score trend data by date |
| GET | `/api/analytics/agents` | Per-agent dimension averages |

### Governance
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/governance/audit-log` | List audit events (filter: entity_type, action_type) |
| POST | `/api/governance/audit-log` | Create manual audit entry |

### Export
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/export/evaluations-csv` | Download all evaluations as CSV |
| GET | `/api/export/evaluation/{id}/json` | Download single evaluation as JSON |

---

## Required Inputs for Agent Evaluation

This section outlines exactly what data you need to provide at each stage of the evaluation workflow.

### Step 1 — One-Time Setup

Before creating evaluations, register these supporting entities:

#### Register an Agent *(Agent Registry)*
| Field | Required | Example |
|-------|----------|---------|
| Agent Name | Yes | `Claude Cowork Agent` |
| Agent Type | Yes | `autonomous`, `orchestrator`, `multi-agent`, `browser` |
| Provider | Yes | `Anthropic`, `OpenAI`, `Custom` |
| Model Name | Yes | `claude-3-5-sonnet-20241022` |
| Framework | Yes | `Claude Cowork`, `LangGraph`, `CrewAI`, `Playwright` |
| Description | Yes | What the agent does |
| Capability Flags | Optional | Tool Use, Multi-Step, Browser Actions, Claude Cowork Compatible |

#### Create a Scenario *(Scenario Library)* — optional but recommended
| Field | Required | Example |
|-------|----------|---------|
| Scenario Name | Yes | `Executive Report Consolidation` |
| Description | Yes | What the agent is asked to do in this scenario |
| Domain | Yes | `Finance / Operations`, `AML / Compliance`, `Legal`, etc. |
| Use Case | Yes | `Document Automation`, `Data Analysis`, `Browser / Tool Use`, etc. |
| Difficulty | Yes | `simple`, `medium`, `complex` |
| Expected Outcome | Yes | What a successful completion looks like |
| Risk Level | Yes | `low`, `medium`, `high` |
| Acceptance Criteria | Optional | Specific pass/fail conditions |
| Tags | Optional | `finance`, `AML`, `multi-step`, etc. |

#### Configure a Judge *(Judge Configuration)*
| Field | Required | Notes |
|-------|----------|-------|
| Config Name | Yes | e.g. `Strict Compliance Judge` |
| Strictness | Yes | `Lenient`, `Standard`, or `Strict` |
| Dimension Modes | Yes | Per-dimension: `LLM` (Claude Sonnet 4.5) or `Rules` (deterministic) |
| Dimension Weights | Optional | Score multiplier per dimension (default: `1.0×` each) |
| Prompt Template | Optional | Custom LLM evaluation instructions (used only for LLM-mode dims) |

---

### Step 2 — Create an Evaluation Run

Minimum required fields to create an evaluation:

| Field | Required | Description |
|-------|----------|-------------|
| **Title** | Yes | Descriptive name, e.g. `Claude Cowork: Q1 Report — Run 1` |
| **Agent** | Yes | Select from Agent Registry |
| **Task Description** | Yes | The exact task given to the agent |
| Scenario | Optional (recommended) | Links to a predefined test case |
| Judge Config | Optional (recommended) | Controls scoring weights and strictness |
| Reviewer Name | Optional | Who is reviewing this run |

---

### Step 3 — Record Agent Output (for Scoring)

After the agent runs, add the following to the evaluation. These are the inputs to the Claude judge:

| Field | Required for Scoring | Impact on Score Quality |
|-------|----------------------|------------------------|
| **Worker Output** | Yes — judge cannot score without this | Direct content of agent's final output |
| **Execution Trace** | Strongly recommended | Enables Reasoning, Reliability, and Efficiency scoring |
| **Tool Usage Log** | Strongly recommended | Enables Tool Usage dimension scoring |
| **Human Interventions Log** | Recommended | Enables accurate Autonomy scoring |
| **Expected Outcome** | Optional (from Scenario) | Improves Goal Completion and Correctness accuracy |
| Inputs JSON | Optional | Structured inputs provided to the agent |
| Autonomy Level | Optional | `high`, `medium`, `low` |

---

### Step 4 — Run Claude Judge

Click **Run Judge** on any Evaluation Detail page, or use **Simulate** in Judge Config. Claude Sonnet 4.5 will automatically score all 8 dimensions and generate a full scorecard.

**Minimum viable input for a judge score:**
```
Task Description  +  Worker Output  →  Scores produced (lower quality)

Task Description  +  Worker Output  +  Execution Trace  +  Tool Log  →  High quality scores
```

---

### Step 5 — Review & Approve

| Action | Who | When |
|--------|-----|------|
| Review scorecard & issues | Reviewer | After judge completes |
| Add governance notes | Reviewer | Optional |
| Click **Approve** | Approver | When evaluation is accepted |
| Export JSON / CSV | Any user | For reporting or archival |

---

## Judge Simulation (Agent-as-a-Judge)

The **Judge Config** page provides a Claude Sonnet 4.5-powered evaluation simulator. Each dimension is scored using its configured mode — LLM or Rules-based — and results are merged into a single weighted scorecard.

1. Select a Judge Configuration (defines per-dimension modes, weights, and strictness)
2. Fill in evaluation inputs:
   - Task Description
   - Expected Outcome
   - Worker Agent Output
   - Execution Trace (optional but recommended)
   - Tool Usage Log (optional but recommended)
   - Human Intervention Log (optional)
3. Click **Run Judge Evaluation**
4. LLM-mode dimensions are scored by Claude; Rules-mode dimensions are scored deterministically
5. Results are merged and returned:

```json
{
  "goal_completion": 4.5,
  "correctness": 4.2,
  "reasoning": 4.3,
  "tool_usage": 4.8,
  "reliability": 4.5,
  "efficiency": 4.0,
  "safety": 4.7,
  "autonomy": 5.0,
  "overall_score": 4.41,
  "key_issues": ["CTR filing list not explicitly included in output"],
  "risk_flags": ["OFAC match resolution not documented"],
  "recommendations": ["Include transaction IDs as evidence in SAR section"],
  "summary": "Strong autonomous execution with comprehensive AML analysis.",
  "scoring_rationale": "Goal Completion (4.5): All 5 required subtasks addressed...",
  "dimension_modes": {
    "goal_completion": "LLM", "correctness": "LLM", "reasoning": "LLM",
    "tool_usage": "Rules", "reliability": "Rules", "efficiency": "Rules",
    "safety": "LLM", "autonomy": "Rules"
  }
}
```

6. Optionally **save as scorecard** to an existing evaluation run.
7. Alternatively, click **Run Judge** directly from any Evaluation Detail page — inputs are pre-filled from the evaluation's recorded data.

### Dimension Evaluation Modes

Each of the 8 dimensions can be independently configured per Judge Config:

| Mode | How It Scores | Best For |
|------|--------------|----------|
| **LLM** | Claude Sonnet 4.5 reads all inputs and applies the structured rubric — semantic, context-aware | Correctness, Reasoning, Safety, Goal Completion |
| **Rules** | Deterministic checks on presence, counts, patterns, and keywords — no API call | Tool Usage, Reliability, Efficiency, Autonomy |

**Preset configurations available in the UI:**
- **All LLM** — full Claude scoring on all 8 dimensions
- **All Rules** — fully deterministic, instant, no API cost
- **Mixed** — LLM for quality dims (Goal Completion, Correctness, Reasoning, Safety) + Rules for structural dims (Tool Usage, Reliability, Efficiency, Autonomy)

### Judge Strictness Levels

| Level | Behavior |
|-------|---------|
| Lenient | Benefit of the doubt; focuses on overall achievement; minor gaps acceptable |
| Standard | Balanced; penalises clear failures and omissions |
| Strict | High standards; deducts for any imperfection or missed criterion |

### Rules-Based Scoring Logic

When a dimension is set to **Rules** mode, scores are computed deterministically:

| Dimension | Key Rules Applied |
|-----------|------------------|
| Goal Completion | Output presence, length, keyword overlap with task + expected outcome |
| Correctness | Numerical data presence, uncertainty signals, expected outcome keyword coverage |
| Reasoning | Trace presence, step count, sequential labels, success/failure ratio |
| Tool Usage | Tool log presence, unique tool count, success/fail/retry ratio |
| Reliability | Trace failure count, retry recovery, output cleanliness |
| Efficiency | Step count vs complexity, execution time, duplicate step detection |
| Safety & Compliance | PII pattern scan, dangerous keyword check, hallucination signals |
| Autonomy | Intervention log parsing — "None"=5.0, count-based deduction |

---

## Data Model

### Agent
```json
{
  "id": "agent_cowork01",
  "agent_name": "Claude Cowork Agent",
  "agent_type": "autonomous",
  "provider": "Anthropic",
  "model_name": "claude-3-5-sonnet-20241022",
  "framework": "Claude Cowork",
  "supports_tool_use": true,
  "supports_multi_step_execution": true,
  "supports_browser_actions": true,
  "claude_cowork_compatible": true
}
```

### Evaluation Run (summary)
```json
{
  "id": "eval_cc001",
  "evaluation_id": "EVL-2025-0001",
  "title": "Claude Cowork: Q1 Executive Report",
  "agent_id": "agent_cowork01",
  "scenario_id": "scen_001",
  "status": "Approved",
  "task_description": "...",
  "expected_outcome": "...",
  "final_output_text": "...",
  "execution_trace_text": "...",
  "tool_usage_log_text": "...",
  "human_interventions_text": "...",
  "autonomy_level": "high",
  "reviewer_name": "Sarah Chen",
  "approver_name": "James Williams"
}
```

### Scorecard
```json
{
  "evaluation_run_id": "eval_cc001",
  "goal_completion_score": 4.5,
  "correctness_score": 4.2,
  "reasoning_score": 4.3,
  "tool_usage_score": 4.8,
  "reliability_score": 4.5,
  "efficiency_score": 4.0,
  "safety_score": 4.7,
  "autonomy_score": 4.5,
  "weighted_overall_score": 4.36,
  "key_issues": [],
  "recommendations": [],
  "risk_flags": []
}
```

---

## Seed Data Reference

### Agents
| ID | Name | Framework | Cowork? |
|----|------|-----------|---------|
| agent_cowork01 | Claude Cowork Agent | Claude Cowork | Yes |
| agent_langgraph01 | LangGraph Orchestrator | LangGraph | No |
| agent_crewai01 | CrewAI Research Team | CrewAI | No |
| agent_browser01 | Generic Browser Agent | Playwright | No |

### Sample Scenarios
| ID | Name | Type | Difficulty |
|----|------|------|-----------|
| SCN-001 | Executive Report Consolidation | Claude Cowork | Complex |
| SCN-002 | Structured Data Analysis Summary | Data Analysis | Medium |
| SCN-003 | Browser Research Compilation | Browser | Medium |
| SCN-004 | Multi-Step Business Workflow | Multi-Step | Complex |
| SCN-005 | AML Transaction Pattern Analysis | AML/GRC | Complex |
| SCN-006 | Contract Review and Legal Summarization | Doc Automation | Complex |
| SCN-007 | Competitive Intelligence Research | Generic | Medium |
| SCN-008 | Data Pipeline Validation | Data Analysis | Medium |
| SCN-009 | Risk Assessment Report Generation | AML/GRC | Complex |
| SCN-010 | Email Triage and Response Drafting | Generic | Simple |

### Judge Configurations
| Name | Mode | Strictness | Notable Weights |
|------|------|-----------|----------------|
| Standard LLM Judge | All LLM | Standard | Goal Completion×1.5, Safety×1.2 |
| Strict Compliance Judge | All LLM | Strict | Safety×2.5, Correctness×1.5 |
| Rules-Based Quick Check | All Rules | Lenient | Goal Completion×2.0, Safety×1.5 |
| Mixed: LLM Quality + Rules Structure | Mixed (4 LLM + 4 Rules) | Standard | Goal Completion×1.5, Safety×1.5 |

**Mixed config detail — LLM dims:** Goal Completion, Correctness, Reasoning, Safety
**Mixed config detail — Rules dims:** Tool Usage, Reliability, Efficiency, Autonomy

---

## Environment Variables

### Backend (`backend/.env`)
| Variable | Description |
|----------|-------------|
| `MONGO_URL` | MongoDB connection string |
| `DB_NAME` | Database name (default: `agenteval_db`) |
| `EMERGENT_LLM_KEY` | Emergent universal LLM key for Claude access |
| `CORS_ORIGINS` | Comma-separated allowed origins (use `*` for dev) |

### Frontend (`frontend/.env`)
| Variable | Description |
|----------|-------------|
| `REACT_APP_BACKEND_URL` | Backend base URL (e.g. `http://localhost:8001`) |

---

## Scoring Calculation

The weighted overall score is calculated as:

```
overall = Σ(dimension_score × weight) / Σ(weights)
```

Default weights are `1.0` for all 8 dimensions unless overridden by the selected Judge Config.

**Score interpretation:**

| Range | Meaning |
|-------|---------|
| 4.5 – 5.0 | Excellent |
| 3.5 – 4.5 | Good with minor issues |
| 2.5 – 3.5 | Adequate with notable gaps |
| 1.5 – 2.5 | Poor with significant failures |
| 0 – 1.5 | Failed or critically inadequate |

---

## Export Options

| Format | How |
|--------|-----|
| **Print / PDF** | Open any Evaluation Detail → click "Print" button |
| **CSV (all evaluations)** | Evaluations page → "Export CSV" button |
| **JSON (single eval)** | Evaluation Detail → "Export JSON" button (includes agent, scenario, judge config, scorecard) |
| **Audit Log CSV** | Governance page → "Export CSV" button |
| **Bulk Import CSV** | Evaluations page → "Bulk Import" button — upload CSV to create multiple evaluations at once |

Download a blank bulk import template from the Bulk Import dialog. Required columns: `title`, `agent_name`, `task_description`. See `/docs/sample-inputs/bulk_import_sample.csv` for a complete example.

---

## Roadmap

### Next (P1)
- Evidence file attachment support (upload PDFs, screenshots)
- Email notification when an evaluation is approved
- Role-based access control (Reviewer / Approver / Admin)
- Bulk comparison view for multiple evaluation runs
- Evaluation templates (copy from existing run)

### Future (P2)
- Multi-tenant enterprise setup
- Direct trace import from LangSmith, LangFuse, and other platforms
- Model registry integration
- Workflow approval engine with email routing
- Webhooks for external integrations
- Semantic search across evaluation history

---

## License

MIT License — see `LICENSE` for details.

---

*Built with React, FastAPI, MongoDB, and Claude AI.*
