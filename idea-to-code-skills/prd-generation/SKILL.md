---
name: prd-generation
description: "Use this skill to draft a first-pass Product Requirements Document (PRD) from a raw idea or ask. Triggers whenever the user asks to 'write a PRD', 'draft requirements', 'turn this into a PRD', or describes a feature and wants it formalized into a structured requirements document with acceptance criteria and non-goals. Reads a Vetting Note if one exists. Produces a DRAFT for a Product Manager to edit and finalize — does not replace human sign-off. Do NOT use this for technical/architecture design (use design-doc-generation) or for breaking a PRD into engineering tickets (use story-creation)."
---

# PRD Generation

**Requires:** the raw idea/ask; a Vetting Note if one exists (recommended, not strictly required).
**Produces:** a PRD — problem statement, acceptance criteria, non-goals, success metric.

*(Produces a first full draft for a Product Manager to edit and finalize. If a Vetting Note exists, this skill should read it and reflect its findings; if none exists, it proceeds on the raw ask alone but flags that prior-art/constraint checking hasn't happened yet.)*

```
You are drafting a first-pass PRD for a Product Manager to edit and finalize. Your job is to
turn a raw idea/ask into a structured, falsifiable requirements document, not to invent product
strategy. Where the ask is ambiguous about scope or priority, surface the ambiguity explicitly
rather than resolving it yourself.

CONTEXT:
- Raw idea/ask: [paste the original request, however informal]
- Vetting Note, if one exists: [paste link/content, or state "none — vetting not yet done"]
- PRD template: [paste your Confluence template structure]

INSTRUCTIONS:
1. If a Vetting Note exists, read it first and reflect its findings directly in the PRD: if it
   found partial existing functionality, scope the PRD around what's actually new rather than
   restating something that already exists; if it found a past rejection that no longer applies,
   note why in the PRD so that context isn't lost; if it found a platform constraint, either write
   it in as a non-goal/limitation or flag it as an open question if it seems blocking. If no
   Vetting Note exists, say so explicitly in the output rather than silently treating the ask as
   pre-vetted.
2. Write the problem statement in terms of the user/business outcome, not the solution — if the
   raw ask already prescribes a specific technical solution, separate "what problem this solves"
   from "the asker's proposed solution" so downstream design work isn't anchored to an
   unvalidated approach.
3. Write acceptance criteria as specific, testable statements ("a user can X and sees Y"), not
   vague goals ("improve the experience"). Every acceptance criterion should be falsifiable —
   someone should be able to look at the finished feature and say yes/no, did this happen.
4. Write explicit non-goals: what this PRD is deliberately NOT solving, especially anything
   adjacent that someone might assume is included. This is what stops scope from silently
   expanding during design or implementation.
5. Propose a success metric that's measurable post-launch, not just "users are happy."
6. Flag anything in the raw ask that's ambiguous about priority, scope, or audience as an open
   question for the PM — do not silently pick an interpretation.
7. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT (mirror your Confluence PRD template):
## Problem Statement
[user/business problem, separated from any prescribed solution]

## Target User / Context
[who this is for, when/why they'd encounter it]

## Requirements & Acceptance Criteria
- [requirement] — acceptance criteria: [specific, testable]

## Non-Goals
- [explicitly out of scope, especially adjacent things someone might assume are included]

## Success Metric
[measurable, post-launch]

## Open Questions for PM
- [ambiguity in the raw ask that needs a human decision, including any blocking item flagged
  in the Vetting Note, if one exists]

## Self-Verification
**Grounded (evidence-backed):**
- [PRD element] — backed by: [specific line in the raw ask, or specific Vetting Note finding]

**Assumed (no hard evidence, but reasonable):**
- [PRD element] — assumption: [what's being assumed about scope/priority/audience] — risk if
  wrong: [what breaks downstream if this assumption is false]

**Could Not Verify / Gaps:**
- [thing this skill needed but didn't have — e.g. no Vetting Note existed] — who can unblock:
  [role/team]

**For the Next Skill (system impact analysis):**
- [which requirements are firm vs. likely to shift — impact analysis should treat firm ones as
  scope and flag if it finds something the shifting ones didn't anticipate]
```
