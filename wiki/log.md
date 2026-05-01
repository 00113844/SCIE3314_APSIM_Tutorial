# SCIE3314 APSIM Wiki — Ingest & Query Log

**Purpose**: Chronological append-only record of all ingests, queries, and maintenance actions.

---

## [2026-05-01] bootstrap | Wiki structure initialized

**Type**: Bootstrap  
**Details**: 
- Created three-layer architecture: raw/ + wiki/ + image_placeholders/
- Subfolder structure: entities/, concepts/, sources/, synthesis/, faq/, exercises/
- Created CLAUDE.md schema (1.0)
- Created README.md project overview
- Created index.md and log.md (this file)

**Pages Created**: 
- CLAUDE.md (schema)
- README.md (project overview)
- wiki/index.md (master catalog)
- .gitignore (git configuration)

**Status**: Ready for first source ingest

---

## Ingest Workflow Log

### Format: `## [YYYY-MM-DD] [type] | [source_title]`

**Types**:
- `ingest` — Source added to raw/; wiki updated
- `query` — Question answered; potentially new FAQ/synthesis created
- `lint` — Health check performed; issues flagged/fixed
- `update` — Wiki page updated (external fix or clarification)

---

## Query Workflow Log

*[Queries and answers will be logged here as they occur]*

---

## Maintenance Log

### Monthly Lint Checks

*[To be scheduled monthly]*

---

## Notes

- One ingest/query per line for readability
- Complex ingests may span multiple lines or reference detail files
- Use grep for quick searches: `grep "^## \[" log.md | tail -10` (last 10 entries)

---

**Next Action**: Add first source to raw/sources/; trigger ingest workflow
