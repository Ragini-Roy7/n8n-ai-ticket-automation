# AI Ticket Triage — n8n + Google Gemini (Proof of Concept)

An n8n workflow that ingests customer support tickets, classifies them using an LLM-based AI Agent, and recommends whether a ticket should be escalated to a human — built as a proof of concept to explore agent-based automation patterns beyond simple prompt/response LLM calls.

> **Status:** Proof of concept — tested with sample ticket data. Not deployed to production. Requires the user's own n8n instance and Gemini API credentials to run.

**Demo video:** https://drive.google.com/file/d/1zW_8S4_wXaqwLDbCVjsjNOwx3-RUk-7p/view?usp=sharing

---

## What it does

Given a support ticket (customer name, email, subject, message), the workflow:

1. Accepts ticket input through a webhook or an n8n form
2. Processes ticket information using a Basic LLM Chain and an AI Agent
3. Uses Gemini to analyze the ticket and classify its category, priority, sentiment, summary, and recommended action
4. Allows the AI Agent to use a sub-workflow as a tool when human escalation is required

This was built to understand how an LLM can act as a decision-making component inside an automation pipeline — not just as a text generator — including tool-calling and conditional escalation logic.

## Architecture

**Webhook-based ingestion**

```
Customer support ticket (JSON)
        │
        ▼
     Webhook
        │
   ┌────┴─────┐
   ▼          ▼
Basic LLM    AI Agent (Gemini)
Chain          │
   │           └─ Tool → Escalation sub-workflow
   ▼
Structured
Output
```

**Form-based ingestion**

```
n8n Form  →  Edit Fields  →  Normalized ticket JSON
```

The form collects Customer Name, Email, Subject, and Message, then maps them into a consistent JSON payload:

```json
{
  "customer_name": "...",
  "email": "...",
  "subject": "...",
  "message": "..."
}
```

## Ticket classification

Each ticket is analyzed across five dimensions:

| Field              | Description                                         |
| ------------------ | --------------------------------------------------- |
| Category           | Payment, Technical, Account, Order, Delivery, Other |
| Priority           | Low, Medium, High, Critical                         |
| Sentiment          | Positive, Neutral, Frustrated, Angry                |
| Summary            | LLM-generated description of the issue              |
| Recommended Action | Suggested next step                                 |

## Escalation logic

The AI Agent decides whether a ticket needs human intervention, escalating when:

* Priority is Critical
* There's an unauthorized payment or security concern
* A serious payment issue requires human judgment
* The customer explicitly asks for human support

When escalation is warranted, the agent calls an n8n sub-workflow as a tool to trigger it — rather than the escalation being hardcoded with `if/else` logic.

## Example

**Input**

```json
{
  "customer_name": "Priya",
  "email": "priya@example.com",
  "subject": "Cannot login to my account",
  "message": "I have tried resetting my password three times, but I still cannot log into my account. This is very frustrating."
}
```

**Output**

```json
{
  "category": "Account",
  "priority": "High",
  "sentiment": "Frustrated",
  "summary": "Customer is unable to access their account despite multiple password reset attempts.",
  "recommended_action": "Verify the account and investigate the login or password reset issue."
}
```

## Tech stack

* **n8n** — workflow orchestration
* **Google Gemini** — LLM for classification and reasoning
* **AI Agent (n8n)** — tool-calling and decision-making
* **Structured Output Parser** — structured LLM output
* **Webhooks & n8n Forms** — ticket ingestion

## Repository structure

```
n8n-ai-ticket-automation/
├── ai-ticket-automation.json   # Exported n8n workflow
├── README.md
└── .gitignore
```

## Setup

1. Install and run [n8n](https://docs.n8n.io/hosting/) (or use n8n Cloud).
2. Import `ai-ticket-automation.json` into your instance.
3. Add your own Google Gemini credentials in n8n's credential manager.
4. Review the AI Agent's system prompt and the escalation sub-workflow configuration.
5. Activate the workflow.
6. Submit a test ticket via the webhook endpoint or the n8n form.

> API credentials are **not** included in this repository. Configure your own before running.

## What this explores

This project was primarily about learning how LLMs fit into automation pipelines as reasoning/decision components, specifically:

* Structured output parsing for downstream JSON
* AI agent tool-calling (invoking a sub-workflow rather than static branching)
* Conditional human escalation
* Connecting webhook and form-based inputs to AI processing

## Notes

This repo contains an exported n8n workflow for portfolio and learning purposes. It is a proof of concept tested with sample data, not a production deployment — review and adapt the workflow before using it against real customer data.

