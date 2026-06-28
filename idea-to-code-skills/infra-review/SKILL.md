---
name: infra-review
description: "Use this skill to review infrastructure-as-code changes (Terraform, Kubernetes manifests, IAM policies, network rules, queue/topic provisioning) against actual current infra state and shared-platform risk. Triggers whenever a diff includes .tf files, k8s manifests, IAM policy changes, or any cloud resource provisioning, or the user asks to 'review this infra change' or 'check this Terraform PR'. Do NOT use this for application code review (use implementation-review or security-review)."
---

# Infrastructure / IaC Review

**Requires:** an infrastructure-as-code diff; the Design Doc, if infra was anticipated there; a System Impact Map.
**Produces:** an infra change verdict — current vs proposed state, permission changes, cost/scaling impact, shared-infrastructure risk, rollback independence.

*(Run whenever a diff includes infrastructure-as-code changes — Terraform, Kubernetes manifests, IAM policies, network rules, queue/topic provisioning. These carry a different risk profile than application code and are easy to under-review if treated as "just more diff.")*

```
You are reviewing infrastructure-as-code changes against the actual current infrastructure state
and the rest of the platform, not just checking that the syntax is valid.

CONTEXT:
- IaC diff: [paste or point to branch — Terraform/k8s manifests/IAM policies/etc.]
- Design Doc: [link, if infra was anticipated there]
- System Impact Map: [link/content]

INSTRUCTIONS:
1. Read the actual current state of whatever this diff modifies (existing Terraform state/
   manifests in the repo) — confirm what's changing relative to what's actually deployed, not
   relative to what the diff author assumed was deployed.
2. For any new or widened permission (IAM role, security group rule, network policy), state
   exactly what access it grants and to what — flag anything broader than the stated need
   (e.g. a wildcard resource ARN where a specific one would do).
3. For any new compute/queue/topic/resource, check for cost and scaling implications by comparing
   to similar existing resources in the repo (instance sizes, autoscaling config, retention
   settings).
4. Check whether this change could affect other services sharing the same infrastructure
   (a shared VPC, a shared cluster, a shared queue) — cross-reference against the System Impact
   Map's listed dependencies.
5. Confirm there's a way to revert this specific infra change independently of the application
   code deploy (e.g. can the Terraform apply be rolled back without also reverting the app
   version, or are they coupled in a way that complicates rollback).
6. Run the Self-Verification step below before finalizing — it's part of the deliverable, not an
   optional footer.

OUTPUT FORMAT:
## Current vs Proposed State
- [resource] — current: [...] — proposed: [...]

## Permission Changes
- [permission/rule] — grants: [exact scope] — broader than needed: [yes/no, why]

## Cost/Scaling Impact
- [resource] — comparison to existing: [finding]

## Shared-Infrastructure Risk
- [shared resource touched] — other services affected: [list, from Impact Map]

## Rollback Independence
[Infra rollback is independent of app deploy / Coupled — explain risk]

## Verdict
[Clear / Must fix before merge] — [list blocking items]

## Self-Verification
**Verified (direct evidence):**
- [current-state/permission/cost finding] — confirmed at [file:line in IaC/state]

**Could Not Verify:**
- [anything you couldn't confirm against live infra — e.g. you read the IaC source but couldn't
  confirm what's actually deployed right now] — blocked by: [no access to live state/console] —
  who can unblock: [platform/SRE team]

**Confident But Not Proven:**
- [a cost/scaling estimate or shared-infra risk made by comparison rather than direct
  measurement] — basis: [comparable resource] — confidence: [Medium/Low]

**For the Next Skill (release readiness review):**
- [anything here that should block release vs. anything safe to proceed on with a flagged
  follow-up]
```
