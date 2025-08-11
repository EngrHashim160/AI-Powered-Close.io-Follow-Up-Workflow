# 🤖 AI-Powered Close.io Follow-Up Workflow

An **n8n automation** that integrates **Close.io** with **OpenAI** to automatically craft and send highly personalized follow-up messages to leads after a "Follow-Up" task is completed.  
The workflow analyzes the lead’s data, recent communications, and opportunity details to choose the best channel (Email or SMS) and generate a tailored, response-driven message.

---

## 🚀 Features

- **Webhook Trigger** – Starts when a "Follow-Up" task is completed in Close.io.
- **Close.io Data Enrichment** – Retrieves:
  - Lead details
  - Contact information
  - Opportunities and status
  - Recent email and SMS communications
- **AI Message Generation** – Uses OpenAI GPT to:
  - Analyze communication history and opportunity details
  - Reference specific client context and roadblocks
  - Create unique, engaging follow-up messages
- **Channel Optimization** – Selects the best follow-up method (Email or SMS) based on history.
- **Automated Sending** – Posts the AI-generated follow-up back to Close.io for immediate delivery.

---

## 🔄 Workflow Logic

1. **Trigger**:  
   - Webhook listens for "Follow-Up" task completion events from Close.io.

2. **Lead Data Retrieval**:  
   - Get lead info (`/lead/{id}`)  
   - Get contacts linked to the lead (`/contact`)  
   - Get opportunity details (`/opportunity`)  
   - Get last 50 communications (email, SMS, replies)

3. **Data Merging & Prompt Creation** (`Code - Merge Lead Data`):  
   - Combines contact, opportunity, and communications data  
   - Detects preferred channel (Email/SMS) based on history  
   - Prepares an enhanced AI prompt with all details

4. **AI Processing** (`OpenAI`):  
   - Generates a JSON response with:
     ```json
     {
       "recommended_channel": "SMS" or "Email",
       "message": "Follow-up message",
       "reasoning": "Why this channel was chosen"
     }
     ```

5. **Response Parsing** (`Code - Process AI Response`):  
   - Extracts message, reasoning, and channel choice from AI output

6. **Routing** (`If - Route by Channel`):  
   - If **Email** → Send via Close.io email API  
   - If **SMS** → Send via Close.io SMS API

---

## 🛠 Requirements

### Credentials Needed in n8n:
- **Close.io API Key** (HTTP Basic Auth)
- **OpenAI API Key**

### Close.io Setup:
- Ensure the webhook endpoint in Close.io is set to your n8n webhook URL
- Event trigger: `task.lead` with action `completed`
- Task text filter: `Follow-Up`

---

## ⚙️ Environment Variables
You can store API keys securely in n8n credentials:

- `Close.io Credentials`
- `OpenAi account`

No `.env` file is required for this workflow.

---

## 📂 Node Overview

| Node Name | Purpose |
|-----------|---------|
| **Webhook** | Trigger when a follow-up task is completed in Close.io |
| **If** | Check if the task text is "Follow-Up" |
| **Close connection** | Register webhook with Close.io |
| **HTTP Request - Lead Info** | Fetch lead details |
| **HTTP Request - Contact** | Fetch contacts linked to the lead |
| **HTTP Request - Get Opportunities** | Retrieve open opportunities |
| **HTTP Request - Get Communications** | Get recent email & SMS history |
| **Code - Merge Lead Data** | Merge retrieved data and build AI prompt |
| **OpenAI** | Generate personalized follow-up content |
| **Code - Process AI Response** | Parse AI output and extract message/channel |
| **If - Route by Channel** | Route message to correct sending method |
| **HTTP Request - Send Email** | Send follow-up via email |
| **HTTP Request - SMS Send** | Send follow-up via SMS |

---


