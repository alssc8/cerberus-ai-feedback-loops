# Run log

One block per run of the ai-feedback-loop protocol on this repository. The
template every run follows is `plugins/cerberus/skills/ai-feedback-loop/templates/run-log.md`.

---

## Run: 2026-09-06 — Generalize the Ground Truth Register (shipped as 1.1.0)

**Shape:** 1 implementer (sonnet, effort high) + 1 verifier (opus, effort
xhigh, fresh context); orchestrator on Fable 5.1. Gate 0: qualified — checkable
output, reusable, headed to colleagues. Zero correction rounds.
**Outcome:** shipped. Brief and verdict in `runs/2026-09-06-ground-truth-register/`.

### What the verifier caught

1. **A role named as a source of record.** One catalogue row listed "a bilingual
   reviewer" as a source. Class: *source of record must be an artifact a stranger
   can read, never a person or a team.*
2. **A reference rule broader than the gate it serves.** The "downgrade to
   opinion" disposition could be read as licensing the softening of an
   untraceable figure that Gate 1 rule 2 forbids. Class: *reference text must
   be scoped at least as tightly as the rule it elaborates.*
3. Outside the worker's scope, the verifier noted that the skill's own
   `description` still listed only marketing deliverables. Applied by the
   orchestrator.

### Which brief field was missing

None for the worker — it stayed inside its boundaries and returned evidence.
The gap was the **orchestrator's own coverage check**: the request "make it
more general" also applied to the skill's `description`, which the brief did
not put in scope. This is precisely the decomposition failure 1.0.1 added the
Gate 2 coverage check for; the check was applied to the worker's scope but not
to the orchestrator's.

### Escalations

| Part | From | To (effort or tier) | Same-tier verdict? | Did it fix it? |
|---|---|---|---|---|
| — | — | none needed | no | — |

Deviation: the two minor, located findings were applied by the orchestrator at
synthesis instead of returning them to the worker. Cheaper than a round trip for
one-line fixes; recorded so the pattern is visible if it starts hiding larger
edits.

### Checks that should exist but did not

- A lint for the register's *source of record* column: any row whose source
  contains "reviewer", "team", "expert", "ask", or a person's role fails
  automatically.

### Cost

Worker ≈41k tokens / 14 tool calls; verifier ≈46k / 14; orchestrator overhead
on top. Roughly 3× a single-pass edit. Worth it: the file is reference material
that will be read by every future run and by colleagues, and both catches were
real — small, but they would have shipped.

---

## Standing defect classes

Promoted from individual runs once seen twice. These become permanent
acceptance criteria.

| Defect class | First seen | Now checked by |
|---|---|---|
| Client-derived figures used as "examples" | 2026-09-06 (pre-publication sweep) | grep for known names/figures before every push |
| Decomposition gap — a request item with no owner | 2026-09-06 (1.0.1) | Gate 2 coverage check; verifier receives the original request |

---

## Retired checks

| Check | Runs survived | Reason retired |
|---|---|---|
| | | |
