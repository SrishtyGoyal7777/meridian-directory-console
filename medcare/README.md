# MedCare Roster Confirmation

Working prototype for the MedCare provider portal case study. Single static page,
no backend, no login, no build step. Synthetic data only.

## The idea
Instead of asking a practice to re-enter its roster, the app shows the practice what
the plan already holds, marks the listings MedCare's validation believes are wrong,
says why, and asks for a confirmation or a correction. The answer writes back to the
plan's directory and moves every plan-side number.

## Both sides of the loop
- **Practice manager** - roster ranked by what looks wrong, per-listing explanation,
  confirm / correct / remove at the provider-location level, submission history.
- **Plan network operations** - verification queue ranked by each listing's position in
  its own 90-day window, explainable score, CSV export.
- **Plan compliance** - directory health KPIs, basis mix, six-month trend, readiness
  checks and the confirmation log.

## Rules implemented
- 90-day verification cycle per listing; publication within 2 business days.
- Flag weights: never verified 35, past window 20, no claims in 14 months 30,
  dropped from the roster file 25, address mismatch 15, member reported 25,
  provider at 4+ locations 15, invalid NPI 10. Score capped at 100.
- Confidence from four corroborating signals: High 3-4, Medium 2, Low 0-1.
- Basis per listing: attested, inferred, carried over, never verified - never blended.
- NPI check digit by Luhn over the 80840 prefix.

## Why "provider at 4+ locations" is a flag
CMS found the largest directory error class is "provider not at this location", caused
by practices submitting at group level so every provider appears at every site. The app
treats the provider-location attachment as its primary object and tracks corrections to
it separately.

## Data
20 practices, 48 locations, 431 providers, 764 listings and 70 member reports, generated
deterministically (mulberry32, seed 20261111) with an as-of date of 2026-11-11. State
lives in the browser tab and resets on refresh.

## Self-tests
Settings > Run self-tests executes 25 checks of the rules and calculations in the browser.

## Run locally
Open `index.html`. There is nothing to install.
