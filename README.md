# Weekly Task Planner

A single-file local app for assigning tasks to people across weeks.

## Run it

No build step, no dependencies. Just open the file in a browser:

```bash
open index.html
```

(or double-click `index.html` in Finder). To serve it over http instead:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

> **Note:** the shared-data feature (below) needs the page served over http(s),
> not opened as a `file://` URL, because it fetches `data.json`.

## Sharing with your team (GitHub Pages)

The planner can be shared through a GitHub repo: `data.json` in the repo is the
single source of truth, and the app is hosted for free on GitHub Pages.

### One-time setup

1. Create a new **empty** GitHub repo (e.g. `weekly-task-planner`).
2. From this folder, point it at your repo and push (the folder is already a git
   repo with an initial commit):

   ```bash
   git remote add origin git@github.com:<you>/<repo>.git
   git push -u origin main
   ```

3. In the repo on GitHub: **Settings → Pages → Build and deployment → Source: GitHub Actions**.
   The included workflow (`.github/workflows/pages.yml`) then deploys the site on every push to `main`.
4. Share the Pages URL (`https://<you>.github.io/<repo>/`) with your teammates.

### Day-to-day workflow

- **Teammates (read latest):** just open the Pages URL. The app fetches `data.json`
  automatically. First-time visitors get the shared board straight away; if you already
  have local edits, a banner appears offering to **Load shared** (Merge or Overwrite).
  Use **🔄 Load shared** anytime to pull the latest.
- **You (save changes):** after editing, click **💾 Save to repo**.
  - On Chrome/Edge this writes `data.json` directly into your local clone (pick the file
    once; later saves reuse it).
  - On Safari/Firefox it downloads `data.json` — move it into your repo folder.
  - Then commit and push:

    ```bash
    git add data.json && git commit -m "Update planner" && git push
    ```

  GitHub Pages redeploys automatically, so teammates see it on their next load.

### Handling overlapping edits

Because sync is manual (Git), two people editing at once can diverge. Rather than
resolving a raw JSON merge conflict by hand, pull the other person's `data.json`
(or have them send it) and use **⬆︎ Import → Merge with current** — people are
matched by name, tasks by text, and weeks align by column position, so both sets
of changes are combined non-destructively.

## Features

- **People × Weeks grid** — rows are people, columns are weeks. Add/remove either.
- **Editable names** — the first column holds editable name fields, auto-formatted to Title Case.
- **Auto-numbered weeks** — new columns become "Week 1", "Week 2", … and renumber when one is removed.
- **Task list** — add/remove tasks.
  - Exact duplicates are blocked with a clear **error**.
  - Near-duplicates (similar wording) are still added but show a softer **warning**.
- **Drag & drop** — drag a task from the list into any person/week cell. The task stays in the
  list, so you can assign it to multiple cells. Remove a task from a cell with its ✕.
- **Unassigned highlight** — tasks that aren't placed in any cell are marked *unassigned*.
- **Dark / light mode** — toggle in the top-right; your choice is remembered.
- **CSV / JSON import & export** — export your whole board to a `.csv` (spreadsheet-friendly)
  or `.json` (lossless) file, and import either back in. Importing opens a dialog that lets you
  **Merge** (adds new people, weeks and tasks and combines assignments, keeping your current
  data) or **Overwrite** (replaces everything). Nothing changes until you choose.
- **Team sync via GitHub** — `Save to repo` / `Load shared` keep a shared `data.json` in a
  GitHub repo in sync; host the app on GitHub Pages so teammates just open a URL. See
  [Sharing with your team](#sharing-with-your-team-github-pages).

Your data (people, weeks, tasks, assignments, theme) is saved in the browser's local storage,
so it persists across reloads on the same machine/browser. Theme is a personal preference and
is **not** shared through `data.json`.

## Import / export

Two export formats:

- **JSON** (`Export JSON`) — a lossless snapshot of the whole board; best for backups.
- **CSV** (`Export CSV`) — spreadsheet-friendly, with two sections:

```
#TASKS
Write Report
Review Pull Requests
#GRID
Name,Week 1,Week 2
Alex Johnson,"Write Report
Review Pull Requests",
Sam Rivera,,Review Pull Requests
```

- The `#TASKS` block lists every task (so tasks not assigned anywhere are preserved).
- The `#GRID` block is a people (rows) × weeks (columns) table; a cell can hold multiple
  tasks, one per line within the cell.

`Import` accepts either format (detected automatically). CSV import is forgiving: it also
accepts a plain grid CSV (just a `Name,Week 1,Week 2,…` header row followed by people rows)
and derives the task list from the cell contents.

On import you choose:

- **Merge with current** — people are matched by name, tasks by text, and weeks are aligned
  by column position; anything new is added and assignments are unioned. Non-destructive.
- **Overwrite everything** — replaces all current data (can't be undone).


