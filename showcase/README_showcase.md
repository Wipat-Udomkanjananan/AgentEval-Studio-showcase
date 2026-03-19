# AgentEval Studio — Showcase Assets

This folder contains **screenshots**, sample output files, and export examples to demonstrate the capabilities of **AgentEval Studio** — an enterprise-grade AI agent evaluation platform.

---

## Screenshots

| Screen | File | Description |
|---|---|---|
| Login | `screenshot_01_login.png` | Google OAuth login page with feature highlights |
| Dashboard | `screenshot_02_dashboard.png` | KPI cards + Score Trend + Agent Comparison charts |
| Evaluations | `screenshot_03_evaluations.png` | Full evaluation runs table with status, scores, filters |
| Evaluation Detail | `screenshot_04_evaluation_detail.png` | 5-tab detail view with Run Judge + Approve actions |
| Scorecard | `screenshot_05_scorecard.png` | 8-dimension score bars + radar chart + risk flags |
| Agent Registry | `screenshot_06_agents.png` | Agent list with type, model, framework, capability tags |
| Scenario Library | `screenshot_07_scenarios.png` | Scenario card grid with difficulty/risk badges |
| Judge Config | `screenshot_08_judge_config.png` | Judge configs with weight multipliers + Simulate button |
| Analytics | `screenshot_09_analytics.png` | Radar comparison + grouped bar + performance heatmap |
| Governance | `screenshot_10_governance.png` | Immutable audit trail with entity/action filters |

---

## Sample Output Files

### 1. `sample_evaluation_export.json`
A full JSON export of a single evaluation run (`EVL-2025-0001: Claude Cowork Q1 Executive Report Consolidation`).

Contains:
- Evaluation metadata (ID, title, status, dates, reviewer/approver)
- Agent details (name, type, provider, model, framework)
- Linked scenario (domain, difficulty, risk level, expected outcome)
- Full scorecard with all 8 evaluation dimensions
- Scoring rationale, key issues, risk flags, and recommendations
- Execution trace and tool usage log

**Use case:** Export individual evaluation reports for audits, regulatory submissions, or archival.

---

### 2. `sample_evaluations_export.csv`
A bulk CSV export of all evaluation runs across agents and scenarios.

Columns:
```
Evaluation ID, Title, Agent, Status, Date, Reviewer,
Goal Completion, Correctness, Reasoning, Tool Usage,
Reliability, Efficiency, Safety, Autonomy, Overall Score
```

**Use case:** Import into Excel, BI tools (Power BI, Tableau), or data pipelines for trend analysis and governance reporting.

---

### 3. `sample_judge_result.json`
A raw output from the **Agent-as-a-Judge** feature powered by **Claude Sonnet 4.5**.

The judge evaluated an AML transaction analysis task using `Strict` strictness and returned:
- Scores for all 8 evaluation dimensions (0.0–5.0 scale)
- Weighted overall score
- Detailed scoring rationale
- Key issues identified
- Risk flags raised
- Actionable recommendations

**Use case:** Demonstrates real LLM-powered evaluation — no human scoring needed. Claude reviews agent outputs end-to-end and generates structured scoring JSON.

---

### 4. `sample_analytics_overview.json`
Aggregated analytics across all evaluation runs.

Includes:
- Total runs, average overall score, safety avg, autonomy avg
- Status distribution (Draft / In Review / Completed / Approved)
- Per-dimension score averages (all 8 dimensions)
- Agent comparison table
- Top recurring issues
- Recent evaluation activity

**Use case:** Feed into dashboards, reporting pipelines, or compliance tools.

---

## Evaluation Dimensions (8-Axis Scoring System)

| Dimension | Description | Scale |
|---|---|---|
| Goal Completion | Did the agent complete the task end-to-end? | 0–5 |
| Correctness | Is the output factually and operationally correct? | 0–5 |
| Reasoning Quality | Did the agent decompose and sequence work correctly? | 0–5 |
| Tool Usage | Were tools chosen and used effectively? | 0–5 |
| Reliability | Was the agent consistent across multi-step execution? | 0–5 |
| Efficiency | Did the agent complete the task with reasonable steps? | 0–5 |
| Safety & Compliance | Did the agent avoid risky or non-compliant behavior? | 0–5 |
| Autonomy Level | How independently did the agent operate? | 0–5 |

---

## Key Features Demonstrated

- **Agent-as-a-Judge** — Claude Sonnet 4.5 evaluates agent outputs using configurable rubrics and weight profiles
- **Multi-agent support** — Evaluate Claude Cowork, LangGraph, CrewAI, and custom agents
- **Scenario library** — Pre-built test scenarios across domains (Finance, AML, Legal, Operations, Research)
- **Governance & audit trail** — Immutable audit log for all evaluation actions and approvals
- **Bulk exports** — CSV for BI tools, JSON for downstream integrations
- **Configurable judges** — Custom prompt templates, strictness levels (Lenient / Standard / Strict), and per-dimension weight multipliers

---

## Quick API Reference

```bash
# Run a judge evaluation (Claude Sonnet 4.5)
POST /api/judge/simulate
{
  "task_description": "...",
  "worker_output": "...",
  "strictness": "Standard",
  "save_scorecard": true,
  "evaluation_run_id": "eval_xxx"
}

# Export all evaluations as CSV
GET /api/export/evaluations-csv

# Export single evaluation as JSON
GET /api/export/evaluation/{eval_id}/json

# Get analytics overview
GET /api/analytics/overview
```

---

## Tech Stack

- **Backend:** FastAPI + Motor (async MongoDB)
- **Frontend:** React + Tailwind CSS + Recharts + shadcn/ui
- **LLM Judge:** Claude Sonnet 4.5 via Emergent Integrations
- **Database:** MongoDB

See the main [README.md](../../README.md) for full setup instructions.
