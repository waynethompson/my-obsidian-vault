# Emplimate Architecture

## Purpose
This document defines the target architecture for Emplimate as a multitenant job board platform with AI-assisted candidate filtering, cross-posting, and future network APIs.

It focuses on:
- multitenant platform structure
- tenant creation and branding
- job distribution and cross-posting
- API extensibility
- Blazor rendering and SEO requirements for public pages

## Product shape
Emplimate starts as a core platform on **emplimate.com** and expands into a network of tenant job boards.

Initial rollout:
- Core platform: **emplimate.com**
- First tenant: **Thai IT jobs market**

Long term, Emplimate should support:
- centrally hosted niche job boards
- self-serve board creation
- recruiter-operated branded boards
- cross-posting between tenants
- inbound job posting from external platforms via API

## Architecture principles
- Multitenant first
- SEO-first for all public discovery pages
- Free job posting, paid filtering tools
- Shared platform, tenant-level branding and configuration
- Canonical job source with controlled syndication
- API-ready data model so federation can be added later
- Public pages optimised for crawlability, speed, and structured data

## Recommended technical direction
### Backend
- C# / .NET
- PostgreSQL
- Background workers for scraping, indexing, matching, notifications, and syndication

### Frontend
- **Blazor Web App**
- Prefer **server-side rendering for public pages**
- Use interactive Blazor only where user interaction is needed

### AI and search
- Search index for jobs, employers, and candidates
- Embeddings or semantic ranking layer for candidate-job matching
- Resume parsing and screening pipeline

### Data ingestion and automation
- Scraping and ETL pipeline for seed jobs, with source tracking and compliance controls
- Background workers for indexing, notifications, job expiry, and syndication updates

## High-level system components
### 1. Tenant management
Responsible for:
- tenant creation
- domain mapping
- branding
- tenant-specific schema configuration
- pricing and feature flags

### 2. Job publishing
Responsible for:
- job creation
- job updates and expiry
- job custom fields
- screening questions
- approval workflow
- canonical job ownership

### 3. Distribution and syndication
Responsible for:
- cross-posting between tenants
- inbound jobs from trusted partners
- source tracking
- deduplication
- application routing

### 4. Candidate and employer management
Responsible for:
- employer accounts
- recruiter accounts
- candidate profiles
- applications
- saved searches
- messaging and workflow history

### 5. AI filtering and ranking
Responsible for:
- resume parsing
- skills inference
- screening answer analysis
- shortlist generation
- explainable fit scoring

### 6. Public content and SEO
Responsible for:
- tenant homepages
- category and location pages
- job detail pages
- metadata
- sitemap generation
- structured data
- canonical URL policy

### 7. External API and integrations
Responsible for:
- future inbound job posting from external systems
- recruiter and ATS integrations
- authentication and rate limiting
- event or webhook support for updates

## Multitenancy model
The preferred model is a single hosted multitenant platform.

Each tenant should have:
- its own domain or subdomain
- branding settings
- supported job categories and taxonomies
- custom job fields
- screening templates
- moderation and publishing rules
- pricing controls for AI tools
- distribution permissions

### Why multitenant first
- simpler to launch
- simpler to manage SEO
- simpler to support shared identity
- simpler to build cross-posting
- simpler to operate than federation

## Self-serve tenant creation
Users should be able to create a tenant from inside Emplimate.

### Tenant creation flow
1. Choose board name
2. Choose region, industry, or niche
3. Upload logo and favicon
4. Pick from approved colour themes
5. Choose Emplimate subdomain or connect custom domain
6. Configure categories, fields, and question templates
7. Configure whether inbound cross-posting is allowed

### Tenant branding constraints
- limited theme palette for consistency
- configurable logo
- configurable favicon
- tenant-specific hero text and SEO copy
- no arbitrary layout editor in early phases

## Domain and routing model
Routing should resolve tenant context from hostname first.

Examples:
- `emplimate.com`
- `thaiitjobs.example`
- `defencejobs.com`

Each request should resolve:
1. tenant
2. tenant configuration
3. public route or authenticated route

## Canonical job model
Every job should have one canonical source record.

Related copies on other tenants should reference:
- source tenant
- source job ID
- syndication status
- destination tenant
- publication timestamps

This avoids duplication and makes cross-posting measurable.

### Core job fields
- title
- employer
- location
- employment type
- salary range
- description
- requirements
- benefits
- application method
- publish status
- expiry date

### Tenant-configurable job fields
- certifications
- licences
- clearance level
- languages
- travel requirements
- roster details
- remote eligibility
- any tenant-defined structured field

## Screening question model
Each job can include tenant-aware custom questions.

Supported question types:
- yes/no
- multiple choice
- short answer
- knockout question

Requirements:
- employers can create questions per job
- employers can save reusable question templates
- answers feed filtering and ranking
- some questions can be marked as mandatory

## Cross-posting and network model
Emplimate.com should act as the central platform and job hub, but jobs can be distributed to other tenants.

### Cross-posting requirements
- per-job target tenant selection
- default posting rules by employer or recruiter
- destination approval workflow
- canonical source tracking
- deduplication
- analytics per tenant and per source

### Application flow options
Support the following modes:
- central apply: all applications go to the source tenant
- local apply: destination tenant owns applications
- synced apply: destination collects and syncs back to source

Recommended early default:
- central apply for simplicity

## External API architecture
Future phases should expose API access so external systems can post jobs into Emplimate.

### API use cases
- ATS pushes jobs into a tenant
- recruiter CRM posts and updates jobs
- partner site submits jobs for moderation
- external site requests syndication into selected tenants

### API capabilities
- create job
- update job
- expire job
- attach screening questions
- request cross-posting targets
- query publication status

### API requirements
- tenant-scoped authentication
- rate limiting
- audit logging
- idempotent writes
- webhook or event support for status changes

## Blazor rendering approach
Blazor should be used in a way that preserves strong SEO and good public performance.

### Recommended rendering model
Use **Blazor Web App** with:
- **server-side rendering by default for public pages**
- interactive components only where needed
- authenticated dashboards using interactive server components

### Page rendering policy
#### Public pages
Use SSR or prerendered output:
- tenant homepages
- category pages
- location pages
- job detail pages
- employer profile pages

These pages must be available as complete HTML on first response.

#### Authenticated application areas
Use interactive Blazor:
- employer dashboard
- recruiter tools
- candidate profile management
- tenant admin
- AI filtering workflows

### What to avoid
- Do not rely on client-only rendering for SEO-critical pages
- Do not require JavaScript for core job content to appear
- Do not hide metadata generation behind client hydration

## SEO requirements
SEO is a core platform requirement because tenant growth depends on discoverable public pages.

### Global requirements
- clean human-readable URLs
- per-tenant metadata
- canonical tags
- sitemap generation per tenant
- robots.txt support per tenant if needed
- Open Graph and social metadata
- schema.org structured data where relevant
- fast first contentful response

### Tenant pages
Examples:
- homepage
- about page
- employer landing pages
- top-level location or niche pages

Requirements:
- SSR HTML output
- unique title and meta description per tenant
- tenant-specific H1 and supporting copy
- tenant branding in metadata
- canonical URL bound to tenant domain
- indexable unless explicitly disabled

### Category pages
Examples:
- `/jobs/software-engineer`
- `/jobs/bangkok`
- `/jobs/software-engineer/bangkok`

Requirements:
- SSR HTML output
- stable canonical URL strategy
- crawlable pagination
- breadcrumb markup
- avoid indexing low-value filter combinations
- allow curated landing pages for important category and location combinations
- include visible job summaries in HTML

Recommended rule:
- index core category and location pages
- noindex thin or over-filtered combinations

### Job detail pages
Examples:
- `/job/senior-dotnet-developer-bangkok-12345`

Requirements:
- SSR HTML output with full job content
- unique canonical URL
- JobPosting structured data
- title, description, employer, location, salary, and date fields present in HTML
- expiry handling for expired jobs
- related jobs section rendered server-side where practical

Recommended rule for expired jobs:
- keep page temporarily with clear expired state and related jobs
- eventually noindex or redirect based on SEO testing

## Sitemap and indexing strategy
Generate per-tenant sitemaps for:
- job pages
- category pages
- location pages
- employer pages

Exclude:
- duplicate syndicated URLs where not canonical
- thin filtered pages
- dashboard pages
- internal search result pages

## Performance requirements
- public pages should render quickly from server-side HTML
- page payloads should stay light on public pages
- tenant branding should not require large client bundles
- caching should be used for public category and job pages where safe

## Data and privacy considerations
- clear ownership of applicant data
- explicit rules for where data is stored when jobs are cross-posted
- tenant-level moderation controls
- consent for candidate data use in AI ranking
- auditable API and syndication actions

## Suggested phase breakdown
### Phase 1
- core platform on emplimate.com
- first Thai IT tenant
- SSR public pages
- free job posting
- tenant-configurable job fields
- custom screening questions
- manual central-to-tenant cross-posting

### Phase 2
- self-serve tenant creation
- shared identity
- self-serve cross-posting
- employer subscriptions
- candidate search
- AI filtering tools

### Phase 3
- public API for external job ingestion
- partner integrations
- federation-friendly APIs
- richer analytics
- network distribution controls

## Open decisions
- exact search technology
- exact embeddings/vector storage choice
- whether public search pages are path-based, query-based, or hybrid
- expired job retention policy
- whether syndicated jobs are indexed on destination tenants or only the source tenant
- when to introduce white-label hosting versus standard tenant plans

## Architecture next steps
### Product and data model
- Define the base job schema and deployment-specific custom field system
- Define the screening question model and employer UX for creating reusable templates
- Define the canonical job model and cross-posting rules between source and destination sites

### Platform and identity
- Confirm the first architecture path: multitenant first, with federation-compatible APIs later
- Define central login and identity requirements for multi-site employers and recruiters
- Define the self-serve tenant creation flow, branding constraints, and custom domain setup process

### API and integrations
- Define future API requirements for external job posting, updates, authentication, and rate limits

### AI and matching
- Prototype the initial matching pipeline using embeddings plus ranking logic

### SEO and rendering
- Finalise the Blazor rendering approach for tenant, category, location, and job detail pages
- Define sitemap, canonical URL, and indexing rules for source versus syndicated jobs
