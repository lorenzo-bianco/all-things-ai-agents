## 🛡️ **Moderation AI Agents for marketplace content quality and safety**

A collection of plug-and-play AI Agents designed to keep marketplace environments clean, safe and high-quality.

They reduce manual review load, detect risky or low-quality content, and enforce consistent decisions at scale.

Perfect for products with large volumes of user-generated content where speed, consistency and trust are critical.

## 🔍 **What’s inside**

→ [user report_moderator/](https://github.com/lorenzo-bianco/all-things-ai-agents/tree/main/moderation_agents/user_report_moderator)

AI Agent that receives a user-submitted report, fetches additional context on demand (past reports, user attributes, item signals, message history, etc) and decides whether to ignore, deactivate content, or deactivate the user.

→ chat_moderator/

AI Agent that monitors buyer–seller conversations, flagging harmful behaviour, scam patterns, unsafe interactions or policy-violating messages.

## 🛠️ **Structure of each agent**

Each agent folder uses the same structure so you always know where to look:

•	README.md → general context and high-level info about how the agent works

•	engine/ → n8n automation setup overview + JSON of the workflow template

•	prompt.md → system + execution prompts defining the agent’s logic

•	use_cases.md → concrete examples showing how the agent behaves

## 👉 **Why these agents matter**

Moderation is expensive, slow and inconsistent when powered only by humans or rigid rules.

These AI Agents are built to:
	
•	Reduce manual review time

•	Catch risky cases earlier

•	Improve content quality before it reaches users

•	Enforce clear, explainable decisions

•	Scale trust and safety without growing operational teams
