# GFN Programs Dashboard — "What's Going On"

A lightweight TV dashboard showing all active GFN programs, their timelines, and key milestones. Hosted on GitHub Pages.

---

## Setup

### 1. Push to GitHub

Create a new GitHub repo (e.g., `gfn-dashboard`) and push this folder to it.

### 2. Enable GitHub Pages

In your repo: **Settings → Pages → Source → Deploy from branch → `main` / `/ (root)`**

The dashboard will be live at `https://<your-org>.github.io/gfn-dashboard/`.

### 3. Display on the office TV

Open that URL in a browser on the TV. The page auto-refreshes every 5 minutes.

---

## Updating programs

### Option A — Edit manually (simplest)

Edit `programs.json` directly in GitHub. The dashboard reflects changes on the next auto-refresh.

Each program entry:

```json
{
  "id": "unique-id",
  "name": "Program Name",
  "status": "active",
  "description": "One-line summary.",
  "startDate": "2026-01-01",
  "endDate": "2026-12-31",
  "lead": "Person Name",
  "milestones": [
    { "name": "Milestone name", "date": "2026-03-01", "completed": false }
  ]
}
```

**Status values:** `active` · `on-hold` · `completed`

---

### Option B — Sync from Asana (automated)

The workflow in `.github/workflows/sync-asana.yml` fetches your Asana project every hour and rewrites `programs.json` automatically.

**How it maps Asana → dashboard:**
| Asana | Dashboard |
|---|---|
| Section | Program |
| Tasks in section | Milestones |
| Task `due_on` | Milestone date |
| All tasks complete | Status: completed |
| Any task complete | Status: active |

**Setup steps:**

1. Get your **Asana Personal Access Token** from [app.asana.com/0/my-apps](https://app.asana.com/0/my-apps).
2. Get your **Project GID** — it's the number in the URL when you open a project: `app.asana.com/0/PROJECT_GID/...`
3. In your GitHub repo: **Settings → Secrets and variables → Actions → New repository secret**
   - `ASANA_TOKEN` — your personal access token
   - `ASANA_PROJECT_GID` — your project GID
4. The sync runs automatically every hour. Trigger it manually from **Actions → Sync from Asana → Run workflow**.

---

## Project structure

```
gfn-dashboard/
├── index.html        ← Dashboard UI (single file, no dependencies)
├── programs.json     ← Program data (edit manually or auto-synced)
└── .github/
    └── workflows/
        └── sync-asana.yml   ← Optional Asana sync workflow
```
