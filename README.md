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

Your data (people, weeks, tasks, assignments, theme) is saved in the browser's local storage,
so it persists across reloads on the same machine/browser.

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


