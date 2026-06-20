<!--
USAGE: This is a prompt template for Claude Code, run from the root of ONE service's
repo on the target machine. Run it once per service. Fill in the bracketed placeholders
before running (especially [SERVICE_NAME] and the file paths under "Templates to use").

Why one prompt for the whole set instead of 13 separate prompts: Claude Code needs the
FULL picture of the codebase to make the early docs accurate (e.g. it can't write a
correct architecture overview without first understanding the package structure that
also informs CLAUDE.md). Asking for all of them in one pass, in a defined order, lets
later docs reference earlier ones instead of contradicting them.

Recommended invocation: paste this whole prompt as your first message to Claude Code in
the service's repo root. If the repo is large, see the "If the codebase is large" note
near the bottom before running.
-->

# Prompt: Generate AI-context documentation from existing codebase

I'm setting up documentation so AI assistants (including you, in future sessions) can
work efficiently and safely in this codebase. This service has not been documented this
way before, and the existing code does not necessarily follow current best practices —
**do not assume existing patterns are the standard to describe approvingly.** Your job
right now is to accurately describe what exists, and separately flag what looks like an
anti-pattern, not to silently endorse or silently fix anything.

## Ground rules for this task

1. **Read before writing.** Explore the actual repo structure, build files, source code,
   tests, and any existing docs/READMEs before drafting anything. Don't infer from the
   service name or folder names alone.
2. **Distinguish observed fact from inference.** When describing what a class/endpoint/
   service does, base it on what the code actually does. If you're inferring intent
   (e.g. *why* a design choice was made) rather than just describing behavior, say so
   explicitly — e.g. "Inferred intent — not stated anywhere in code: ..." Do not present
   a guess as settled fact.
3. **If something is ambiguous or you're not confident, say so in the doc itself**, in a
   visible `[NEEDS HUMAN INPUT: ...]` marker, rather than picking a plausible-sounding
   answer and moving on. I would much rather see "I couldn't determine X" than a
   confident guess that turns out wrong.
4. **Flag anti-patterns separately from documenting them as fact.** If you notice
   something that looks like a bad practice (see list below), note it in the relevant doc
   under a clearly marked "Known issues / anti-patterns observed" section — do not refactor
   or fix anything in this pass. This is a documentation task, not a refactor task.
5. **Do not invent details you can't verify.** No made-up team names, no guessed Slack
   channels, no assumed SLAs — leave these as `[FILL IN: ...]` placeholders if not
   discoverable from the repo itself (e.g. from a CODEOWNERS file, README, or CI config).

## What to produce

Generate the following files, **in this order** (each builds on the last):

1. `docs/codebase-audit.md` — a working document, not a polished doc. Capture:
   - High-level structure: packages, layers, key classes per layer
   - Tech stack actually in use (read build.gradle/pom.xml, don't assume)
   - Testing setup actually in use (frameworks, in-memory vs. real DB, coverage if measurable)
   - API endpoints that exist (scan controllers)
   - Async/messaging usage if any (scan for Kafka/RabbitMQ/etc. config and producers/consumers)
   - **Anti-patterns observed**, each with a file/line reference and which rule it violates
     (use the categories below as a checklist, but call out anything else notable too):
     - Tests that only verify mock calls (`verify(x).method(any())`) with no real assertions
     - Tests covering only happy paths, no edge cases
     - Business logic in controllers instead of a service/domain layer
     - Field injection instead of constructor injection
     - Entities returned directly from APIs instead of DTOs
     - Magic numbers/strings without named constants
     - Missing null/empty/error handling on inputs that need it
     - God classes/methods doing too many unrelated things
     - Any synchronous call to another service with no apparent fallback/circuit breaker
   - This file is for my review — flag uncertainty heavily here, I'll clarify before you
     proceed to the next step. **Stop after this file and summarize what you found before
     continuing to step 2**, so I can correct any misunderstanding early.

2. `CLAUDE.md` (repo root) — using the structure in the reference template below.
   Fill in Purpose, Tech Stack, Build/Run/Test commands (verify these actually work if
   possible — try running them), Package Structure (from what you observed), Coding
   Conventions (describe what's actually used, even if inconsistent — note inconsistency
   explicitly rather than picking one as "the" convention), and Things to Know /
   Common Pitfalls (pull from your anti-pattern findings — phrase these as "watch out for"
   notes for someone new, not as instructions to fix them yet).
   Include the "Legacy code notice" section near the top, since this codebase predates
   documented standards.

3. `README.md` (repo root) — service responsibility, API contract summary, data model,
   upstream/downstream dependencies (trace actual code/config, not assumptions), events
   produced/consumed if applicable, configuration (from actual application.yml/.properties
   files), local dev setup (verify the steps work).

4. `docs/openapi.yaml` — generate from the actual controllers/endpoints you found. If the
   framework has annotations that can auto-generate this (e.g. springdoc-openapi is
   already a dependency), tell me that instead of hand-writing a possibly-incomplete spec.

5. `docs/events.md` — only if you found actual messaging/event code. Document topics,
   event schemas (from actual producer/consumer code, not assumed), and note any schema
   evolution practices you can observe (or their absence).

6. `docs/deployment.md` — from actual Dockerfile, CI config, k8s/helm manifests if present
   in this repo. If deployment config lives in a separate repo, say so and leave this file
   minimal rather than guessing.

7. `docs/testing-standards.md` — **this one is prescriptive, not descriptive.** Use the
   reference template's rules as the target standard (Arrange-Act-Assert, one behavior per
   test, edge case coverage, no over-mocking, etc.) — don't water it down to match what you
   found in step 1. Add a short "Current state" note at the top summarizing how far the
   existing test suite is from this standard, referencing specific findings from
   `codebase-audit.md`.

8. `docs/refactoring-protocol.md` — use the reference template as-is, adjusted only for
   service-specific details (e.g. actual ADR folder location if different).

**Do not generate:** `docs/architecture-overview.md` (org-wide, not per-service — I'll
handle that separately once multiple services are documented), ADRs (I'll write these
myself since they require knowing actual historical reasoning, not something inferable
from code), design docs (forward-looking, not applicable here), or the PR checklist /
training guide (org-wide artifacts, not per-service).

## Reference templates
Use these as structural references for tone, sections, and level of detail — adapt
content to what you actually find, don't copy placeholder text verbatim:
[Point to your local copies, e.g.: `/path/to/doc-templates/01-CLAUDE.md`,
`03-service-README.md`, `04-openapi-skeleton.yaml`, `05-event-schemas.md`,
`06-deployment-infra.md`, `10-testing-standards.md`, `12-refactoring-protocol.md`]

## If the codebase is large
If the repo is too large to read in full in one pass, tell me your exploration plan
first (which directories/modules you'll prioritize and why) before generating docs, so
I can redirect you if you're about to miss something important like a core domain module.

## When you're done
Give me a short summary listing: which files were created, which sections have
`[NEEDS HUMAN INPUT]` or `[FILL IN]` markers I need to resolve, and the top 3-5 most
significant anti-patterns found (by risk/impact, not just count) so I know where to focus
first.
