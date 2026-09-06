# Worker brief — generalize the Ground Truth Register

Run of the ai-feedback-loop protocol on the plugin itself. 6 September 2026.
Orchestrator: Fable 5.1 (this session). Worker: cerberus:loop-implementer.
Verifier: cerberus:loop-verifier.

## Objective

Make Gate 1's Ground Truth Register domain-neutral so the plugin serves any
kind of deliverable, with a full catalogue and domain packs in a reference file.

## Output format

1. `plugins/cerberus/skills/ai-feedback-loop/SKILL.md` — replace ONLY the Gate 1
   table. Max 12 rows, three columns (Claim surface | Source of record | Check),
   domain-neutral. Add one pointer line after it to
   `${CLAUDE_SKILL_DIR}/references/ground-truth.md`. Everything else byte-identical.
   File stays under 240 lines.
2. `plugins/cerberus/skills/ai-feedback-loop/references/ground-truth.md` — new,
   max 220 lines: what counts as a source of record; the general catalogue
   (18–22 claim surfaces); domain packs (marketing & editorial, software & data,
   finance & accounting, legal & compliance, research & science, operations,
   design & localization); what to do when no source exists; how to add rows.

## Sources and tools

ALLOWED: read everything under `plugins/cerberus/` to match terminology.
FORBIDDEN: editing any other file; web search; real client, agency, person or
figure in any example.

## Boundaries

Do not touch verification.md, verdict.md, README, agents, or any other gate.
Do not renumber gates. The diagram and the version bump are the orchestrator's.

## Acceptance criteria

1. Every row in both files has all three cells; each check is phrased so a fresh
   agent can run it and get pass/fail.
2. The SKILL.md diff touches only the Gate 1 table and the pointer line.
3. No real client/agency/personal names or real figures anywhere.
4. Terminology matches the verifier rubric (source of record, primary/secondary,
   blocker/material/minor, Unverifiable, run log).
5. `claude plugin validate .` passes from the marketplace root.
6. SKILL.md < 240 lines; ground-truth.md ≤ 220 lines.

## Budget and stop condition

20 tool calls. Stop when both files exist, validate passes, and both have been
re-read once. Return paths, line counts, validate output, Unverifiable, Not done.
