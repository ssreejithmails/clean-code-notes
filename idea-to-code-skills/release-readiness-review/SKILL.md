---
name: release-readiness-review
description: "Use this skill to compile the audit-ready release record before a change ships — checking the full ticket chain, rollback plan, sign-offs, and cross-team dependency status. Triggers on phrases like 'is this ready to release', 'compile the release record', 'check release readiness', or whenever a change is approaching a release train and needs a final go/no-go check. Do NOT use this for checking individual test evidence (use verification-review) or for post-deploy monitoring (use post-release-validation)."
---

# Release Readiness Review

**Requires:** the full ticket chain (idea/PRD → Design Doc → ADR → PR(s) → test evidence); the risk tier.
**Produces:** an audit-ready release record — traceability check, rollback plan assessment, sign-off status, cross-team dependency risk, release recommendation.

```
You are compiling the audit-ready release record for this change before it ships in the next
release train.

CONTEXT:
- Ticket chain: [Idea/PRD -> Design Doc -> ADR -> PR(s) -> test evidence, all links]
- Risk tier: [paste]

INSTRUCTIONS:
1. Verify every link in the chain actually resolves and is internally consistent (e.g. the PR
   really implements the linked design, the test evidence really matches the linked NFRs —
   cross-check against findings from earlier reviews if available).
2. Confirm a rollback plan exists and is specific (not "we'll roll back if needed" — check for
   an actual mechanism: feature flag, previous version tag, migration down-script).
3. For your highest risk tier: confirm security/compliance sign-off is recorded, not just
   requested.
4. Identify anything in this change that depends on another team's unreleased work or an
   unreleased migration, by checking actual code dependencies (imports, API calls to
   not-yet-deployed endpoints), and cross-check against any dependency-firm-ask outcomes if
   available.
5. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer. This is the record an auditor reads, so be explicit about what you
   independently re-confirmed versus what you're trusting from an earlier skill's output.

OUTPUT FORMAT:
## Traceability Chain Check
[Complete and consistent / Broken at: specify link]

## Rollback Plan
[Specific mechanism found: describe / Missing or vague — flag]

## Sign-offs
- [required approver] — [recorded: yes/no, link]

## Cross-Team Dependency Risks
- [dependency found in code] — [status: deployed/not yet deployed]

## Release Recommendation
[Clear to release / Hold — reasons]

## Self-Verification
**Verified (direct evidence):**
- [chain link or sign-off] — independently confirmed at [link/file:line], not just trusted because
  an earlier skill said so

**Could Not Verify:**
- [any chain link, sign-off, or dependency status you couldn't independently confirm] — blocked
  by: [no access to the sign-off record / can't check another team's deploy status] — who can
  unblock: [role/team]

**Confident But Not Proven:**
- [anything carried forward from an earlier skill's "Assumed" or "Confident But Not Proven" item
  without independently re-checking it] — basis: [trusting the upstream skill's finding] —
  confidence: [Medium/Low]

**For the Next Skill (post-release validation):**
- [anything not fully closed here that post-release monitoring should specifically watch for]
```
