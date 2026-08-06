# Skills

A personal collection of agent skills for Claude Code. Built around the workflows I actually use day-to-day on PHP/Laravel and JS projects.

> Inspired by [mattpocock/skills](https://github.com/mattpocock/skills).

## Installation

Skills live in your Claude Code skills directory. Drop the skill folder in alongside your other skills:

```bash
# Clone into your Claude skills directory
git clone https://github.com/IvalinV/skills.git ~/.claude/skills/ivalinv-skills

# Or copy individual skills you want
cp -r answer-after-research ~/.claude/skills/
```

Claude Code auto-discovers skills on the next session. Invoke a skill by trigger phrase or with the `Skill` tool.

## Skills

### `answer-after-research`

Forces Claude to answer Laravel / PHP / JS package questions from authoritative sources rather than from memory. When active, every claim about a method signature, config key, or framework behavior must come from docs or source read in the current turn — no "I believe" or "typically" answers allowed.

**Triggers on:**

- The phrase *"answer after research"* (or *"research first"*, *"research before answering"*)
- Any question about Laravel core or its ecosystem (Cashier, Fortify, Sanctum, Pennant, Socialite, Nova, Reverb, Telescope, Boost, Pest, Inertia)
- Any question about external PHP / JS packages (including VueUse)
- A documentation URL supplied alongside a question

**What it does:**

1. Identifies the package and installed version (checks `composer.json` / `package.json`)
2. Consults sources in priority order — user-provided URL → Laravel Boost `search-docs` → official docs → source under `vendor/` or `node_modules/`
3. Verifies the doc version matches the installed version
4. Cites every non-trivial claim with a file path + line number or doc URL
5. Flags uncertainty explicitly when a source cannot be found

See [`answer-after-research/SKILL.md`](answer-after-research/SKILL.md) for the full skill definition.

### `consult-me`

Enforces a propose-first gate on code changes. Research and diagnosis run freely, but the moment Claude would *change* anything, it stops and puts a four-part proposal (approach, evidence, scope, uncertainties) in front of you and waits for your go-ahead. This is the default for every change, not a mode you have to turn on.

**Triggers on:**

- Being about to change code, propose a solution, or act on research findings — before any `Edit` / `Write` / `NotebookEdit`, mutating command, or commit
- Warning signs like "the fix is obvious", "I'll just make the change and explain it", or adding a "design note for review" after already editing

**What it does:**

1. Gates all changing actions (file edits, migrations, commits, deletes) behind your explicit approval
2. Keeps reading, searching, and read-only analysis always allowed — that's how the proposal gets built
3. Requires a four-part proposal before editing: approach (with tradeoffs), evidence cited as `file:line`, scope, and uncertainties
4. Supports a "user is driving" mode where Claude advises on your edits without taking over
5. Names the common rationalizations for skipping the gate and treats each as a signal to stop and propose

See [`consult-me/SKILL.md`](consult-me/SKILL.md) for the full skill definition.

### `step-back`

Turns any halt phrase into an immediate stop plus a structured recap you can course-correct from. Work ceases before the next action rather than after it, and the session stays held — across every following turn — until you explicitly tell it to act again.

**Triggers on:**

- *"step back"*, *"take a break"*, *"hold on"*, *"hang on"*, *"wait"*, *"stop"*, *"pause"*, *"slow down"*, *"let's think"*, *"time out"* — including as a mid-turn message arriving between tool calls
- Any signal that the current approach is wrong, or that you want to interrupt before the next step lands
- Ambiguity resolves toward halting: if it is unclear whether the phrase was aimed at Claude, it halts anyway

**What it does:**

1. Stops before the next action — no edits, no state-changing commands, no new subagents, no continuing to the next planned step, even when one line from done
2. Requires `git status` / `git diff --stat` before describing state, so the recap reports the working tree rather than what Claude remembers doing
3. Returns a fixed five-part recap: where it stopped, what changed, **what it assumed without your approval**, what it was about to do next, and what it is least confident about
4. Holds the session until an instruction to act — agreeing with the recap, answering a question, or approving an *approach* does not resume work
5. Never reverts, stashes, or tidies on its own initiative; reverting is offered, never performed unprompted

See [`step-back/SKILL.md`](step-back/SKILL.md) for the full skill definition.

## Why these skills exist

The three failure modes these skills target:

**Drift from memory.** Frameworks and packages change. An answer that was correct against Laravel 10 may quietly mislead on Laravel 11. `answer-after-research` removes the temptation to answer from training data when the actual docs and source are right there in the project.

**Plausible-sounding fabrication.** "I believe the method accepts a callback" is the kind of statement that wastes an afternoon. Verify-or-don't-answer is cheaper than debugging a hallucinated API.

**Momentum past the point of usefulness.** Saying "hold on" reliably stops an agent, but what comes back is a progress report — not the decisions it made on your behalf or the things it never verified. `step-back` makes those two the centre of the recap, and keeps the session held afterward instead of drifting back into work.

## Adding your own skills

Each skill is a folder containing a `SKILL.md` file with YAML frontmatter:

```markdown
---
name: my-skill
description: One-sentence trigger description. Use when [conditions].
---

# My Skill

Instructions for Claude when this skill is active...
```

The `description` field is what Claude reads to decide whether to invoke the skill, so make the trigger conditions specific and unambiguous.

## License

MIT