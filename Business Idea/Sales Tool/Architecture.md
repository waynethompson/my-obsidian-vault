# AI Sales Tool — Architecture

---

## Deployment

### Decisions

- **v1 is single-user, single-tenant**
- **Deployed as a local web app, opened in the browser**
- **Packaged as Docker Compose on one machine**
- **No cloud-only dependencies** — the product must be fully runnable as a self-hosted install

### Runtime components

- React frontend (served locally)
- ASP.NET Core API
- PostgreSQL database
- Background job worker (same process or separate container)

### Future SaaS path

The architecture is self-hostable by design. If the product later becomes a multi-tenant SaaS, add:

- tenant model with tenant-scoped data access
- billing
- per-tenant secrets and integration management
- richer role and permission model

---

## Tech stack

| Layer            | Choice                                                            |
| ---------------- | ----------------------------------------------------------------- |
| Frontend         | React + Vite + TypeScript                                         |
| Backend          | ASP.NET Core (modular monolith)                                   |
| ORM              | EF Core                                                           |
| Database         | PostgreSQL                                                        |
| Auth             | ASP.NET Core Identity                                             |
| Background jobs  | C# hosted services / background workers                           |
| AI orchestration | C# application code with structured tool calling                  |
| Email / calendar | One provider first (Gmail or Outlook) behind a thin abstraction   |
| Telephony        | Not in scope                                                      |
| SMS              | Phase 2 — Twilio (or compatible provider) behind a thin abstraction |
| Dev orchestration | .NET Aspire AppHost                                               |
| Logging          | Serilog → OpenTelemetry → Seq (production)                        |
| Caching          | Postgres first; Redis added only if a concrete bottleneck appears |
| Orchestration    | Temporal added only if workflow complexity clearly demands it     |
| Vector search    | pgvector added only if semantic search proves necessary           |

### Backend

C# / ASP.NET Core is the primary backend language. It handles all business logic, AI orchestration, CRM data access, background jobs, and email/calendar integration.

### Frontend

React + Vite serves the local browser UI. The frontend is kept thin — it renders data and routes user actions. All business logic, workflow orchestration, and integrations live in the backend.

### Email / calendar

Phase 1 supports one provider (Gmail or Outlook). The integration is built behind a thin `IEmailProvider` / `ICalendarProvider` abstraction so the second provider can be added in Phase 2 without rewriting core logic. The provider choice is deferred until implementation begins.

---

## .NET Aspire

### What it does

.NET Aspire is the development orchestrator and wiring layer for the project. It is not a production runtime — it generates the production deployment artefacts.

The solution includes an **AppHost** project that describes all services and their relationships:

- ASP.NET Core API
- Background job worker
- PostgreSQL (via Npgsql Aspire integration)
- Seq (for production log viewing)
- React frontend (served via a proxy or static file server)

During development, running the AppHost starts all services together, wires up connection strings automatically, and opens the Aspire Dashboard.

### Aspire Dashboard

The built-in Aspire Dashboard provides logs, distributed traces, and metrics during development. It is the primary observability tool during development and replaces the need for Seq in that environment.

In production, Seq receives the OpenTelemetry output as a persistent searchable log store.

### Docker Compose generation

The AppHost definition is used to generate the Docker Compose file for production deployment. This keeps the dev and production service definitions in sync and avoids maintaining a separate `docker-compose.yml` by hand.

Use `aspirate` (or the Aspire manifest export) to produce the production Docker Compose from the AppHost.

### OpenTelemetry

Aspire pre-configures OpenTelemetry for all .NET services in the solution — traces, metrics, and logs are exported automatically. No manual OTEL wiring is needed beyond adding the Aspire service defaults NuGet package.

---

## Lead scraper — implementation

### Query parsing

The user's natural language query ("hair salons in Newtown") is parsed by the LLM into a structured search request:

```json
{ "business_type": "hair salon", "location": "Newtown, NSW" }
```

### Google Places API

The structured request is sent to the **Google Places API** (Text Search endpoint). This returns:

- business name
- address
- phone number
- website URL
- Google category
- rating / review count

This is the official API and should be used in preference to scraping Google Maps HTML directly.

### Website scraping

For each result with a website, the scraper:

1. Fetches the homepage and `/contact` page
2. Extracts email addresses using regex and DOM parsing
3. Passes the page text to the LLM to extract contact names, roles, and any other structured data
4. Stores raw source URLs alongside extracted fields for auditability

### Deduplication

Before showing the review screen, each result is checked against existing CRM records by:

- email address (exact match)
- website domain (normalised)
- business name + suburb (fuzzy match)

Matches are flagged rather than silently dropped so you can decide.

### C# implementation notes

- `HttpClient` for website fetching (with a configurable timeout and user-agent)
- Google Places API client (Google.Maps.Places NuGet or plain HTTP)
- HTML parsing via **AngleSharp**
- LLM extraction call using the same tool-calling pattern as the rest of the application
- All scrape jobs run as background workers; results are written to a staging table pending your review
- Failed scrapes (timeout, bot block, no content) are recorded and surfaced in the UI

---

## AI implementation

### Tool calling pattern

The LLM has a defined set of tools it can call:

- `send_email` — drafts only; requires approval before sending
- `update_lead_stage`
- `create_task`
- `schedule_follow_up`
- `summarize_thread`
- `enrich_company`

The backend validates every tool call before executing it. The AI never writes directly to the CRM without going through the application layer.

### LLM orchestration

Written in C# application code. No third-party orchestration framework is required in Phase 1.

### RAG / knowledge base

Not included in Phase 1. Add only if access to playbooks or documents is clearly needed.

---

## State machine

Each lead and deal is modelled as a state machine:

- a current **state**
- allowed **transitions**
- an append-only **event log**

The AI recommends transitions. The backend validates them. No freeform state mutation is permitted.

This prevents CRM drift and makes every change explainable.

Lead states: New → Enriched → Qualified → Contacted → Replied → Meeting Booked → Opportunity Open → Nurture → Closed Won / Closed Lost

---

## Observability

### Application logging and tracing

All instrumentation uses **OpenTelemetry** so the backend is never locked to a specific logging tool.

| Tool | Role |
|---|---|
| **Serilog** | Structured logging library inside the C# app |
| **OpenTelemetry SDK** | Traces, metrics, and log export from ASP.NET Core |
| **Seq** | Self-hosted log and trace viewer — one Docker container, free for single user |

Seq runs as part of the Docker Compose stack and receives all structured logs and traces from the application. It provides full-text search over structured log fields, trace timelines, and alerting.

If the product later grows into multiple services, the OTEL exporter target can be switched to Uptrace, Grafana (Loki + Tempo), or any other OTEL-compatible backend without changing the application instrumentation.

### AI action event log

Every AI action is also stored as a first-class event record in Postgres containing:

- input context
- output
- confidence score
- tool called
- whether you approved, edited, or overrode it
- timestamp

This is separate from application logs — it is the foundation of the timeline and explainability features in the CRM UI, and it must remain queryable from within the product itself.

---

## Memory model

### Structured memory — stored in Postgres

- last reply date
- relationship summary
- objections heard
- next step
- preferred tone
- buying signals
- risk flags
- do-not-contact flags

### Unstructured memory — raw artifacts

- raw email threads
- notes

The LLM reads structured memory first and pulls raw artifacts only when needed. Avoid accumulating fuzzy unstructured context — it degrades reliability over time.

---

## Build strategy

- Build the pipeline, timeline, tasks, and approval queue first
- Every AI action must be visible, reversible, and auditable from day one
- Delay Temporal, Redis, pgvector, and multi-tenancy until there is a clear and specific reason to add them
- The goal for Phase 1 is a working outbound prospecting workflow, not a complete platform
