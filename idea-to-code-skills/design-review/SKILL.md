---
name: design-review
description: "Use this skill to review an existing Design Doc against the actual codebase, verifying its claims and checking for unaddressed items from the System Impact Map. Triggers whenever the user asks to 'review this design doc', 'check this RFC', or 'is this design doc accurate', and a Design Doc already exists to review. Acts as a second architect, not a rubber stamp. Do NOT use this to draft a new Design Doc (use design-doc-generation), to check NFR targets specifically (use nfr-review), or to review actual code (use implementation-review)."
---

# Design Review

**Requires:** a Design Doc; the System Impact Map it was drafted against.
**Produces:** a verdict on the design — verified claims, contradicted claims, pattern consistency, impact-map coverage gaps.

```
You are reviewing a Design Doc against the actual codebase, acting as a second architect —
not rubber-stamping, and not nitpicking style.

CONTEXT:
- Design Doc: [paste link or content]
- System Impact Map it was drafted against: [paste link or content]

INSTRUCTIONS:
1. Read the Design Doc fully before touching the codebase.
2. For every claim the doc makes about existing code ("X currently does Y", "we can reuse Z",
   "this won't affect W") — verify it against the actual code. Quote the file/line that confirms
   or contradicts the claim. Do not take the doc's claims at face value.
3. Check whether the proposed approach is consistent with existing patterns in the codebase for
   similar problems (e.g. is there already an established pattern for retries, auth checks,
   pagination, etc. that this design ignores or duplicates?).
4. Check the alternatives-considered section: are there simpler existing mechanisms in the
   codebase that would solve this, which the doc didn't mention?
5. Identify anything in the System Impact Map that the design doesn't actually address.
6. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Verified Claims
- [claim from doc] — confirmed by [file:line]

## Contradicted or Unverifiable Claims
- [claim from doc] — actual code shows [file:line, what it actually does]

## Pattern Consistency
- [existing pattern found elsewhere in codebase] — design [follows it / diverges from it: explain]

## Impact Map Coverage Gaps
- [item from the Impact Map not addressed in design]

## Verdict
[Approve / Approve with changes / Needs rework] — [reasoning]

## Self-Verification
**Verified (direct evidence):**
- [claim from doc] — confirmed at [file:line]

**Could Not Verify:**
- [claim from doc you couldn't confirm or deny] — blocked by: [code not found / ambiguous spec /
  needs the original author's intent] — who can unblock: [architect]

**Confident But Not Proven:**
- [pattern-consistency or coverage-gap finding made by inference rather than a direct quote/
  contradiction] — basis: [what the inference rests on] — confidence: [Medium/Low]

**For the Next Skill (NFR review, data model review, test strategy):**
- [which parts of the design are now confirmed solid vs. which are still provisional pending the
  architect resolving a Contradicted Claim or Coverage Gap above]
```
