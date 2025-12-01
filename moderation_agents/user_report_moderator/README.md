# User Report Moderation Agent

This is a plug-and-play, AI-powered moderation agent that proactively starts whenever a user submits a report.

It acts immediately to address issues raised by users, preventing a single bad experience from spreading to others.

Once triggered, the agent fetches all relevant context on demand (item report history, user attributes, item details, user message history, and any other data sources you connect) and returns a single enforcement action with a clear, auditable rationale.

The result is a consistent and scalable moderation pipeline that keeps your marketplace safe and clean without adding operational overhead.

## 👉 Repository structure 

```
user_report_moderator/
│
├── README.md
│   → Overview of the agent, high-level behaviour, examples.
│
├── engine/
│   ├── workflow_n8n.json
│   │   → The full n8n workflow template, ready to import.
│   └── README.md
│       → Setup instructions: how to use and adapt the workflow engine.
│
├── prompt.md
│   → Contains both prompts used by the LLM:
│       1. System prompt (identity + rules + constraints)
│       2. Execution prompt (the step-by-step moderation logic)
│
└── use_cases.md
    → Practical examples showing how the agent behaves
      with different scenarios, inputs, and expected outputs.
```

## 👉 High-level flow

**1. Input**

Triggered whenever a report arrives through any of the supported sources:

•	Webhook POST from your platform

•	New row in a Google Sheet

•	DB event or message queue

•	Manual payload for debugging

Minimum recommended fields:

```
{
  "item_id": "",
  "reported_user_id": "",
  "reporter_user_id": "",
  "report_type": "",
  "report_reason": "",
  "metadata": { ... }
}
```
⸻

**2. Context Gathering + Moderation Decision**

The agent receives the input payload and then pulls additional context on demand using the available tools.

Depending on the specific scenario, it may fetch:

•	Item report history (other reports about the same item)

•	User attributes (behaviour, trust signals, past violations of the reported item owner)

•	Item details (description, metadata, category, content signals)

•	User message history (if your product has a chat)

•	Any other datasource you connect


The agent decides what to fetch and when to fetch it, based on the logic defined in the prompt.

After combining the collected signals, the agent outputs one final action:

```
{
  "action": "ignore | disable_item | disable_user",
  "reason": "string"
}
```

You can extend the schema (severity, confidence score, flags, metadata) directly inside the prompt.

⸻

**3. Action Execution**

The workflow then performs the operational steps:

•	Calls your API to enforce the decision

•	Logs the decision (for analytics, monitoring, audits purposes)

•	Sends optional notifications (internal alert or external user communication)

⸻

## 👉 Example input
```
{
  "item_id": "A1234",
  "reported_user_id": "U5678",
  "report_type": "scam",
  "report_reason": "suspicious payment link",
  "metadata": {
    "message_id": "M9123"
  }
}
```

## 👉 Example AI output

```
{
  "action": "disable_item",
  "reason": "Multiple consistent reports and suspicious off-platform contact",
  "reports_summary": [
    {
      "topic": "scam",
      "content": "asked for external payment link",
      "reporter": "user_11"
    }
  ],
  "item_id": "A1234"
}
```

## 👉 Requirements
	
•	Event source (webhook / Sheets / DB / queue)

•	Datasource for historical reports (recommended)

•	Enforcement API endpoints

•	LLM provider (OpenAI, Anthropic, Gemini, etc.)

•	n8n (or similar orchestration layer)
