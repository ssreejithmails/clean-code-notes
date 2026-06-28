---
name: implementation-review
description: "Use this skill to review a pull request against its linked Design Doc and ADR for design conformance and correctness — not security or infrastructure specifically. Triggers on phrases like 'review this PR', 'check this diff against the design', or 'does this implement what was approved'. Do NOT use this for a security-focused review (use security-review), for infrastructure-as-code changes (use infra-review), or for checking NFR test evidence (use verification-review)."
---

# Implementation Review (PR-time)

**Requires:** a PR diff; the Design Doc and ADR(s) it implements.
**Produces:** a design-conformance and correctness verdict — verified, deviated, missed call sites, blocking/non-blocking findings.

```
You are reviewing a pull request against its linked Design Doc and ADR — checking whether the
code actually implements what was approved, not just whether the code looks clean.

CONTEXT:
- PR diff: [paste or point to branch]
- Linked Design Doc: [link/content]
- Linked ADR(s): [link/content]

INSTRUCTIONS:
1. Read the Design Doc and ADR(s) first to know what was actually approved.
2. Go through the diff and map each significant change to a part of the design. Flag anything
   in the code that isn't traceable to the design (scope creep) and anything in the design that
   isn't in the code (incomplete implementation).
3. Check the NFR checklist items from the design (metrics added, migration tested, etc.)
   against actual code — e.g. if the design said "add latency metric on endpoint X," grep for
   the actual instrumentation call in the diff, don't take a checked checkbox as proof.
4. Check for usages of this changed code elsewhere in the codebase that the PR author may have
   missed (search for all callers/importers of changed functions/classes).
5. Standard correctness review: logic errors, unhandled edge cases, error handling, test
   coverage for the new behavior — but ground every finding in the actual diff and surrounding
   code, not generic advice.
6. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Design Conformance
- Implemented as designed: [list]
- Deviations from design: [list, with file:line — flag if undocumented]

## NFR Checklist Verification
- [NFR item] — [verified in code at file:line / NOT found in diff]

## Missed Call Sites
- [function/class changed] — also used in [file:line] — checked: [yes, compatible / yes, BREAKS / not checked]

## Correctness Findings
- 🔴 [blocking issue] — [file:line] — [why]
- 🟡 [non-blocking concern] — [file:line] — [why]

## Verdict
[Ready to merge / Changes requested] — [summary]

## Self-Verification
**Verified (direct evidence):**
- [conformance/correctness finding] — confirmed at [file:line]

**Could Not Verify:**
- [a call site or usage you couldn't fully trace — e.g. dynamic dispatch, reflection, a consumer
  in a repo you don't have access to] — blocked by: [reason] — who can unblock: [role/team]

**Confident But Not Proven:**
- [a correctness concern raised without a concrete failing scenario] — basis: [pattern/heuristic]
  — confidence: [Medium/Low]

**For the Next Skill (security review, infra review, verification review):**
- [anything this review didn't have time/access to fully chase down that those reviews should be
  aware of]
```
