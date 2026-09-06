# Worker brief

Copy this per worker. All six fields are mandatory. A brief missing a field is
the most common cause of a wasted worker run.

---

## Objective

<!-- One sentence. The outcome, not the activity.
     Bad:  "Research the market."
     Good: "Produce the funnel-stage diagnostic for the newsletter subscription flow,
            with each stage's conversion rate traced to the subscriptions export." -->

## Output format

<!-- Exact structure, length, and destination file.
     - File: outputs/funnel-diagnostic.md
     - Structure: one table (stage | rate | YoY delta | source), then findings
     - Length: findings under 400 words
     - Every figure carries an inline source label. -->

## Sources and tools

<!-- Named. Allowed and forbidden.
     ALLOWED:   data/subscriptions-export.csv, data/analytics-raw.csv, WebFetch on the client site
     FORBIDDEN: web search for market benchmarks (worker 3 owns that);
                any figure not present in the two exports above. -->

## Boundaries

<!-- What this worker must not touch, and who owns it. Write as a prohibition.
     - Do not write pricing recommendations. Worker 3 owns pricing.
       If your analysis implies a pricing change, state the implication and stop.
     - Do not draft client-facing copy. -->

## Acceptance criteria

<!-- The Gate 1 checks that apply here. Each must be checkable by a fresh agent
     that has never seen your work. Aim for 3-5.
     1. Every rate in the table recomputes from the named export.
     2. Any figure not derivable from the exports is listed under "Unverifiable"
        rather than stated.
     3. YoY deltas name both periods explicitly.
     4. No claim about competitor performance appears anywhere. -->

## Budget and stop condition

<!-- A ceiling and an explicit end.
     - Ceiling: 15 tool calls.
     - Stop when: the table has every stage, each with a source label, and the
       findings section names the single largest drop. Do not continue past that.
     - If a required export is missing, stop and report the gap. Do not estimate. -->

---

## Return contract

Return the artifact **plus its evidence**:

- The file you wrote.
- For each figure: the file and the computation that produced it.
- Anything you could not verify, listed explicitly.
- What you did not do, and why.

Do not assert success. Show the check.
