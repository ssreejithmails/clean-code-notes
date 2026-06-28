# Idea-to-Code Workflow

A standard process for turning an idea into production code — covering system impact, cross-team dependencies, data models, NFRs, and a real audit trail — designed for teams shipping on a shared platform where AI does a lot of the drafting and a human stays accountable for every gate.

This README is for humans. If you're looking for the actual AI prompts, they live in `.claude/skills/` (or wherever you've installed `idea-to-code-skills.zip`) — each one is a separate `SKILL.md` file.

---

## Why this exists

Most "idea to code" processes break in one of these ways:
- A feature ships and breaks something nobody realized it touched.
- What got built doesn't match what was actually asked for.
- Performance, security, or scaling problems get discovered in production instead of in design.
- Nobody can reconstruct, six months later, why a decision was made — which is a real problem if you're in a regulated industry and someone's asking.
- The process is so heavy that small changes take forever, so people start skipping it.

This workflow is built around one core idea: **not every change deserves the same amount of process.** A typo fix and a new payment flow that touches three teams' services should not go through the same number of approvals. So the first real decision in this workflow is always: *how risky is this, really?*

## How it's organized

There are three layers, and it helps to know which one you're looking at:

| Layer | What it is | Where it lives |
|---|---|---|
| **The Workflow** | The actual process — stages, who's accountable, what gets escalated and when | `idea-to-code-workflow.md` |
| **AI Skills** | The actual prompts that do the work at each stage (drafting docs, reviewing code, writing the implementation) | `.claude/skills/*/SKILL.md` (19 separate skills) |
| **PI Planning** | A separate tool for planning a whole quarter's worth of features across multiple teams against a fixed deadline | `pi-planning-skill.md` |

You mostly won't read the AI Skills directly — they get triggered automatically when you ask Claude to do something like "draft a design doc for this" or "review this PR." This README and the Workflow doc are what you read to understand what's *supposed* to happen at each step.

---

## The risk tiers (read this first)

Every idea gets a tier. The tier decides how much process applies. This is the single most important concept in the whole workflow — it's what keeps small changes fast while keeping risky changes safe.

| | **Tier 1 — Low** | **Tier 2 — Standard** | **Tier 3 — High** |
|---|---|---|---|
| Examples | Bug fix, copy change | New feature in one service | New service, schema change, cross-service flow |
| Data | None / non-sensitive | Internal business data | PII / PHI / financial data |
| Process | Light — basically just a PR | Full process, async approvals | Full process, live review + security sign-off |

**Automatic Tier 3, no matter how small it feels:** anything touching the data model, sensitive data, authentication, infrastructure, or crossing a service boundary with a breaking change.

If you're not sure what tier something is, the first real stage of the workflow (`system-impact-analysis`) will tell you — it looks at your code and recommends a tier based on what it actually finds, not just what you think it touches.

---

## Walking through the stages

Here's the path a Tier 2/3 idea actually takes, start to finish. (Tier 1 skips almost all of this — see the [Fast Path](#tier-1-fast-path) below.)

### 1. Requirement Vetting
*"Has this already been built, tried, or rejected for a reason that still applies?"*

Before anyone writes a requirements doc, this checks the codebase for existing functionality, past tickets for prior attempts, and architecture docs for anything this idea would conflict with. This is the cheapest place to catch a bad idea — way cheaper than catching it after a design doc and a sprint of work already exist.

### 2. PRD (Product Requirements Document)
*"What problem are we actually solving, and what does 'done' look like?"*

A first draft gets written from the raw idea (informed by the vetting step above), with explicit acceptance criteria and — just as important — explicit **non-goals**: things this deliberately does *not* cover. A Product Manager edits and finalizes it. This is what stops "wait, I thought we were also doing X" conversations three weeks into a project.

### 3. System Impact Analysis
*"What does this actually touch?"*

This is the stage that prevents the "we didn't realize this would break Team X's service" problem. It searches the actual codebase — not just the obvious area, but everything connected to it — and produces a map of every service, data table, and external dependency involved. It also recommends the risk tier.

### 4. Dependency Coordination (if other teams are involved)
*"Does another team need to know about this, and what do we actually need from them?"*

This happens in two stages on purpose:
- **Early heads-up** — sent as soon as a dependency is found, before anything is designed. It's deliberately vague ("this will probably need something from your service, roughly by this timeframe") because asking for a firm commitment this early just produces a guess dressed up as a promise.
- **Firm ask** — sent once real stories exist. This forces an actual answer: **Committed** (here's the date), **Conditional** (possible if X happens first), or **Conflict** (not possible without something else slipping). No more "we'll try."

### 5. Design Doc
*"How are we going to build this, and what are we explicitly choosing not to do?"*

A first draft gets written — current state, proposed approach, real alternatives considered, and a section for Non-Functional Requirements (performance, security, scalability, etc. — see below). It's tagged so a human reviewer can tell at a glance what's solid evidence versus what's a guess versus what needs an actual decision from them. For anything complex enough to need one, this also includes a diagram of the system/flow.

Three things review this draft from different angles before it's approved:
- **Design review** — does this match how the codebase actually works, and is the approach sound?
- **NFR review** — are the performance/security/scalability targets realistic given what the system can actually do today?
- **Data model review** — if this touches the database, who else reads or writes that data, and will they break?

### 6. Test Strategy
*"What actually needs to be tested, and at what level?"*

Decided *before* any code is written — not as an afterthought during the PR. This explicitly maps out unit tests, integration tests, and — critically for a multi-service platform — **contract tests** for every place this change crosses a service boundary, since that's the thing that's easiest to skip and most likely to cause a cross-team incident.

### 7. Story Creation
*"What are the actual units of work, and which ones need a human decision first?"*

The design gets broken into stories. Each story is a complete vertical slice (touches every layer it needs to, end to end) rather than "build all the API endpoints" as one story and "build all the database queries" as another — the latter can look done while nothing actually works yet.

Every story gets tagged:
- **AFK** ("Away From Keyboard") — fully specified, an AI agent can implement it without anyone needing to weigh in.
- **HITL** ("Human In The Loop") — needs a real decision from a person before it can be implemented safely.

This tagging is what lets AI move fast on the bulk of the work while still pulling a human in exactly where it matters — not everywhere, and not nowhere.

### 8. Implementation
*"Write the code."*

An AI agent implements one story at a time, against its acceptance criteria, building exactly the tests the Test Strategy called for. If it hits something genuinely ambiguous, it's instructed to stop and flag it rather than guess. Three separate reviews then check the result — **never the same AI session that wrote the code**, since it shares the same blind spots:
- **Implementation review** — does the code actually match what was designed?
- **Security review** — does the code introduce a vulnerability, regardless of whether the design was secure?
- **Infra review** — if this touches infrastructure (cloud config, permissions, etc.), is it safe?

**A human always has to approve the merge. No exception, regardless of tier.** AI writing the code doesn't mean AI approves it — there's a named person accountable for every merge, the same as if a junior engineer had opened the PR.

### 9. Verification
*"Does the evidence actually prove this works, or does it just look like it does?"*

Checks whether the attached test results genuinely prove the performance/security targets were met — e.g., a "load test" that only sent 10 requests doesn't prove a 10,000-requests-per-second target.

### 10. Release Readiness
*"Are we actually ready to ship this?"*

Compiles the full record — idea → design → approval → tests → sign-offs, all linked — and checks there's a real rollback plan, not just "we'll figure it out if something breaks." This record is also what makes audits painless later, since it's a byproduct of doing the work normally rather than a scramble before an audit.

### 11. Post-Release Validation
*"Is it actually behaving the way we expected, now that it's live?"*

Checked shortly after deployment, using real production data — not assumptions.

### 12. Retrospective
*"Did our predictions match reality, and what should we fix about the process itself?"*

Compares what was predicted to touch (back in step 3) against what actually shipped. If something was missed, this is where the process itself gets improved — not just "we'll be more careful next time," but a specific, concrete change to a template or a rule.

---

## Tier 1 Fast Path

For low-risk changes, almost everything above is skipped on purpose: a quick search to make sure nothing similar already exists, a ticket with a clear problem statement and acceptance criteria (no full PRD), straight to a PR, standard tests, merge. No design doc, no impact map, no extra approvals.

**The one safety net:** if anyone — human or AI — realizes mid-change that it actually touches the data model, crosses a service boundary, or involves sensitive data, it automatically gets bumped to Tier 2. That's treated as a process bug to fix going forward, not an exception to quietly wave through.

## Emergency Changes

Production incidents don't wait for the next scheduled release. There's a separate, narrow path for genuine emergencies: an on-call lead can approve a fix in the moment, but it never skips the human merge gate or a security check for anything touching auth/data. Within 48 hours, the change gets the same paperwork as planned work would have gotten — filed after the fact instead of before, so the audit trail stays complete and nothing falls through a "it was urgent" gap.

---

## What makes this different from "just write good docs"

A few specific things are designed in on purpose, because they're the parts that usually fail quietly:

- **Self-Verification.** Every AI skill ends by explicitly stating what it directly confirmed with evidence, what it couldn't check and why, and what it's saying with some confidence but no hard proof. This is what lets the next step (and you) know exactly what to trust as-is versus double-check — instead of everything reading as equally certain when it isn't.
- **A human merge gate, always.** AI can draft, review, and implement — but a named person approves every merge, at every risk tier, no exceptions. If you ever hear "the AI approved it," that's a process violation, not a feature.
- **Vertical slices, not horizontal ones.** Stories are built so that each one actually works end-to-end on its own, rather than splitting work by technical layer in a way that can look 80% done while nothing actually functions yet.
- **Two-touch dependency coordination.** Other teams get an early, low-pressure heads-up before being asked for a real commitment — so nobody gets blindsided by a firm date out of nowhere, and nobody gets asked to commit to something that isn't real yet.

---

## Quick reference: which skill do I use?

| If you need to... | Use this skill |
|---|---|
| Check if an idea has already been tried/built | `requirement-vetting` |
| Write up requirements for a new feature | `prd-generation` |
| Figure out what a change will actually touch | `system-impact-analysis` |
| Give another team an early heads-up | `dependency-heads-up` |
| Get a firm date/commitment from another team | `dependency-firm-ask` |
| Draft a technical design | `design-doc-generation` |
| Review someone's design doc | `design-review`, `nfr-review`, or `data-model-review` (pick based on what you're checking) |
| Decide what to test | `test-strategy` |
| Break a design into tickets | `story-creation` |
| Actually write the code for a ticket | `implementation` |
| Review a pull request | `implementation-review`, `security-review`, or `infra-review` |
| Check if test evidence is actually solid | `verification-review` |
| Check if something's ready to ship | `release-readiness-review` |
| Check how something's doing in production | `post-release-validation` |
| Do a retro on a shipped change | `retrospective-review` |
| Plan a whole quarter across multiple teams | see `pi-planning-skill.md` (separate tool) |

If you're not sure which review skill fits, each `SKILL.md` file has a one-line description at the top that says exactly what it's for and what it's *not* for — that's the fastest way to check.

---

## A note on what this isn't

This is a starting framework, not a finished policy. It's meant to be piloted with one team on a couple of real features before being rolled out everywhere, and the tier thresholds, templates, and gates are all things you should expect to adjust once you see how it actually behaves with real work. If something in here feels like it's adding ceremony without adding safety, that's worth raising — the whole point of risk tiering is that rigor should be proportional, and if it isn't in practice, the tiering rules are what should change.
