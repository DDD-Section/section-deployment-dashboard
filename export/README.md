# Section Coach — Completion Export

A single-file, browser-only tool for exporting per-user course, completion, and certificate
data from Section Coach as CSV — for upload into an LMS such as Workday Learning.

Companion to the Section Deployment Dashboard: same API key, same client-side model, charts
stripped, output is CSV downloads.

## What it does

1. Paste your Section Reporting API key (the same key the dashboard uses).
2. Pick a time period (Contract to date / Last 90 days / Last 30 days).
3. Download any of three CSVs:

| Export | Endpoint | Grain | Completion date? |
|---|---|---|---|
| **Certificates** | `/v1/reports/employee-certificate-map` | user × certificate | **Yes** — `completion_date` |
| **Course engagement** | `/v1/reports/employee-course-map` | user × course | No — `engagement` status only |
| **User summary** | `/v1/reports/employees` | user | No |

## Completion dates

`completion_date` is carried on the **certificate** report only. Course rows carry an
`engagement` status (`Enrolled` / `Watched Lessons` / `Completed`) but no date. For an LMS that
requires a completion date, drive off the **Certificates** export. The certificate CSV always
emits its header even when no certificates exist yet, so the downstream mapping can be built
before data lands.

## Tech

- Single `index.html` — all HTML, CSS, JS inline. No backend, no build step, no CDN dependencies.
- Auth: `Authorization: Bearer <api key>`, stored in `localStorage` only — never sent to Section.
- All requests go browser → `https://prof.ai` → browser.
- Works opened directly from disk (`file://`), suitable for restricted environments.

## Running it

Open `index.html` in a browser, paste the API key, click **Pull data**. To host it for a team,
serve the file from any static host.
