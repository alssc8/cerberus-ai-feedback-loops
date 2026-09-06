# Verification brief — cerberus v1.0.0, pre-publication pass

Written to the skill's own Gate 3 template. Second pass: the package changed
after the first Fable verification (effort settings, no-Fable operation, cost
governance rewrite, README requirements), and it is about to be published for
colleagues who may not have Fable.

**Worker:** Fable 5.1 (implementer, this session)
**Verifier:** Fable, fresh context, never saw the drafting conversation
**Date:** 6 September 2026

## Objective

Confirm the plugin is structurally valid, internally consistent, and works on a
machine that has `opus`, `sonnet` and `haiku` but **no `fable`** — so that a
colleague adding the marketplace gets a working skill and three working agents
with no silent degradation.

## Output format

Verdict per `templates/verdict.md`: scores table, PASS/FAIL, located findings
with severity, unverifiable list, checks-not-run.

## Sources and tools

ALLOWED: every file under `claude-plugins/`; the Claude Code plugin, skill and
subagent reference documentation at code.claude.com; `claude plugin validate`.

FORBIDDEN: rewriting any file. Verification only.

## Boundaries

Do not assess whether the protocol works on real deliverables — that needs an
eval set which does not exist and is out of scope. Structure, consistency,
loadability, and no-Fable operation only.

## Acceptance criteria

1. `plugin.json` and `marketplace.json` parse and carry every required field;
   `source` resolves to a real directory; no personal email remains in either.
2. Skill and agent files sit where Claude Code discovers them; every agent named
   in SKILL.md exists in `agents/` with valid frontmatter and a valid `model`.
3. Every `${CLAUDE_SKILL_DIR}` path in SKILL.md resolves to an existing file.
4. SKILL.md frontmatter is valid YAML with `disable-model-invocation: true`.
5. SKILL.md is under 500 lines and contains no research justification.
6. **No hard Fable dependency.** Every mention of `fable` in SKILL.md,
   cost-governance.md and the agents has an explicit path for machines without
   it; no agent frontmatter pins `fable`; the protocol never *requires* it.
7. **Effort values are valid for their model.** `effort:` in agent frontmatter is
   a documented value, and the tier it is pinned to supports it (note: Haiku 4.5
   has no effort control).
8. Internal consistency: the new effort and no-Fable rules in SKILL.md and
   cost-governance.md do not contradict Gate 5, Gate 6, verification.md, or the
   agent definitions.
9. README claims match the files: install commands, requirements, agent table,
   invocation forms.
10. No claim in `references/evidence.md` is stated more strongly than its source
    supports; the blog-sourced convention stays labelled as a convention.

## Budget and stop condition

Ceiling: 40 tool calls. Stop when every criterion has a score and each failure is
located. Do not fix anything.
