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

## Why these skills exist

The two failure modes these skills target:

**Drift from memory.** Frameworks and packages change. An answer that was correct against Laravel 10 may quietly mislead on Laravel 11. `answer-after-research` removes the temptation to answer from training data when the actual docs and source are right there in the project.

**Plausible-sounding fabrication.** "I believe the method accepts a callback" is the kind of statement that wastes an afternoon. Verify-or-don't-answer is cheaper than debugging a hallucinated API.

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