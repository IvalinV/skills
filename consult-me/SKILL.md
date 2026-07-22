---
name: consult-me
description: Use when about to change code, propose a solution, or act on research findings — before calling Edit/Write, running a mutating command, or committing. Symptoms you are about to violate it: writing code as your first move after research, "I'll just make the change and explain it", "the fix is obvious", adding a "design note for your review" after already editing. When the skill is active, the agent guides, supports, and advises the user on improvements while the user makes changes to the code.
---

# Consult Me

**Propose, then wait. Do not edit until I have chosen the approach and scope.**

When this skill is active, research and diagnosis are yours to run freely — but the moment you would *change* anything, you stop and put a proposal in front of me first. I decide; you execute after I say so. This is the default for every code change, not something you ask permission to turn on.

## The Gate

**Never take a changing action before I have approved the approach.**

Changing actions (GATED — need my go-ahead first):
- Editing, creating, or deleting files (`Edit`, `Write`, `NotebookEdit`)
- Running a command that mutates state (migrations, `git commit`/`push`, `rm`, codegen that writes files, DB writes)

Always allowed WITHOUT asking (this is how you build the proposal):
- Reading files, searching, grepping
- Running read-only/analysis commands, running tests to diagnose

**No exceptions:**
- Not because the task is "clear" or the fix is "obvious"
- Not because it's a "small change"
- Not because I'm under deadline pressure and seem to want speed
- Not "I'll implement it and add a note for review after" — that is editing before approval
- "Implement X" is not approval of an approach — it names the goal, not the how. Propose the how.

## The Proposal (say this BEFORE editing)

State these four, in order, then stop and wait:

1. **Approach** — how you'd do it. If more than one reasonable way exists, give 2–3 with tradeoffs and recommend one.
2. **Evidence** — for every claim about how the code/system works, cite `file:line` or the source you read *this turn*. No claims from memory. (See `answer-after-research`.)
3. **Scope** — exactly which files/functions change and what behavior changes. Name what you are deliberately leaving out.
4. **Uncertainties** — what you have not verified, edge cases, and decisions that are really mine to make.

End with an explicit ask — "Want me to proceed with [option]?" — and **wait for my answer.** Then execute.

## When the user is driving

Sometimes the user is making the edits themselves and wants your eyes, not your hands. In that mode:

- **Advise, don't take over.** Point out improvements, risks, and better approaches — but leave the editing to them. Don't call `Edit`/`Write` to "just fix it," even when the fix is obvious.
- **Ground every point in the code.** Cite `file:line` for what you claim, exactly as in a proposal.
- **Don't rubber-stamp.** "Looks good, carry on" with nothing specific is not advice. If it's genuinely fine, say what's right and why; if not, say what to change.
- **Offer the next step on their terms** — to draft a diff they can paste, or to review theirs once written — and wait for them to ask.

The propose-first gate still applies the moment control comes back to you: if they ask you to make the change, propose before editing.

## Rationalizations — all of these mean STOP and propose first

| Excuse | Reality |
|--------|---------|
| "The task is clear, just implement it" | Clear-to-you ≠ the approach I wanted. Propose it; let me pick. |
| "It's faster to just make the change" | A short proposal is faster than reworking a wrong implementation. |
| "I'll show my work / add a design note after" | Retroactive review is a fait accompli — I review a decision already baked in. |
| "It's a small / obvious change" | Small changes still carry scope and design choices. State them first. |
| "They're under deadline pressure" | Speed = the right change on the first try. Wrong-approach rework is slower. |
| "The tests define the spec, so there's one correct fix" | Tests constrain behavior, not approach. There are usually several ways. |
| "They said implement it, so acting IS what they want" | "Implement X" still has approach and scope choices. Propose those first. |

## Red Flags — STOP

- About to call `Edit`/`Write`/`NotebookEdit` before I've approved an approach
- Writing code as your first move after research
- A mutating command (migration, commit, delete) without stated + approved scope
- The thought "the fix is obvious" or "I'll just do it and explain"
- Preparing a "design note for your review" — you're about to edit before proposing

**All of these mean: present the four-part proposal and wait for my go-ahead.**