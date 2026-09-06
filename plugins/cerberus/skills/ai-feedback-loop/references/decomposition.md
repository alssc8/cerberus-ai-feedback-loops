# Decomposition reference

How to split work across workers without losing the thing that makes it good.

## The rule

**Group work by the context it needs, not by the stage it belongs to.**

Every handoff between agents is a lossy channel. Natural language is ambiguous,
formats mismatch, and earlier decisions get dropped. Inter-agent misalignment is
the single largest failure category in studied multi-agent systems (~37%), and
the hardest to debug because nothing errors - the output just quietly drifts.

So: minimise handoffs, and make each remaining one carry a written artifact
rather than a spoken summary.

## Phase-based splitting is the trap

The intuitive split is the workflow you already run in your head:

> research -> draft -> edit -> fact-check -> format

This is the wrong split for agents. Each arrow is a handoff, and by the fact-check
stage the agent has no idea why a claim was phrased the way it was. Four handoffs
is four chances for the deliverable to lose its spine.

## Context-based splitting

Ask: **what would one person need loaded in their head to do this part well?**
Everything that answers to the same body of context is one worker.

| Deliverable | Wrong split (phase) | Right split (context) |
|---|---|---|
| Multi-market campaign | researcher / writer / editor | one worker per market, each owning research through copy |
| Subscription funnel diagnostic | data puller / analyst / writer | one worker per funnel stage, each owning its data and its finding |
| Podcast episode into assets | transcript reader / clipper / caption writer | one worker per output surface, each reading the full transcript |
| Competitive teardown | scraper / summarizer / synthesizer | one worker per competitor, each doing full-depth analysis |
| Localization | translator / reviewer | one worker per language, owning register and typography |

Notice the pattern: the right column duplicates *reading* across workers. That is
intentional. Re-reading a transcript in two contexts is cheap; losing the reason
a quote mattered is not.

## When sequential is genuinely required

Only when part B cannot start without part A's *output*, not merely A's existence.

- Real dependency: the message hierarchy must exist before per-audience matrices
  can be written against it.
- Fake dependency: "research first" - workers can research their own slice.

For a real dependency, pass a **file**, not a message. The downstream worker
reads the artifact directly. This is the difference between a handoff and a
telephone game.

## Sizing

| Signal | Split further |
|---|---|
| One worker's brief lists more than ~5 acceptance criteria | Yes |
| Two parts share no sources | Yes |
| Two parts share most sources and one voice | No - keep together |
| A part cannot be verified independently | No - it is not a part |

That last row matters most. **If you cannot verify a piece on its own, it is not
a valid unit of work.** Merge it into the piece it depends on.

## Boundaries

Every brief names its neighbours and what belongs to them. Without this, workers
duplicate the easy parts and leave the seams empty. The seam between two workers
is the orchestrator's responsibility, and it is where synthesis earns its cost.

Write the boundary as a prohibition, not a description:

> Do not write the pricing recommendation - worker 3 owns it. If your analysis
> implies a pricing change, state the implication and stop.

## Parallel execution

Default to parallel. It is the main reason the pattern is worth its token cost -
parallel exploration cuts wall-clock time dramatically on broad tasks.

Launch every independent worker in a single dispatch rather than one at a time.
Sequential dispatch of independent work is pure latency with no benefit.
