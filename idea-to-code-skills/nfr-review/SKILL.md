---
name: nfr-review
description: "Use this skill to review the Non-Functional Requirements (performance, scalability, availability, security, observability, backward compatibility) section of a Design Doc against what the codebase can actually deliver today. Triggers whenever the user asks to 'check the NFRs', 'review performance/scalability targets', or 'are these NFRs realistic'. Do NOT use this for general design/architecture review (use design-review), for checking schema changes specifically (use data-model-review), or for verifying NFR targets were actually met after implementation (use verification-review)."
---

# NFR Review

**Requires:** a Design Doc's NFR section; the relevant service(s) from a System Impact Map.
**Produces:** an NFR coverage table against actual codebase baselines, missing NFRs, blocking issues.

```
You are reviewing the Non-Functional Requirements section of a Design Doc against what the
codebase can actually deliver today — not against the doc's own assertions.

CONTEXT:
- Design Doc NFR section: [paste]
- Relevant service(s): [from the System Impact Map]

INSTRUCTIONS:
1. For each NFR category below, check the codebase for current actuals, then compare to the
   doc's stated target. If the doc gives no target for a category, flag it as missing — don't fill
   it in yourself.
   - Performance/latency: find existing benchmarks, load test configs, or APM dashboards-as-code
     if present. What's the current baseline for similar endpoints?
   - Scalability: check current resource limits/autoscaling config (k8s manifests, Terraform,
     etc.) — does the stated expected load fit within them?
   - Availability: check existing SLOs/error budgets in repo config if defined.
   - Security/data classification: check how similar sensitive-data flows are currently handled
     (encryption at rest/in transit, access control middleware) and whether this design matches
     that bar.
   - Observability: check whether the design's proposed metrics/logs follow existing
     instrumentation patterns (same libraries, naming conventions, dashboards-as-code).
   - Backward compatibility: if this changes a contract, check actual current consumers
     (found in the Impact Map) for what would break.
2. Do not accept "should be fine" or "no impact" as a target — every NFR needs a number or an
   explicit, code-grounded justification for why it doesn't apply.
3. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## NFR Coverage Table
| Category | Doc's Stated Target | Current Codebase Baseline | Gap/Risk |
|---|---|---|---|
| Performance | ... | ... | ... |
| Scalability | ... | ... | ... |
| Availability | ... | ... | ... |
| Security | ... | ... | ... |
| Observability | ... | ... | ... |
| Backward compat | ... | ... | ... |

## Missing NFRs
- [category with no target stated in doc]

## Blocking Issues
- [anything that should stop this from moving to implementation as-is]

## Self-Verification
**Verified (direct evidence):**
- [category] — baseline confirmed at [file:line/config]

**Could Not Verify:**
- [category with no discoverable baseline] — blocked by: [no benchmarks/SLOs/dashboards exist for
  this] — who can unblock: [whoever owns the missing instrumentation]

**Confident But Not Proven:**
- [a Gap/Risk judgment made without a hard current-state number] — basis: [comparable service/
  pattern] — confidence: [Medium/Low]

**For the Next Skill (test strategy):**
- [which NFR targets have a real baseline to verify against vs. which have no baseline at all —
  the latter need a verification method invented from scratch, not assumed achievable]
```
