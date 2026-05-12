---
name: answer-after-research
description: Use when the user writes "answer after research" (or "research first", "research before answering"), asks a question about a Laravel/PHP/JS package or framework feature, or provides a documentation URL alongside a question. Covers Laravel core, Laravel ecosystem packages (Cashier, Fortify, Sanctum, Pennant, Socialite, Nova, Reverb, Telescope, Boost, Pest, Inertia), and external PHP/JS packages including VueUse.
---

# Answer After Research

When this skill is active, you MUST NOT answer from memory. Every claim about a package, framework feature, method signature, or behavior must come from an authoritative source consulted in this turn.

## Triggers

Activate when any of the following is true:

- User writes "answer after research" (or a close paraphrase like "research first", "research before answering").
- User asks a question about a Laravel package (Cashier, Fortify, Sanctum, Pennant, Socialite, Nova, Reverb, Telescope, Boost, Pest, Inertia, etc.), Laravel core, or any external PHP/JS package (including VueUse and other Vue composition utility libraries).
- User provides a documentation URL with a question.

## Workflow

1. **Identify the source surface.** Determine which package/feature the question targets and which version is installed (check `composer.json` / `package.json` if unclear).

2. **Consult authoritative sources in this order:**
   1. If the user provided a doc URL → fetch it with `WebFetch` and read it.
   2. For Laravel ecosystem packages → use Laravel Boost `search-docs` with multiple broad, topic-based queries. Pass the `packages` array to scope.
   3. For external packages → fetch the official docs (GitHub README, project site). **Never invent or guess a documentation URL.** If you do not already have one and cannot locate it via the package's `homepage`/`repository` field in `vendor/<pkg>/composer.json` (PHP) or `node_modules/<pkg>/package.json` (JS), stop and ask the user for the URL before proceeding.
   4. For behavior questions where docs are thin → read the source. Locate the package under `vendor/<vendor>/<package>/` (PHP) or `node_modules/<package>/` (JS) and read the implementing file.

3. **Verify version alignment.** Only cite information that matches the installed version. If the docs you found are for a different major version, say so and continue searching.

4. **Synthesize the answer.** For each non-trivial claim, cite the source — file path with line number for source code, or doc heading/URL for documentation. Quote method signatures verbatim rather than paraphrasing.

5. **Flag uncertainty.** If a source could not be found, or contradicts the user's premise, state that explicitly. Never fill the gap with plausible-sounding inference.

## Prohibitions

- Do not assert method names, parameter lists, return types, config keys, or default values from memory.
- Do not say "I believe", "I think", or "typically" about package behavior — either you verified it this turn or you did not answer it.
- Do not skip step 2 because the question "seems simple". Simple-looking questions are where memory-based answers most often drift from current package behavior.
- Do not guess a documentation URL. If you cannot find an authoritative URL via the package manifest, ask the user before fetching anything.

## When the user provides documentation

Treat the user-provided document as the highest-priority source. Read it in full (or the relevant section) with `WebFetch` / `Read` before answering. If the question can be fully answered from that document, you may skip `search-docs`.