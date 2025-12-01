# User report moderation agent

AI-powered workflow that processes every user report on a marketplace listing, aggregates report history, enriches context, and returns a final enforcement action. It's very easy to implement and it might make you able to save money if you are currently moderating content with humans or with linear, simple logic-driven workflows.

## 👉 What this agent does

This agent receives a reported item (listing) and automatically decides what to do with it by combining:
	•	the current report received
	•	the history of reports for that item/user (from Google Sheets, DB, or any data source)
	•	the reported user’s attributes
	•	the content attributes
	•	the user’s past messages (if provided)

The AI Agent then produces a final outcome:
	•	ignore → reports not credible or false positive
	•	deactivate_content → content violates rules
	•	deactivate_user → severe abuse, repeat offender, or high-risk behavior

## 👉 High-level flow

### 1. Input

The workflow is triggered every time a report is received.
Input may come from:
	•	a webhook POST from your platform
	•	a Google Sheet row created/updated
	•	a manual payload (debug/testing)

The payload must include at least:
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
### 2. Data enrichment

Before calling the AI agent, the workflow gathers context:
	
•	Fetch report history for the same item/user

•	Fetch user attributes (past behaviour, trust signals, violations)

•	Fetch item details (title, description, category, etc.)

•	Fetch user messages (optional, if relevant for abuse checks)

This enriched payload becomes the decision surface.

### 3. AI Moderation Decision

The LLM agent receives a consolidated JSON containing:

•	the incoming report

•	all historical reports

•	user data

•	content data
	
•	messages

•	moderation rules

The agent must output:
```
{
  "outcome": "ignore | deactivate_content | deactivate_user",
  "rationale": "string"
}
```
You may extend the output schema in the prompt as needed (severity score, confidence, etc.).

### 4. Action Execution

Workflow nodes perform operational tasks:
	•	Log the decision (analytics, audits, continuous tuning)
	•	Send API call to apply the enforcement:
	•	deactivate item
	•	deactivate user
	•	or do nothing
	•	Optional notifications
	•	internal team alerts
	•	user communication

### 👉 Example input

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

### 👉 Example AI output

```
{
  "action": "<ignore | disable item | disable user>",
  "reason": "<short, clear explanation of why this action was selected>",
  "reports_summary": [
    {
      "topic": "...",
      "content": "...",
      "reporter": "..."
    }
  ],
  "item_id": "{{ $json.item_id }}"
}
```

## 👉 When to use this agent

Use it when you need to:

•	process all incoming reports automatically
  
•	enforce actions consistently
  
•	remove bias or manual inconsistencies

•	scale trust & safety without proportional headcount

•	create a clean, auditable moderation pipeline

## 👉 Requirements

•	An event source (webhook, sheet, or queue)

•	DB or Sheet containing report history (optional but recommended)

•	API endpoints to deactivate items/users

•	LLM provider (OpenAI, Anthropic, Gemini, etc.)

•	n8n (or similar) as the orchestration layer
