---
name: project-docs
description: Write, update, or review a repo's AGENTS.md, README.md, and PRODUCT.md using a fixed, lean section convention. Use when creating or editing any of these files, bootstrapping project docs, restructuring existing docs, or deciding which file a piece of project information belongs in.
---

# Project docs

Three files, one reader each. Every fact lives in exactly one file. The others link to it.

| File | Reader | Sections, in this order |
|---|---|---|
| `PRODUCT.md` | Someone deciding what to build | Problem · Assumption · Solution · Reference |
| `README.md` | A person getting the project running | Overview · Project Setup · Command · Reference |
| `AGENTS.md` | A coding agent working in the repo | Structure · Rule · Reference |

## Placement

- `PRODUCT.md` lives only at the repo root. There is one per repo.
- `README.md` and `AGENTS.md` live at the root and may also be nested in any directory that needs its own. A nested file covers only its directory's subtree. It inherits everything from its parents and links up instead of repeating them.

Each section is a `##` heading with exactly that name. Add no other sections. If a section has nothing true to say yet, write `None.` under it.

## PRODUCT.md

- **Problem**: who has the problem, and what it costs them. Don't mention the solution here.
- **Assumption**: what must be true for the solution to work. Write each one so it can be proven wrong.
- **Solution**: what the product does for the user, described as behavior, not implementation.
- **Reference**: links only.

## README.md

- **Overview**: one or two sentences on what the project is. Link to PRODUCT.md for why it exists.
- **Project Setup**: prerequisites, then copy-pasteable steps from clone to running.
- **Command**: the everyday commands (run, test, lint, build), each with a short note.
- **Reference**: links only.

## AGENTS.md

- **Structure**: one line per path, saying what lives there. List only what an agent couldn't guess from the names.
- **Rule**: conventions and constraints an agent would get wrong unless told. Write each as a one-line imperative.
- **Reference**: links to README.md, PRODUCT.md, and external docs. Don't copy their content.

Next to every `AGENTS.md`, root or nested, there must be a `CLAUDE.md` that contains only `@AGENTS.md`, so Claude Code loads it. If it's missing, create it.

## Rules

- **Lean**: use one-line bullets, not paragraphs. Cut any line the reader wouldn't miss.
- **No duplication**: before writing a fact, check whether another file owns it, including a parent README.md or AGENTS.md. If one does, link to it. For example, AGENTS.md links to the Command section of README.md instead of listing the commands again.
- **Don't describe code**: don't explain what functions, classes, or modules do or how they work. Code is the source of truth, and prose about it goes stale. Structure says where things live, not how they work.
- **Only what is true now**: no roadmaps, TODOs, or plans.
- **Restructuring an existing file**: move its content into these sections, and drop duplicates and code descriptions. Afterwards, list what you dropped so the user can restore anything they want to keep.
