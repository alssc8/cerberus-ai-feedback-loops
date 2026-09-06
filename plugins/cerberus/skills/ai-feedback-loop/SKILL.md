---
name: ai-feedback-loop
description: Produce a checkable deliverable through a verified multi-agent loop - strategy documents, funnel diagnostics, fact-checked editorial, campaign systems, client reports, competitive teardowns, localization. A frontier orchestrator decomposes, briefs and verifies; cheaper models produce. Use when the work has an external source of truth and is too large or too consequential for one pass.
when_to_use: Invoke manually with /cerberus:ai-feedback-loop - this skill never fires on its own. Reach for it when a deliverable spans several markets, platforms or funnel stages, when every claim must be checked against an external source of record, or when a wrong claim reaching a client carries legal, factual or financial cost.
disable-model-invocation: true
---

# AI Feedback Loop

You are the **orchestrator**. Brief, verify, synthesize. Do not draft the
deliverable — workers do that.

Work through the gates in order. Each gate blocks the next.

---

## Gate 0 — Triage

Run the loop only if **two or more** hold:

- The deliverable has an external source of truth.
- Parts need different context, not just different steps.
- Inputs exceed one context window.
- A wrong claim reaching a client is expensive.
- The output is reusable IP.

Otherwise say "single pass plus one verification round" and stop.

Do not run the loop for: work with no checkable output, parts needing tight
real-time coordination, or anything describable in one sentence of instruction.

---

## Gate 1 — Ground truth register

Do not pass this gate without a check. State what would prove the output wrong.

Build the register — one row per factual surface:

| Claim surface | Source of record | Check |
|---|---|---|
| Metrics, conversion, traffic | Raw export or first-party dashboard | Recompute; flag anything untraceable |
| Product or ingredient claims | Spec sheet, INCI list, supplier doc | Line-by-line match |
| Quotes | Source transcript, timestamped | Verbatim; speaker intent preserved |
| Legal or promotional terms | Regulation text, statute, platform policy | Clause by clause; re-read dates and numbers |
| Competitor or market claims | Named primary source, dated | Primary required; secondary is a finding |
| Brief compliance | The brief file | Each requirement present, nothing extra |
| Voice | Voice guide, approved assets | Name the failure type: tonal, structural, factual, conversion-logic |

Rules:

- Every check must be runnable by a fresh agent with no memory of the work.
- An untraceable figure is a finding. Cut it or disclose it. Never soften it.
- Prefer pass/fail checks over opinions.
- A claim surface with no source of record goes in Gate 7 open items.

---

## Gate 2 — Decompose by context

Split by the context the work needs, never by workflow stage.

Cap agent count:

| Task shape | Agents | Tool calls each |
|---|---|---|
| Single fact, single source | 0 — do it yourself | 3–10 |
| One artifact, one context | 1 worker + 1 verifier | 10–15 |
| Comparison across 2–4 surfaces | 2–4 workers, parallel | 10–15 |
| Multi-market or multi-platform | 5–10 workers, hard boundaries | 15+ |

Default to parallel. Dispatch independent workers in one message. Use sequential
only for a real output dependency, and pass a file, not a summary.

See `${CLAUDE_SKILL_DIR}/references/decomposition.md` when the split is unclear.

---

## Gate 3 — Brief

Six mandatory fields per worker. Template:
`${CLAUDE_SKILL_DIR}/templates/brief.md`

1. **Objective** — one sentence, the outcome not the activity.
2. **Output format** — structure, length, destination file.
3. **Sources and tools** — named; allowed and forbidden.
4. **Boundaries** — what this worker must not touch, and who owns it. Write as a
   prohibition.
5. **Acceptance criteria** — the Gate 1 checks that apply here. Three to five.
6. **Budget and stop condition** — a ceiling, and an explicit end.

A brief missing a field is the most common cause of a wasted run.

---

## Gate 4 — Produce

Delegate to `loop-implementer` (sonnet) or `loop-researcher` (haiku).

Hold the brief while they work. Watch for drift: a lead-capture page becoming a
sales page, a neutrality requirement dropped, scope creeping outward.

Require the artifact **plus its evidence** — the command run, the export read,
the line quoted. Reject an assertion of success with no evidence.

---

## Gate 5 — Verify

Delegate to `loop-verifier`. Never the agent that produced the work, and not its
model tier — with one disclosed exception, defined in Gate 6.

The verifier receives the artifact and the criteria only — never the drafting
conversation. One judge, one rubric. Returns
`${CLAUDE_SKILL_DIR}/templates/verdict.md`: 0.0–1.0 per criterion, pass/fail,
each failure located.

Bias controls:

- Score against the rubric, not as a preference between drafts.
- Randomize order on comparisons and re-run. Verdict flips on swap = a tie.
- Length is not a quality signal.
- No tier grades its own output. Sole exception: a part that must run on `opus`
  on a machine without `fable` — Gate 6 says how, and the verdict is marked
  *same-tier*.

Instruct the verifier: report only failures affecting correctness or a stated
requirement. Style preferences are not findings.

Rubric: `${CLAUDE_SKILL_DIR}/references/verification.md`

---

## Gate 6 — Bounded iteration

Two correction rounds maximum. Then pick one:

| Situation | Action |
|---|---|
| Same failure twice | The brief is wrong. Rewrite Gate 3, restart clean. |
| Failures shrinking | Escalate once — effort first (`sonnet` at `xhigh`); tier only if the part is already there. |
| Verifier and worker disagree on fact | Read the source of record yourself. |
| Failure is a judgment call | Escalate to the human as a decision. |

Do not start round three.

**Escalating a worker to `opus` costs you the independent verifier**, whose
frontmatter pins `opus`. Effort escalation avoids that entirely, which is why it
comes first. If the part genuinely must move to `opus`:

- `fable` available: override `loop-verifier` to `fable` for that check.
- No `fable`: verify on `opus` in a fresh context at `max` effort, mark the
  verdict *same-tier* in the run log, and check every blocker-level claim
  against the source of record yourself. This is the protocol's one same-tier
  exception — disclosed, never silent.

---

## Gate 7 — Synthesize and log

Merge verified parts into one voice. Resolve worker conflicts explicitly. State
what is not covered.

Append to the run log (`${CLAUDE_SKILL_DIR}/templates/run-log.md`):

1. Defect **class** the verifier caught, not the instance.
2. Which brief field was missing when a worker went wrong.
3. Escalations, and whether they were needed.
4. Checks that should exist and did not.

Promote a defect class to a permanent Gate 1 criterion on second sighting.

---

## Output contract

- The deliverable.
- Verdict table: criterion, score, source of record.
- **Open items** — every unverifiable claim, named.
- Cost: agents, tiers, escalations.

Never report a run as clean when a check was skipped. Name it and say why.

---

## Tiers

| Role | Model | Use for |
|---|---|---|
| Orchestrator, hard verification | `fable` where available, else `opus` | Decomposition, adversarial verification, synthesis |
| Orchestrator, default verifier | `opus` | Most orchestration |
| Implementer | `sonnet` | Drafting, structured production, analysis |
| Researcher | `haiku` | Retrieval, extraction, mechanical transformation |

The orchestrator is whichever model runs the session — `opus` or better
recommended. On a `sonnet` session the loop still holds, because the verifier is
pinned to `opus` in its own frontmatter. Nothing in the protocol requires `fable`;
it is the preferred verifier for escalated or high-stakes checks when present.

Orchestrator output costs 2.5–10× worker output depending on the pairing (opus
over sonnet is 2.5×; fable over haiku is 10×). The orchestrator stays short:
brief, verify, synthesize — never draft.

**Effort is the dial within a tier — turn it before changing models.** The
verifier runs `xhigh`; drop an implementer to `medium` effort for routine
production before dropping it to a smaller model. `haiku` has no effort dial.

Rates and cascade rules: `${CLAUDE_SKILL_DIR}/references/cost-governance.md`

## Why these rules

Evidence, sources and the failure data behind each gate:
`${CLAUDE_SKILL_DIR}/references/evidence.md`
