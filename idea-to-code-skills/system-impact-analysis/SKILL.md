---
name: system-impact-analysis
description: "Use this skill to map what a proposed change will actually touch across the codebase — services, modules, data, and external dependencies — BEFORE any design or code is written. Triggers on phrases like 'what does this affect', 'impact analysis', 'what will this touch', 'system impact', or whenever a PRD or ticket needs its blast radius assessed across a multi-service codebase. Produces a System Impact Map with a recommended risk tier. Do NOT use this for reviewing a Design Doc's architecture (use design-review) or for checking a single PR's correctness (use implementation-review)."
---

# System Impact Analysis

**Requires:** a PRD (or, for low-risk work, a ticket summary); repo access.
**Produces:** a System Impact Map — services/modules touched, cross-service dependencies, data touched, new external dependencies, a risk tier recommendation.

```
You are reviewing a proposed change for its impact across the codebase BEFORE any design or
code is written. Your job is to find what this idea will actually touch — not to evaluate
whether it's a good idea.

CONTEXT:
- PRD or ticket summary: [paste link/content — a full PRD is strongly recommended for anything
  beyond trivial changes]
- Affected area (if known): [e.g. "checkout-service" or "unsure"]

INSTRUCTIONS:
1. Read the requirements and acceptance criteria fully — your job is to map impact against what's
   actually being committed to, including any stated non-goals (so you don't over-scope the
   analysis to things explicitly excluded).
2. Search the codebase for the area(s) implicated. Do not assume structure — locate the actual
   services, modules, or packages involved by reading directory structure, README files, and
   CLAUDE.md/architecture docs if present.
3. For each service/module you identify as touched, find:
   - Its declared API contracts (REST/GraphQL schemas, gRPC .proto files, event/message schemas)
   - Any other service in the repo that calls it or is called by it (search for client imports,
     HTTP calls, message queue topics/subscriptions, shared DB tables)
   - Whether it owns or shares any database tables/schemas that this change would need to touch
4. Check whether this idea is likely to require any NEW external dependency (a new third-party
   package, a new SaaS/external API integration) based on what's being asked for and what's
   already available in the codebase. Flag this explicitly — new external dependencies carry
   license, supply-chain, and compliance review needs that are easy to miss until someone's
   already mid-implementation.
5. Flag anything that crosses a service boundary, even if the requester didn't mention it.
6. Cross-check your findings against the stated acceptance criteria — if a criterion implies a
   touchpoint you haven't found evidence for in the code, flag it as an open question rather than
   assuming it doesn't apply.
7. Do not speculate about code you have not opened — if you reference a file or function,
   you must have actually read it.
8. Note your confidence level for each finding (High = read the code directly, Medium = inferred
   from naming/structure, Low = guessed).
9. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer. It summarizes the per-finding confidence above into something the next skill
   can act on directly.

OUTPUT FORMAT (use exactly this structure):
## Services/Modules Touched
- [service name] — [file paths as evidence] — confidence: [High/Medium/Low]

## Cross-Service Dependencies Found
- [service A] -> [service B] via [API/event/shared table] — [file path evidence]

## Data Touched
- [table/schema name] — shared with: [other services/teams, if any]

## New External Dependencies Likely Needed
- [package/integration] — [why it appears needed] — flag for license/supply-chain review

## Risk Tier Recommendation
[Tier 1/2/3] — [one-sentence justification based on auto-escalation rules: PII/schema/auth/infra/cross-service]

## Open Questions for Service Owners
- [anything you couldn't resolve from the code alone — who needs to be looped in]

## Self-Verification
**Grounded (evidence-backed):**
- [all High-confidence findings above — list by reference, don't repeat the evidence]

**Assumed (no hard evidence, but reasonable):**
- [all Medium-confidence findings above] — risk if wrong: [what breaks downstream]

**Could Not Verify / Gaps:**
- [all Low-confidence findings, plus anything you couldn't find at all] — blocked by: [reason] —
  who can unblock: [role/team]

**For the Next Skill (design doc generation):**
- [which parts of this map the design can build on directly vs. which need confirming before
  the design locks in an approach that depends on them]
```
