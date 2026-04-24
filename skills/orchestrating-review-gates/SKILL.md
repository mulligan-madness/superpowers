---
name: orchestrating-review-gates
description: Use when completed work needs independent spec, quality, and regression-risk review before acceptance, especially when fixes for one concern could invalidate another.
---

# Orchestrating Review Gates

## Overview

Use separate review gates when completed work must satisfy both requirements and quality standards. A gate is only current for the artifact it reviewed; if later fixes change the other gate's assumptions, rerun the affected gate.

## Required Skills

Before running this loop, load and follow:

- `superpowers:requesting-code-review` for review dispatch and severity format.
- `superpowers:receiving-code-review` before handling any finding.
- `superpowers:verification-before-completion` before reporting success.

This skill coordinates those skills. Do not replace them with a summary from memory.

## Review Packet

Build the review packet from real files and source evidence. Include:

- Work product, diff, commit range, or artifact under review.
- Governing spec, issue, plan, checklist, or acceptance criteria.
- Quality rubric for this artifact type.
- Relevant source evidence and local conventions.
- Validation already run, with command and status.
- Explicit boundaries and out-of-scope items.
- Previous gate results when this is a rerun.

Reviewers must not depend on hidden thread context, optimistic reports, or unstated assumptions.

## Gate Sequence

1. Build the review packet.
2. Run spec-compliance review first.
3. Use `superpowers:receiving-code-review` for every Critical or Important spec finding.
4. Fix, technically reject with evidence, or report blockers. Rerun spec review until no unresolved Critical or Important spec findings remain.
5. Run quality review second with a different prompt and lens.
6. Use `superpowers:receiving-code-review` for every Critical or Important quality finding.
7. Fix, technically reject with evidence, or report blockers. Rerun quality review until no unresolved Critical or Important quality findings remain.
8. Apply regression rerun rules.
9. Run fresh verification before any completion claim.

## Regression Reruns

Rerun spec review when a quality fix changes scope, behavior, source-of-truth rules, public interfaces, artifact boundaries, validation meaning, or requirement satisfaction.

Rerun quality review when a spec fix changes maintainability, discoverability, operability, pedagogy, trigger behavior, ergonomics, or other quality criteria.

Repeat until both gates are clean at the same time, or until remaining blockers are explicit and evidenced.

## Finding Policy

- Critical and Important findings block completion unless fixed, technically rejected with evidence, or explicitly blocked.
- Unclear findings block partial implementation until clarified.
- Minor findings are optional. Do not implement Minor or out-of-scope suggestions merely to satisfy the reviewer.
- Verify each finding against actual files and source evidence before editing.
- Keep reviewer prompts independent; provide the review packet, not your session history.

## Completion Standard

Completion requires:

- Spec gate has no unresolved Critical or Important findings.
- Quality gate has no unresolved Critical or Important findings.
- Required regression reruns are complete or explicitly blocked.
- Fresh verification has run after the last material change.
- Final report distinguishes passed, failed, blocked, and not-run checks.

## Red Flags

- "Both reviews passed at least once" after later edits changed scope or quality assumptions.
- Fixing quality language that changes requirements without rerunning spec review.
- Fixing spec gaps that change trigger behavior, usability, or maintainability without rerunning quality review.
- Implementing Minor feedback just to appease a reviewer.
- Letting a reviewer rely on hidden Linear, issue, chat, or thread context.
- Reporting completion from stale validation.
