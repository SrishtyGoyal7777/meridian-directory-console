# Roster validation and submission layer

Working prototype for a provider data management case study. Single static page,
no backend, no login, no build step. Synthetic data only.

## The idea
Rosters reach health plans as spreadsheets by email, occasional SFTP files and
telephone calls, and are corrected weeks later. This validates every row against
registry, licensing and exclusion reference sources before it is submitted, shows
the submitter exactly what differs from the plan record, and gives the plan a
versioned review queue that writes through to the provider master and emits
typed events to downstream systems.

## Three submitter types, one plan
The three submitter types behave differently and the prototype carries all three:
an independent practice (8 clinicians, one location, occasional manual file), a
large group credentialing team (34 clinicians, four locations, monthly bulk file),
and a delegated entity data steward (96 clinicians, nine locations, contractual
accountability for accuracy).

## What it does
- **Upload** a CSV, paste rows, or load the organisation's current file.
- **Validate** every row against twelve checks, each carrying its rule, its source,
  whether it is deterministic or inferred, and a confidence. Errors block
  submission; warnings may be submitted with an attestation. Rows are editable
  in place and re-validate on demand.
- **Compare** against the plan record: new, changed, terminated, unchanged, and
  listings absent from the file (an implied termination).
- **Attest and submit**, producing a versioned submission.
- **Review** row by row on the plan side, approve or reject with a reason, then
  apply to the provider master.
- **Measure**: attested share, attested within cycle, first-pass validation,
  turnaround, activation and downstream events, each with a definition, source,
  owner, threshold and next action. Per-field freshness is reported separately.

## Validation checks
NPI format; NPI check digit (Luhn over the 80840 prefix); NPI present in the
registry extract; TIN format; licence status; exclusion list; taxonomy code;
panel status; effective date validity and future dating; termination date
validity, ordering and past dating; telephone format; duplicate NPI and location;
location on file for the TIN; identity match confidence against the provider master.

## Attested against inferred
Deterministic checks are labelled as rules. Checks consulting a reference source
are labelled as inferred and carry a confidence. The two are never blended, and
the dashboard reports attested, inferred and carried-over shares separately. A
registry extract carries a reporting lag, so it is treated as a reference and
never as ground truth; the submitter attestation is the freshest signal.

## Data
138 listings across three submitter types, 14 locations, with a registry extract,
a licensing feed and an exclusion list, all generated deterministically
(mulberry32, seed 20261111) with an as-of date of 2026-11-11. Every record is
synthetic. State lives in the browser tab and resets on refresh.

## Self-tests
Settings > Run self-tests executes 32 checks of parsing, validation, identity
matching, diffing, submission, review and measurement in the browser.

## Run locally
Open `index.html`. There is nothing to install.
