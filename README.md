# Cerberus

Claude Code plugins where nothing ships unchecked. Currently one plugin, also
called **cerberus**: a multi-agent delivery loop in which a frontier model
orchestrates and verifies while cheaper models produce, and every claim is
checked against an external source of record before it reaches anyone.

The name is the architecture. Three heads — an implementer, a verifier, a
researcher — guarding eight gates that a deliverable has to pass in order.

## Installing

```
/plugin marketplace add https://github.com/alssc8/cerberus-ai-feedback-loops.git
/plugin install cerberus@cerberus
```

If the install summary says to, run `/reload-plugins`. Then:

```
/ai-feedback-loop
```

(or the fully qualified `/cerberus:ai-feedback-loop`)

The `alssc8/cerberus-ai-feedback-loops` shorthand works too, but GitHub
shorthand sources clone over **SSH**, so it fails for anyone without an SSH key
registered with GitHub — there is no automatic fallback to HTTPS. The full URL
above has no such requirement, which is why it is the one to hand round.

## Requirements

Any Claude Code account with the standard `opus`, `sonnet` and `haiku` aliases.
**Fable is optional.** Where it exists, the skill uses it for escalated and
high-stakes verification; where it does not, the skill escalates effort instead
of model tier and discloses any same-tier verification in the run log. Nothing
fails or silently degrades on a machine without it.

The effort settings assume first-party Claude Code (Anthropic API or claude.ai),
where `opus` resolves to Opus 5 and `sonnet` to Sonnet 5. On Bedrock, Vertex or
Foundry those aliases can resolve to 4.6-generation models that lack the `xhigh`
level — pin full model IDs in the agent files if you install there.

Run the session on `opus` if you can. It also works on `sonnet`: the verifier is
pinned to `opus` in its own definition, so verification stays independent of
whatever your session runs on.

## What cerberus gives you

One skill and three agents.

**`/ai-feedback-loop`** — an eight-gate protocol for producing a deliverable that
has been checked against something real. The orchestrator decomposes the work,
writes a brief per worker, and verifies the result in a fresh context. It does
not draft.

**The three heads** — used by the skill, also callable directly:

| Agent | Model | Effort | Job |
|---|---|---|---|
| `loop-implementer` | sonnet | high | Produces one artifact from a six-field brief |
| `loop-verifier` | opus | xhigh | Adversarial check against sources, fresh context |
| `loop-researcher` | haiku | — | Evidence with verbatim citations |

`loop-verifier` is worth knowing about on its own. Point it at any deliverable
plus the sources it should match and it returns a located, severity-graded
verdict — no rewriting, no opinions on style.

## The one rule

If you cannot name what would prove the output wrong, the loop will not help you.
A model correcting itself with no external signal tends to get worse, not better —
so Gate 1 stops the run before you spend anything. It refusing to run is the
feature, not a bug.

Sources of truth in practice: a raw analytics export, an ingredient list, a
regulation text, a timestamped transcript, the brief file itself.

## Deliberate design choices

- **`disable-model-invocation: true`** — the skill never fires on its own.
  Multi-agent runs cost roughly 15× a chat, so starting one is your decision,
  not Claude's.
- **Detail lives in `references/`** — loaded only when needed, so it costs
  nothing until it's read. `references/evidence.md` carries the research behind
  each gate that rests on a published finding.
- **The author never grades.** Enforced structurally: the verifier is a
  different agent on a different tier and never sees the drafting conversation.
  The single exception — an `opus` worker on a machine without Fable — is
  disclosed in the verdict and has its blockers checked by a human.
- **Effort before tier.** Every escalation raises effort first; model tier only
  when a part is already at `xhigh`.
- **Workers cannot delegate.** Every agent pins an explicit `tools:` list, and
  none of them can spawn a subagent — so no worker can route around the verifier
  by launching its own.

## Not yet done

There is **no eval set**. Nothing here has been measured against known-correct
cases — the design follows published findings, but its effectiveness on your
work is unverified. Building one (ten clean deliverables, ten carrying defects
you have actually caught) is the next step, and the skill's own advice.

## Editing and releasing

Edit files in `plugins/cerberus/`, then test the change locally before pushing:

```
claude --plugin-dir ./plugins/cerberus
```

Bump `version` in `plugins/cerberus/.claude-plugin/plugin.json`. That is the
only place it is pinned: the marketplace entry deliberately omits `version`,
because when both set it `plugin.json` wins silently and a stale entry can mask
the version you meant to ship. Validate:

```
claude plugin validate .
```

Commit and push. Colleagues pick up the release with:

```
/plugin marketplace update cerberus
/plugin update cerberus@cerberus
```

then run `/reload-plugins`. **Without the version bump, nobody receives the
change** — the marketplace refresh alone does not reinstall a plugin whose
version hasn't moved.

After editing your own copy, `/reload-plugins` picks up the change without
restarting the session.
