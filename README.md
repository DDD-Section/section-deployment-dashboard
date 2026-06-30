# Section Deployment Dashboard

A self-serve, browser-only dashboard for viewing ProfAI and AI Academy deployment data. Customers paste their API key and everything runs client-side — no backend, no data stored on our systems.

## How it works

1. Customer opens the dashboard
2. Pastes their Section Reporting API key
3. Browser fetches data directly from the ProfAI API (`prof.ai`)
4. Key is saved in `localStorage` only — never sent to Section

## What it shows

- **ProfAI Program** — KPIs (users, onboarded, active, completed, use cases), completion funnel, onboarding timeline, skill completion grid
- **AI Academy** — course enrollment/completion, videos watched, course catalog
- **User Detail** — searchable/sortable table with per-user status
- Auto-detects single vs. multi-domain deployments and adjusts the UI accordingly

## Tech

- Single `index.html` — all HTML, CSS, JS in one file
- [Chart.js 4.5](https://www.chartjs.org/) via CDN
- Google Fonts (Montserrat + Inter)
- No build step, no dependencies, no server

## API Endpoints

All POST requests to `https://prof.ai`:

| Endpoint | Format | Returns |
|---|---|---|
| `/v1/reports/profai` | JSON | Users, skills, use cases, onboarding dates |
| `/v0/reports/courses` | CSV | Course catalog with enrollment/completion |
| `/v0/reports/employees` | CSV | Per-user video/course/badge counts |
| `/v0/reports/employee-course-map` | CSV | User-to-course mapping |

## Completion Export (`/export/`)

A companion single-page tool for exporting per-user course, completion, and certificate data
as CSV — for upload into an LMS such as Workday Learning. Same API key, same browser-only model,
no charts. Open `export/index.html` (or the `/export/` path when hosted) and download:

- **Certificates** (`/v1/reports/employee-certificate-map`) — one row per user × certificate,
  including `completion_date`. This is the only report that carries a true completion date, and
  the right source for LMS completion records.
- **Course engagement** (`/v1/reports/employee-course-map`) — one row per user × course with
  engagement status (Enrolled / Watched Lessons / Completed). Course rows carry status, not a date.
- **User summary** (`/v1/reports/employees`) — per-user views, completions, badges, and status.

See `export/README.md` for details.

## Deploy

Host `index.html` (and the `export/` folder) anywhere that serves static files.

## License

MIT
