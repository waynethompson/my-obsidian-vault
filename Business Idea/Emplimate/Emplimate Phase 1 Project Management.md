# Emplimate Phase 1 Project Management

## Phase 1 goal
Launch the first usable version of Emplimate on **emplimate.com** and prove the multitenant model with the first tenant focused on the **Thai IT jobs market**.

Phase 1 should validate:
- employers will post jobs for free
- candidates will apply through the platform
- a tenant can run with its own branding and domain configuration
- jobs can be cross-posted from the core platform to the first tenant
- public pages can rank and be indexed with the Blazor SEO approach

## Phase 1 scope
### In scope
- Core platform on emplimate.com
- First Thai IT jobs tenant
- Free job posting
- Candidate applications
- Tenant-aware branding and configuration
- Deployment-specific job fields
- Per-job custom screening questions
- Manual or admin-driven cross-posting from emplimate.com to the first tenant
- Seeded jobs via compliant scraping and manual curation
- SEO-friendly tenant, category, and job detail pages

### Out of scope
- Full AI candidate matching and ranking
- Self-serve tenant creation
- Public API for external job ingestion
- Subscription billing automation
- Partner federation or open-source hosting
- Advanced recruiter workflow automation

## Success criteria
- Core platform is live on emplimate.com
- Thai IT tenant is live with distinct branding
- Employers can post jobs without payment friction
- Candidates can apply to jobs end to end
- Cross-posted jobs display correctly on both source and tenant sites
- Public pages are crawlable and have tenant-aware metadata
- Seed inventory exists for launch content

## Deliverables
1. Core multitenant application shell
2. Tenant resolution by domain/subdomain
3. Employer job posting flow
4. Candidate application flow
5. Configurable job field system
6. Screening question support
7. Admin cross-posting workflow
8. Public SEO page templates
9. Seed job ingestion workflow
10. Launch configuration for emplimate.com and Thai IT tenant

## Workstreams
### 1. Product and UX
- Define employer posting workflow
- Define candidate apply workflow
- Define job field configuration UX
- Define screening question UX

### 2. Platform architecture
- Set up multitenant foundation
- Define tenant configuration model
- Define canonical job and syndication model
- Define application routing rules

### 3. Public website and SEO
- Build tenant homepage template
- Build category and location pages
- Build job detail pages
- Implement metadata, sitemap, and structured job data

### 4. Backoffice and admin tools
- Tenant administration
- Job moderation where needed
- Cross-posting controls
- Basic reporting for source and destination jobs

### 5. Data seeding
- Identify seed sources for Thai IT jobs
- Define scraping and import workflow
- Curate launch inventory
- Track source provenance

### 6. Go-to-market
- Prepare launch copy for emplimate.com
- Prepare Thai IT tenant copy and landing pages
- Identify initial employers or recruiters
- Prepare manual onboarding support

## Milestones
### Milestone 1: Foundation
- Confirm scope
- Confirm tenant model
- Confirm page types and routing
- Confirm job schema and screening model

### Milestone 2: Platform skeleton
- Multitenant app running
- Tenant-aware theming working
- Core public routes working
- Basic admin access working

### Milestone 3: Posting and applications
- Employers can create jobs
- Candidates can submit applications
- Screening questions captured
- Jobs render publicly

### Milestone 4: SEO and seeded launch content
- Tenant, category, and job pages render correctly
- Metadata and sitemap generation working
- Thai IT seed inventory loaded
- Initial landing pages prepared

### Milestone 5: Cross-posting validation
- Jobs can be posted on emplimate.com
- Selected jobs can be cross-posted to the Thai IT tenant
- Source tracking and canonical handling confirmed

### Milestone 6: Launch readiness
- Core smoke flows work
- Launch content is ready
- Initial outreach list exists
- Platform ready for pilot usage

## Key decisions for Phase 1
- Multitenant hosted model, not federation
- Blazor with server-side rendering for public pages
- Central apply model for cross-posted jobs
- Free posting, with paid filtering deferred to later phases
- Manual/admin cross-posting before self-serve distribution

## Risks and mitigations
| Risk | Impact | Mitigation |
|---|---|---|
| SEO pages are not indexed well | Weak discovery | Use SSR, structured data, clean metadata, and test crawlability early |
| Multitenancy adds too much complexity | Slower delivery | Limit tenant customization in Phase 1 to branding, fields, and domain config |
| Seed jobs are low quality | Weak first impression | Combine scraping with manual review and curation |
| Cross-posting creates duplication confusion | Poor UX / SEO issues | Use canonical source tracking and keep central apply as the default |
| Posting flow becomes too heavy | Lower employer adoption | Keep free posting simple and defer advanced tooling |

## Dependencies
- Domain and hosting setup for emplimate.com
- Domain decision for Thai IT tenant
- Initial branding assets
- Seed source list for Thai IT jobs
- Privacy and data handling rules for applications

## Open questions
- What is the exact domain for the Thai IT tenant?
- Which pages are indexed on both source and destination tenants for syndicated jobs?
- What minimum moderation flow is needed before launch?
- What search/filtering depth is necessary in Phase 1?
- What fields are required for Thai IT jobs specifically?

## Phase 1 action list
### Immediate
- Finalise Phase 1 scope
- Confirm Thai IT tenant name and domain
- Confirm base job schema
- Confirm public page templates

### Next
- Build multitenant foundation
- Build employer posting and candidate apply flows
- Implement job fields and screening questions
- Implement admin cross-posting
- Prepare seed ingestion workflow

### Pre-launch
- Load initial Thai IT jobs
- Review SEO metadata and sitemaps
- Prepare onboarding content for first employers
- Validate end-to-end posting and application flows
