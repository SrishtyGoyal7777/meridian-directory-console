# Meridian Directory Accuracy Console

A working prototype of a provider directory accuracy console for Meridian Health Plan,
an invented mid-size US payer. Single static page, no backend, no login, no build step.

## What it does
Scores every provider listing for directory risk, ranks the remediation queue, records
verification field by field, and recomputes every KPI, chart and map from the underlying
data as you work.

- **Executive dashboard** - verified in cycle, sampled accuracy, removal-window breaches,
  high-risk records, linked complaints; recommendation mix, six-month trend, risk by state.
- **Segments** - slice by state, specialty, group, type and risk band.
- **Action queue** - ranked by explainable risk score, sortable, bulk assignment, CSV export.
- **Record detail** - field-level verification outcomes, score breakdown, audit history.
- **Outreach planner** - campaign targeting with a channel capacity model.
- **Data quality** - proportional stratified sampling with a 95% confidence margin, plus an
  audit-readiness check.
- **Duplicates** - candidate pairs on NPI or name, state and phone, with merge.
- **Insights** - steps and time per verification, measured from your own interaction.
- **Settings** - editable cycle lengths, removal window, thresholds and score weights.

## Rules implemented
- Verification cycle 90 days, 365 for hospitals.
- Removal breach when a recorded departure is more than 5 business days old.
- Risk score (capped at 100, high risk at 50): never verified 40, past cycle 30, past 75%
  of cycle 15, roster over 120 days 15, roster 60-120 days 7, address mismatch 20, no
  claims in 12 months 15, two or more failed outreach attempts 10.
- Confidence: High with 3-4 signals, Medium with 2, Low with 0-1.
- Sample accuracy margin: 1.96 x sqrt(p(1-p)/n).

## Data
600 records (540 practitioners, 60 facilities of which 20 hospitals), 12 groups, 90
complaints, generated deterministically (mulberry32, seed 20261111) with an as-of date of
2026-11-11. All records are synthetic. State lives in the browser tab and resets on refresh.

## Self-tests
Settings > Run self-tests executes 22 checks of the rules and calculations in the browser,
including the fixed test records P-0001 to P-0006 and F-0001.

## Run locally
Open `index.html` in a browser. There is nothing to install.
