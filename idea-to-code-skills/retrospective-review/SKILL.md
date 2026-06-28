---
name: retrospective-review
description: "Use this skill to do a closed-loop retrospective comparing what a System Impact Map predicted against what actually shipped and happened in production, producing a concrete process-improvement signal. Triggers on phrases like 'do a retro on this change', 'what did we miss', or 'how did our impact analysis hold up'. This explicitly feeds back into system-impact-analysis for future work — always name a specific, concrete process change, not generic advice. Do NOT use this for immediate post-deploy health checks (use post-release-validation)."
---

# Retrospective Review

**Requires:** a System Impact Map; the actual PR diff(s) that shipped; any incident/anomaly reports.
**Produces:** a predicted-vs-actual comparison and a concrete process-improvement signal.

```
You are doing a closed-loop retrospective on this change: did the original impact analysis
actually match what happened in the codebase and in production?

CONTEXT:
- System Impact Map: [link/content]
- Actual PR diff(s) that shipped: [link]
- Any incident/anomaly reports: [link, if any]

INSTRUCTIONS:
1. Compare the services/modules predicted as "touched" in the Impact Map against what the actual
   merged diff(s) touched (search the real diff, don't just trust the PR description).
2. Identify anything touched in reality that was NOT predicted — this is the specific signal
   that should improve the Impact Map template or the auto-escalation tiering rules.
3. If there was a production anomaly, check whether it traces to something missed during impact
   analysis or design review, and name which review should have caught it.
4. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Predicted vs Actual Impact
- Correctly predicted: [list]
- Missed (touched in reality, not predicted): [list, with file:line evidence]

## Process Improvement Signal
[Specific, concrete suggestion for the Impact Map template, tiering rule, or a specific skill
above that would have caught the miss — not a generic "be more thorough"]

## Closure
[Retro complete — link this output back into the System Impact Map template repository]

## Self-Verification
**Verified (direct evidence):**
- [predicted-vs-actual comparison] — confirmed by diffing [the actual merged PR(s)]

**Could Not Verify:**
- [anything you couldn't fully trace — e.g. an incident report with no clear root-cause link to
  this change] — blocked by: [reason] — who can unblock: [incident owner]

**Confident But Not Proven:**
- [a process-improvement suggestion based on this single instance, not a confirmed pattern] —
  basis: [this one miss] — confidence: [Medium/Low — note explicitly if this needs more than one
  occurrence before it's worth changing the template/tiering rule]

**For the Next Skill (system-impact-analysis, on the next feature):**
- [the specific, concrete change to make to that skill's instructions or the Impact Map template
  — this is the actual feedback loop; without a specific change named here, this retro doesn't
  close the loop, it just documents the miss]
```
