# Why these rules

The evidence behind each gate. Loaded on demand — read it when a rule looks
arbitrary, when someone asks you to justify the protocol, or when deciding
whether to change a threshold.

---

## Gate 0 — why triage exists

Agents use roughly **4× the tokens of a chat**; multi-agent systems roughly
**15×**. Anthropic reports teams spending months on elaborate multi-agent
architectures only to find that better prompting of a single agent matched them.

Multi-agent pays off on breadth-first work that parallelises, exceeds one context
window, or interfaces with many tools. It does not pay off on tasks needing tight
real-time coordination between parts.

## Gate 1 — why an external check is mandatory

**Intrinsic self-correction fails.** When a model reviews and revises its own
reasoning with no outside signal, accuracy frequently degrades rather than
improves (Huang et al., *Large Language Models Cannot Self-Correct Reasoning
Yet*, ICLR 2024).

The well-known positive results are not all the same shape, and the difference
matters.

**Reflexion** put an external signal in the loop — its agents received
test-execution results (91% HumanEval pass@1, up from 80%). That is the pattern
this protocol copies.

**Self-Refine did not.** It is explicitly a single-model loop: the same model
generates, critiques and revises, with no external signal and no tool. Its
reported ~20% average gain and Huang et al.'s negative result are in genuine
tension. The reconciliation Huang et al. offer is that self-refinement gains
concentrate on tasks where "better" is a preference or surface property, while on
reasoning tasks with no oracle available, self-correction degrades accuracy.

Treat that as unsettled rather than resolved. This protocol takes the
conservative reading: build the loop around a signal the model cannot argue with,
and do not rely on self-critique where correctness is what is at stake.

## Gate 2 — why context, not phase

Inter-agent misalignment accounts for roughly **37%** of observed multi-agent
failures and is the hardest category to debug, because nothing errors — the
output drifts (Cemri, Pan, Yang et al., *Why Do Multi-Agent LLM Systems Fail?*,
MAST taxonomy, 1,600+ traces across 7 frameworks, NeurIPS 2025).

Phase-based splits maximise handoffs, and every handoff is where context is lost.
Context-centric decomposition minimises them.

The effort-scaling numbers come from Anthropic's production research system:
1 agent with 3–10 tool calls for simple fact-finding, 2–4 subagents with 10–15
calls each for comparisons, 10+ for complex research. Early versions without
these caps spawned 50 subagents for simple queries.

## Gate 3 — why six mandatory fields

Three MAST failure modes are brief failures, not model failures:

| Mode | Share |
|---|---|
| Step repetition | 15.7% |
| Unaware of termination conditions | 12.4% |
| Disobey task specification | 11.8% |

Together with inter-agent misalignment that is over 60% of failures, all
addressable by a written brief with explicit boundaries and an explicit stop
condition. Vague delegation produced duplicated and misaligned work in
Anthropic's system until briefs carried an objective, an output format, tool and
source guidance, and clear task boundaries.

## Gate 5 — why a separate verifier, and why frontier over cheap

Three findings from *Variation in Verification: Understanding Verification
Dynamics in Large Language Models* (Zhou et al.):

1. **Errors from weaker generators are easier to detect than errors from stronger
   ones.** A capable model's mistakes are plausible and camouflaged.
2. Verifiers certify correct answers more reliably on easy problems; error
   detection does not track difficulty the same way.
3. In one measured case verification closed **75.7%** of the gap between a small
   model and one three times its size.

Point 1 is why the architecture is sound rather than merely cheap: the model
whose errors are easiest to catch is also the cheapest to run. The cost asymmetry
and the detection asymmetry run the same direction.

The limit is real too — general-purpose judges are not oracles, and a stronger
verifier buys less over a weaker one than intuition suggests. Verification
supplements ground truth; it does not substitute for it.

**One judge, one rubric:** Anthropic found a single judge outputting 0.0–1.0 with
a pass/fail grade more consistent than multiple specialised judges.

**Judge biases** (Zheng et al., *Judging LLM-as-a-Judge*; Wataoka et al.,
*Self-Preference Bias in LLM-as-a-Judge*):

- **Position** — swapping candidate order can shift judgments by more than 10%.
- **Verbosity** — judges prefer longer, more fluent, more formal text regardless
  of substance, though this has weakened across model generations.
- **Self-preference** — models score text closer to their own distribution
  higher, measured via perplexity.

**Over-reporting:** a reviewer told to find gaps reports gaps whether or not they
exist. Chasing every finding produces defensive hedging, redundant caveats and
abstraction nobody needed.

## Gate 6 — why bounded

Two rounds is a **discipline, not a measured optimum**. The claim circulating in
practitioner writing — that rounds one and two capture ~75% of achievable
improvement — traces to secondary blog sources, not a primary study. Treat it as
a convention with a rationale: later rounds drift toward the verifier's
preferences rather than the brief's requirements.

Raise the cap only if your own run log shows round three earning its cost.

The escalation ladder is cascade routing. FrugalGPT (Chen, Zaharia & Zou) reports
matching top-model quality at up to **98%** cost reduction by escalating only on
failure. RouteLLM (Ong et al.) held 95% of frontier performance while sending 14%
of queries to the strong model — note it routes upfront on a predicted difficulty
score rather than escalating on failure, so it supports "route cheap first" but is
not evidence for this protocol's failure-triggered ladder specifically.

## Gate 7 — why the run log is not optional

MIT NANDA's *The GenAI Divide: State of AI in Business 2025* reviewed 300+
disclosed initiatives, 52 structured interviews and 153 survey responses. Against
$30–40bn of enterprise spend, **95% of organisations reported no measurable P&L
return**.

The diagnosis was not talent, infrastructure or regulation. It was the learning
gap: tools that could not retain feedback, adapt to context, or improve over time.
The 5% that worked were solving for memory, learning and workflow adaptation.

A loop that does not write down what it caught is the same loop, run again, at
full price, making the same mistake.

## Where the method comes from

Constitutional AI (Bai et al., Anthropic) trained a model to critique and revise
its outputs against a written set of principles, then used AI-generated
preferences in place of human labels. The load-bearing ingredient was the
constitution — an explicit, external, written standard. Every rubric here is a
small constitution and works for the same reason.

Multi-agent debate (Du, Li, Torralba, Tenenbaum & Mordatch, MIT CSAIL, ICML 2024)
reaches the same end differently: several model instances propose and critique
each other's reasoning across rounds, improving factuality on mathematical and
strategic reasoning. Useful when there is no external source of record to check
against; more expensive and less decisive when there is.

---

## Sources

- Huang et al., [Large Language Models Cannot Self-Correct Reasoning Yet](https://arxiv.org/abs/2310.01798) — ICLR 2024
- Madaan et al., [Self-Refine](https://arxiv.org/abs/2303.17651)
- Shinn et al., [Reflexion](https://arxiv.org/abs/2303.11366)
- Zhou et al., [Variation in Verification](https://arxiv.org/abs/2509.17995)
- Du et al., [Improving Factuality and Reasoning through Multiagent Debate](https://arxiv.org/abs/2305.14325) — MIT CSAIL, ICML 2024
- Cemri, Pan, Yang et al., [Why Do Multi-Agent LLM Systems Fail?](https://arxiv.org/abs/2503.13657) — MAST, NeurIPS 2025
- Bai et al., [Constitutional AI](https://arxiv.org/abs/2212.08073)
- MIT NANDA, [The GenAI Divide: State of AI in Business 2025](https://www.aigl.blog/state-of-ai-in-business-2025/)
- Chen, Zaharia & Zou, [FrugalGPT](https://arxiv.org/abs/2305.05176)
- Ong et al., [RouteLLM: Learning to Route LLMs with Preference Data](https://arxiv.org/abs/2406.18665)
- Zheng et al., [Judging LLM-as-a-Judge](https://arxiv.org/abs/2306.05685)
- Wataoka et al., [Self-Preference Bias in LLM-as-a-Judge](https://arxiv.org/abs/2410.21819)
- Anthropic, [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)
- Anthropic, [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)
- Anthropic, [When to use multi-agent systems](https://claude.com/blog/building-multi-agent-systems-when-and-how-to-use-them)
