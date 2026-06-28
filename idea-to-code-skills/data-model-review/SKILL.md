---
name: data-model-review
description: "Use this skill to check a proposed database schema change against the actual current schema and every consumer of the affected tables across all services. Triggers whenever the user asks to 'review this migration', 'check this schema change', or mentions a database/table change that needs review before a formal schema review board. Produces evidence FOR that formal review — does not replace it. Do NOT use this for general design review (use design-review) or for reviewing already-implemented code (use implementation-review)."
---

# Data Model Review

**Requires:** a proposed schema change; the Design Doc it belongs to.
**Produces:** evidence for formal schema review — current schema, all consumers found, migration assessment, compliance flags.

*(Feeds into your existing formal schema review — this skill prepares the evidence for that review, it doesn't replace it.)*

```
You are preparing a data model change for formal schema review by checking it against actual
current schema and usage in the codebase.

CONTEXT:
- Proposed schema change: [paste migration/DDL or description]
- Design Doc: [link]

INSTRUCTIONS:
1. Find the current schema definition for every table being changed (migration files, ORM
   models, schema.sql, etc. — use what the codebase actually has, not what the doc assumes).
2. Search the codebase for every read/write access to these tables — across ALL services, not
   just the one being changed. Look for raw SQL, ORM model usage, and any reporting/analytics
   jobs that query these tables directly.
3. For each consumer found, assess: does the proposed change break it (column removed/renamed/
   retyped, nullable changed, etc.)?
4. Check whether a migration plan is specified for existing data (backfill, default values,
   nullable transition period) — flag if missing.
5. Check whether this table is referenced in any data retention/compliance-relevant logic
   (look for PII handling, encryption, audit-log table references).
6. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer. Be especially honest here: missed consumers are the single most common way
   this kind of review fails silently.

OUTPUT FORMAT:
## Current Schema (as found in code)
[table: columns, types, constraints, with file path]

## All Consumers Found
- [service/job] — [file:line] — access type: [read/write] — broken by this change: [yes/no/unsure]

## Migration Plan Assessment
[present and adequate / present but gaps: list / missing entirely]

## Compliance-Relevant Flags
- [any PII/audit-log/retention implication found]

## Recommendation for Schema Review Board
[Ready for review / Needs more migration detail / Needs consumer sign-off from: list teams]

## Self-Verification
**Verified (direct evidence):**
- [consumer found and assessed] — confirmed at [file:line]

**Could Not Verify:**
- [any service/repo you couldn't search — e.g. a service in a repo you don't have access to, or
  a reporting job in an external BI tool] — blocked by: [reason] — who can unblock: [role/team]

**Confident But Not Proven:**
- [any "broken by this change: unsure" item — call it out explicitly here too] — basis: [why you
  suspect this either way] — confidence: [Medium/Low]

**For the Next Skill (formal schema review board, test strategy):**
- [which consumers are fully confirmed safe/unsafe vs. which need a human to manually check a
  system this skill couldn't search]
```
