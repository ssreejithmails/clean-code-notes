---
name: test-strategy
description: "Use this skill to decide what kinds of tests (unit, integration, contract, e2e) and what NFR verification methods a change needs, BEFORE any implementation code is written. Triggers on phrases like 'what should we test', 'define the test plan', 'test strategy', or whenever a Design Doc is approved/drafted and a test approach hasn't been decided yet. Do NOT use this to write the actual tests (use implementation) or to verify test evidence after the fact (use verification-review)."
---

# Test Strategy

**Requires:** a Design Doc (draft or approved); a System Impact Map; NFR targets (from `nfr-review` or the Design Doc itself); existing test setup/CI config.
**Produces:** a Test Strategy — the test pyramid for this change, NFR-to-verification mapping, explicit out-of-scope notes, standing contract-test gaps.

*(Produces the test plan as its own reviewable artifact — not a checkbox inside the Design Doc — so the shape of testing is a constraint on implementation, not an afterthought discovered at PR time. Run after a Design Doc draft exists and before implementation starts.)*

```
You are defining the test strategy for an approved-pending Design Doc, BEFORE any implementation
code is written. Your job is to decide what kinds of verification this change actually needs and
at what level of the test pyramid — not to write the tests themselves yet.

CONTEXT:
- Design Doc (draft or approved): [paste link/content]
- System Impact Map: [paste link/content]
- NFR targets: [paste, from the Design Doc or a separate NFR review]
- Existing test setup: [point to test directories, CI config, contract test tooling if any]

INSTRUCTIONS:
1. Read the existing test suite structure for every service touched (per the Impact Map) — find
   what test types already exist (unit, integration, contract, e2e, load, security) and how
   they're run, rather than assuming a textbook pyramid applies uniformly.
2. For each service/boundary in the Impact Map, decide the minimum sufficient test pyramid for
   THIS change specifically:
   - Unit: which new logic/branches need isolated coverage (list functions/modules, not "good
     coverage" generically)
   - Integration: which real (or realistically faked) dependencies must be exercised together
     — e.g. service + its actual DB, service + its actual cache
   - Contract: for every cross-service boundary found in the Impact Map, is there a contract test
     verifying the producer and consumer agree on the schema? If none exists today for this
     boundary, flag it as a gap to close, not optional
   - End-to-end: only where a multi-service user-facing flow changes — name the specific flow,
     don't default to "add an e2e test" for everything
   - NFR-specific verification: map each NFR target to a concrete verification method (load test
     for performance/scale targets, security scan for the security target, chaos/failure-
     injection for availability targets) — an NFR with no verification method is a target nobody
     will actually check
3. Be explicit about what's OUT of scope for this change's testing and why (e.g. "not
   re-testing existing checkout flow, unchanged by this work") — this is what keeps the pyramid
   proportional instead of growing unboundedly.
4. Flag any cross-service boundary from the Impact Map that has NO contract test today,
   regardless of whether this change requires one — this is a standing gap worth surfacing
   even if it's not blocking.
5. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Test Pyramid for This Change
| Layer | What's Covered | New or Existing | Owner |
|---|---|---|---|
| Unit | ... | ... | ... |
| Integration | ... | ... | ... |
| Contract | ... | ... | ... |
| E2E | ... | ... | ... |

## NFR Verification Mapping
| NFR Target | Verification Method | New or Existing |
|---|---|---|

## Explicitly Out of Scope
- [area not being tested by this change, and why that's safe]

## Standing Gaps Found (not blocking, but flag)
- [cross-service boundary with no contract test, etc.]

## Recommendation
[Test plan sufficient to proceed to implementation / Gaps must close first — list which]

## Self-Verification
**Grounded (evidence-backed):**
- [pyramid layer/mapping decision] — backed by: [existing test file found at path, or NFR target
  with a clear verification method]

**Assumed (no hard evidence, but reasonable):**
- [pyramid layer/mapping decision] — assumption: [e.g. "no existing load test pattern found, so
  proposing a new one based on similar service X"] — risk if wrong: [test ends up not actually
  validating the target]

**Could Not Verify / Gaps:**
- [any service/boundary whose existing test setup you couldn't find or inspect] — blocked by:
  [reason] — who can unblock: [role/team]

**For the Next Skill (story creation, implementation):**
- [which layers of the plan are solid enough to implement directly vs. which are a best-guess
  that the implementing skill should sanity-check once it's actually in the code]
```
