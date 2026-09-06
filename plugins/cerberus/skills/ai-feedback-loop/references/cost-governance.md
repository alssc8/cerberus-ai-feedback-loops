# Cost governance

Multi-agent work uses roughly **4x the tokens of a single agent and ~15x a chat**.
That is only worth paying when the task is genuinely parallel, genuinely large, or
genuinely high-stakes. Otherwise it is an expensive way to be wrong more slowly.

## Tier rates

Anthropic first-party rates, cached June 2026 - re-check before quoting to a
client.

| Alias | Model ID | Context | In $/MTok | Out $/MTok |
|---|---|---|---|---|
| `fable` | `claude-fable-5-1` | 1M | 10.00 | 50.00 |
| `opus` | `claude-opus-5` | 1M | 5.00 | 25.00 |
| `sonnet` | `claude-sonnet-5` | 1M | 2.00 | 10.00 |
| `haiku` | `claude-haiku-4-5` | 200K | 1.00 | 5.00 |

### Why only four models

More Claude models are served than these four, but every one of them is an older
generation at the same or higher price for less capability: Opus 4.6/4.7/4.8 cost
the same as Opus 5, Fable 5 costs the same as Fable 5.1, and Sonnet 4.6 costs
*more* than Sonnet 5 ($3/$15 vs $2/$10). The lineup has exactly four price rungs
and this skill sits on all four. Use an older generation only to reproduce past
behavior, never for new routing.

Adding models to the roster also has two hidden costs: routing itself becomes a
failure surface (misrouting is a documented multi-agent failure mode), and prompt
caches are model-scoped - every extra model in a cascade forfeits cache reuse
across it.

### Effort: the dial within a tier

Each tier except `haiku` takes an effort setting - `low`, `medium`, `high`
(default), `xhigh`, `max` - that trades thoroughness against token spend on the
same model. Current Anthropic cost guidance is to tune effort per route before
building a multi-model cascade, because lower effort on a newer model often
matches higher effort on an older one, and one model keeps one cache.

| Work | Effort |
|---|---|
| Verification, correctness-sensitive judgment | `xhigh` (the verifier's default); `max` only when a miss is very expensive |
| Drafting and structured analysis | `high` - pinned in `loop-implementer`; an agent that omits effort inherits whatever the session runs at |
| Routine production, high-volume routes | `medium`, dropping to `low` where quality demonstrably holds |
| Retrieval and extraction | runs on `haiku`, which has no effort dial |

So within one part: raise effort before raising tier - and per Gate 6, a failure
that repeats unchanged is a brief problem, which no amount of effort or tier
buys you out of. Judge the result per completed deliverable, not per request.

Output is the expensive half - 5x input on every tier. **Orchestrator output
costs 2.5-10x worker output, depending on the pairing** - opus over sonnet is
2.5x, fable over haiku is 10x. That ratio dictates the architecture.

## The economic rule

> The orchestrator briefs, verifies, and synthesizes. It never drafts.

Drafting is long output. If the frontier model writes the deliverable, you are
paying $50/MTok for text a $10/MTok model produces acceptably - and you have
spent your independent verifier, because the author cannot grade itself.

Orchestrator output should be short: briefs, verdicts, and a synthesis pass.
Worker output is where the volume lives. If your orchestrator is producing more
tokens than your workers, the architecture is inverted.

## Assign tiers by failure cost, not by task prestige

| Work | Tier | Why |
|---|---|---|
| Extraction, retrieval, mechanical transformation | `haiku` | Verifiable by inspection; errors are obvious |
| Drafting, structured analysis, production | `sonnet` | Volume work; a verifier catches what it misses |
| Verification, synthesis, decomposition | `opus` | Judgment; errors here propagate everywhere |
| Novel strategy, adversarial verification on high-stakes output | `fable` where available, else `opus` at `max` effort | Where being wrong is expensive and hard to detect |

### Without Fable

The protocol needs three tiers, not four: `opus` to verify, `sonnet` to produce,
`haiku` to retrieve. Fable only ever holds two jobs - verifying an escalated
`opus` worker, and the highest-stakes checks - and both have a substitute:

- **Escalate effort, not tier.** A `sonnet` worker at `xhigh` is the first
  escalation everywhere, and on a no-Fable machine it is the only one that keeps
  the verifier independent.
- **Same-tier verification is a disclosed degradation, not a silent one.** If a
  part must run on `opus`, verify on `opus` in a fresh context at `max` effort,
  mark the verdict *same-tier* in the run log, and have a human check every
  blocker-level claim against the source of record. Fresh context and a fixed
  rubric reduce self-preference bias but do not remove it - the bias is a
  property of the text's distribution, not of the conversation - which is why
  the human check on blockers is mandatory here, not optional.

## Cascade, don't blanket-upgrade

Route the cheap tier first and escalate only what fails. Published cascades hold
quality while cutting cost **50-98%**, because only a small minority of tasks
actually need the top tier.

The escalation trigger is the verifier, not a guess. Escalate a part when it
fails verification with shrinking errors - not when the task *sounds* hard.

## Before reaching for a bigger model

Two cheaper levers usually beat a tier upgrade:

1. **Lower effort on a newer model** often matches higher effort on an older one.
   Try that before spending 5x per token.
2. **A better brief.** Anthropic's multi-agent write-up notes that early prompt
   changes can swing results dramatically - its illustration is a jump from 30%
   to 80% success. Treat the figure as illustrative, but the point holds: this is
   a larger swing than a model upgrade buys, at zero marginal cost.

Model choice, token volume, and tool-call count together explain most of the
variance in agent performance - but the brief is what determines whether those
tokens are spent on the right thing.

## Budget per run

Set a ceiling before you start and put it in every brief.

| Run shape | Workers | Rough ceiling |
|---|---|---|
| Single artifact + verify | 1 + 1 | 1 orchestrator pass, 2 worker passes |
| Comparison | 2-4 parallel + 1 verifier | 15 tool calls per worker |
| Program | 5-10 + verifier per part | Verify per part, synthesize once |

**Stop conditions belong in the brief.** Agents unaware of termination conditions
are a documented failure mode. "Continue until it is good" is not a stop
condition; "stop when the table has all seven rows and each cites a source" is.

## What to measure

Log per run:

- Workers spawned, and their tiers.
- Escalations, and whether the escalation actually fixed the failure.
- Correction rounds used.
- Which checks fired, and which found nothing.

A check that never fires is either a check on something that never breaks - drop
it - or a check written so loosely it cannot fail. Both are worth knowing.

Judge cost **per completed deliverable**, not per request. A cheap run that needs
three correction rounds is not cheap.
