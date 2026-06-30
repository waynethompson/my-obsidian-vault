# Product Spec

Related notes:

- [[Business Idea/Thailand Social Inbox AI/Thailand Social Inbox AI]]
- [[Business Idea/Thailand Social Inbox AI/Go To Market]]
- [[Business Idea/Infinite Brain/Sales Tool/Product Features and Phases]]

## Purpose

Create an AI product for businesses that mainly interact with customers through Facebook and LINE. The product should automate repetitive conversation work while keeping humans in control of high-risk or high-value cases.

## Core jobs to be done

1. Respond quickly to new inbound messages.
2. Answer repetitive questions using approved business knowledge.
3. Detect intent such as lead, booking, quote request, order status, support, or spam.
4. Capture structured customer details from chat.
5. Move high-value or risky conversations to a staff member.
6. Sync qualified conversations into a CRM or follow-up workflow.
7. Show owners what the inbox is producing.

## MVP scope

### 1. Channel connectors

- Facebook Page inbox
- LINE Official Account

### 2. Unified conversation record

Each chat should map to a structured object with:

- customer name or handle
- channel
- timestamps
- message history
- detected language
- intent
- status
- assigned human owner if escalated

### 3. AI reply engine

The system should:

- reply in Thai or English based on the conversation
- use business-approved playbooks and FAQs
- ask follow-up questions when information is missing
- summarize the conversation for staff on handoff

### 4. Lead and booking capture

For qualified inquiries, the AI should collect:

- service needed
- location or branch
- preferred date or time
- budget or urgency where relevant
- contact method

Then it should either:

- send a booking link
- create a callback task
- create a quote request
- escalate to a human

### 5. Human control layer

Phase 1 should use guardrails instead of full autonomy.

- approved FAQ and qualification flows can auto-send
- pricing exceptions, complaints, refunds, and unusual requests should require human review
- staff should be able to take over and pause automation at any time

### 6. CRM and workflow sync

The product should create or update records in a simple CRM workflow:

- new lead
- qualified lead
- booked
- quote requested
- needs follow-up
- handed to staff
- closed

The first integration target can be the [[Business Idea/Infinite Brain/Sales Tool/Product Features and Phases|Sales Tool]] concept.

## Non-goals for Phase 1

- full omnichannel support across every social platform
- open-ended AI free chat without business guardrails
- deep ERP or POS integrations
- voice calls
- complex team permission systems

## Suggested workflow

1. Customer messages the business on Facebook or LINE.
2. AI identifies intent and language.
3. AI sends an approved first reply immediately.
4. AI answers FAQ or asks qualification questions.
5. If the case is straightforward, AI collects booking or quote details.
6. If the case is sensitive or valuable, AI hands off to staff with a summary.
7. The conversation outcome is written back into CRM status and reporting.

## Data and knowledge inputs

The AI should pull from:

- business profile and opening hours
- branch information
- service catalog
- price guidance or pricing rules
- FAQ answers
- promotion rules
- escalation rules

## Product phases

### Phase 1

- Facebook + LINE connectors
- approved FAQ responses
- lead qualification
- booking or quote capture
- human handoff
- simple dashboard

### Phase 2

- deeper Sales Tool CRM sync
- campaign attribution
- re-engagement of cold conversations
- branch and agent performance analytics
- richer workflow builder

### Phase 3

- payments or deposits
- upsell and remarketing flows
- multilingual knowledge base management
- multi-location rollups for chains and franchises

## Technical shape

The most practical first version is:

- web app for admin and reporting
- connectors for Facebook Graph API and LINE Messaging API
- workflow engine with approval rules
- AI extraction and reply drafting layer
- conversation store plus structured CRM events

This can start as a productized service with reusable internal tooling, then become a cleaner product once the playbooks stabilize.
