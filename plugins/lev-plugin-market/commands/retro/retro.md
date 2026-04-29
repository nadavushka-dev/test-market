---
description: Retrospective on the current session. Scans what the user corrected, where you got stuck, or what required extra searching, then proposes concrete diffs to `.claude/rules/`, `.claude/skills/`, or `CLAUDE.md` that would have prevented it.
allowed-tools: Read, Grep, Glob
disable-model-invocation: true
---

Retrospective on this session.

The point is **not** to summarize what happened. The point is to find dents in the project's AI context (`CLAUDE.md`, `.claude/rules/*`, `.claude/skills/*`) that caused friction — and propose fixes the user can accept or reject.

## 1. Scan the session for signals

Look back through the conversation for these concrete signals:

-   **User corrections** — "no, do X instead", "stop doing Y", reverting your edits, rewriting your output. Each correction is a data point: either the rules did not cover this, or they covered it wrong, or they were right but you did not read them.
-   **Dead ends** — you searched the codebase for something that a rule or skill should have told you directly.
-   **Repeated work** — you did the same multi-step procedure that a skill could have packaged.
-   **Surprises** — facts about the project that were not in any rule/skill and that the user had to explain mid-task.
-   **Validated choices** — non-obvious approaches the user confirmed worked. These are also learnings (keep doing X), not just corrections.

Ignore: normal code-writing churn, bug fixes that did not involve misunderstanding the project, one-off questions.

## 2. Diagnose each signal

For each signal, pick one:

-   **Rule gap** — no rule covered this. Which file in `.claude/rules/` should it land in? Does it match the file's `paths:` frontmatter so it auto-loads at the right moment?
-   **Rule wrong/outdated** — rule said X, reality is Y. Which file, which line?
-   **Rule right, unused** — the rule was in context but you did not apply it. The rule phrasing is the likely culprit — imperative and explained, or vague and ignorable?
-   **Skill-worthy procedure** — the task was a repeatable recipe (3+ coordinated steps across files). Propose a new skill at `.claude/skills/<name>/SKILL.md` with a pushy description that will actually trigger.
-   **Wiki-worthy fact** (once `docs/wiki/` exists) — durable project knowledge, not a rule. Park it there instead.
-   **Retro-worthy** — friction came from `/retro` itself: missed a pattern, surfaced noise, over-generalized from one session. `/retro` is subject to the same loop as everything else.
-   **Not worth codifying** — one-off, unlikely to repeat. Say so explicitly and drop it. Do not let the rule set rot with single-incident generalizations.

## 3. Propose diffs, do not write them

Output a short list of proposed changes. For each:

-   **Signal** — one sentence on what happened in the session
-   **Diagnosis** — which of the categories above
-   **Target** — exact file path (and `paths:` frontmatter change, if rule placement depends on it)
-   **Proposed change** — the actual new text or a concrete diff. Keep it tight — new rules should be imperative and include a **why**, per the project's own conventions.

Then stop. **Do not edit any file.** The user reviews the list and tells you which to apply.

## 4. Calibration

-   If the session was uneventful, say "nothing to learn here" and stop. A `/retro` that forces findings is how rule sets rot.
-   Three proposals is usually plenty. Ten is a red flag — you are over-generalizing from one session.
-   Prefer **removing or rewriting** an existing rule over adding a new one when possible. Rule bloat has a real cost: every token auto-loaded is a token the model pays for forever.
