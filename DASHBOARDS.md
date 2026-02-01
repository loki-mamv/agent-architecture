# MAMV Agent Dashboards — Specification

*Core analytics dashboards for the agent architecture. Atlas (Analytics Agent) extends these with ad-hoc analysis.*

---

## Dashboard 1: Agent Health Overview

**Purpose:** Real-time status of all agents at a glance.

| Metric | Source | Display |
|--------|--------|---------|
| Agent Status | Heartbeat check | 🟢 Active / 🟡 Idle / 🔴 Error |
| Last Heartbeat | Timestamp | "2m ago", "15m ago" |
| Uptime % | Successful heartbeats / total | 99.2%, 87.5% |
| Current Task | Kanban API | Task name or "Idle" |
| Error Count (24h) | Session logs | Badge count |

**Layout:** Card grid, one card per agent with avatar, status indicator, and key metrics.

---

## Dashboard 2: Cost & Token Analytics

**Purpose:** Track spending per agent, identify cost anomalies.

| Metric | Calculation | Display |
|--------|-------------|---------|
| Tokens Today | Sum of session tokens | Bar chart by agent |
| Tokens This Week | Rolling 7-day sum | Line chart trend |
| Cost Per Agent | Tokens × model rate | Dollar amounts |
| Total Daily Cost | Sum all agents | Big number |
| Cost vs. Budget | Actual / target | Progress bar |
| Anomaly Flag | > 2x 7-day avg | Alert badge |

**Layout:** 
- Top: 4 stat cards (Total Cost Today, Week, Avg Per Agent, Budget %)
- Middle: Stacked bar chart (tokens by agent by day)
- Bottom: Cost trend line (7-day)

---

## Dashboard 3: Output & Velocity

**Purpose:** Measure what agents actually produce.

| Metric | Source | Display |
|--------|--------|---------|
| Tasks Completed | Kanban "Done" lane | Count per agent |
| Tasks In Progress | Kanban lane | Current count |
| Avg Task Duration | Start → Done timestamps | Hours/minutes |
| Output by Type | Task tags | Pie chart (content, outreach, code, research) |
| Velocity Trend | Tasks/day over time | Sparkline |
| Blocked Tasks | Kanban blockers | Count + list |

**Layout:**
- Top: Stat cards (Completed Today, This Week, Avg Duration)
- Middle: Bar chart (output by agent) + Pie chart (by type)
- Bottom: Activity feed (recent completions)

---

## Dashboard 4: Quality & Performance

**Purpose:** Spot drift, errors, and quality issues early.

| Metric | Source | Display |
|--------|--------|---------|
| Error Rate | Failed tool calls / total | % per agent |
| Retry Count | Session logs | Count |
| Drift Score | Output relevance (Loki assessment) | 1-10 scale |
| Review Queue | Items pending Loki review | Count |
| Quality Flags | Manual flags from reviews | List |
| Model Version | OpenRouter response headers | Version string |

**Layout:**
- Top: Health bars per agent (green/yellow/red)
- Middle: Error trend (line chart, 7-day)
- Bottom: Quality log (flagged items with timestamps)

---

## Dashboard 5: Activity Timeline

**Purpose:** See what happened, when.

| Event Type | Data |
|------------|------|
| Heartbeat | Agent woke, status |
| Task Started | Agent, task name |
| Task Completed | Agent, task name, duration |
| File Written | Agent, file path |
| Error | Agent, error type, message |
| Alert | Agent, alert type |

**Layout:** Chronological timeline with filters (by agent, by event type, by date range).

---

## Atlas (Analytics Agent) Responsibilities

Atlas is a Tier 2/3 agent specialized in analytics. Core dashboards above are built-in. Atlas handles:

1. **Ad-Hoc Queries** — "How much did we spend on content this month?"
2. **Custom Reports** — Weekly summaries, board presentations
3. **Anomaly Investigation** — Deep-dive when alerts fire
4. **Trend Analysis** — Spot patterns humans miss
5. **Dashboard Suggestions** — Propose new metrics based on data

Atlas reads from shared memory files where agents log their activity, plus direct API access to:
- Kanban (task data)
- Session logs (token usage, errors)
- OpenRouter (cost data)

---

## Implementation Notes

- **Tech Stack:** HTML/CSS/JS (static), same pattern as main deck
- **Data:** Initially mock data, later pull from real APIs
- **Location:** `analytics.html` in the agent-architecture repo
- **Access:** Via Tailscale, same as Lokiban

---

*Spec v1 — Feb 2026*
