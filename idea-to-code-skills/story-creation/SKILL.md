---
name: story-creation
description: "Use this skill to break an approved Design Doc and Test Strategy into vertically-sliced Jira stories, each tagged HITL (needs a human decision) or AFK (fully specified, ready for AI implementation). Triggers on phrases like 'break this into stories', 'create tickets for this', 'slice this into tasks', or whenever a design is approved and needs to become implementable units of work. Do NOT use this to write the implementation itself (use implementation) or to draft the original requirements (use prd-generation)."
---

# Story Creation

**Requires:** an approved Design Doc; an approved Test Strategy; a PRD.
**Produces:** vertically-sliced, HITL/AFK-tagged Jira stories, each traceable to a PRD requirement and a Design Doc section.

*(Produces the actual stories an implementer — human or AI — will work from. This exists because a vague, horizontally-sliced story breakdown is exactly what causes an implementing AI to fill gaps with assumptions instead of flagging them.)*

```
You are breaking an approved Design Doc and Test Strategy into implementable Jira stories. Your
job is to produce vertical-slice stories specific enough that someone (human or AI) could
implement each one without needing to re-derive scope from the design doc — not to summarize the
design into vague restatements, and not to slice it horizontally by layer.

CONTEXT:
- Approved PRD: [paste link/content]
- Approved Design Doc: [paste link/content]
- Approved Test Strategy: [paste link/content]
- Jira project/template conventions: [paste, if you have a standard story format]

INSTRUCTIONS:
1. Read the Design Doc and Test Strategy fully. Every distinct piece of the proposed approach
   should map to at least one story — do not collapse the whole design into a single story, and
   do not split it so finely that individual stories are meaningless on their own.
2. Slice VERTICALLY, not horizontally: each story should cut through every layer it touches
   end-to-end (e.g. API + service logic + data access for one capability), not isolate a single
   layer across the whole feature (e.g. "build all the API endpoints" as one story and "build all
   the DB queries" as another). A horizontal slice can look done while nothing actually works
   end-to-end yet — that's the failure mode this rule exists to prevent.
3. For each story, write acceptance criteria that trace back to a specific PRD requirement and a
   specific part of the Design Doc — cite which requirement/section, don't just paraphrase the
   design generically.
4. For each story, note which Test Strategy layer(s) (unit/integration/contract/e2e/NFR
   verification) it's responsible for, so test creation isn't an afterthought bolted on at the
   end of implementation.
5. Tag each story HITL or AFK:
   - **HITL** (Human-In-The-Loop): the story requires a human decision mid-implementation — an
     architectural judgment call, an ambiguous UX/product decision, or anything earlier review
     flagged as an open question that this story would need resolved before an implementer could
     proceed without guessing.
   - **AFK** (Away-From-Keyboard): the story is fully specified by its acceptance criteria and the
     Design Doc — an implementer (human or AI) can complete it without a mid-stream decision.
   Default to HITL when genuinely uncertain — a story wrongly tagged AFK is the one that produces
   a silent bad guess; a story wrongly tagged HITL just costs someone a quick confirmation.
6. Sequence the stories in a sensible implementation order (e.g. data model changes before the
   code that depends on them, shared utilities before their consumers) and flag any dependencies
   between stories explicitly.
7. After drafting all stories, do a coverage check: list every requirement in the PRD and every
   proposed-approach element in the Design Doc, and confirm each one is covered by at least one
   story. Flag anything from the design that isn't covered by any story (a drop) and anything in
   a story that isn't traceable to the design (an addition) — neither should happen silently.
8. Do not write stories so broad that "acceptance criteria" amounts to "implement the feature" —
   if a story can't be falsified by a reviewer, it's not specific enough yet.
9. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Stories
### [Story title]
- **Traces to:** PRD requirement [X], Design Doc section [Y]
- **Acceptance criteria:** [specific, testable]
- **Test Strategy layers owned:** [unit/integration/contract/e2e/NFR — which ones]
- **Slice check:** [confirm this cuts end-to-end through its layers, not isolated to one layer]
- **Tag:** [HITL — reason it needs a human decision / AFK — confirmed fully specified]
- **Depends on:** [other story, if any]

[repeat per story]

## Coverage Check
- PRD requirements covered: [list, with story references]
- Design Doc elements covered: [list, with story references]
- Dropped (in design, not in any story): [list, or "none found"]
- Added (in a story, not traceable to design): [list, or "none found"]

## HITL Stories Requiring Decisions Before Implementation
- [story] — decision needed: [specific question for the human]

## Suggested Implementation Order
[sequenced list with dependency notes, noting where HITL stories block AFK stories downstream]

## Self-Verification
**Grounded (evidence-backed):**
- [stories with a clean, direct trace to a specific PRD requirement + Design Doc section]

**Assumed (no hard evidence, but reasonable):**
- [any story where the trace to PRD/Design Doc required some interpretation] — assumption: [what
  was inferred] — risk if wrong: [story scope ends up wrong]

**Could Not Verify / Gaps:**
- [any Coverage Check "Dropped" or "Added" item above — repeat it here so it's not buried in a
  different section] — who can unblock: [architect/PM]

**For the Next Skill (implementation):**
- [which stories are fully AFK-safe vs. which AFK tags are a closer call the implementing skill
  should be more willing to second-guess if something feels underspecified once it's in the code]
```
