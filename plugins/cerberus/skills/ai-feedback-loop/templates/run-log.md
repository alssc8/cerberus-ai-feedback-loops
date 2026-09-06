# Run log

Append one block per run. This file is the only part of the system that improves -
without it, the loop repeats its mistakes at full price every time.

Most deployed AI work produces no measurable return, and the studied reason is
not model quality: it is that the systems cannot retain feedback or adapt to
context. This file is the countermeasure.

---

## Run: <!-- YYYY-MM-DD - deliverable name -->

**Shape:** <!-- workers x tier, verifier tier, rounds used -->
**Outcome:** <!-- shipped / escalated to human / abandoned -->

### What the verifier caught

<!-- The defect CLASS, not the instance.
     Not: "line 34 said 4.2% instead of 0.9%"
     Yes: "worker restated a prior-year figure as current when the export had
          both columns - class: period ambiguity in source data" -->

### Which brief field was missing

<!-- When a worker went wrong, which of the six fields would have prevented it?
     This is the highest-value line in the log. Most worker failures are brief
     failures. -->

### Escalations

| Part | From | To (effort or tier) | Same-tier verdict? | Did it fix it? |
|---|---|---|---|---|

<!-- An escalation that did not fix the failure means the problem was the brief
     or the ground truth, not the model. Note that. -->

### Checks that should exist but did not

<!-- Anything the verifier could not check because no source of record was
     registered. Add it to the Gate 1 register before the next run. -->

### Cost

<!-- Workers spawned, rounds used, and whether the run was worth the multi-agent
     overhead versus a single pass. Be honest when it was not. -->

---

## Standing defect classes

Promoted from individual runs once seen twice. These become permanent
acceptance criteria.

| Defect class | First seen | Now checked by |
|---|---|---|
| | | |

---

## Retired checks

Checks that never fired across many runs. Either the thing never breaks, or the
check was written too loosely to fail. Record which, and remove it.

| Check | Runs survived | Reason retired |
|---|---|---|
| | | |
