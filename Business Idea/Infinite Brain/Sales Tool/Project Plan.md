# Sales Tool Project Plan

Related notes:

- [[Lightweight CRM Spec]]
- [[Product Features and Phases]]
- [[Architecture]]
- [[Brand Naming]]

---

## Goal

Launch a usable multi-tenant SaaS sales tool that helps organisations discover leads from Google Maps and other online sources, enrich and review those leads, import them into a lightweight CRM, and manage outreach activity from one workspace.

## Planning assumptions

- Phase 1 is focused on a usable beta, not a fully mature CRM
- Google Maps / Places uses tenant-provided API credentials
- website crawling is used for enrichment, not as the primary source of truth
- company matching should support broker-backed enrichment and LinkedIn resolution
- emails and calls are logged manually first; native provider integrations come later
- social publishing is lightweight and approval-based

## Success criteria

- a tenant can onboard and configure its workspace
- a user can search for businesses and stage leads for review
- the system can enrich leads from websites and propose company matches
- approved leads can be imported into the CRM without polluting data quality
- the CRM supports tasks, notes, manual communication logs, and pipeline tracking
- the first beta users can run real prospecting workflows end to end

---

## Phases

### Phase 0: Product framing and delivery plan

**Goal:** lock the first beta scope and remove ambiguity before build work starts.

**Current recommendation**

Build the first beta for small organisations that do outbound prospecting and need a simple system to discover, enrich, review, and manage leads. The first beta should prove one core loop:

1. search for businesses
2. enrich and match results
3. review staged leads
4. import approved records into the CRM
5. track follow-up manually inside the same workspace

**Phase 1 beta scope freeze**

**In scope**
- multi-tenant SaaS foundation
- organisation onboarding and workspace setup
- tenant-provided Google Maps / Places credentials
- natural-language business search
- website crawling for enrichment
- staged lead review queue
- duplicate detection and company matching
- broker-backed enrichment hooks
- lightweight CRM records for company, contact, lead, activity, and task
- manual email and call logging
- LinkedIn posting only if tenant permissions and platform support allow it

**Out of scope**
- full marketing automation
- deep role hierarchy or enterprise permissions
- native mailbox sync in Phase 1
- Twilio or SMS workflows in Phase 1
- Facebook publishing in Phase 1
- fully autonomous AI actions without review
- broad workflow automation beyond the core lead-to-CRM loop

**Primary customer profile**

Recommended first customer profile:

- small agencies
- recruiters
- outbound-focused service businesses
- small sales teams that prospect local or niche businesses

These customers are a better first fit than large enterprises because they feel lead-generation pain immediately and can tolerate a narrower beta product if it saves time.

**First launch use cases**

- find local businesses by niche and area
- enrich business and contact information from websites
- match a discovered company to a likely LinkedIn company
- review candidate leads before import
- track tasks, notes, and manual communications after import

**Core entities confirmed**

- tenant
- user
- company
- contact
- lead
- staged lead
- acquisition job
- enrichment job
- company match
- activity
- task
- communication log
- social connection
- social post

**Lead lifecycle and review workflow**

Recommended lead lifecycle for Phase 1:

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

Recommended review workflow:

1. acquisition creates staged leads
2. crawl/enrichment adds contact and company data
3. deduplication and company matching propose conflicts or likely matches
4. user chooses import, merge, dismiss, or manual review
5. approved leads become CRM records with source provenance

**Phase 1 success metrics**

- a new tenant can onboard and configure Google credentials successfully
- a user can run a search and receive reviewable staged leads
- enrichment produces usable website/contact data for a meaningful share of results
- duplicate and company-match suggestions reduce bad imports instead of hiding data
- approved leads can be imported into the CRM with source attribution intact
- a user can manage follow-up using tasks, notes, and manual communication logs

**Integrations: now vs later**

**Required now**
- Google Maps / Places API
- website crawler / spider
- tenant secret storage

**Useful now if feasible**
- one broker/enrichment provider for company identity resolution
- LinkedIn publishing where platform approval and permissions allow it

**Later**
- SendGrid or similar email delivery
- mailbox sync
- Twilio for calls/SMS
- Facebook publishing
- broader third-party source connectors

**Open questions still worth resolving**

- which customer segment should be the first design target: agencies, recruiters, or SMB sales teams?
- should LinkedIn posting be in the first beta or held until after the core CRM loop is proven?
- which enrichment provider should be researched first for company identity matching?
- what minimum role model is enough for beta: owner/admin/member, or owner/member only?

**Tasks**
- [x] confirm Phase 1 beta scope and explicitly freeze out-of-scope items ✅ 2026-04-29
- [x] confirm primary customer profile and first launch use cases ✅ 2026-04-29
- [x] confirm core entities: tenant, user, company, contact, lead, staged lead, activity, task ✅ 2026-04-29
- [x] confirm lead lifecycle and review workflow ✅ 2026-04-29
- [x] confirm Phase 1 success metrics ✅ 2026-04-29
- [x] confirm which integrations are required now vs later ✅ 2026-04-29

**Exit criteria**
- [x] one stable Phase 1 scope exists ✅ 2026-04-29
- [x] open product questions are reduced to a short list ✅ 2026-04-29

### Phase 1: UX, workflow, and information architecture

**Goal:** define how users move through the system before implementation.

**Tasks**
- [ ] design organisation onboarding flow
- [ ] design integration settings flow for Google API keys
- [ ] design search and acquisition flow
- [ ] design staged lead review queue
- [ ] design company/contact/lead detail pages
- [ ] design activity timeline and task workflow
- [ ] design manual communication logging flow
- [ ] design social draft and approval flow
- [ ] design company match review flow

**Outputs**
- [ ] screen list
- [ ] user flow map
- [ ] wireframes or rough mockups

### Phase 2: Core platform foundation

**Goal:** establish the SaaS and data foundation for the product.

**Tasks**
- [ ] set up multitenant architecture
- [ ] implement tenant-aware authentication and organisation membership
- [ ] define role model for owner/admin/member
- [ ] implement tenant-scoped data access rules
- [ ] implement tenant settings storage
- [ ] implement encrypted secret storage for API keys and provider tokens
- [ ] define audit/event logging model
- [ ] define usage tracking model for future billing and quotas

**Exit criteria**
- [ ] tenant isolation is clear and enforceable
- [ ] integrations can be stored per tenant safely

### Phase 3: Lead acquisition and crawler pipeline

**Goal:** reliably discover businesses and capture enrichment data.

**Tasks**
- [ ] implement natural-language business search parsing
- [ ] connect Google Places / Maps search using tenant-provided credentials
- [ ] store acquisition jobs and job status
- [ ] implement website crawling for high-value pages
- [ ] extract emails, phones, names, and service signals from crawled pages
- [ ] store provenance for every extracted field
- [ ] surface crawl failures and partial results clearly
- [ ] add rate limiting and retry strategy

**Outputs**
- [ ] staged lead candidates created from acquisition jobs
- [ ] website enrichment data attached to results

### Phase 4: Matching, deduplication, and enrichment

**Goal:** prevent messy CRM data and improve company identity resolution.

**Tasks**
- [ ] implement duplicate detection using email, domain, phone, and company/location signals
- [ ] implement company matching model with confidence scoring
- [ ] support matching Google Maps companies to likely LinkedIn companies
- [ ] support broker/enrichment provider connectors
- [ ] store company match reason codes and provenance
- [ ] create review workflow for low-confidence or conflicting matches
- [ ] define canonical company identity rules

**Exit criteria**
- [ ] duplicate handling is visible and reviewable
- [ ] LinkedIn/company matching works as a proposed match, not a silent overwrite

### Phase 5: Staging, import, and lightweight CRM

**Goal:** turn acquired leads into usable CRM records.

**Tasks**
- [ ] build staged lead review queue
- [ ] allow import, merge, dismiss, and manual review actions
- [ ] create company, contact, and lead record creation flow
- [ ] implement lead stages and controlled state transitions
- [ ] implement notes, activities, and tasks
- [ ] implement source attribution on imported records
- [ ] implement simple pipeline and list views
- [ ] implement search/filtering across CRM data

**Outputs**
- [ ] reviewable staged lead workflow
- [ ] usable CRM for daily prospecting work

### Phase 6: Communication and social workflows

**Goal:** support outreach operations without overbuilding the platform.

**Tasks**
- [ ] implement manual logging for emails sent and received
- [ ] implement manual logging for calls made and received
- [ ] implement follow-up date and next action tracking
- [ ] implement activity timeline views
- [ ] implement LinkedIn social connection flow where supported
- [ ] implement social draft, approval, and publish workflow
- [ ] store published post metadata back in the CRM
- [ ] design future provider abstraction for email, telephony, and SMS

**Later-phase tasks**
- [ ] evaluate SendGrid for outbound email delivery
- [ ] evaluate mailbox sync approach separately from delivery provider
- [ ] evaluate Twilio for call and SMS workflows
- [ ] evaluate Facebook page publishing support

### Phase 7: AI assistance layer

**Goal:** add AI where it improves speed and quality without taking control away from the user.

**Tasks**
- [ ] implement extraction prompts for website and contact parsing
- [ ] implement company summarisation and ICP fit suggestions
- [ ] implement next-step recommendations
- [ ] implement outreach draft suggestions
- [ ] implement social draft suggestions
- [ ] ensure every AI action is logged with input, output, and confidence
- [ ] require approval for risky or user-visible actions
- [ ] define clear boundaries for what AI can and cannot mutate

**Exit criteria**
- [ ] AI assists workflow without becoming the source of truth

### Phase 8: Beta readiness and launch

**Goal:** make the product usable for real pilot customers.

**Tasks**
- [ ] create seed/demo tenant data for internal testing
- [ ] test end-to-end onboarding, search, review, import, and follow-up flows
- [ ] test tenant-scoped integration setup and failure handling
- [ ] write onboarding guidance for first beta users
- [ ] define support workflow for tenant issues
- [ ] define pricing and beta access rules
- [ ] prepare landing page and positioning copy
- [ ] onboard first beta organisations

**Outputs**
- [ ] beta-ready product
- [ ] first pilot users

---

## Milestones

### Milestone 1: Scope locked
- [ ] scope confirmed
- [ ] workflows confirmed
- [ ] first-beta boundaries documented

### Milestone 2: SaaS foundation ready
- [ ] tenants work
- [ ] users work
- [ ] settings and secrets work

### Milestone 3: Acquisition pipeline working
- [ ] Google Maps search works
- [ ] crawl enrichment works
- [ ] staged leads appear

### Milestone 4: CRM workflow usable
- [ ] staged review works
- [ ] import works
- [ ] CRM records and tasks work

### Milestone 5: Enrichment and publishing added
- [ ] LinkedIn/company matching works
- [ ] manual communications are tracked
- [ ] social posting workflow works

### Milestone 6: Beta live
- [ ] first organisations onboarded
- [ ] first real prospecting workflows completed

---

## Key risks

| Risk | Impact | Mitigation |
|---|---|---|
| Scope grows too quickly | Delayed beta | Keep Phase 1 tightly focused on core acquisition + CRM workflow |
| Source quality is inconsistent | Messy data | Use staging, confidence scoring, and review-first import |
| LinkedIn matching is unreliable | Bad enrichment | Treat matching as proposed identity resolution with provenance |
| API/provider policy constraints block features | Roadmap slips | Use provider abstractions and degrade gracefully where needed |
| Multitenancy adds operational complexity | Slower delivery | Keep early roles and billing simple |

## Immediate next actions

1. [ ] Confirm the Phase 1 beta boundary from this plan.
2. [ ] Create rough wireframes for onboarding, search, staged review, and CRM detail pages.
3. [ ] Break Phase 2 through Phase 5 into implementation tickets.
4. [ ] Choose the first broker/enrichment providers to research.
5. [ ] Define the first beta tenant profile and onboarding path.
