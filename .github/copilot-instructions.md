# Copilot instructions for this repository

## Build, test, and lint

This repository is an **Obsidian vault**, not an application codebase. There are currently **no repository-level build, test, or lint commands** defined at the vault root.

Many notes under `Projects/` and `Business Idea/` contain commands such as `pip install`, `npm run dev`, or local filesystem paths. Treat those as **content about external projects**, not commands to run from this repository unless the note explicitly points you to another repo.

## High-level architecture

The vault is organized by **note purpose** rather than by software package:

- `README.md` is the top-level human index to the major vault areas.
- `Business Idea/` holds idea backlogs, product specs, and longer-form planning documents. Some subfolders are effectively product-spec areas for future apps.
- `Projects/` contains overview notes for projects, often with summaries, repo links, local paths, and quick-start notes for code that lives elsewhere.
- `Todos/` contains lightweight task lists and personal tracking notes.
- `Daily Note/` contains date-based daily notes.
- `Xtras/Templates/` contains reusable Obsidian templates that shape daily-note and work-log structure.
- `Orphans.md` is a maintenance note that tracks notes with no inbound wiki-links.
- `copilot/copilot-custom-prompts/` stores reusable custom Copilot prompt files in markdown.

The important distinction is that **this repo stores knowledge and workflow artifacts**, while many notes describe software projects that live in other repositories.

## Key conventions

- **Preserve Obsidian syntax.** Notes use wiki-links like `[[Business Idea/All Ideas]]`, inline metadata like `tags::`, and embedded query/template syntax such as Templater (`<% ... %>`) and Dataview code blocks. Do not rewrite these into plain markdown unless asked.
- **README uses normal markdown links, notes use wiki-links.** Keep the existing style of the file you are editing instead of normalizing everything to one format.
- **Daily notes are date-named.** Files in `Daily Note/` use `YYYY-MM-DD.md`. The daily-note and work-note structure comes from templates in `Xtras/Templates/`.
- **Templates are source files, not rendered output.** Files in `Xtras/Templates/` intentionally contain raw Templater expressions; keep those expressions intact.
- **Project notes often point outside the vault.** Notes such as `Projects/* - Project Details.md` may include local paths like `C:\Users\wayne\Code\...`, GitHub repository links, and setup commands. Update those notes as references/documentation, but do not assume the referenced code exists in this repo.
- **Cross-linking matters.** `Orphans.md` exists to surface notes without inbound links, so when adding or reorganizing notes, prefer adding contextual wiki-links rather than creating isolated pages.
- **Custom Copilot prompts are markdown files with frontmatter.** Files in `copilot/copilot-custom-prompts/` contain YAML frontmatter plus a prompt body; preserve both when editing or adding prompts.
