# Snippet Index — Developer Knowledge Base

A single-file web app for storing, browsing, and copying code snippets — organized like a library card catalog. Each snippet gets a catalog number (`PY-001`, `JS-014`, ...) and a color-coded "spine" based on its language.

## What it does

- **Syntax highlighting** — powered by [highlight.js](https://highlightjs.org/) (Atom One Dark theme), applied to both card previews and the full detail view
- **Search** — live filtering across titles, code content, and tags (`Cmd/Ctrl+K` to jump to the search box)
- **Filter by language or tag** — sidebar lists every language and tag in use, with counts
- **Add / edit / delete** — a form modal for creating or updating snippets (title, language, tags, code)
- **One-click copy** — copy button on every card and in the detail view, via `navigator.clipboard.writeText()`
- **Seed data** — ships with 7 real example snippets (debounce, SQL upsert, git aliases, Python venv setup, flexbox centering, list chunking, typed fetch helper)

## Tech stack

| Layer | Choice |
|---|---|
| Structure | Single static HTML file (no build step) |
| Styling | Plain CSS, custom properties for theming |
| Syntax highlighting | highlight.js (via CDN) |
| Fonts | IBM Plex Mono / IBM Plex Sans / JetBrains Mono (Google Fonts) |
| Logic | Vanilla JavaScript, no framework |

## Running it

Just open `snippet-index.html` in a browser — no install, no server, no build step required.

## Current limitation: no persistence

This version keeps all data **in memory**. Anything you add, edit, or delete resets on page reload. It's a frontend prototype of the full data model described in the project brief, not yet wired to a database.

## Suggested next steps (to add real persistence)

1. **Pick a backend**
   - Fastest to stand up: **Supabase** or **Firebase** (hosted DB + auth, minimal backend code)
   - Full control: **FastAPI** (Python) or **Express/NestJS** (Node) + **SQLite** (local/single-user) or **PostgreSQL** (multi-device/live)

2. **Schema** (already reflected in the app's data model)
   ```sql
   CREATE TABLE snippets (
     id          TEXT PRIMARY KEY,
     title       TEXT NOT NULL,
     code        TEXT NOT NULL,
     language    TEXT NOT NULL,
     tags        TEXT[],        -- or a join table if normalizing
     catalog_no  TEXT,
     created_at  TIMESTAMP DEFAULT now()
   );
   ```

3. **Wire up CRUD** — replace the in-memory `snippets` array and its `renderAll()` calls with `fetch()` calls to your API (`GET /snippets`, `POST /snippets`, `PUT /snippets/:id`, `DELETE /snippets/:id`).

4. **Optional upgrades**
   - Swap the plain `<textarea>` for **CodeMirror** or **Monaco Editor** if you want real code editing (line numbers, auto-indent, bracket matching) instead of a plain text box
   - Add auth if this needs to be multi-user
   - Package as a desktop app with **Tauri** (lighter) or **Electron** (more mature ecosystem) if you want it running outside the browser

## File

- `snippet-index.html` — the entire app (HTML + CSS + JS in one file)
