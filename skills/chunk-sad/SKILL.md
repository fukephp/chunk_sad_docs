---
name: chunk-sad
description: Chunk source documents into SAD or TDD section markdown.
disable-model-invocation: true
---

# Chunk SAD

Output folder is `docs/` unless the user names another. This run overwrites the 12 section files and `roadmap.md` there; every other file in the folder stays.

Ask every gap. Wait for answers or skips. Then write.

## Steps

1. **Collect.** List every attached, @-mentioned, or named source with path and type. Done when the list is complete. An empty list is not done: ask which files to chunk.

2. **Extract.** Turn every source into readable text. Done when every source is text in context. Office sources (`.docx`, `.xlsx`, `.xls`): read [EXTRACT.md](EXTRACT.md).

3. **Classify.** Assign every passage to every section it informs; split a mixed passage. Mark the rest out of scope. Done when every source is accounted for and every section is Documented, Partial, or Missing.

4. **Ask gaps.** Pose concrete, skippable questions for every Missing or Partial section, grouped by section. Use AskQuestion when available. Recommend an answer only when the sources almost say it. Skips are valid. Done when every such section has its questions in chat. Wait for answers or skips.

5. **Apply replies.** Done when every posed question is answered (content ready to write) or skipped (stays a gap).

6. **Write sections.** Write all 12 files into the output folder using the skeleton. Done when all 12 exist.

7. **Write coverage.** Write `roadmap.md`. Done when it lists all 12 with status, remaining gaps, the source list, and Implemented vs planned — taken from the sources, or one gap line that the distinction is missing. Chat names every skipped section.

## Sections

Filename and H1 are verbatim.

- `overview-and-stack.md` — Overview and Stack — purpose, users, tech stack, high-level shape
- `system-context.md` — System-Context — actors, external systems, boundaries, C4 context
- `data-model.md` — Data-Model — entities, schemas, storage, relationships
- `decisions.md` — Decisions — ADRs, trade-offs, chosen options
- `frontend.md` — Frontend — UI, clients, rendering
- `backend.md` — Backend — APIs, services, domain logic
- `requirements-and-goals.md` — Requirements and Goals — functional and non-functional requirements, goals
- `deployment-and-infrastructure.md` — Deployment and Infrastructure — hosting, CI/CD, environments
- `security-and-compliance.md` — Security and Compliance — auth, threats, regulations
- `dynamic-view.md` — Dynamic View — sequences, runtime flows, processes
- `cross-cutting-concerns.md` — Cross-Cutting Concerns — logging, errors, observability, i18n
- `quality-attributes-and-constraints.md` — Quality Attributes and Constraints — performance, scalability, SLAs, constraints

## Section file

```markdown
# <H1 from catalog>

Status: Documented | Partial | Missing

## Content

<source material and answers>

## Gaps

- <unanswered question>
```

Documented: Content covers the section; omit Gaps. Partial: Content exists and Gaps remain. Missing: Content is empty; keep Gaps.

## coverage file

`roadmap.md`:

```markdown
# Roadmap

## Coverage

- Overview and Stack — <status>
- System-Context — <status>
- Data-Model — <status>
- Decisions — <status>
- Frontend — <status>
- Backend — <status>
- Requirements and Goals — <status>
- Deployment and Infrastructure — <status>
- Security and Compliance — <status>
- Dynamic View — <status>
- Cross-Cutting Concerns — <status>
- Quality Attributes and Constraints — <status>

## Open questions

- <section>: <gap>

## Sources

- <path>

## Implemented vs planned

<from sources, or: Missing — sources do not distinguish implemented from planned.>
```
