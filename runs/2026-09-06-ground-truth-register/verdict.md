# Verdict — Ground Truth Register generalization

**Artifact:** `plugins/cerberus/skills/ai-feedback-loop/SKILL.md` (Gate 1 table) and
`plugins/cerberus/skills/ai-feedback-loop/references/ground-truth.md` (new)
**Brief:** `brief.md` in this folder
**Verifier tier:** opus, effort xhigh, fresh context (cerberus:loop-verifier)
**Date:** 6 September 2026

## Scores

| Criterion | Score | Basis |
|---|---|---|
| 1. Three cells filled; checks runnable pass/fail by a fresh agent | 0.95 | All 8 SKILL.md rows and all 49 ground-truth.md rows parsed with three non-empty cells; every Check cell names an executable comparison. Deduction: one row named a person as a source (finding 1). |
| 2. Diff confined to the Gate 1 table plus one pointer line | 1.0 | `git diff HEAD`: a single hunk, 9 insertions / 6 deletions; intro, rules and every other gate unchanged; pointer uses the same `${CLAUDE_SKILL_DIR}` form as the three existing pointers and its target exists. |
| 3. No real client, agency, person, company, product or figure | 1.0 | Digit scan finds only list numerals and "the Q3 number"; proper-noun scan returns API, CVE, ID, SKILL only; the one real-world standard at HEAD ("INCI list") was removed. |
| 4. Terminology matches the verifier rubric | 1.0 | "source of record", primary/secondary hierarchy, "source-quality failure", the verdict's "Unverifiable" section and "run log" all match verification.md, verdict.md and run-log.md. |
| 5. `claude plugin validate .` passes | 1.0 | Passed, exit 0 (see Unverifiable for its reach). |
| 6. Line budgets | 1.0 | SKILL.md 221 (< 240); ground-truth.md 173 (≤ 220). |
| 7. Domain-neutral coverage against the original request | 1.0 | 21 general-catalogue rows and 7 packs; marketing is one pack among equals. Software, finance, legal and research each map to a pack plus catalogue rows. Nine further deliverable classes stress-tested; none homeless. Thinnest surface: IP/licensing and rights clearance — reachable via provenance, contractual and dependency rows, not named. |
| 8. Internal consistency with Gate 1 and verification.md | 0.90 | Concordant on runnability, untraceable-figure handling, secondary-source policy, pass/fail preference and disposition of sourceless surfaces. Two minor tensions (findings 1–2). |

**Verdict: PASS**

## Findings

| # | Location | Claim as written | Source of record says | Severity |
|---|---|---|---|---|
| 1 | ground-truth.md:60 | Source of record: "Source-language text plus a bilingual reviewer or reference glossary" | The file's own row test (164–168) and SKILL.md:55 require an artifact a fresh agent can read — not a person | minor |
| 2 | ground-truth.md:148 vs SKILL.md:56 | Disposition 3, "downgrade the claim to opinion", offered for any sourceless claim | Gate 1 rule 2: an untraceable figure is cut or disclosed, never softened | minor |

Both applied by the orchestrator at synthesis (see RUN-LOG.md).

## Unverifiable

- Depth of `claude plugin validate .` — its output names only the marketplace manifest; whether it parses SKILL.md frontmatter or reads `references/` is not shown.
- Runtime resolution of `${CLAUDE_SKILL_DIR}` in the new pointer — not exercised; only the form and the target file's existence were checked.

## Checks not run

- No end-to-end run of the skill against a live software, finance, legal or research deliverable; criterion 7 assessed by mapping deliverable classes onto rows.
- No diff of ground-truth.md against a prior version (new, untracked file).
- Other reference files read or grepped only around the relevant terms.

## Assessment

The register is genuinely domain-neutral and meets all eight criteria; the diff
is exactly the Gate 1 table plus the pointer line. The two findings are minor
internal-consistency snags in ground-truth.md, neither blocking. Separately,
the skill's `description` still framed the skill in marketing terms — outside
the permitted diff, so an orchestrator decision rather than a defect in the
artifact.
