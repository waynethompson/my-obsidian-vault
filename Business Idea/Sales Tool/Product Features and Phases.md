# AI Sales Tool — Product Features and Phases

An AI-assisted outbound sales system with a built-in lightweight CRM. The AI acts as a copilot — it researches leads, drafts messages, suggests actions, and keeps the CRM current. Every outbound message requires approval before it is sent.

---

## System overview

### 1. AI Sales Agent Layer

Handles:

- lead qualification
- outbound email drafting
- follow-up drafting
- meeting booking
- next-step recommendations
- objection handling
- account research

### 2. Lightweight CRM Layer

The system of record. Tracks:

- leads
- account status
- last touch
- next action
- conversation history
- confidence / qualification score
- pipeline stage

### 3. Human Review Layer

Provides:

- a simple dashboard
- alerts for deals needing attention
- approval queue for all outbound
- visibility into why the AI changed something

---

## CRM data model

These are the required CRM objects for Phase 1.

### Contacts

- name
- title
- email
- phone
- LinkedIn
- company
- status

### Companies / Accounts

- company name
- website
- industry
- size
- ICP fit score
- notes

### Opportunities / Deals

- pipeline stage
- value
- urgency
- close probability
- last activity
- next step
- risk flags

### Activities

- emails sent
- replies received
- meetings
- AI-generated summaries
- tasks created
- stage changes

### Tasks

- due date
- type
- AI suggested vs human created
- completion status

### AI State / Agent Memory

- qualification summary
- current strategy
- objections heard
- recommended next action
- confidence score
- do-not-contact / compliance flags

---

## Lead pipeline states

Each lead moves through a defined state machine. The AI recommends transitions; the system validates them.

- **New**
- **Enriched**
- **Qualified**
- **Contacted**
- **Replied**
- **Meeting Booked**
- **Opportunity Open**
- **Nurture**
- **Closed Won**
- **Closed Lost**

---

## Lead scraper

The primary way to generate leads is a natural language search that returns real businesses ready to import into the CRM.

### How it works

You type a plain English query:

> "Give me all the hair salons in the Newtown area"
> "Find plumbers in Brisbane CBD"
> "Show me gyms in the Northern Beaches"

The system:

1. Parses your query to extract a **business type** and **location**
2. Searches Google Maps / Google Places for matching businesses
3. Visits each business website to find **contact details** (email, contact form, owner/manager name)
4. Uses AI to extract and structure the contact data
5. Checks for duplicates against your existing CRM records
6. Presents a **review screen** before anything is imported

### Review screen

Before any lead enters the CRM you see a list of results with:

- business name
- address
- phone
- website
- found email(s) and contact name(s)
- ICP fit score (AI estimated)
- whether it already exists in your CRM

You select which ones to import. Nothing enters the CRM without your confirmation.

### What gets populated on import

Each imported lead creates:

- a **Company** record (name, website, address, industry, size estimate)
- a **Contact** record (name, email, phone if found)
- an initial **AI enrichment note** (what the AI found and its confidence)
- a **lead stage** of "New"
- an AI-suggested first outreach message in the approval queue

### Data sources

| Source | What it gives |
|---|---|
| Google Maps / Places API | Business name, address, phone, website, category, hours |
| Business website scraping | Email addresses, contact page, staff names, "About" content |
| AI extraction | Structured contact fields from unstructured web content |

### Limitations and honesty

- Not every business publishes an email address — the scraper records what it finds and flags what it could not locate
- Google Places API has usage costs and rate limits — the scraper respects these
- Website scraping can break when sites change structure — failures are logged and surfaced to you
- The quality of contact data varies significantly by industry and region

### Phase roadmap for the scraper

**Phase 1:** Google Maps query → website scraping → manual review → CRM import

**Phase 2:** Scheduled repeat searches (e.g. find new businesses in an area each week), smarter deduplication, LinkedIn enrichment

**Phase 3:** Broader data sources, industry-specific scrapers, automated lead scoring against your ICP profile

---

## Workflow design

Phase 1 optimises for outbound prospecting and follow-up.

### 1. Lead enters system

From:

- lead scraper (Google Maps search)
- manual entry
- inbound email

AI does:

- enrich company / contact
- score ICP fit
- assign initial stage
- suggest first outreach message

You can:

- approve or edit the outreach sequence
- approve each outbound message before it is sent

### 2. Outreach

AI does:

- personalise the message
- draft follow-ups for approval
- stop when a reply arrives
- classify the response:
    - interested
    - not now
    - not a fit
    - referral
    - objection
    - unsubscribe

Every approved step updates the CRM automatically.

### 3. Reply handling

AI drafts a response and suggests a next action:

- book demo
- answer objection
- share case study
- nurture later

All replies require your approval before sending in Phase 1.

### 4. Opportunity tracking *(Phase 2+)*

AI continuously updates:

- deal health
- momentum score
- next action recommendation
- stalled warning
- likely blockers

You can see:

- which deals are active
- which need intervention
- why the AI thinks a deal is at risk

---

## AI autonomy rules

### Safe to automate — no approval needed

- lead enrichment
- summarisation
- tagging
- follow-up reminders
- drafting messages
- CRM field updates with high confidence
- stale deal alerts

### Always require your approval

- all outbound emails in Phase 1
- pricing or discount discussions
- contract or legal statements
- procurement or security responses
- aggressive re-engagement

---

## Timeline and explainability

Every CRM record must show:

- what happened
- whether AI or human did it
- why it happened
- what changed

Example:

> **AI updated stage from "Contacted" to "Qualified"**
> Reason: prospect confirmed budget, timeline this quarter, and requested demo
> Confidence: 0.87

This is the feature that makes the system trustworthy.

---

## UI

### Main views

- **Pipeline view**
- **Lead inbox / queue**
- **Account detail page**
- **AI actions feed**
- **Approval queue**
- **Stalled deals dashboard**

### On each account page

- current stage
- last touch
- next recommended action
- recent messages
- AI summary
- risk / confidence score
- action buttons: Approve draft · Edit response · Escalate · Mark won/lost · Pause automation

---

## Phase roadmap

### Phase 1 — lightweight CRM + AI copilot

Scope:

- contacts, companies, deals, tasks
- lead capture via AI web scraping and Google Maps
- activity timeline
- email sync and calendar integration
- AI summaries and suggested replies
- approval queue (required for every outbound message)
- lead scoring
- search and filter
- basic reporting
- settings and integrations

Goal: one operator managing outbound prospecting, with AI drafting every message and human approval required on every send.

### Phase 2 — operational CRM + workflow automation

Add:

- automated follow-up sequences (AI sends after approval rules are configured)
- meeting booking automation
- automatic stage updates
- stalled deal detection
- deduplication
- audit trails
- richer reporting
- second email / calendar provider
- **SMS outreach** — AI drafts SMS messages for approval, same pattern as email; integrated via Twilio or similar

### Phase 3 — mature platform

Add:

- account research agents
- forecast support
- broader channel support (beyond email and SMS)
- richer role model
- multi-tenant model if the product evolves that way
- **voice / telephony** — only if there is a clear use case that SMS and email cannot cover

Start with Phase 1. Do not skip ahead.

---

## Core design principle

**The CRM owns the truth. The AI serves the CRM.**

The AI agent:

- reads CRM state
- proposes or performs actions
- logs every action back to the CRM
- escalates uncertainty to you

Think of it as:

> **AI operator + structured workflow engine + minimal CRM visibility layer**

Not "chatbot with a database."
