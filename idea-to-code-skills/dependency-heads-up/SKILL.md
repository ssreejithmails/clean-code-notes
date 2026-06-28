---
name: dependency-heads-up
description: "Use this skill to draft an early, lightweight heads-up notice to another team whose service or system a change depends on, based on a System Impact Map. Triggers on phrases like 'give Team X a heads up', 'let the other team know early', or whenever a System Impact Map identifies a cross-team dependency and no firm ask has been made yet. This is deliberately NOT a commitment request — it only asks for acknowledgment and an early conflict flag. Do NOT use this once concrete stories exist and a firm date is needed (use dependency-firm-ask instead)."
---

# Dependency Coordination — Early Heads-Up

**Requires:** a System Impact Map listing one or more dependent teams.
**Produces:** an early heads-up notice per dependent team — no commitment requested, just acknowledgment and an early conflict flag.

*(Deliberately lightweight. The goal is "no dependent team is blindsided later," not "get a yes today." Send this as soon as a System Impact Map identifies a dependency, before design locks in scope.)*

```
You are drafting an early heads-up notice to a dependent team, based on a System Impact Map. Your
job is to give them a genuinely useful early signal — specific enough to act on, light enough
that it doesn't read as a demand for an answer they can't give yet.

CONTEXT:
- System Impact Map: [paste link/content — REQUIRED]
- Dependent team(s) identified: [list, from the Impact Map]
- Rough target timeframe (program end date, UAT date, or team date, if known): [paste]

INSTRUCTIONS:
1. For each dependent team in the Impact Map, write a separate notice — don't send one combined
   notice to multiple teams, since each team needs to scan only what's relevant to them.
2. State what was found in the Impact Map that implicates this team, in their own domain
   language where possible (e.g. "this touches the /orders endpoint your team owns," not "this
   touches a service").
3. Give a rough shape of what's likely needed (a new field, a new endpoint, increased throughput,
   a schema change) — but explicitly flag that this is not final, since design hasn't happened
   yet. Don't let the notice read as a commitment request.
4. Give a rough timeframe, anchored to whatever target date is known, but phrase it as "likely
   needed by approximately X" rather than a firm date — the firm date comes later, once concrete
   stories exist (see the firm-ask skill).
5. State explicitly what response you need: not a commitment, just acknowledgment that they've
   seen this and a flag if it obviously conflicts with something already on their roadmap.
6. State the acknowledgment SLA (e.g. 2 business days) and what happens if there's no response by
   then (escalation path) — make this visible to the recipient, not just something tracked
   internally.
7. Run the Self-Verification step below before finalizing.

OUTPUT FORMAT (one block per dependent team):
## Notice to: [Team Name]
**What this touches:** [specific to their domain, with reference to the Impact Map finding]
**Rough shape of the ask (not final):** [what's likely needed]
**Rough timeframe:** [approximate, anchored to known target date]
**What we need from you now:** Acknowledge receipt within [SLA]. Flag now only if this obviously
conflicts with existing commitments — a full answer isn't needed yet, that comes once design is
locked.
**If no response by [SLA deadline]:** Escalates to [requesting tech lead's manager] and
[dependent team's manager].

## Tracking Note
[one line per team: notice sent, SLA deadline, escalation contacts — for the requester to track
against]

## Self-Verification
**Grounded (evidence-backed):**
- [each "what this touches" claim] — backed by: [specific Impact Map line]

**Assumed (no hard evidence, but reasonable):**
- [each "rough shape" or "rough timeframe" claim] — assumption: [what's being assumed] — risk if
  wrong: [team gets a notice that turns out irrelevant or under-scoped]

**Could Not Verify / Gaps:**
- [team where contact info, ownership, or domain context was unclear] — who can unblock: [role]

**For the Next Skill (dependency-firm-ask, once stories exist):**
- [which rough shapes/timeframes are likely to firm up as-is vs. which are likely to change —
  the firm ask should flag any team where the actual need turned out different from this notice]
```
