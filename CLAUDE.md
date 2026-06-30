# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is an **Obsidian vault** — a personal knowledge base. It contains notes, plans, and documents. There are no build, test, or lint commands at the vault root.

Notes under `Projects/` and `Business Idea/` may reference commands (e.g. `npm run dev`, `pip install`) or local paths — treat these as **content about external projects**, not commands to run here.

## Vault structure

- `Business Idea/` — idea backlogs, product specs, and planning docs; some subfolders are full product-spec areas for future apps
- `Projects/` — overview notes for external projects (repo links, local paths, setup notes for code that lives elsewhere)
- `Todos/` — lightweight task lists and personal tracking
- `Daily Note/` — date-based daily notes (`YYYY-MM-DD.md`)
- `Xtras/Templates/` — reusable Obsidian templates for daily notes and work logs
- `copilot/copilot-custom-prompts/` — reusable Copilot prompt files (markdown with YAML frontmatter)
- `Orphans.md` — maintenance note tracking notes with no inbound wiki-links

## Conventions

- **Preserve Obsidian syntax**: wiki-links like `[[Note Name]]`, inline metadata like `tags::`, Templater expressions (`<% ... %>`), and Dataview code blocks. Do not convert these to plain markdown unless asked.
- **README uses normal markdown links; notes use wiki-links.** Match the style of the file being edited.
- **Templates contain raw Templater expressions** — keep them intact; they are not rendered output.
- **Custom Copilot prompts** have YAML frontmatter plus a prompt body — preserve both when editing.
- **Cross-linking matters**: prefer adding wiki-links to new notes rather than leaving them isolated (Orphans.md surfaces link-less notes).

## Architecture defaults for new project specs

When drafting architecture documents for any project in this vault, use this standard unless overridden:

- **C# / ASP.NET Core** — web app, API, business logic, background jobs
- **TypeScript** — frontend
- **PostgreSQL** — primary system-of-record database
- **Python** — AI workflows, extraction pipelines, embeddings, evaluations
- **Rust** — only for clearly performance-critical components

Principles: modular monolith deployable to cheap hosting; simple deployment over microservices; C# as product core, TypeScript as UI, PostgreSQL as source of truth, Python as AI layer.

**Template selection** (from `copilot/copilot-custom-prompts/Prototype architecture defaults.md`):
- Next.js RSC — needs SEO + dynamic server rendering + rich app behaviour
- Next.js Static — mostly content/SEO, largely static
- React SPA — mostly app/dashboard, SEO not central
