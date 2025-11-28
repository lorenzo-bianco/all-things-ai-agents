# 📄 Listing Moderation Agent

AI Agent that evaluates listings metadata to detect prohibited content, scams, inconsistencies or low-quality patterns before publication.
It helps reduce manual review workload and keeps the marketplace safe and trustworthy.

⸻

🎯 Problems

1. User-generated listings often contain misleading info, risky content or policy violations
2. Manual moderation is slow, inconsistent and expensive.
3. Bad listings reaching users damage trust and conversion.

This agent aims to automate the first moderation layer so humans handle only real edge cases.

⸻

🧩 Context

•	Runs when a listing is created or edited

•	Useful for both pre and post publication checks

•	Supports trust and safety content teams

•	Designed for high-volume marketplaces

⸻

📥 Inputs & Outputs (overview)

→ Inputs

•	title

•	description

•	metadata (title, description, price, location, listing type, characteristics, etc)

•	optional: user signals (user related attributes, user marketplace latest interactions, etc)

→ Outputs

•	decision: approve, reject, manual_review
  
•	reason: explanation of why
  
•	suggestions: optional improvements

⸻

⚙️ How it works

1.	Reads metatadata and context

2.	Identifies risky or prohibited 

3.	Checks coherence between fields

4.	Evaluates clarity and completeness

5.	Produces a structured moderation result
	
6.	Passes output to automation (n8n) for routing

⸻

📂 Files in this folder

•	system_prompt.md — core moderation logic

•	examples.md — sample inputs/outputs

•	workflow_n8n.json — automation flow

•	evaluation.md — criteria for quality

•	schema.md — input/output contract

⸻

⚠️ Limitations

•	Needs webhook events or manual inputs to run

•	Ambiguous listings may require manual review

•	Model accuracy depends on text completeness

⸻

🚀 Next steps

•	Add images as another moderation input

•	Add more risk signals

•	Expand evaluation set

•	Improve suggestions for low-quality text
