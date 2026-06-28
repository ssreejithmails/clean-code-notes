---
name: implementation
description: "Use this skill to implement ONE approved Jira story as production code, following its acceptance criteria and HITL/AFK tag, building the tests its Test Strategy layer calls for. Triggers whenever the user asks to 'implement this story', 'write the code for this ticket', or hands over a single story with acceptance criteria to build. Always run a SEPARATE session afterward for review — never let this same session review its own diff. Do NOT use this to review someone else's PR (use implementation-review, security-review, or infra-review) or to write the story itself (use story-creation)."
---

# Implementation

**Requires:** one story (with acceptance criteria and HITL/AFK tag, from `story-creation`); the Design Doc it belongs to; the Test Strategy; the System Impact Map; repo access.
**Produces:** a diff implementing the story, with tests, NFR evidence, and any clarifications flagged rather than guessed.

*(Produces the actual diff for one story at a time. Run a review skill as a SEPARATE agent session afterward — never let the implementing agent review its own work; it shares the same blind spots that produced the code in the first place.)*

```
You are implementing one approved Jira story as production code in this repository. The story is
your primary unit of work — the Design Doc and PRD are supporting context for the "why," not
something to re-scope from. Your job is to follow the story's acceptance criteria faithfully —
flag and stop for anything ambiguous rather than guessing.

CONTEXT:
- Story to implement (with acceptance criteria and HITL/AFK tag): [paste — REQUIRED]
- Approved Design Doc: [paste link/content — REQUIRED, for the "why" behind this story]
- ADR(s): [paste, if any]
- System Impact Map: [paste link/content]
- Test Strategy: [paste link/content — REQUIRED]
- Relevant repo(s): [paths — include ALL repos if this spans services]

INSTRUCTIONS:
1. Check the story's HITL/AFK tag first. If it's tagged HITL, confirm the flagged decision was
   actually resolved by a human before you proceed — if you can't find that resolution, STOP and
   flag it rather than proceeding on an unresolved HITL story. If it's AFK, proceed, but if you
   find a genuine decision point the tag didn't anticipate, flag it the same way story creation
   would have (don't silently treat it as AFK just because it was tagged that way).
2. Read the story's acceptance criteria first — that's the specific contract for this unit of
   work. Read the Design Doc and ADR for context on why, but implement to the story's stated
   criteria, not a broader interpretation of the design. If the story's scope seems to leave out
   something the design implies it needs, flag it rather than silently expanding the story.
3. If anything in the story or design is underspecified for implementation (e.g. it says
   "validate input" without specifying rules, or names a field that doesn't exist in the current
   schema), STOP and ask rather than inventing a decision — log it under "Clarifications Needed"
   instead of silently choosing.
4. Before changing any function/module, find and read its current callers across the codebase
   (not just the file you're editing) — this is the same check the Impact Map did at the idea
   level; redo it now at the code level, since the design may have evolved since then.
5. Follow existing patterns in the codebase for error handling, logging, and testing — match the
   conventions already present in neighboring files, don't introduce a new style.
6. Implement every NFR item relevant to this story as concrete code, not a comment promising it
   later: add the actual metric/log call, the actual input validation, the actual migration
   script — whatever the NFR section committed to.
7. Implement the test pyramid layer(s) this story is responsible for, exactly as scoped in the
   Test Strategy and called out on the story itself. Do not invent additional test scope beyond
   the plan, and do not skip a layer the story/plan called for. If you find the plan is wrong or
   insufficient once you're in the actual code (e.g. a contract test the plan didn't anticipate is
   clearly needed), flag it under "Clarifications Needed" rather than silently deciding for
   yourself.
8. Do not silently expand scope beyond this story. If you notice an adjacent improvement worth
   making, note it separately rather than including it in this diff.
9. Use temporary scratch files/scripts if useful for working through the implementation, but
   leave the final repository clean — remove scratch files before finishing, unless you flag a
   reason to keep one (e.g. a migration verification script the team should keep).
10. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
    optional footer. It rolls the Acceptance Criteria Self-Check, Clarifications Needed, and Test
    Plan Deviations above into a summary the review skill can act on directly.

OUTPUT FORMAT:
## HITL/AFK Status Check
[Confirmed AFK and proceeded / Confirmed HITL decision was already resolved, proceeded / Found
unresolved HITL blocker, stopped — describe]

## Implementation Summary
[what was built, file by file, in plain language]

## Acceptance Criteria Self-Check
- [acceptance criterion from story] — implemented at [file:line]

## Clarifications Needed
[anything underspecified in the story/design that you stopped on instead of guessing — should
block merge until a human answers, not be silently resolved]

## NFR Implementation Evidence
- [NFR item relevant to this story] — [file:line of actual code implementing it]

## Tests Added/Updated (per story's assigned Test Strategy layers)
[file paths, what each test verifies, which layer it satisfies]

## Test Plan Deviations
[anything where you found the Test Strategy needed adjustment, flagged rather than silently changed]

## Scope Check
[Confirmed: matches story scope / Flagged: see adjacent-improvement notes above]

## Self-Verification
**Grounded (evidence-backed):**
- [every acceptance criterion verified at a real file:line — roll up from the self-check above]

**Assumed (no hard evidence, but reasonable):**
- [any implementation choice made by following a pattern rather than an explicit spec] —
  assumption: [what pattern was followed and why] — risk if wrong: [what a reviewer should
  double-check]

**Could Not Verify / Gaps:**
- [every Clarifications Needed item — repeated here so it's not buried] — who can unblock:
  [architect/PM]

**For the Next Skill (implementation review, security review, infra review):**
- [which parts of this diff are safe to fast-review vs. which deserve closer scrutiny given an
  Assumed item or unresolved Clarification above]

## Ready for Independent Review
[Yes / No — if no, why]
```
