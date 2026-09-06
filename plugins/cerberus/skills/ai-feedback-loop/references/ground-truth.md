# Ground truth reference

Gate 1 asks for a register, one row per claim surface. This file is the full
catalogue behind the short table in SKILL.md, plus domain packs for
deliverables the short table cannot name individually.

---

## What counts as a source of record

A source of record is the artifact a claim must trace back to before it ships.
Not the model's account of that artifact — the artifact itself.

Hierarchy, strongest first:

1. **Execution or measurement** — run the code, recompute the figure, take the
   reading. Nothing beats reproducing the result yourself.
2. **Primary document** — the export, the contract, the transcript, the spec
   sheet, the dataset as generated.
3. **Official record** — a registry, filing, statute, or system of record
   maintained by an authority other than the worker.
4. **Reputable secondary source** — a citation of the primary, used only when
   the primary is unavailable, and flagged as secondary in the register.

Primary sources describe the thing itself; secondary sources describe someone
else's description of the thing. A secondary source standing in for an
available primary one is a source-quality failure, not a citation.

**The model's own memory is never a source of record.** Training data, prior
runs, and "I recall that…" do not count, however confident the phrasing.
Anything resting only on memory belongs in the verdict's Unverifiable section,
not in the body of the deliverable.

A check only qualifies if a fresh agent — one with no memory of the work, no
access to the drafting conversation, and only the named source — can run it
and return pass or fail. If the check requires reasoning the fresh agent
cannot reproduce, it is not a check yet.

---

## General catalogue

| Claim surface | Source of record | Check |
|---|---|---|
| Quantitative claims (rates, totals, calculations) | Raw export or dataset as generated | Recompute from the export; every figure reproduces or it is a finding |
| Behaviour of code or systems | Running code, test suite, execution trace | Execute it; observed output must match the claim |
| Configuration or environment claims | The config file or deployed environment itself | Read the live file; a comment or doc is not the config |
| Quotations and attributed statements | Source transcript or recording, timestamped | Verbatim match; speaker and context preserved |
| Third-party or competitor claims | Named primary source, dated | Primary required; secondary is a finding, not a citation |
| Legal or regulatory terms | Statute or regulation text, current version | Clause by clause; re-read every date and number |
| Contractual terms | The executed contract or signed agreement | Clause by clause against the signed document, not a summary |
| Financial figures | Ledger, statement, or reconciled export | Recompute; reconcile to the statement, not a prior draft |
| Identities and dates | Official record: registry, filing, calendar, ID | Cross-check spelling, date and identity against the record |
| Provenance or authorship claims | Chain-of-custody record or original artifact | Confirm origin against the record, not the summary |
| Scientific or research claims | The dataset, methods section, or published study | Trace the number to the study; note sample size and method |
| Statistical claims | The underlying dataset and stated method | Recompute with the stated method; flag if the method is unstated |
| Process or procedure claims | The documented procedure or standard operating record | Step matches the record; deviations are named, not implied |
| Requirement or brief compliance | The brief file | Each requirement present, nothing extra |
| Voice, format or style requirements | Style guide, template, or approved reference asset | Name the failure type: tonal, structural, factual, or logical |
| Translation or localization accuracy | Source-language text plus the reference glossary | Back-translate or compare term-by-term against the glossary |
| Design or accessibility compliance | The design system spec or the accessibility standard cited | Check each element against the spec, not against taste |
| Timeline or sequence-of-events claims | Dated primary records (logs, filings, timestamps) | Order events from the timestamps; a narrative is not a timestamp |
| Risk or safety claims | Incident record, test result, or safety data sheet | Trace the claim to the record; absence of a record is a gap |
| Claims about external events | Reputable primary account, dated | Trace to the primary account; secondary-only is disclosed |

---

## Domain packs

Short, domain-specific extensions of the general catalogue. Use these when the
work sits squarely in one domain; use the general catalogue when it spans
several.

### Marketing & editorial

| Claim surface | Source of record | Check |
|---|---|---|
| Campaign metrics | First-party analytics export | Recompute from the export; flag anything untraceable |
| Product or ingredient claims | Spec sheet or supplier documentation | Line-by-line match against the spec |
| Customer quotes or testimonials | Signed release or timestamped transcript | Verbatim; consent on file |
| Voice and tone | Approved brand voice guide | Name the failure type: tonal, structural, factual |
| Competitor comparisons | Named primary source, dated | Primary required; secondary is a finding |

### Software & data

| Claim surface | Source of record | Check |
|---|---|---|
| Function or API behaviour | The code itself, executed | Run it; output must match the documented claim |
| Test coverage claims | Test suite run output | Re-run the suite; count matches the claim |
| Performance claims | Benchmark run on the stated hardware | Reproduce the benchmark; figures within stated tolerance |
| Data pipeline claims | The pipeline's own logs or output dataset | Re-run the pipeline; row counts and schema match |
| Security or dependency claims | Dependency manifest or scan report | Re-run the scan; version and CVE match |

### Finance & accounting

| Claim surface | Source of record | Check |
|---|---|---|
| Reported figures | General ledger or reconciled statement | Recompute; reconciles to the statement, not a prior draft |
| Period-over-period comparisons | Dated ledger extracts for both periods | Both periods named explicitly; figures pulled from each extract |
| Tax or regulatory figures | Filed return or current tax code | Match to the filed document or current code text |
| Valuation or projection assumptions | Named model and its inputs | Inputs traced to source; assumption labeled as assumption |

### Legal & compliance

| Claim surface | Source of record | Check |
|---|---|---|
| Statutory or regulatory claims | Current statute or regulation text | Clause by clause; check the effective date |
| Contractual obligations | The executed contract | Clause by clause against the signed document |
| Case or precedent claims | The published opinion or docket entry | Citation resolves to the actual holding, not a headnote |
| Compliance status claims | Audit report or certification record | Status matches the current, dated record |

### Research & science

| Claim surface | Source of record | Check |
|---|---|---|
| Study findings | The published study or dataset | Trace the number to the study; note sample size and method |
| Statistical significance claims | Underlying data and stated test | Recompute with the stated test; flag if unstated |
| Citation claims | The cited source itself | The source says what the citation claims, not something narrower |
| Methodology claims | The registered protocol or methods section | Method as executed matches the method as described |

### Operations & process

| Claim surface | Source of record | Check |
|---|---|---|
| Process step claims | Documented standard operating procedure | Step matches the record; deviations named |
| Throughput or capacity claims | Operational logs or system telemetry | Recompute from logs; flag anything untraceable |
| Incident claims | Incident report or ticketing record | Timeline and cause match the record |
| Vendor or supplier claims | Signed agreement or supplier documentation | Terms match the document, not a summary |

### Design & localization

| Claim surface | Source of record | Check |
|---|---|---|
| Design system compliance | The design system specification | Each element checked against the spec, not against taste |
| Accessibility claims | The accessibility standard cited (e.g. contrast ratio rule) | Measure the element; value meets the stated threshold |
| Translation accuracy | Source-language text and reference glossary | Term-by-term comparison; idioms flagged, not literalized |
| Locale-specific formatting | The target locale's formatting convention | Dates, currency and units match the convention, not the source locale |

---

## When no source of record exists

A claim surface with no available source of record has exactly four honest
dispositions:

1. **Obtain it** — get the export, the document, the transcript before shipping.
2. **Disclose it as unverified** — ship with the gap named, not hidden.
3. **Downgrade the claim to opinion** — reframe as a judgment, not a fact.
   Non-quantitative claims only: an untraceable figure is cut or disclosed,
   never reframed (Gate 1, rule 2).
4. **Cut it** — remove the claim entirely.

There is no fifth option. It never gets softened into confident prose, and it
never gets rounded to a plausible-sounding figure. Every such claim goes in the
verdict's Unverifiable section, and the gap goes in the run log so the next
run starts with a source instead of rediscovering the same hole.

---

## Adding your own rows

A claim surface earns a row only if it passes this three-column test:

1. **Name the surface** — the type of claim, stated generally enough to cover
   more than one instance (not "the Q3 number" but "quantitative claims").
2. **Name the artifact that settles it** — the specific document, export,
   execution, or record a stranger could go and read. Not a person's
   recollection, not "ask the team."
3. **Write the check so someone who never saw the work could run it** — a
   fresh agent, given only the artifact, must be able to return pass or fail
   without asking the original worker anything.

If you cannot fill the third column with something a stranger could actually
execute, you do not have a check — you have a hope. Do not add the row until
you can.
