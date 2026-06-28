---
name: requirement-vetting
description: "Use this skill whenever a new idea, feature request, or ask needs to be checked BEFORE any PRD or requirements doc is written. Triggers on phrases like 'has this been tried before', 'check if this already exists', 'vet this idea', 'is this feasible', or any raw feature request that hasn't been turned into a requirements document yet. Searches the codebase for existing functionality, past tickets for prior rejections, and ADRs/architecture docs for platform constraints. Always run this BEFORE prd-generation, not after. Do NOT use this for reviewing an already-written PRD (there is no PRD yet at this stage) or for checking code/design quality (use design-review or implementation-review instead)."
---

# Requirement Vetting

**Requires:** a raw idea or request, repo access, access to past tickets/stories if available, access to ADRs/architecture/capacity docs if available.
**Produces:** a Vetting Note — recommendation on whether to proceed, with what's already been tried and what platform constraints apply.

*(Run before writing any requirements document. This is the cheapest place to fail fast — catching a flawed premise here means nobody writes acceptance criteria, a design, or code against something that shouldn't be built as asked. Whatever produces the requirements/PRD afterward should receive this skill's output as an input, not redo this work itself.)*

```
You are vetting a raw idea/request BEFORE any PRD or requirements document is written. Your job
is to surface anything that should change the ask or stop it entirely — not to evaluate whether
it's a good idea on its merits, and not to write requirements yourself.

CONTEXT:
- Raw idea/ask: [paste the original request, however informal]
- Repo(s) to check: [paths]
- Where to search for past tickets/stories: [Jira project(s)/board(s), if accessible; otherwise
  note what you cannot check]
- Where platform facts live: [ADR log, architecture docs, capacity/roadmap docs if available]

INSTRUCTIONS:
1. Codebase check: search for existing functionality that already does what's being asked, fully
   or partially. Read the actual code, don't infer from naming alone — open files and confirm
   what they actually do before claiming something exists or doesn't.
2. Prior-art check: search for past tickets/stories describing the same or a closely related ask.
   For anything found, note the outcome — was it built, rejected, deprioritized, or abandoned —
   and if rejected or abandoned, find and state the stated reason, not just that it happened. A
   past rejection with a reason that no longer holds is different from one that still applies.
3. Platform constraint check: check ADRs, architecture docs, or capacity/roadmap notes (whatever
   you have access to) for anything this idea would conflict with — a documented architectural
   decision it contradicts, a capacity limit it would exceed, a roadmap commitment it competes
   with for the same resources/timeline.
4. Do not soften or talk yourself out of a conflict you found — if something contradicts this
   idea, state it plainly and let the requester/PM decide what to do with that information.
5. If you cannot check a category (e.g. no access to a roadmap doc), say so explicitly rather than
   silently skipping it — a vetting note with an honest gap is more useful than one that looks
   complete but quietly wasn't.
6. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Existing Functionality Found
- [feature/code found] — [file:line] — overlap with this ask: [full/partial/none]

## Related Past Tickets
- [ticket] — outcome: [built/rejected/abandoned] — stated reason: [quote or summarize] —
  still applies today: [yes/no/unclear]

## Platform Constraints/Conflicts
- [ADR/constraint/roadmap item] — conflict: [describe specifically]

## Recommendation
[Proceed to requirements as-is / Proceed with modifications: specify / Needs a human decision
before proceeding — here's why]

## Self-Verification
**Grounded (evidence-backed):**
- [finding above] — backed by: [file:line, specific ticket, or specific ADR]

**Assumed (no hard evidence, but reasonable):**
- [finding above] — assumption: [what's being assumed] — risk if wrong: [concrete consequence]

**Could Not Verify / Gaps:**
- [category you couldn't check] — blocked by: [no access to X / not searchable / etc.] — who can
  unblock: [role/team]

**For the Next Skill (PRD generation):**
- [which findings the PRD should treat as settled vs. flag as an open question vs. resolve before
  proceeding]
```
