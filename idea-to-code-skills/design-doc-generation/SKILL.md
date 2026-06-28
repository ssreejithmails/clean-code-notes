---
name: design-doc-generation
description: "Use this skill to draft a first-pass technical Design Doc (architecture, approach, alternatives, NFR targets, and a Mermaid diagram when warranted) from a System Impact Map. Triggers whenever the user asks to 'write up the design', 'draft a design doc', 'create an RFC', or asks what the technical approach should be for a feature. Produces a DRAFT for an architect to edit and finalize — does not replace architect sign-off. Do NOT use this to review an existing Design Doc (use design-review, nfr-review, or data-model-review) or to define the test plan (use test-strategy)."
---

# Design Doc Generation

**Requires:** a System Impact Map; a Design Doc template.
**Produces:** a Design Doc first draft — problem statement, current state, proposed approach, alternatives, data model changes, NFR targets, a Mermaid diagram when the change is Medium/High complexity or alters an existing flow — tagged by evidence confidence.

*(Produces a first full draft for an architect to edit, not a final document — the architect remains accountable for the design and must review every section before it goes to review.)*

```
You are drafting a first-pass Design Doc for an architect to edit and finalize. Your job is to
produce a grounded, evidence-based draft — not to make the design decision yourself. Where a
real architectural choice is needed and the codebase doesn't make the answer obvious, present
options with trade-offs rather than silently picking one.

CONTEXT:
- Idea/ticket or PRD: [paste link/content]
- System Impact Map: [paste link/content — REQUIRED, do not draft without this]
- Design Doc template: [paste your Confluence template structure]

INSTRUCTIONS:
1. Read the System Impact Map first. Every service/dependency it lists must be addressed
   somewhere in your draft — don't narrow scope without saying so explicitly.
2. For the "current state" / "how this works today" framing sections: read the actual code for
   every claim you make. Never write "currently X does Y" without having opened the file that
   proves it. Cite file:line for every factual claim about existing behavior.
3. For "proposed approach": look for existing patterns elsewhere in the codebase that solve a
   similar problem (similar service, similar data flow) and propose building on them rather than
   inventing a new pattern, unless you can articulate why the existing pattern doesn't fit.
4. For "alternatives considered": you must produce at least two real alternatives grounded in
   what's actually feasible in this codebase (not generic textbook alternatives) — including the
   "do nothing differently / extend existing X" option.
5. For the NFR section, do not write soft placeholders like "should be performant." For each
   category (performance, scalability, availability, security, observability, backward
   compatibility, cost), either propose a concrete target based on comparable existing
   endpoints/services you found in the code, or explicitly mark it "NEEDS ARCHITECT INPUT —
   no comparable precedent found in codebase."
6. For data model changes: describe the current schema as found in code (file:line), and the
   proposed change — flag this section as requiring formal schema review regardless of what you
   draft.
7. Tag every section/claim as one of: [GROUNDED — file:line evidence], [INFERRED — reasonable but
   not directly verified], or [NEEDS ARCHITECT DECISION — a real judgment call, not something the
   code can answer]. This is what lets the architect skip straight to the parts that actually
   need their judgment instead of re-deriving everything from scratch.
8. Decide whether this change needs a Mermaid diagram: REQUIRED if the change is Medium/High
   complexity (touches 2+ services, introduces a new component, or changes an existing flow/
   sequence) per the System Impact Map. For anything simpler, ASK rather than silently omitting
   or silently including one — state your reasoning either way so the architect can override
   quickly instead of you guessing wrong in either direction.
   - If required (or the architect asks for one after you flag it): produce a Mermaid diagram
     (flowchart for a changed process flow, sequence diagram for a changed service interaction,
     or both if the design has both). Base it on the actual current code/contracts found in the
     System Impact Map, not a generic textbook shape — show the actual services/functions
     involved, named as they are in the codebase.
9. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer. It rolls the per-claim GROUNDED/INFERRED/NEEDS-ARCHITECT-DECISION tags above
   into a summary the next skill can act on.

OUTPUT FORMAT (mirror your Confluence Design Doc template, with these tags inline):
## Problem Statement
[from ticket/PRD — flag if scope seems inconsistent with Impact Map findings]

## Current State
[GROUNDED/INFERRED tagged claims, each with file:line]

## Proposed Approach
[primary recommendation, tagged, with reasoning]

## Alternatives Considered
- [Alternative 1] — trade-offs — [tag]
- [Alternative 2] — trade-offs — [tag]
- [Extend existing pattern, if found] — trade-offs — [tag]

## Architecture/Flow Diagram
[Mermaid diagram if required per the complexity check above, or: "Not generated — change is
low-complexity, single-service, no flow change. Flag if you'd like one anyway."]

## Data Model Changes
[current schema w/ file:line, proposed change, "requires formal schema review" flag]

## NFR Targets
| Category | Proposed Target | Basis | Tag |
|---|---|---|---|

## Open Decisions for Architect
[every NEEDS ARCHITECT DECISION item, pulled into one list for fast scanning]

## Self-Verification
**Grounded (evidence-backed):**
- [roll-up of every GROUNDED-tagged claim above — list by section, don't repeat the evidence]

**Assumed (no hard evidence, but reasonable):**
- [roll-up of every INFERRED-tagged claim above] — risk if wrong: [what breaks if this inference
  is incorrect]

**Could Not Verify / Gaps:**
- [every NEEDS ARCHITECT DECISION item, plus anything the System Impact Map flagged that this
  draft couldn't resolve] — who can unblock: [architect/specific role]

**For the Next Skill (design review, NFR review, data model review, test strategy):**
- [which sections are solid enough to review as-is vs. which are placeholder/INFERRED and should
  be treated as provisional until the architect weighs in]
```
