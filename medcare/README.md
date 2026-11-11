# Roster Validation and Submission Layer

Working prototype for a provider roster case study. Single static page, no backend,
no login, no build step. Synthetic data only.

## What it is
Not a provider portal. A validation and submission layer that a health plan white-labels
and a provider organisation reuses: submit a roster once, in your own format, have every
row checked before it reaches the plan, and let approved changes flow straight into the
plan's record and its downstream systems.

## The loop
1. **Provider administrator** uploads or pastes a CSV roster, or starts from one of three
   sample files. Every row is validated and diffed against the plan record.
2. **Validation report** shows blocking errors, warnings and changes. Each finding names
   the rule, the source it checked and its confidence, so the practice can disagree with it.
   Rows can be fixed inline and revalidated.
3. **Attestation** records who confirmed what and when. Submission is blocked while any
   blocking error remains.
4. **Provider data operations** reviews the submission line by line against the plan record,
   approves or rejects each change, and must give a reason the practice can act on when
   rejecting.
5. **Completion** updates the plan record, sets the basis to attested, writes the audit log
   and emits typed events to claims, the directory, prior authorisation and care coordination.

## Validation rules
Nineteen rules across format, registry, licensing, exclusion, entity resolution and contract
scope: NPI check digit, NPI not in registry, name and taxonomy mismatch, unrecognised
taxonomy, expired or lapsing licence, exclusion-list hit, duplicate row, ambiguous entity
match, uncontracted location, address mismatch, TIN and phone format, invalid panel status,
missing or inconsistent effective and termination dates. Each is an error, a warning or
information, and each carries its source and confidence.

## Dashboard
First-pass validation rate, median roster turnaround, provider-attested share, verified
within 90 days, submitter activation and share submitted through the layer. Every figure
opens its definition, source, owner and threshold. Per-field freshness shows which values
are attested and which are inferred, and how often each refreshes.

## Data
12 contracted organisations across independent practices, large groups and delegated
entities, 26 locations, 258 registry records, 400 listings, generated deterministically
(mulberry32, seed 20261111) with an as-of date of 2026-11-11. The registry, licensing board
and exclusion list are synthetic stand-ins. State lives in the browser tab and resets on
refresh.

## Self-tests
Settings > Run self-tests executes 32 checks of the CSV parser, every validation rule, the
diff, the submission and review guards, the effect of approvals on the plan record, the
event feed and the metrics.

## Run locally
Open `index.html`. There is nothing to install.
