\# AI Ticket Automation with n8n + Gemini



An AI-powered customer support ticket automation workflow built with \*\*n8n\*\* and \*\*Google Gemini\*\*.



The workflow takes an incoming customer support ticket, analyzes the issue using an LLM, classifies the ticket based on category, priority and sentiment, generates a summary and recommended action, and can escalate critical issues to human support.



\## Workflow Overview



\### Webhook-based workflow



```text

Customer Support Ticket

&#x20;       ↓

&#x20;    Webhook

&#x20;       ↓

&#x20;  ┌────┴────┐

&#x20;  ↓         ↓

Basic LLM   AI Agent

&#x20;Chain        ↓

&#x20;  ↓       Gemini

&#x20;Gemini       ↓

&#x20;  ↓     Escalation Tool

Structured

Classification

```



\### Form-based input



```text

n8n Form

&#x20;  ↓

Edit Fields

&#x20;  ↓

Normalized Ticket Data

```



The form collects:



\* Customer Name

\* Email

\* Subject

\* Message



The \*\*Edit Fields\*\* node maps these form fields into a consistent JSON structure:



```json

{

&#x20; "customer\_name": "...",

&#x20; "email": "...",

&#x20; "subject": "...",

&#x20; "message": "..."

}

```



\## AI Ticket Classification



The workflow analyzes each ticket across the following dimensions:



| Field              | Possible Values                                     |

| ------------------ | --------------------------------------------------- |

| Category           | Payment, Technical, Account, Order, Delivery, Other |

| Priority           | Low, Medium, High, Critical                         |

| Sentiment          | Positive, Neutral, Frustrated, Angry                |

| Summary            | Generated description of the issue                  |

| Recommended Action | Suggested next step                                 |



\## AI Agent and Escalation



The AI Agent is instructed to determine whether a ticket requires human intervention.



Escalation can be triggered when:



\* The priority is Critical

\* There is an unauthorized payment or security concern

\* A serious payment issue requires human intervention

\* The customer explicitly requests human support



The AI Agent can use an n8n workflow tool to trigger the escalation workflow.



\## Technology Used



\* \*\*n8n\*\* — workflow automation and orchestration

\* \*\*Google Gemini\*\* — LLM for ticket analysis and classification

\* \*\*AI Agent\*\* — decision-making and tool usage

\* \*\*Structured Output Parser\*\* — structured ticket classification

\* \*\*Webhooks\*\* — HTTP-based ticket ingestion

\* \*\*n8n Forms\*\* — customer ticket submission



\## Example Input



```json

{

&#x20; "customer\_name": "Priya",

&#x20; "email": "priya@example.com",

&#x20; "subject": "Cannot login to my account",

&#x20; "message": "I have tried resetting my password three times, but I still cannot log into my account. This is very frustrating."

}

```



\## Example Classification



```json

{

&#x20; "category": "Account",

&#x20; "priority": "High",

&#x20; "sentiment": "Frustrated",

&#x20; "summary": "Customer is unable to access their account despite multiple password reset attempts.",

&#x20; "recommended\_action": "Verify the account and investigate the login or password reset issue."

}

```



\## Key Learning



This project helped me understand how LLMs can be integrated into an automation workflow rather than being used only for text generation.



The main concepts explored were:



\* LLM-based classification

\* AI agent workflows

\* Structured outputs

\* Tool-based escalation

\* Webhook-based automation

\* Input normalization

\* Human-in-the-loop support workflows



\## Project Structure



```text

n8n-ai-ticket-automation/

│

├── ai-ticket-automation.json

├── README.md

└── .gitignore

```



\## Setup



1\. Install or run n8n.

2\. Import `ai-ticket-automation.json`.

3\. Configure your own Google Gemini credentials in n8n.

4\. Review the workflow and escalation sub-workflow configuration.

5\. Activate the workflow.

6\. Send a test ticket through the webhook or n8n form.



> API credentials are not included in this repository. Users should configure their own credentials in n8n.



\## Note



This repository contains the exported n8n workflow configuration for learning and portfolio demonstration. The workflow should be reviewed and configured with the user's own credentials and environment before use.



