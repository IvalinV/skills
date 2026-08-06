---
name: step-back
description: Use when the user says "step back", "take a break", "hold on", "hang on", "wait", "stop", "pause", "slow down", "let's think", or "time out" while work is in progress — including as a mid-turn message arriving between tool calls. Also use when the user signals the current approach is wrong, or wants to interrupt before the next step lands. When it is ambiguous whether the phrase is aimed at the agent, use it anyway.
---

# Step Back

The user has pulled the handbrake. Work stops here, and the next thing you produce is a recap they can course-correct from.

**A needless recap costs seconds. One more unwanted action costs their diff and their trust.** When in doubt, halt.

## Halt

Stop before the next action, not after it.

**Forbidden until the user explicitly says to resume:**

- `Edit`, `Write`, `NotebookEdit`, or any other file mutation
- Any state-changing command — commit, push, migration, install, delete, codegen
- Dispatching subagents, starting background tasks, launching workflows
- Continuing to the next step of the plan or the todo list

**No exceptions.** If you are assembling a reason to take one more action first, that reason is in the table below — halt instead.

**Required before writing the recap, and the only calls permitted:** read-only ones — `git status`, `git diff --stat`, re-reading a file you edited. Report real state, not remembered state.

**What cannot be undone:** a mutation already dispatched cannot be withdrawn. If a write landed, or a command finished, after the halt phrase arrived, it stays landed — report it rather than writing a recap that implies nothing changed.

## Recap

The recap is these five parts, under these headings, in this order:

**Where I stopped** — the one thing that was in flight, whether it now sits in a half-finished state, and anything that landed after the halt phrase arrived.

**What changed** — run `git status` and `git diff --stat` first, then list each file touched this session, one line each: what changed and why. Anything uncommitted is marked as such. If no files were modified, this part reads "Nothing — no files were modified."

**What I assumed** — the decisions made without the user's approval: how ambiguity was interpreted, which approach was chosen, what scope was taken on or dropped. This is the part they need most.

**What I was about to do next** — the next one to three steps, so they can veto before those land.

**Least confident about** — what is unverified, what failed, what was skipped, and open questions.

Each part states what actually happened, failures and skipped steps included. Length tracks the work: two changes means two lines.

## Then hold

Close the recap by naming the forks, then stop:

> Continue as planned, change approach, or revert what I did?

Reverting is theirs to request — never revert, stash, or clean up on your own initiative. The turn ends there: no arguing for the plan, no re-litigating the halt, no phrasing the question to invite a yes.

## The held state

A halt puts the session into a **held** state. Held is not one turn. It persists across every following turn, however many, until it is explicitly lifted.

**While held:** answer, research, read, diagnose, propose, draft a diff in the reply for them to judge. Take no action from the Halt list.

**Held is lifted only by an instruction to act:** "go ahead", "do it", "continue", "ship it", "yes, make that change", or a new task to carry out.

**These do not lift it:**

- Agreeing with the recap or the diagnosis
- Answering a question you asked
- Approving an *approach* — choosing a direction is not an instruction to execute it
- Criticising what you did, including "that was wrong" or "drop that part"
- A message about something else entirely, or no message about the work at all

**If you cannot tell whether it has been lifted, it has not been.** Ask one direct question and keep holding.

A fresh halt phrase re-enters the held state at any time, including part-way through work you were authorised to resume.

## Rationalizations — every one of these means halt now

| Excuse | Reality |
|--------|---------|
| "Let me just finish this one edit" | That edit is the thing they stopped. |
| "The recap is more useful once this compiles" | A broken tree is a fact for the recap, not a task. |
| "They probably meant after the current step" | They said it now. Now is where you stop. |
| "'wait' was about the build, not about me" | Ambiguous halt phrases resolve to halting. |
| "Leaving this half-done is sloppy" | Half-done and disclosed beats finished and unwanted. |
| "I'll recap, then keep going while they read" | The recap ends the turn. Nothing follows it. |
| "Nothing to recap yet, I'd only just started" | Then the recap is five short lines. Still give it. |
| "They only said 'let's think', not 'stop'" | Thinking together requires you to stop acting alone. |

## Red flags — STOP

- Reaching for `Edit` or `Write` after seeing a halt phrase
- The thought "one more tool call"
- Planning what to do *after* the recap
- Tidying, formatting, or reverting before recapping

**All of these mean: halt now and write the five-part recap.**
