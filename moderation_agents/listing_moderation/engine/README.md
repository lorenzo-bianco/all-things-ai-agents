## 🤖 Overview of the moderation engine anatomy

The moderation agent needs a simple, explicit rule engine that orchestrates how content flows through the system. 

At a high level, the process is:

1.	Input
   
A payload containing all content attributes enters the system (via webhook, sheet update, or manual submission). This payload is the event trigger and the source of truth for all downstream steps.

2.	Content processing

The workflow nodes use that payload to call an LLM-powered moderation agent. The agent returns a decision (approval/rejection) and an action (activate/deactivate) that defines what must happen to the content and, if needed, to the user who submitted it.

3.	Output

The agent outputs decision + action + rationale.
The workflow then executes the required moderation action via API call to your platform or records it for manual review / escalation to a third party (user, moderator, internal team).




## 🛠️ Into the specifics of each phase

👉 [Phase 1] Input: triggers and data intake

For this agent, incoming data can originate from multiple sources:

a.	Webhook POST call
  
  If your platform emits events with attributes, simply forward the “publish request” (or any moderation-relevant event) to the n8n webhook endpoint. The entire event becomes the payload.

b.	Google Sheet update

If content lives in a spreadsheet, adding a new row (with each cell representing a content attribute) can trigger the workflow.
You can also run scheduled batch moderation (e.g., every X minutes).
Just connect your Google Sheet credentials and use the When new row added trigger.

c.	Manual input

Useful in early stages. You can manually paste the payload or use an n8n Form node where each field maps to a content attribute.

Which input path you choose depends on your stack maturity:

•	Webhook pipelines → platforms with event systems (Segment, in-house events, etc.)

•	Sheet triggers → early-stage products or lightweight PoCs without a full data pipeline

⸻

👉 [Phase 2] Content processing: generating the moderation decision

The agent receives the payload only when execution windows allow it. The payload must contain all information required for the agent to make a reliable decision.

The moderation agent relies on four components:

1. Prompt & System Message

•	The system message defines identity, reasoning style, and operational boundaries.

•	The prompt defines the workflow: what to analyze, how to output the decision, and which rules apply. A dedicated section explains this in detail (/listing_moderation/prompt).

2. Chat Model

The model choice depends on complexity and cost:

•	Simple content → low-cost models (OpenAI, Gemini, Anthropic, etc.).

•	Complex/ high-risk content → more robust models with stronger reasoning.

Also consider expected moderation volume, latency, and budget.

3. Tools

The agent may use external tools to contextualize the moderation decision. For example:

•	Query a database (BigQuery, Sheets, Pinecone, etc.) for user attributes, trust signals, or behavioral history.

•	Follow an HTTP request to fetch and analyze external HTML or metadata.

More context → better decisions, but always balanced with cost and execution time.

4. Settings

_Work in progress..._
