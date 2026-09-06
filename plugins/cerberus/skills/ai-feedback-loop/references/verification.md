# Verification reference

The verifier is the only component that makes the loop worth running. Everything
else is production; this is the part that makes production trustworthy.

## Why a separate verifier

A model asked to critique its own output, with no external signal, frequently
makes the answer *worse*. The failure is not laziness - it is that the same
process that produced the error also produces the assessment of it.

Three findings drive the design:

1. **Self-correction without external feedback degrades accuracy.** The loop must
   contain something the model cannot argue with: a file, an export, a statute,
   a transcript.
2. **The reviewer must not be the author, and must not see the author's working.**
   A verifier that reads the reasoning inherits its blind spots and tends to
   ratify them.
3. **Errors from weaker generators are easier to detect than errors from stronger
   ones.** A frontier verifier over cheap workers is an efficient pairing, not a
   compromise - the cost asymmetry runs in your favour.

## The rubric

One judge. One rubric. Multiple specialized judges score less consistently than a
single rubric-driven judge.

Score each criterion 0.0-1.0, then a hard pass/fail on the whole:

| Criterion | Question | Fails when |
|---|---|---|
| **Factual accuracy** | Does every claim match its source of record? | Any figure, date, name, or number not traceable to a named source |
| **Attribution accuracy** | Does each cited source actually support the claim made from it? | A source is cited but says something narrower or different |
| **Completeness** | Is every brief requirement present, and does the deliverable cover the original request? | A requirement is missing, answered at the wrong depth, or a category of the original request has no owner at all |
| **Source quality** | Are primary sources used where they exist? | A secondary or SEO-optimized source stands in for an available primary one |
| **Brief fidelity** | Has the artifact drifted from its stated purpose? | The conversion goal, neutrality requirement, or voice has shifted |
| **Scope discipline** | Did anything appear that was out of scope? | Content outside the boundary, however good |
| **Tool efficiency** | Were the right sources used, a reasonable number of times? | Repeated searching for a source that does not exist |

A single 0.0 on **factual accuracy** or **brief fidelity** fails the whole
artifact regardless of other scores. These are not averaged.

## What the verifier receives

- The artifact.
- The acceptance criteria from the brief.
- The sources of record it is allowed to check against.
- The original request, so it can check coverage - does the deliverable answer
  what was asked, not only what the briefs listed? A category nobody owned is a
  decomposition failure and is reported as the orchestrator's, not a worker's.

**Not** the worker's reasoning, its intermediate drafts, or its self-assessment.
A verifier that reads the working inherits the blind spots.

## What the verifier returns

Use `templates/verdict.md`. Each failure must be located - the exact line, claim,
or figure. "The tone is off" is not a finding. "Line 34 states X as fact; the
source says Y" is a finding.

## Bias controls

LLM judges have measured, reproducible biases. Apply all four:

1. **Rubric, not preference.** Score against fixed criteria. Do not ask "which is
   better" unless you must.
2. **Randomize order on comparisons, and re-run.** Swapping the presentation order
   of two candidates can shift judgments by more than 10%. If the verdict flips
   on swap, you have a tie, not a winner.
3. **Discount length.** Judges prefer verbose, fluent, formal text regardless of
   substance. State explicitly that length is not a quality signal.
4. **Never self-judge.** Models score their own generations higher. If the worker
   ran on sonnet, the verifier does not run on sonnet. The protocol's one
   exception - a part that must run on `opus` where no `fable` exists - is
   verified in a fresh context at `max` effort, marked *same-tier*, and has its
   blocker-level claims checked by a human (Gate 6).

## The over-reporting problem

A reviewer instructed to find gaps will report gaps even when the work is sound -
that is what it was asked to do. Unchecked, this produces over-engineering:
defensive hedging, redundant caveats, and disclaimers nobody needed.

Put this in the verifier prompt verbatim:

> Report only failures that affect correctness or a stated requirement in the
> brief. Style preferences, alternative phrasings, and things you would have done
> differently are not findings. If the artifact meets the criteria, say so.

## Escalation ladder

| Verdict | Next step |
|---|---|
| Pass | Synthesize. Stop. |
| Fail, located, first time | One correction round with the located failures. |
| Fail, same class, second time | The brief is wrong. Rewrite Gate 3, restart clean. |
| Fail, shrinking | Escalate once - effort (`xhigh`) first; tier only if the worker is already there. |
| Verifier and worker disagree on a fact | Read the source of record yourself. Neither is authoritative. |
| Failure is a judgment call | Escalate to the human as a decision. |

Do not run a third correction round on the same brief. Early rounds carry nearly
all the achievable gain; later ones drift toward the verifier's preferences
rather than the brief's requirements. Two is a discipline, not a measured
optimum - raise it only if your run log shows round 3 earning its cost.

## Calibrating the rubric

Run the verifier against **20 known cases** - ten you know are good, ten with
defects you planted. Anthropic's multi-agent write-up illustrates how much early
prompt changes can move results (its example: 30% to 80% success), which is why
calibration is not optional if the loop will run repeatedly.

Where the verifier disagrees with you, the rubric wording is wrong, not the
verifier. Fix the rubric and re-run the same 20.

## Human evaluation still catches what rubrics miss

Automated judging missed that agents systematically preferred SEO-optimized
content farms over authoritative but lower-ranked sources. No rubric caught it;
a person reading transcripts did. Read a full run's transcript periodically.
