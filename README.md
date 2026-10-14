# Koda Iron Games 2027 — Athlete Survey

Survey site sent to current, past and prospective Iron Games athletes to plan
2027 and the move toward a Colorado Fitness Festival.

**Live (once published):** https://kevschuetz3-lgtm.github.io/iron-games-survey/

## How it works
- Static single page (GitHub Pages, `main` branch root). Branding matches the
  sponsor and volunteer sites: Anton + Poppins, black / #c13b4a red, same photos.
- Questions: name, email, times competed (0-6), time of year, number of days,
  fitness events to add (+ Other), CrossFit divisions to add, most important
  (+ Other), and "what would make you 100% sign up".
- Submit POSTs `{action:'ironGamesSurvey'}` to the shared Koda Apps Script backend
  (`koda-coaching/apps-script/Code.js`, see `ironGamesSurvey_`). The backend:
  - writes to the **"Koda Iron Games 2027 Athlete Survey"** Google Sheet
    (auto-created, script prop `IRON_SURVEY_SHEET_ID`): a **Summary** tab with
    live counts + percentages for every option, and a **Responses** tab
  - keeps **one row per email** — a resubmit overwrites that athlete's answers,
    so retries never create duplicates
  - emails `IRON_SURVEY_NOTIFY` (kevschuetz3@gmail.com) for each *new* respondent
    (set it to `""` in Code.js to stop the per-response emails)
- The page only shows the thank-you screen when the backend answers `saved:true`.

## Handy GET actions (on the Koda web app `/exec` URL)
| Action | What it does |
|---|---|
| `?action=ironGamesSurveyInfo` | Sheet URL + response count (creates the sheet if missing) |
| `?action=ironGamesSurveyClearTests` | Deletes rows whose name contains the word "test" |
| `?action=ironGamesSurveyRebuildSummary` | Recreates the Summary tab (after editing options) |

## Local preview
Run the `iron-survey-static` config (port 8768). On localhost the form runs in
**preview mode**: it logs the payload to the console instead of sending
(important — the backend treats unknown actions as a coach-availability
submission). Add `?live=1` to send for real from localhost.

## Editing questions
Option strings live in the arrays at the bottom of `index.html` (`SEASONS`,
`DAYS`, `ASPECTS`, `DIVISIONS`, `IMPORTANT`) **and** in the `IRON_SURVEY_*`
arrays in `Code.js` — they must match, because the backend drops unknown
options and the Summary tab counts by these exact strings. After changing
Code.js: `clasp push` + `clasp redeploy <deployment id>` from
`koda-coaching/apps-script/` (same URL). If you add/rename options after the
sheet exists, open `?action=ironGamesSurveyRebuildSummary` to rebuild the
Summary tab.
