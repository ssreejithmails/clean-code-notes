---
name: post-release-validation
description: "Use this skill to check, shortly after deployment, whether a change is behaving as predicted using actual observability data (metrics, logs, dashboards). Triggers on phrases like 'check how this is doing in prod', 'validate this release', 'is this behaving as expected post-deploy'. Do NOT use this before something has actually been deployed (use release-readiness-review for pre-release checks) or for the longer-term closed-loop retrospective (use retrospective-review)."
---

# Post-Release Validation

**Requires:** what was deployed (PR/release link); predicted NFR targets; access to logs/metrics/dashboards.
**Produces:** actuals-vs-predicted comparison, anomalies traced to specific code, a recommendation.

```
You are checking, shortly after deployment, whether this change is behaving as predicted —
using actual observability data and code, not assumptions.

CONTEXT:
- What was deployed: [PR/release link]
- Predicted NFR targets: [paste]
- Access to: [logs/metrics/dashboards, however available to you]

INSTRUCTIONS:
1. Compare actual post-deploy metrics (latency, error rate, throughput) against the predicted
   targets, where data is available to you.
2. Check error logs/alerts for anything related to the changed code paths (search by service/
   endpoint name, not generic log scanning).
3. If anomalies are found, trace them back to the specific code change using the actual diff,
   rather than describing them generically.
4. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Actuals vs Predicted
| Metric | Target | Actual | Status |
|---|---|---|---|

## Anomalies Found
- [anomaly] — traced to: [file:line / "could not trace with available data"]

## Recommendation
[Healthy, no action / Monitor / Recommend rollback — reasoning]

## Self-Verification
**Verified (direct evidence):**
- [actual metric] — read directly from [dashboard/log source]

**Could Not Verify:**
- [any predicted target with no corresponding live metric/dashboard] — blocked by: [no
  instrumentation exists for this target] — who can unblock: [whoever owns observability for
  this service]

**Confident But Not Proven:**
- [an anomaly attributed to this change without a fully traced causal link] — basis: [timing
  correlation / similar past incident] — confidence: [Medium/Low]

**For the Next Skill (retrospective review):**
- [which targets had real data to check against vs. which were unmeasurable — the retro should
  treat the latter as a process gap worth fixing, not as "nothing went wrong"]
```
