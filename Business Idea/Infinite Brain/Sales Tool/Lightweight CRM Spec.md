# Lightweight CRM Spec

Related notes:

- [[Project Plan]]
- [[Product Features and Phases]]
- [[Architecture]]
- [[Spider prompt]]

---

## Product summary

This product is a lightweight CRM built for outbound lead generation and delivered as a SaaS product to multiple organisations. Its core advantage is that it can ingest leads from Google Maps, business websites, and other online sources, then stage, enrich, deduplicate, and import those leads into a simple CRM built for action rather than admin overhead.

The CRM is not meant to compete with large enterprise systems. It is designed to be the fastest way for a small business, agency, recruiter, or sales team to:

- discover local or niche businesses
- extract usable contact data
- review and approve lead imports
- track outreach and next actions
- record manual emails and calls in the same workspace
- publish approved social content from one workspace
- keep a clean pipeline without heavy CRM complexity

---

## Problem

Most CRMs are good at storing records but weak at creating new pipeline. Most scraping tools can collect data but do not turn that data into a usable workflow. The gap is a product that combines:

- lead discovery
- web crawling and extraction
- human review
- lightweight CRM tracking
- AI-assisted enrichment and next-step recommendations

into one focused system.

---

## Primary user

The initial customer is an organisation that wants a steady stream of outbound leads and wants a simple system of record without buying a full enterprise CRM. Within each customer account, the day-to-day user is typically a founder, agency owner, recruiter, sales manager, or individual salesperson.

Phase 1 assumes:

- multi-tenant SaaS deployment
- small teams per organisation
- tenant-scoped data isolation
- tenant-scoped integration settings
- outbound prospecting is the main workflow

---

## Core jobs to be done

1. Search for businesses by type and location.
2. Pull lead candidates from Google Maps and other online sources.
3. Crawl the business website and contact pages.
4. Extract structured contact and company data.
5. Review staged leads before import.
6. Prevent duplicates and bad data from polluting the CRM.
7. Track outreach, follow-up, and lead status in one place.
8. Record manual email and call interactions before deeper communication integrations exist.

---

## Product goals

- Create pipeline directly from external sources, not just manual data entry.
- Keep the CRM simple enough to use daily without training.
- Preserve auditability by storing source URLs and extraction confidence.
- Require human approval before importing uncertain or low-trust data.
- Support future AI-assisted enrichment and outreach without making AI the system of record.
- Allow each organisation to connect and manage its own acquisition integrations, starting with Google Maps / Places credentials.
- Let organisations connect approved social channels such as LinkedIn and Facebook for lightweight publishing workflows.

---

## Non-goals for Phase 1

- enterprise account hierarchies
- complex workflow automation
- complex role hierarchies
- full marketing automation
- telephony or SMS workflows
- scraping Google Maps HTML directly instead of using official APIs where available
- becoming a full social media management suite

---

## Product scope

### 1. Lead source search

The user enters a natural language query such as:

> hair salons in Newtown  
> plumbers in Brisbane CBD  
> physiotherapy clinics in Bangkok

The system converts the query into a structured search request and runs source-specific acquisition steps.

Phase 1 lead sources:

- Google Places / Google Maps API using tenant-provided credentials
- business websites linked from source results

Phase 2 lead sources:

- business directories
- LinkedIn company pages where legally and technically appropriate
- niche industry directories
- imported CSV lists for enrichment
- data broker and enrichment providers that help resolve company identity across sources

### 2. Crawl and scrape layer

The acquisition layer has two distinct responsibilities:

- **source lookup** to discover candidate businesses
- **site crawling** to inspect owned web properties for contact details and business signals

There should also be a third capability:

- **entity matching and enrichment** to resolve whether a Google Maps business, website, broker record, and LinkedIn company page refer to the same company

Preferred approach:

- use the Google Places API for business discovery
- use a spider-based crawler for business websites
- fetch homepage, contact page, about page, and other obvious lead-relevant pages
- extract emails, phone numbers, contact forms, staff names, service descriptions, and location signals

Each tenant should be able to connect its own Google API key so API usage and billing stay aligned with that organisation's account. The product may later support a platform-managed key model, but bring-your-own-key is the cleanest Phase 1 SaaS approach.

The crawler should store:

- source URL
- page URL
- raw text or markdown snapshot
- extraction timestamp
- extraction confidence

### 3. Company matching and enrichment

The CRM should be able to match a company discovered from Google Maps with a likely LinkedIn company profile when sufficient evidence exists.

This should not rely on LinkedIn scraping as the only path. The product should support enrichment from:

- first-party website and domain analysis
- data broker / enrichment tools
- LinkedIn company URLs obtained through permitted APIs, broker datasets, or customer-supplied data

Matching signals can include:

- company name similarity
- normalized website domain
- phone number
- address or suburb / city
- category / industry similarity
- broker-provided company identifiers

The output should be a scored match rather than a silent overwrite.

For each proposed match, the system should store:

- matched platform or provider
- matched external company ID
- matched LinkedIn company URL if available
- confidence score
- reason codes explaining the match
- source provider
- review status

### 4. Staging and review

No discovered lead should go directly into the CRM.

Each lead first lands in a staging area where the system shows:

- business name
- address
- phone
- website
- category
- discovered contact details
- matched existing record if any
- confidence score
- source provenance

The user can then:

- import
- merge with existing record
- dismiss
- mark for manual review

### 5. Lightweight CRM

The CRM is the operational system of record after import.

It should support:

- organisation workspace
- users within an organisation
- account and contact records
- lead status tracking
- notes and activity history
- tasks and next actions
- source attribution
- simple pipeline visibility

### 6. AI assistance

AI is optional assistance layered on top of structured workflow. It can:

- extract structured fields from crawled content
- summarize a company
- estimate ICP fit
- suggest first outreach
- suggest next action

AI must not bypass:

- validation rules
- import review
- CRM state transitions

### 7. Communication tracking

Phase 1 communication management is deliberately lightweight. The CRM should allow users to manually log:

- emails sent
- emails received
- calls made
- calls received
- meeting outcomes
- next follow-up dates

This gives the product immediate usefulness as a system of record without blocking on provider integrations.

Future phases can add connected communication providers so the CRM can move from manual logging to partially or fully integrated activity capture.

### 8. Social publishing

The CRM should support lightweight outbound publishing to tenant-connected social accounts, starting with LinkedIn and potentially Facebook where platform policy and API access allow it.

This is intended for:

- company updates
- thought leadership posts
- lead magnet promotion
- campaign-linked announcements

It is not intended to replace a dedicated social scheduling platform.

Core behavior:

- tenant connects approved social accounts
- user drafts or AI-assists a post
- post is reviewed and approved
- system publishes through the connected platform integration
- resulting post metadata is stored back in the CRM for traceability

Where platform APIs limit direct publishing, the product should degrade gracefully by offering draft generation or copy-ready output instead of pretending to support unsupported actions.

---

## Functional requirements

## Search and acquisition

- Accept natural language search queries.
- Parse business type and location.
- Query Google Places / Maps through official APIs.
- Create acquisition jobs that can run in the background.
- Support retries, rate limiting, and job status visibility.
- Use the tenant's connected Google API credentials for tenant-owned search jobs.
- Allow enrichment jobs to run after discovery so source search and company matching stay separate concerns.

## Crawling and extraction

- Crawl discovered websites using a spider-based crawler.
- Restrict crawl depth and page count per domain.
- Prioritize contact-relevant pages.
- Extract structured contact and company fields from raw content.
- Save provenance so every extracted field can be traced back to a source page.

## Company matching and enrichment

- Match Google Maps businesses to likely LinkedIn company records when supported by enough evidence.
- Support third-party enrichment and data broker providers as match inputs.
- Use a confidence-scored matching model rather than binary assumptions.
- Preserve original source values alongside enriched values.
- Require review for low-confidence or conflicting matches.
- Store provider provenance and explain why a match was proposed.

## Deduplication

- Detect duplicates by email, phone, domain, and business name plus location.
- Flag likely matches instead of silently discarding them.
- Allow the user to merge or keep separate records.

## CRM records

Minimum record types:

- **Organisation / Tenant**
- **User**
- **Company / Account**
- **Contact**
- **Lead / Opportunity**
- **Activity**
- **Task**
- **Acquisition Job**
- **Enrichment Job**
- **Staged Lead**
- **Company Match**
- **Communication Log**
- **Social Connection**
- **Social Post**

## Workflow

- Imported leads start in a defined state such as `New`.
- State transitions are controlled and logged.
- Every meaningful system action creates an activity record.
- Outreach suggestions should be stored separately from confirmed outbound activity.
- Manual communication logs should be first-class activity inputs, not freeform notes only.
- Social drafts should be stored separately from published posts.
- Proposed company matches should be reviewable before becoming the canonical linked identity.

## Review and approval

- The user must approve imports from staged leads.
- The user must be able to see why a field was extracted.
- Low-confidence records should be visually flagged.
- Social posts should support explicit approval before publication.

## Communication tracking

- Support manual logging of outbound and inbound emails.
- Support manual logging of calls and call outcomes.
- Allow users to attach communication logs to a company, contact, or opportunity.
- Capture date, direction, summary, owner, and follow-up action.
- Support future provider-backed sync and sending without replacing the underlying activity model.

## Social publishing

- Support tenant-scoped connection of LinkedIn pages or profiles where permitted.
- Support tenant-scoped connection of Facebook pages where permitted.
- Store connection status, scopes, and token expiry metadata.
- Allow users to draft, schedule, approve, publish, and retry social posts.
- Persist platform response data such as published post ID, URL, status, and failure reason.
- Link social posts to campaigns, companies, or lead segments where relevant.
- Respect platform-specific rate limits, permissions, and policy constraints.

## SaaS platform requirements

- Every record must belong to a tenant.
- Authentication must support organisation membership.
- Secrets such as API keys must be stored per tenant and encrypted at rest.
- Background jobs must execute in a tenant-aware context.
- Usage events should be tracked for future billing, quotas, or fair-use controls.
- Admin tooling should support tenant support actions without exposing tenant data to other tenants.

---

## CRM data model

### Organisation / Tenant

- organisation name
- plan
- billing status
- created at
- active integrations
- usage counters

### User

- full name
- email
- role
- tenant
- status

### Communication Log

- tenant
- type
- direction
- channel
- subject or summary
- body snippet or notes
- occurred at
- linked company
- linked contact
- linked opportunity
- owner
- follow-up due at
- source type

### Company Match

- tenant
- source company record
- target platform or provider
- matched external company ID
- matched company name
- matched company URL
- confidence score
- reason codes
- source provider
- review status

### Enrichment Job

- tenant
- provider
- input company or domain
- job type
- status
- started at
- completed at
- records returned
- cost or usage units
- errors

### Social Connection

- tenant
- platform
- connected account name
- external account ID
- status
- scopes
- token expiry

### Social Post

- tenant
- platform
- target account
- content
- media references
- status
- scheduled at
- published at
- external post ID
- external post URL
- approval status
- linked campaign or segment

### Company / Account

- name
- website
- domain
- business category
- address
- suburb / city / region
- phone
- industry
- employee size estimate
- ICP fit score
- notes

### Contact

- full name
- role / title
- email
- phone
- LinkedIn URL
- associated company
- source confidence
- source page URL

### Lead / Opportunity

- current stage
- qualification score
- last activity
- next action
- owner
- priority
- outreach status
- risk flags

### Activity

- type
- timestamp
- actor
- summary
- linked record
- source reference if system-generated

### Task

- title
- due date
- linked record
- status
- created by AI or human

### Staged Lead

- source platform
- source query
- business metadata
- extracted contacts
- duplicate match status
- confidence score
- import decision

### Acquisition Job

- job type
- query
- source
- tenant
- status
- started at
- completed at
- pages crawled
- leads found
- errors

---

## Lead lifecycle

Suggested lead states:

- New
- Enriched
- Reviewed
- Qualified
- Contacted
- Replied
- Meeting Booked
- Opportunity Open
- Nurture
- Closed Won
- Closed Lost

---

## Integration design

### Google Maps / Places

Use official Google Places endpoints for business discovery rather than scraping Google Maps HTML directly. This reduces fragility and makes limits, pricing, and structured responses explicit.

For SaaS delivery, the preferred model is:

- each tenant enters its own Google API credentials
- the platform validates those credentials before allowing search jobs
- requests are executed in the tenant context
- quotas, failures, and usage are shown back to that tenant

This reduces platform billing exposure and gives each customer control over its own API limits.

### LinkedIn and Facebook

Social publishing should sit behind a provider abstraction similar to email/calendar integrations.

Principles:

- connect accounts per tenant
- store provider-specific tokens securely
- map platform-specific behavior into a common publishing workflow
- expose capability flags such as `can_publish`, `can_schedule`, and `supports_media`

The product should assume uneven platform support. LinkedIn and Facebook capabilities may differ by app approval, account type, and API policy, so the UX should show what is actually available for that tenant instead of assuming parity across platforms.

### Data broker and enrichment providers

The CRM should support broker-style providers that help map companies across Google Maps, websites, LinkedIn, and other business datasets.

These providers should be treated as enrichment inputs, not unquestionable truth. The system should:

- normalize provider results into a common company match model
- track provider provenance
- support multiple providers over time
- allow the tenant to choose which providers they connect
- surface confidence and conflicts to the user

### Spider-based crawler

Use a dedicated spider / crawler component for business websites. It should:

- obey configurable limits
- use a consistent user agent
- capture page text in a format suitable for extraction
- expose crawl events or page outputs back to the application layer

The crawler is best treated as an infrastructure component, not as CRM logic. The CRM consumes staged results after extraction and validation.

### Other online sources

Future sources should plug into the same staging pipeline:

- source connector
- normalization
- extraction
- deduplication
- review
- import

This keeps the CRM model stable even as acquisition channels expand.

---

## User experience

### Main screens

1. Organisation onboarding and integration settings
2. Search / acquisition screen
3. Staged leads review queue
4. Company and contact detail pages
5. Lead pipeline view
6. Task / next actions view
7. Acquisition job history
8. Social drafts and publishing queue

### UX principles

- fast import workflow
- minimal data entry
- high visibility into source and confidence
- no hidden automation
- easy cleanup of bad or duplicate leads

---

## Compliance and trust

- Respect robots.txt and legal constraints where applicable to owned sites and public sources.
- Avoid pretending scraped data is verified fact.
- Store provenance for every important extracted field.
- Surface failures clearly instead of silently skipping them.
- Make opt-out / do-not-contact status explicit in the CRM model.

---

## Phase plan

### Phase 1

- multi-tenant SaaS deployment
- organisation onboarding
- tenant settings and API credential management
- Google Places search with tenant-provided credentials
- website crawl of high-value pages
- staged lead review queue
- manual import into lightweight CRM
- basic deduplication
- basic company matching using domain, name, phone, and location
- notes, tasks, and lead stages
- manual logging of emails and calls
- LinkedIn publishing if API approval and account support are available

### Phase 2

- scheduled recurring searches
- more source connectors
- stronger enrichment
- data broker-backed LinkedIn company matching
- outreach drafting
- connected email sending and activity syncing
- Twilio-based call or SMS integration
- smarter merge suggestions
- basic billing and plan enforcement
- Facebook page publishing
- scheduled and campaign-linked social posting

### Communication integration roadmap

- **Phase 1:** manual logging only for email and call activity
- **Phase 2:** connected email delivery and sync via providers such as SendGrid where appropriate
- **Phase 2:** Twilio integration for calling and SMS-related workflow where commercially justified
- **Phase 3:** deeper inbox, conversation timeline, and automated workflow support

### Phase 3

- deeper workflow automation
- broader data-source marketplace
- richer reporting
- more advanced role controls and workspace administration
- cross-channel reporting across CRM and social activity

---

## Success criteria

The product is successful if a user can:

1. search for a niche and location
2. review a clean list of candidate businesses
3. match those businesses to the right company identities, including LinkedIn where possible
4. import the best leads into the CRM with confidence
5. track follow-up without needing another system

The defining feature is not just storing leads. It is turning external web data into trusted, actionable CRM records with very little friction.
