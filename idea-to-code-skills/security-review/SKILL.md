---
name: security-review
description: "Use this skill to do a security-focused code review of a diff — auth/authz enforcement, injection risks, secrets handling, sensitive data handling, dependency vulnerabilities — distinct from a design-level security review. Triggers on phrases like 'security review this PR', 'check this for vulnerabilities', or whenever a diff touches auth, PII/PHI/financial data, or a new endpoint and needs a security pass before merge. Do NOT use this for general code correctness (use implementation-review) or for infrastructure permission changes (use infra-review, though that also covers IAM)."
---

# Security Review (code-level)

**Requires:** a PR diff; the Design Doc's security/data-classification section; the risk tier.
**Produces:** a code-level security verdict — auth/authz verification, injection risk, secrets handling, sensitive data handling, dependency risk.

*(Distinct from a design-level security review — a design can be approved as secure and the implementation can still introduce a vulnerability the design never specified at that level of detail. Recommended for any change touching auth or sensitive data; treat as required for your highest risk tier.)*

```
You are doing a security-focused review of this diff against the actual codebase — looking for
vulnerabilities the implementation introduces, not re-litigating whether the design was a good
idea.

CONTEXT:
- PR diff: [paste or point to branch]
- Design Doc security/data-classification section: [paste]
- Risk tier: [paste]

INSTRUCTIONS:
1. For every new or modified endpoint/handler in the diff, check: is authentication/authorization
   actually enforced in the code (find the actual middleware/decorator/check), not assumed because
   a similar endpoint elsewhere has it. Quote the file:line that proves enforcement, or flag its
   absence.
2. For every place user input or external data enters the new code, check for injection risks
   (SQL, command, template, deserialization) by reading how the input is actually used downstream
   — not a generic "looks fine" pass.
3. For any new handling of secrets, tokens, or credentials, confirm they're not logged, not
   written to a field that gets persisted in plaintext, and not passed in a way that would put
   them in version control history.
4. For any new or modified data access touching PII/PHI/financial data (per the Design Doc's
   classification), confirm encryption-at-rest/in-transit matches what similar existing sensitive
   data flows do elsewhere in the codebase — find that existing flow and compare directly.
5. Check dependency changes in this diff (new or upgraded packages) for any known-vulnerable
   version, by checking the actual version pinned in the lockfile.
6. Do not flag generic "best practice" suggestions with no concrete exploit path — every finding
   needs a specific scenario describing how it could actually be exploited.
7. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer. Be unusually strict here: a security "Verified" claim with no real proof is
   worse than flagging it as unverified, since the former gets trusted and the latter gets a
   second look.

OUTPUT FORMAT:
## Auth/Authz Verification
- [endpoint/handler] — enforcement found at [file:line] / **NOT FOUND — flag**

## Injection Risk Review
- [input entry point] — [how it's used downstream] — risk: [none/low/medium/high]

## Secrets Handling
- [any secret/credential touched] — [safe / concern: describe]

## Sensitive Data Handling
- [data touched] — [matches existing pattern at file:line / diverges: describe]

## Dependency Risk
- [new/changed dependency] — [version] — [known issues: yes/no, per available info]

## Findings
- 🔴 [exploitable issue] — [file:line] — [concrete exploit scenario]
- 🟡 [hardening suggestion with concrete risk, not generic advice] — [file:line]

## Verdict
[Clear / Must fix before merge] — [list blocking items]

## Self-Verification
**Verified (direct evidence):**
- [auth/injection/secrets/data finding] — confirmed at [file:line], exploit path demonstrated

**Could Not Verify:**
- [anything you couldn't fully trace — e.g. a known-vulnerable dependency check where you lack a
  live CVE feed, or runtime behavior that static reading alone can't confirm] — blocked by:
  [reason] — who can unblock: [security team / specific tool]

**Confident But Not Proven:**
- [a risk rating given without a concrete demonstrated exploit] — basis: [why you rate it this
  way] — confidence: [Medium/Low] — flag explicitly that this should not be treated as equivalent
  to a Verified finding

**For the Next Skill (verification review, release readiness review):**
- [anything Could-Not-Verify or Confident-But-Not-Proven above that should block release until a
  human security reviewer closes the gap, vs. anything safe to proceed on]
```
