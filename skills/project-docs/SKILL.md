---
name: project-docs
description: Write, update, or review a repo's AGENTS.md, README.md, PRODUCT.md, and doc/engineering/ rule files using a fixed, lean section convention. Use when creating or editing any of these files, bootstrapping project docs, restructuring existing docs, or deciding which file a piece of project information belongs in.
---

# Project docs

Three files, one reader each, plus engineering rule files. Every fact lives in exactly one file. The others link to it.

| File | Reader | Sections, in this order |
|---|---|---|
| `PRODUCT.md` | Someone deciding what to build | Problem · Assumption · Solution · Reference |
| `README.md` | A person getting the project running | Overview · Project Setup · Command · Reference |
| `AGENTS.md` | A coding agent working in the repo | Structure · Rule · Reference |
| `doc/engineering/AGENTS.md` | A coding agent looking for a rule | Index |
| `doc/engineering/<topic>.md` | A coding agent applying a rule | Rule · Reference |

## Placement

- `PRODUCT.md` lives only at the repo root. There is one per repo.
- `README.md` and `AGENTS.md` live at the root and may also be nested in any directory that needs its own. A nested file covers only its directory's subtree. It inherits everything from its parents and links up instead of repeating them.
- `doc/engineering/` lives at the repo root and holds repo-wide rules. Add a `<topic>.md` when a topic has its first rule.
- In a monorepo, an app or package with its own rules gets its own `doc/engineering/` at its root, such as `apps/web/doc/engineering/`. It covers only that app. It inherits the root's rules and doesn't repeat them.

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
- **Rule**: conventions and constraints an agent would get wrong unless told. Write each as a one-line imperative. Design, code-style, and convention rules go in a `doc/engineering/` topic file instead.
- **Reference**: links to README.md, PRODUCT.md, and external docs. Don't copy their content. Every `AGENTS.md` links here to the nearest `doc/engineering/AGENTS.md`: its own app's if there is one, otherwise the root's.

Next to every `AGENTS.md`, root or nested, there must be a `CLAUDE.md` that contains only `@AGENTS.md`, so Claude Code loads it. If it's missing, create it.

## doc/engineering/

- `AGENTS.md` is an index only. **Index**: one line per rule file, `[<topic>.md](<topic>.md)`, then what it covers. No rules here.
- An app's index starts with a line linking to the root index.
- Each `<topic>.md` covers one topic, such as `code-style.md`, `error-handling.md`, or `api-design.md`. Name it after the topic.
- **Rule**: one-line imperatives. Add a short code example only when the rule is unclear without one.
- **Reference**: links only.
- An app's `<topic>.md` holds only rules that add to or override the root's file of the same name. Where they conflict, the app's rule wins inside that app.
- Adding, renaming, or removing a rule file means updating the index in the same change.

## Rules

- **Lean**: use one-line bullets, not paragraphs. Cut any line the reader wouldn't miss.
- **No duplication**: before writing a fact, check whether another file owns it, including a parent README.md or AGENTS.md. If one does, link to it. For example, AGENTS.md links to the Command section of README.md instead of listing the commands again.
- **Don't describe code**: don't explain what functions, classes, or modules do or how they work. Code is the source of truth, and prose about it goes stale. Structure says where things live, not how they work.
- **Only what is true now**: no roadmaps, TODOs, or plans.
- **Restructuring an existing file**: move its content into these sections, and drop duplicates and code descriptions. Afterwards, list what you dropped so the user can restore anything they want to keep.
