---
name: backtracking-workflow
description: Use when a user says "back track", when a broad task grows beyond its original scope, when repeated attempts or regressions suggest circular work, or at major checkpoints during substantial changes. Trigger on the phrase even when it appears as "backtrack" or with different capitalization.
---

# Backtracking Workflow

Backtracking is a hard pause for re-reading the request, auditing the current work, and agreeing on a safe path forward. It is not permission to keep implementing while thinking.

## Trigger And Halt

Trigger this skill when any of these occurs:

- The user says `back track` or `backtrack`, regardless of capitalization.
- The task expands, gains dependencies, or no longer matches the original scope.
- Multiple attempts fail, regress, or revisit the same area without progress.
- A substantial task reaches a major phase checkpoint.

When triggered:

- Stop all edits, mutations, commits, and further implementation immediately.
- Keep the session on hold across subsequent turns.
- Do not treat urgency, sunk effort, or a nearly finished step as permission to continue.

## Backtracking Report

Read the complete user request, constraints, prior approved decisions, current repository state, and relevant diff. Distinguish observed facts from scenario claims; never invent files, diffs, failures, or prior decisions that cannot be verified. Then report these sections in order:

1. **Original requirements** — what the user asked for and the constraints that govern it.
2. **Current state** — what was changed, what is incomplete, and what failed.
3. **Change audit** — classify every verified relevant change as `keep`, `adapt`, `defer`, or `harmful`, and give a brief reason tied to the requirements or agreed plan. Record unverified claimed changes separately instead of classifying them as present.
4. **Missed or conflicting items** — requirements, assumptions, dependencies, or regressions that need attention.
5. **Forward plan** — the smallest ordered set of next steps that returns the work to the requested outcome.

Inspect changes before judging them. Preserve useful work when it contributes to the forward plan. Do not discard or rewrite a change merely because backtracking occurred.

## Approval Gate

End the report by asking the user to approve or revise the forward plan. All work remains on hold until the user explicitly agrees to a clear plan for continuing.

An agent's own plan, a diagnosis, or agreement with the report is not approval. Do not resume because the user answered an incidental question or acknowledged the problem.

If a change is classified as `harmful`, explain the evidence that it conflicts with the agreed plan and request explicit approval before removing or rewriting it. Until that approval arrives, leave the change untouched and plan around it.

After approval, continue only with the approved plan. Re-trigger this workflow if scope changes, another loop appears, or the user says `back track` again.

## Red Flags

- "I'll make one last edit before auditing."
- "The change is probably harmful, so I'll remove it now."
- "The user agreed with the diagnosis, so I can continue."
- "The plan is obvious; approval would only slow this down."
- "I already remember the original prompt."

Each red flag means: stop, re-read, audit the changes, report the forward plan, and wait for explicit approval.
