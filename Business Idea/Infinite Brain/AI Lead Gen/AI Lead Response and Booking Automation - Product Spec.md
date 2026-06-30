# AI Lead Response & Booking Automation — Product Spec (Initial MVP)

## Purpose
Deliver a fixed-scope productized service that immediately responds to inbound inquiries, qualifies leads, and books appointments — turning missed or slow responses into booked revenue for high-value service businesses.

Primary outcome: increase booked appointments and measurable incremental revenue within 30 days of deployment.

---

## Users
- Primary (buyer): Clinic owners, practice managers, real-estate agents — non-technical decision makers.
- Primary end-users: Front-desk staff and sales agents who will receive qualified lead notifications and manage bookings.
- Admin user: Service operator (you) for setup, monitoring, and support.

---

## MVP scope (what this productized service includes)
Core features:
1. Prospect intake & channel connectors
   - Ingest leads from: website contact form (via webhook), Facebook messages/Inbox, and WhatsApp/LINE (via Twilio or LINE gateway).
   - Normalize lead data into a canonical lead object (name, contact, message, channel, timestamp, source URL).

2. Automated first response
   - Send an immediate templated reply per channel (customizable per client) within <1 minute of receipt.
   - Include friendly acknowledgement and short qualification intro.

3. Automated qualification flow
   - Three-question qualification sequence (configurable questions): e.g., service needed, preferred timeframe, budget or basic eligibility.
   - Use simple rules to mark lead as "qualified", "unqualified", or "needs human".
   - Allow fallback to human handoff if NLP confidence is low.

4. Booking integration
   - For qualified leads, present a booking link (Calendly or client booking URL) or create a provisional booking request sent to staff.
   - Option to auto-create calendar events in Google Calendar or send booking requests to staff email.

5. Notifications & dashboard
   - Notify staff via SMS/email/Slack of qualified leads with summary and link to booking or CRM entry.
   - Lightweight admin view (web page) showing recent leads, status, and basic metrics (response time, qualified rate, bookings).

6. Reporting & baseline comparison
   - Capture baseline metrics on day 0 (manually or via short test) and report changes in bookings and conversion after 7/30 days.

Non-MVP features (explicitly excluded initially):
- Full CRM sync (deep integrations) beyond simple calendar/event creation or webhooks
- Advanced NLP or sentiment analysis beyond basic rule/NLP pass-through
- Complex analytics or dashboards beyond simple counts and response-time metrics

---

## Technical architecture (high-level)
- Ingress: channel connectors (webhooks for site forms, Facebook API, Twilio for SMS/WhatsApp, LINE gateway adapter).
- Orchestration: Make.com / n8n for flow design; lightweight webhook microservice (C#/Rust) handles qualification logic, logging, and enrichment.
- Data store: small persistent store (Airtable / Google Sheets / simple DB) to log leads and statuses for reporting.
- Outbound: Twilio/email/LINE for replies; Calendly or direct booking link for scheduling.
- Admin UI: simple static web UI (Netlify/GitHub Pages) backed by Airtable/Sheets API for viewing recent leads and statuses.

Security & privacy:
- Store only business and lead contact details; do not store sensitive PII beyond what the lead provides in inquiry.
- Provide data-retention policy and deletion instructions to clients.
- Use HTTPS, secure API keys, and per-client configuration secrets.

---

## Integration points (MVP)
- Google Places API (Phase 0 prospecting only)
- Facebook Graph API for page inbox
- Twilio for SMS/WhatsApp
- LINE gateway (Thailand-specific) — documented but integrated per-client if used
- Calendly or client booking URL
- Google Calendar (optional)

---

## User flows
1. Inbound flow:
   - Lead submits via site form / FB / WhatsApp
   - Connector posts to orchestration layer
   - Orchestration triggers immediate templated reply
   - Qualification questions sent sequentially; answers stored
   - If qualified, booking link sent and staff notified
   - Lead status updated in the admin view

2. Staff flow:
   - Staff receives notification (SMS/email) with lead summary and booking link
   - Staff can confirm, reschedule, or mark as follow-up in the admin view

---

## Admin / Setup flow (operator)
- Kickoff: collect channels, booking links, business hours, and target qualification questions
- Configure templates and qualification rules in a simple admin form
- Run a short test lead to validate end-to-end flow
- Go live and monitor metrics for first 7 days

---

## Acceptance criteria (for a successful MVP deployment)
- Automated reply successfully sent for >95% of test inbound messages within 60 seconds
- Qualification flow completes end-to-end for test leads and correctly flags "qualified" or "needs human" in >90% of test cases
- Booking link is sent to qualified leads and at least one test booking can be completed end-to-end
- Admin can view lead stream and export CSV of leads within 1 click

---

## Metrics to capture
- Response time to first reply (median)
- % of leads that complete qualification
- % of qualified leads that receive a booking link
- Appointments booked attributable to automation
- Incremental revenue calculation (client-provided avg sale × incremental bookings)

---

## MVP timeline (recommended)
- Phase 0 (prototype & validation): 7–10 days
- Phase 1 (niche & offer selection + assets): 3–5 days
- Phase 2 (build MVP flows & integrations): 5–7 days
- Phase 3 (pilot with first client & iterate): 7–14 days

---

## Deliverables for first paid engagement
- Connected channels and working automation flows
- Admin access and short training doc (1–2 pages)
- Baseline and 30-day performance report
- Case study template for client testimonial

---

## Next steps
1. Approve this product spec or request edits.
2. Start Phase 0 immediately for chosen niche (recommended: aesthetic clinics, Bangkok).
3. Produce Phase 0 deliverables and use results to finalize MVP details and pricing.

## Built-in self-sell features (design requirements)

Design the MVP so it directly supports sales activities.

**Must-have in MVP**

- Missed-lead audit generator: exportable one-page audit PDFs showing missed leads and conservative ROI estimates for prospects.
- Demo/pilot mode: quick install template that runs for 3–5 days and collects uplift metrics for use in pitches.
- One-click ROI snapshot: export a short metrics summary (response time, qualified %, bookings attributable) for sales collateral.

**Good post-pilot additions**

- Testimonial & referral automation: capture client feedback post-booking and trigger templated referral requests.
- Reusable outreach templates: store audit report and metric snippets to personalize follow-ups at scale.

These features make the product a direct contributor to pipeline creation and shortening sales cycles.
