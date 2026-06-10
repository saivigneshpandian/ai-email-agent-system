# AI Email Agent System

A complete multi-workflow AI email automation system built with n8n and Google Gemini. Automatically classifies incoming emails, handles customer support, and manages user onboarding — end to end without manual intervention.

---

## System Overview

This system consists of 3 connected workflows that work together:

```
Incoming Email
      ↓
Main Workflow (Entry Point)
      ↓
Email Classifier (AI)
   ↙          ↘
Customer      Onboarding
Support    ↙          ↘
Agent   Proactive    Reactive
        (Scheduled)  (Reply-based)
```

---

## Workflows

### 1. Main Workflow — Entry Point
**File:** `Email_Agent_Main_Workflow.json`

The brain of the system. Triggers on every incoming Gmail, classifies the email type using AI, extracts company info, segregates leads, and routes to the right sub-workflow automatically.

**What it does:**
- Triggers on Gmail incoming email
- Classifies email — customer support, onboarding, or lead
- Extracts company information using Google Gemini
- Routes to Customer Support or Onboarding workflow
- Stores all data in Google Sheets
- Marks emails as read automatically

**Nodes:** 44 · **Stack:** n8n · Gmail API · Google Gemini · Google Sheets · Google Docs

---

### 2. Onboarding — Proactive
**File:** `Email_Agent_Onboarding_Proactive.json`

Scheduled workflow that runs automatically to send personalized onboarding emails based on where each user is in their journey.

**What it does:**
- Runs on a schedule trigger
- Fetches active users from Google Sheets database
- Calculates which onboarding day each user is on
- Determines the right email type for that day
- Generates personalized email content using AI
- Sends via Gmail automatically
- Updates status in Google Sheets

**Nodes:** 25 · **Stack:** n8n · Gmail API · Google Gemini · Google Sheets · Google Docs

---

### 3. Onboarding — Reactive
**File:** `Email_Agent_Onboarding_Reactive.json`

Handles replies from users during onboarding. When a user responds to an onboarding email, this workflow reads their message, queries the knowledge base, and sends an intelligent reply.

**What it does:**
- Triggers on Slack or workflow call
- Fetches thread messages for context
- Classifies the user query using AI
- Queries Pinecone vector store for relevant answers
- Generates context-aware reply using RAG
- Sends reply via Gmail
- Updates onboarding status in Google Sheets

**Nodes:** 27 · **Stack:** n8n · Gmail API · Google Gemini · Pinecone · Google Sheets · Slack

---

## Screenshots

### Main Workflow
![Main Workflow](screenshots/main-workflow.png)

### Onboarding Proactive
![Onboarding Proactive](screenshots/onboarding-proactive.png)

### Onboarding Reactive
![Onboarding Reactive](screenshots/onboarding-reactive.png)

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| n8n | Workflow orchestration |
| Google Gemini API | AI classification, generation, embeddings |
| Pinecone | Vector store for knowledge base (RAG) |
| Gmail API | Trigger + send emails |
| Google Sheets | Database for users, leads, status tracking |
| Google Docs | Memory and content storage |
| Slack | Notifications and triggers |

---

## Key Features

- **Multi-workflow architecture** — 3 workflows connected and calling each other
- **AI email classification** — automatically sorts support vs onboarding vs leads
- **RAG-powered responses** — answers grounded in knowledge base
- **Proactive + Reactive** — both scheduled outreach and response handling
- **Full audit trail** — every interaction logged to Google Sheets
- **Lead segregation** — company info extracted and categorized automatically

---

## How to Use

### Prerequisites
- n8n instance (cloud or self-hosted)
- Google Gemini API key
- Pinecone account + API key
- Gmail OAuth2 credentials
- Google Sheets OAuth2 credentials
- Google Docs OAuth2 credentials
- Slack App credentials (for reactive workflow)

### Setup
1. Import all 3 JSON files into your n8n instance
2. Connect credentials for each service
3. Set up Google Sheets with required columns
4. Upload knowledge base to Pinecone
5. Activate Main Workflow first, then sub-workflows

### Import Order
1. `Email_Agent_Onboarding_Reactive.json` — import first
2. `Email_Agent_Onboarding_Proactive.json` — import second
3. `Email_Agent_Main_Workflow.json` — import last (calls the others)

> **Note:** No actual API keys are stored in these files. Connect your own credentials after importing.

---

## Results

- Zero manual email processing
- Personalized onboarding at scale
- Instant AI-powered replies to user queries
- Complete lead tracking and segregation
- Full conversation history maintained automatically

---

## Related Projects

- [Email Agent — Customer Support](https://github.com/saivigneshpandian/email-agent-customer-support) — AI customer support automation

---

## Built By

**Saivignesh P** — Agentic AI Engineer
[LinkedIn](https://linkedin.com/in/saivignesh-pandian) · [GitHub](https://github.com/saivigneshpandian)
