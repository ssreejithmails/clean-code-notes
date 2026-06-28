---
name: verification-review
description: "Use this skill to check whether test evidence attached to a ticket actually proves the NFR targets were met — not just that tests exist and passed. Triggers on phrases like 'check the test evidence', 'did we actually prove this meets the NFRs', or 'verify the test results before release'. Do NOT use this to define what should be tested in the first place (use test-strategy) or to compile the full release record (use release-readiness-review)."
---

# Verification & NFR Validation Review

**Requires:** Design Doc NFR targets; a Test Strategy; test evidence attached to the ticket; the relevant test code.
**Produces:** a verdict on whether test evidence actually proves the NFR targets were met.

```
You are checking whether the test evidence attached to this ticket actually proves the NFR
targets were met — not just that tests exist and passed.

CONTEXT:
- Design Doc NFR targets: [paste]
- Test Strategy: [paste link/content]
- Test evidence attached: [paste test results, load test output, scan reports]
- Relevant test code: [point to test files in repo]

INSTRUCTIONS:
1. Read the actual test code (not just the results summary) for each NFR target — confirm the
   test genuinely exercises the scenario claimed (e.g. a "load test" that only sends 10 requests
   doesn't validate a 10,000 req/s target).
2. Compare each test's measured result against the stated target. Flag any target with no
   corresponding test at all.
3. Check the implementation's tests against the Test Strategy layer by layer — confirm every
   layer the plan called for (unit/integration/contract/e2e) actually has corresponding tests in
   the diff, not just the easiest layer to write.
4. For security: check whether the scan tool/config used actually covers the area changed (e.g.
   a SAST scan on the whole repo doesn't substitute for a targeted check on a new auth path).
5. For schema changes: confirm migration was actually tested against a realistic data volume/
   shape, not just an empty test database.
6. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Test Pyramid Coverage Check
| Layer (per Test Strategy) | Tests Found in Diff? | Sufficient? |
|---|---|---|

## NFR Target vs Evidence
| NFR Target | Evidence Provided | Test Actually Validates Target? | Gap |
|---|---|---|---|

## Untested Targets
- [target with no real evidence]

## Recommendation
[Sufficient evidence to proceed / Gaps must be closed before release] — [list blocking gaps]

## Self-Verification
**Verified (direct evidence):**
- [NFR target with a real, traced test] — confirmed by reading [test file:line]

**Could Not Verify:**
- [evidence you couldn't fully assess — e.g. a load test report with no accessible raw data, or
  a scan tool you don't have config visibility into] — blocked by: [reason] — who can unblock:
  [QA/security team]

**Confident But Not Proven:**
- [a "Sufficient"/"Gap" judgment made from the evidence summary rather than the raw test run] —
  basis: [why you lean this way] — confidence: [Medium/Low]

**For the Next Skill (release readiness review):**
- [which targets are genuinely proven vs. which are still a judgment call that release readiness
  should treat as a real risk, not a closed item]
```
