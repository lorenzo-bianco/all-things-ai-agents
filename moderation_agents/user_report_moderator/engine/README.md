# How to read, configure and adapt the workflow engine template

This workflow acts as the foundation on which you can configure and adapt your moderation pipeline to your specific use case.
The image explains the main phases; below are additional clarifications.

<img width="1901" height="588" alt="user-report-moderation-workflow" src="https://github.com/user-attachments/assets/1af34f8d-7efd-4bc0-a960-81588dc20653" />


## Phase 1 — Input

Choose the trigger based on your operational stage.

• For advanced setups, start the flow from an incoming event payload via webhook. You must configure your platform to call the node’s endpoint and send the report attributes in the payload.
• If you use a database trigger (e.g., new row added in a sheet), simply connect the reference table.

Then decide the entry criteria. You can forward all reports to the agent, or apply quantitative filters (e.g., only process if that item has ≥2 reports) or qualitative ones (e.g., only forward specific categories/topics extracted from the payload).

<img width="1912" height="587" alt="template1" src="https://github.com/user-attachments/assets/50d35621-3fc5-4418-a19a-34dacca4c281" />

## Phase 2 — Information processing

The agent must be configured with four components:

a) Prompt

A dedicated prompt folder contains examples, but general rules are:

Explain the context (What platform is this? What type of reports? What type of items? Who reports? How do they report? What is acceptable / not acceptable?) and encode the logic (“if X happens, do Y”).
Then feed all relevant values from the payload and explain what they represent (e.g., price = 40 means the marketplace item costs 40).

b) Tools

Detail the tools the agent can call (e.g., GET_USER_INFO returns all user attributes, including “number_of_reports”) and when to use them.
Configure these tools according to your actual data sources, tables, and collection logic.

c) Desired output

Specify the expected output format. Typically:
• action → what must be executed
• rationale → why the agent chose it (crucial for debugging, QA, and iterative improvement)

d) Model selection

Choose the model (OpenAI, Gemini, etc.) based on complexity, latency and budget constraints.

<img width="1911" height="587" alt="template2" src="https://github.com/user-attachments/assets/6db28fde-6b88-4032-8de3-ece65688159d" />

## Phase 3 — Action execution and follow-up steps

1.	Execute the action by calling your systems. Configure the HTTP Request with your endpoint and include in the payload: the operation, the target item ID, and a workflow identifier for auditability.
2.	Send notifications internally (e.g., support team) and/or externally (e.g., user).
This may be another HTTP Request configured with your email/notification provider (Braze, Mailchimp, etc.), or a built-in n8n node such as Gmail.
3.	Log the action in your database.
This creates a reliable audit trail and enables aggregate analysis to refine moderation rules over time.

<img width="1907" height="585" alt="template3" src="https://github.com/user-attachments/assets/781eca82-eb8f-4a74-bf05-53ef698b15c5" />
