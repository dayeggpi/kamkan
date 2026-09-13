# KamKan

**Markdown-native kanban board manager for desktop**

A free lightweight Tauri + React desktop app that stores all tasks as plain `.md` files — fully readable and editable outside the app, version-control friendly, and zero external dependencies.

See https://github.com/dayeggpi/KamKan-MCP-SKILL for MCP server and AI SKILL (Claude Code, Codex, Gemini CLI) support.

---

## Features

### Boards & Columns
- Create, rename, and delete boards with a custom short ID (e.g. `NOW`, `PROJ`)
- Add, rename, reorder (drag-and-drop), and delete columns
- Mark a column as **Done** — tasks moved there are auto-completed
- **Archive columns** — hidden from active view, keeps old tasks accessible
- **Hide/show** individual columns without deleting them
- Set a **max task limit** per column with overflow detection and resolution UI
- Custom **column accent colors** (hex color picker)
- Column headers support markdown formatting

### Tasks
- Quick-create tasks with `Enter`, or open the full editor with `Alt+Enter`
- **Drag and drop** tasks between columns (with visual drop indicator)
- Reorder tasks within a column via context menu
- **Inline rename** by double-clicking a card title
- Auto-assigned numeric IDs per board (e.g. `NOW-1`, `PROJ-5`)
- **Duplicate ID detection** with auto-repair on board load

### Task Details
Each task can have:
- **Priority** — Extreme, High, Medium, Low (color-coded badges)
- **Tags** — comma/enter-separated, searchable
- **Due date** — DD/MM/YYYY with overdue highlighting
- **Subtasks** — checkbox list with completion counter
- **Dependencies** — "Depends on" and "Blocks" links to other tasks
- **Description** — full markdown editor with live preview

### Markdown Support
Full markdown in task descriptions and column headers:
- Bold, italic, strikethrough, highlight (`==text==`)
- Headings (H1–H6), blockquotes (`> `), horizontal rules (`---`)
- Bullet, numbered, and task lists (`- [ ]` / `- [x]`) with live checkbox toggle
- Inline code and fenced code blocks
- Comments (`%% comment %%`) which wont show on rendered markdown
- External links and bare URL auto-detection (highlight text, copy link, will format it to `[url link](https://google.com/)`)
- **Wikilinks** — `[[BOARD-ID]]` links to a board, `[[BOARD-ID-N]]` links to a specific task
- Wikilink **autocomplete** when typing `[[`
- Internal attachment link (`![[filename]]`)
- Table
- Formatting toolbar in edit mode

### Search & Filtering
- Full-text search across titles, descriptions, and tags (real-time)
- Filter by **priority** (Extreme / High / Medium / Low toggle buttons)
- Filter by **date range**
- Filter to tasks with **descriptions or subtasks**
- Filter to tasks with **open subtasks**
- Filters persist while navigating between tasks

### Bulk Operations
- Toggle bulk mode (`Ctrl+B`) to select multiple tasks
- **Bulk move** selected tasks to any column in the current board
- **Bulk move** selected tasks to another board entirely
- Overflow detection with resolution prompts for bulk moves

### Workspace & Folders
- **Choose workspace folder** — all boards live as `.md` files inside it
- Organize boards into **folders** via sidebar tree (create, rename, delete, drag)
- Drag boards between folders (physically moves files on disk)
- **File watcher** (Chokidar) — detects external changes and auto-reloads (e.g. editing in VS Code)
- Single-instance enforcement (portable builds)

### Theme Customization
20+ individually configurable color properties, organized into groups:
- Base backgrounds (app, panel, card, hover, active)
- Text tiers (primary, secondary, muted, disabled)
- Borders, sidebar, tags, dates
- Priority colors, wikilinks, checkboxes, highlights
- Edit popup, accent, scrollbar

**Import/export themes** as JSON. Changes apply live. Full reset to defaults.

### System Integration
- Custom frameless window with title bar controls (minimize, maximize, close)
- **System tray icon** — click to show/focus window, right-click for menu
- **Minimize to tray** — close button hides instead of quitting (optional)
- **Run on Windows startup** (optional)
- **Zoom level** (`Ctrl+Scroll`) — 60% to 170%, badge shows current level
- Collapsible sidebar

### Local-first online sync
- Ability to use Git (GitHub, GitLab, Gitea etc) to sync up files
- Allows for multi users to share same boards
- Conflict management (beta)

---

## Data Format

All data is stored as plain markdown files — no database, no binary format.

### Board file — `board_ID.md`

```markdown
## Todo [max:10] [color:#5B8DEF]
- [ ] 1|Task title
- [ ] 2|Another task

## In Progress
- [ ] 3|Task in progress

***

## Archive
- [x] 4|Archived task
```

- `[max:N]` — column task limit
- `[color:#HEX]` — column accent color
- `***` — separator before Archive section
- Task format: `- [x] numericId|Title`

### Task detail file — `BOARD_ID/BOARD_ID-N.md`

```markdown
---
ID: 1
Tags: [bug, frontend]
Priority: High
Link:
  Depends_on: [PROJ-2]
  Blocks: [NOW-5]
Due_date: 2025-06-01
---

## Description
Full markdown content here.

## Subtasks
- [ ] Write tests
- [x] Fix the bug
```

### Settings — `settings.json`

Stored in AppData, next to the executable (portable mode), or a custom location.

```json
{
  "workspacePath": "C:\\Users\\you\\boards",
  "minimizeToTray": true,
  "runOnStartup": false,
  "settingsLocation": "appdata"
}
```

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| `Enter` | Create task (quick) |
| `Alt+Enter` | Create task + open detail editor |
| `Escape` | Close modal |
| `Double-click` card | Inline rename |
| `Right-click` card | Open detail editor |
| `Ctrl+F` | Jump to search field |
| `Ctrl+L` | Toggle left panel |
| `Ctrl+B` | Add new column to board |
| `Ctrl+T` | Create new board |
| `Ctrl+A` | Archive all tasks from Done column |
| `Ctrl+Scroll` | Zoom board in/out |

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | React 18, CSS Modules |
| Desktop | Tauri 2 |
| Bundler | Vite 8 + Terser |
| Packaging | Tauri bundler |
| File watching | Chokidar |

---

## Notes

- All data lives in `.md` files — readable and editable in any text editor
- Boards are valid markdown documents with no proprietary encoding
- The file watcher detects external edits and refreshes the UI automatically
- `[[BOARD-ID]]` links a whole board; `[[BOARD-ID-N]]` links a specific task
- Duplicate task IDs are auto-detected on load and can be repaired interactively
