# Emplimate — AI Job Board

Emplimate is a niche-first job board platform augmented with AI to match candidates to roles, automate screening, and surface high-quality hires faster than traditional boards.

The core commercial model is simple: **posting a job is free**, while the AI-powered candidate filtering and matching tools are paid.

## Vision
Enable employers, recruiters, and platform operators to find the right candidate with minimal effort by combining niche SEO-focused job domains with AI-driven candidate inference, matching, and cross-network job distribution.

## Value proposition
- Free job posting removes friction and helps seed supply quickly
- Faster, higher-quality matches using AI (skills inference, semantic search, score + explainability)
- Niche focus reduces noise and increases conversion (industry or geographic microsites)
- Reduced hiring friction: automated shortlists, interview kits, interview scheduling
- Cross-posting expands job reach across a network of compatible Emplimate-powered sites
- Self-serve board creation lets communities, recruiters, and operators launch niche job boards quickly
- SEO-friendly board architecture helps niche sites rank for long-tail job searches

## Key features
- Free job posting across niche domains
- AI matching: embeddings + semantic search to rank candidates for each posting
- Automated screening: resume parsing, skills inference, red-flag detection
- Candidate profiles with inferred skills, sample work, and match score
- Employer dashboard: batch-matching, talent pools, messaging, interview scheduling
- Candidate experience: job recommendations, one-click apply, interview preparation assistant
- Cross-posting controls so posters can publish a role to one or many related domains
- Network-aware distribution for recruiters and partners running their own Emplimate instances
- API access for external platforms to create and manage job postings in future phases
- Seed content: scraping and manual curation to populate listings at launch

## Domain and deployment model
Emplimate should support multiple niche deployments under different domains or brands, with shared platform infrastructure underneath.

In a multitenant version, users should be able to create their own job board in a self-serve way, similar to creating a subreddit or community space. Instead of building a new platform each time, they create a new Emplimate tenant with its own branding and configuration.

Initial rollout:
- Start by building and launching the core platform on **emplimate.com**
- The first tenant should target the **Thai IT jobs market**
- Use that first tenant to prove the multitenant model, SEO approach, and cross-posting workflow

Examples:
- Thai IT jobs board (first tenant)
- defencejobs.com
- FNQ (Far North Queensland) jobs board
- Other vertical or regional microsites to drive SEO and relevancy

Each deployment should have configurable properties, including:
- Brand/domain settings
- Logo and favicon
- Theme colours selected from a controlled palette
- Geographic focus
- Industry/category taxonomy
- Job types, tags, and classification options
- Employer profile fields
- Candidate profile fields
- Moderation rules and listing policies
- Pricing configuration for AI tools
- Cross-posting permissions and partner network rules

## Self-serve board creation
Users should be able to create their own board from inside Emplimate without needing engineering support.

Board creation flow:
- Choose a board name
- Select a category, niche, or region
- Upload a logo
- Pick from a predefined colour set and basic theme options
- Connect a custom domain, or use an Emplimate subdomain
- Configure which job fields, categories, and screening templates are enabled
- Set whether the board can receive inbound cross-posted jobs from the wider network

This makes Emplimate more than a job board product. It becomes a platform for launching and operating niche job communities.

## Network and distribution model
Emplimate.com can act as the central job hub containing all jobs, while employers or recruiters can choose to cross-post roles to other hosted Emplimate deployments.

Examples:
- A role posted on emplimate.com can also appear on defencejobs.com and an FNQ jobs deployment
- A recruitment company can host its own Emplimate-powered site, post jobs once, and distribute them to selected partner instances
- Independent operators can join a shared network and accept inbound jobs from trusted sources

Cross-posting should support:
- Per-job selection of target sites
- Default posting rules by employer or recruiter account
- Approval workflows for jobs entering another deployment
- Canonical source tracking so one job can appear on many sites without duplication issues
- Shared analytics showing where applications and hires came from
- Future API-based ingestion from external job systems, ATS platforms, and partner sites

## Platform architecture options
There are two viable models:

### 1. Multitenant hosted platform
- Emplimate runs multiple domains from one shared application and database
- Cross-posting is simpler because all deployments are inside one system
- Easier billing, moderation, analytics, and shared identity
- Easier to introduce a secure external posting API later
- Enables self-serve creation of new branded boards with custom domains and simple theming
- Best option for getting to market quickly

### 2. Open-source federated network
- Emplimate could be open source so third parties can host their own instances
- A central emplimate.com identity/login could allow employers and recruiters to publish across compatible instances
- External directories could opt into receiving jobs from other Emplimate nodes
- APIs would be needed for posting, syncing, authentication, and trust between instances
- More powerful long term, but more complex due to federation, trust, identity, and syncing

Recommended direction:
- Start with a multitenant hosted model
- Design the job schema, identity model, and APIs so federation can be added later
- Treat the long-term vision as a job network, not just a single SaaS product
- Position board creation as a core workflow: "create your own job board in minutes"

## Job posting model
Each job ad should support both standard fields and deployment-specific customisation.

Standard job ad fields:
- Job title
- Employer name
- Location
- Employment type
- Salary or salary range
- Description
- Requirements
- Benefits
- Application method

Customisable job ad properties:
- Deployment-specific fields such as clearance level, licences, certifications, language requirements, travel requirements, roster, or remote eligibility
- Optional structured fields to improve filtering and AI ranking
- Required vs optional field controls set per deployment

Customisable screening questions:
- Employers should be able to add custom questions to each job
- Questions can be knockout, short answer, multiple choice, or yes/no
- Answers should feed into filtering, ranking, and shortlist generation
- Templates could be saved for recurring job types

Cross-posting options on each job:
- Post only to the current site
- Cross-post to selected Emplimate-hosted deployments
- Cross-post to trusted partner instances in the wider network
- Configure whether applications are collected centrally, locally, or synced back to the source

## MVP (Phase 1)
- Free, niche job board with seeded listings (scraped + curated)
- Candidates can apply to jobs
- Employer posting flow and contact/lead capture
- Deployment-level configurable job fields
- Per-job custom screening questions

## Phase 2
- Candidate profiles that employers can search
- Basic candidate search and filtering
- Paid candidate filtering tools layered on top of free posting
- Manual/concierge matching service to prove product-market fit
- Saved job templates and reusable question sets
- Self-serve cross-posting controls for employers and recruiters
- Shared identity across multiple hosted deployments
- Self-serve tenant creation with custom domain, logo, and colour theme

## Phase 3 (AI features)
- Automated candidate-job matching and ranking
- Premium employer features: prioritized matching, paid outreach, analytics
- Placement marketplace and API for partners
- Explainable candidate scoring based on resumes, answers, and structured job criteria
- Federation APIs so third-party Emplimate instances can publish into the broader network
- Public API for external platforms to submit jobs into Emplimate-hosted sites or network destinations

## Monetisation
- Free to post every job
- Per-job fee to unlock AI filtering, ranking, shortlist generation, and candidate fit scoring
- Employer subscriptions (basic -> pro tiers) for teams hiring regularly
- Pay-per-match or pay-per-lead for high-intent employers
- Placement fees or success fees for hires
- Sponsored listings and featured placements on niche domains
- Network distribution fees for cross-posting into premium partner sites
- White-label or hosted platform subscriptions for recruitment agencies running their own Emplimate deployment
- API and integration plans for ATS, recruiters, and partner platforms
- Paid board plans for operators who want a branded niche board with custom domain and network controls

## Pricing model
- **Free:** post jobs and receive applications
- **Per job:** pay to activate AI candidate filtering, ranked shortlists, and screening analysis for a single role
- **Subscription:** monthly plan for employers or recruiters hiring across multiple roles
- Optional upsells: featured listings, candidate outreach, talent pool access, success-based hiring services
- **Network/hosted plans:** paid tiers for agencies or operators that want their own branded Emplimate site and distribution controls
- **Board creator plans:** subscription tiers for users who want to launch and manage their own niche job board

## Data, privacy & compliance
- Collect minimal PII, secure storage, and GDPR/Australian privacy compliance as needed
- Candidate consent for profile inference and matching
- Clear rules for where applicant data is stored when a role is cross-posted across sites

## Metrics / KPIs
- Time-to-first-match, match-to-hire conversion, employer LTV, candidate activation, churn

## Risks & mitigations
- Data quality: manual curation + validation rules
- Legal/scraping risk: prefer partnerships and public feeds; follow robots.txt
- Market competition: niche focus + AI matching differentiator
- Over-complex posting flows: keep job posting free and simple, and make advanced filtering optional
- Network trust and spam risk: require approval rules, source reputation, and per-deployment moderation controls

## Go-to-market (first 8 weeks)
1. Run a 2–4 week manual pilot: offer free job posting, then sell AI-assisted filtering on either a per-job basis or a subscription to validate pricing
2. Launch emplimate.com as the core platform and central job hub
3. Launch the first tenant for the Thai IT jobs market with seeded listings and targeted SEO content
4. Test cross-posting between emplimate.com and the first tenant to prove the network model
5. Outreach to local recruiters and industry groups for partnerships
6. Collect case studies and early testimonials to drive paid signups

## Roadmap (12 weeks)
- Weeks 0–2: Pilot & data collection (manual matching)
- Weeks 3–6: Launch emplimate.com, then launch the first Thai IT jobs tenant with onboarding workflows and basic central-to-domain cross-posting
- Weeks 7–12: Build core AI matching, employer dashboard, subscription billing, and multi-site posting controls

## Next steps
- Prepare pilot scope and hiring brief templates
- Seed candidate/source data and 5 initial employer partners
