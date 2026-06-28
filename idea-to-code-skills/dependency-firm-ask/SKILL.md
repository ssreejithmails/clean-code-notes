---
name: dependency-firm-ask
description: "Use this skill to convert an earlier dependency heads-up into a firm, answerable request to another team once concrete stories exist — forcing a Committed/Conditional/Conflict response with a real date. Triggers on phrases like 'send the firm ask to Team X', 'get a real commitment from the other team', or whenever stories exist that depend on another team and only a vague early notice has been sent so far. Do NOT use this for the early, lightweight notice before stories exist (use dependency-heads-up instead)."
---

# Dependency Coordination — Firm Ask

**Requires:** concrete stories (from `story-creation`) that depend on another team; an earlier heads-up notice if one was sent (recommended, not required); a target date.
**Produces:** a firm, answerable request per dependent team, with a forced Committed/Conditional/Conflict response and resulting story-tag implications.

*(Converts an earlier heads-up into a specific request now that stories exist. The dependent team's lead should be able to respond Committed/Conditional/Conflict without needing a meeting just to understand the ask.)*

```
You are converting an earlier dependency heads-up (or, if none was sent, a fresh ask) into a
specific, answerable request, now that stories exist. Your job is to make the ask concrete enough
that the dependent team's lead can give a real answer — not another round of "we'll need
something from you eventually."

CONTEXT:
- Earlier heads-up notice sent to this team, if any: [paste, or "none sent yet"]
- Stories that depend on this team: [paste the specific stories — REQUIRED]
- Target date this dependency needs to land by: [paste — anchor to the program/UAT/team date that
  applies to this feature]
- Dependent team's known sprint/PI cadence, if known: [paste, or "unknown — ask"]

INSTRUCTIONS:
1. For each story that depends on this team, state exactly what's needed — an API contract
   change, a schema change, a feature flag, a capacity allocation, a review/approval — not a
   vague restatement of the story. Cite the specific story.
2. State the target date this is needed by, and work backward to state the latest date the
   dependent team would need to START in order to hit it, if you can estimate their typical lead
   time (ask explicitly if you can't estimate it — don't guess at another team's velocity).
3. State explicitly what happens to the requesting team's timeline if this slips — a specific
   downstream impact (e.g. "X feature misses the program date"), not a generic "this would delay
   things." This is what lets the dependent team's lead actually weigh the request against their
   other priorities.
4. Frame the response as a forced choice, not an open question:
   - **Committed** — confirm the date and what capacity is allocated
   - **Conditional** — possible, but only if [specific thing] happens first — name it
   - **Conflict** — not possible without something else on their plate slipping — name what
   Do not let the ask be answerable with a vague "we'll try."
5. If a response isn't given by an agreed deadline, state the escalation path explicitly in the
   ask itself — don't leave escalation as an unstated assumption.
6. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Firm Ask to: [Team Name]
**Stories depending on you:** [list, with specific story references]
**Exactly what's needed:** [specific artifact/capacity/decision, per story]
**Needed by:** [date] — **your estimated start-by date:** [date, or "please confirm your lead
time, we couldn't estimate it"]
**Impact if this slips:** [specific downstream consequence]
**Please respond with one of:**
- Committed — [date], [capacity allocated]
- Conditional — possible if [X] happens first
- Conflict — not possible without [Y] slipping

**If no response by [deadline]:** Escalates to [requesting tech lead's manager] and [dependent
team's manager].

## Self-Verification
**Grounded (evidence-backed):**
- [each "exactly what's needed" item] — backed by: [the specific story it traces to]

**Assumed (no hard evidence, but reasonable):**
- [the estimated start-by date, if you had to infer the dependent team's lead time] —
  assumption: [basis for the estimate] — risk if wrong: [ask undersells/oversells urgency]

**Could Not Verify / Gaps:**
- [anything you couldn't pin down — e.g. dependent team's actual cadence] — who can unblock:
  [the dependent team's lead, directly]

**For the Next Skill (once a response is received, feeding into Outcome Tracking below):**
- [what should make you re-check the request once a response comes back — e.g. if Conditional,
  confirm the named precondition is actually realistic before treating it as a soft commit]

## Outcome Tracking (fill in once response received)
- Response: [Committed/Conditional/Conflict]
- If Conditional: [the named precondition, and who owns resolving it]
- If Conflict: [escalation status]
- Resulting story tag: [if Committed, story keeps its original HITL/AFK tag; if Conditional or
  Conflict, the story is force-tagged HITL regardless of its original tag, since "wait on another
  team" is exactly the kind of mid-stream decision AFK excludes]
```
