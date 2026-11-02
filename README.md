# `wax` — Command-Line Vinyl Collection Tracker & Spin Selector

`wax` is a lightweight command-line interface (CLI) tool for cataloging a physical vinyl record collection, browsing your crate offline, and picking what record to spin next right from the terminal.

---

## Project Goal
Vinyl collectors with growing crates often face two friction points: manually typing tracklists/genres into spreadsheets is tedious, and choosing which record to play leads to choice paralysis. `wax` solves both problems by pulling accurate album metadata automatically from the Discogs API into a fast local JSON file and providing terminal commands to browse, filter, and randomly select records to play.

---

## MVP Scope ("Do" Quadrant)
The Minimum Viable Product focuses strictly on cataloging, browsing, and selecting records:
- **Discogs API Auto-Cataloging (`wax add <query>`):** Searches the Discogs REST API by album title/artist and automatically saves release year, genre, style, and tracklist.
- **Local JSON Crate Storage (`crate.json`):** Stores collection records locally for instant offline querying and easy Git version control.
- **Collection Management (`wax remove <id>`):** Removes records from the local crate.
- **Fast Crate Browsing (`wax list` & `wax show`):** Displays formatted terminal tables with optional `--genre`, `--style`, or `--decade` filters, plus detailed Side A / Side B tracklist views.
- **"What to Play" Selector (`wax pick`):** Recommends a random record from the full crate or a filtered subset (e.g., `wax pick --genre jazz`).

---

## Future Plans — V2.0+ ("Schedule" Quadrant)
Once the MVP is complete, planned V2.0 enhancements include:
- **Git Auto-Sync Backup (`wax sync`):** A Bash wrapper script that automatically commits and pushes changes in `crate.json` to a remote GitHub repository.
- **Custom Mood & Vibe Tagging (`wax tag`):** Allows users to attach custom personal tags (e.g., `rainy-day`, `study`, `late-night`) on top of standard Discogs genres.
- **Terminal Album Cover Art:** Renders ANSI/ASCII block-pixel album covers directly inside the terminal alongside tracklists.
- **Interactive Arrow-Key Crate Browser:** A full-screen TUI mode for flipping through records with keyboard shortcuts.

---

## Eisenhower Matrix

If you saved your matrix as an image (`docs/eisenhower_matrix.png`), you can view it directly below (or view the PDF in [`docs/eisenhower_matrix.pdf`](./docs/eisenhower_matrix.pdf)):

| | **Urgent** | **Not Urgent** |
| :--- | :--- | :--- |
| **Important** | **DO (MVP)**<br>• Discogs API Auto-Cataloging (`wax add`)<br>• Local JSON Crate Storage (`crate.json`)<br>• Fast Crate Browsing (`wax list`, `wax show`)<br>• "What to Play" Selector (`wax pick`) | **SCHEDULE (V2.0+)**<br>• Git Auto-Sync Backups (`wax sync`)<br>• Custom Mood/Vibe Tagging (`wax tag`)<br>• ANSI Terminal Album Cover Art<br>• Interactive Arrow-Key TUI Mode |
| **Not Important** | **DELEGATE (Use a Library)**<br>• CLI Argument Parsing (`Typer`)<br>• Terminal Tables & Panels (`Rich`)<br>• HTTP Requests (`requests`) | **DELETE (Cut It)**<br>• Market Price / Value Tracking<br>• Listening History & Spin Logs<br>• Desktop / Web GUI & Audio Playback |

---

## System Architecture Diagram

![wax System Architecture Diagram](./docs/diagram.png)

---

## Tech Stack
- **Language:** Python 3 (with a Bash helper script for installation and Git backups)
- **Libraries & Tools:**
  - `Typer` — CLI command and flag parsing (`wax add`, `wax list`, `wax pick`)
  - `Rich` — Formatted terminal tables, color output, and Side A/B tracklist panels
  - `requests` — HTTP GET calls to the Discogs API
  - `git` — Version control and automated collection backups via Bash
- **Data Storage:** Local JSON file (`crate.json`) storing saved album dictionaries for fast offline access.
- **External Data Source:** Discogs REST API (accessed via HTTP GET requests using a Discogs Personal Access Token).

---

## Build Milestones
1. Set up Git repo, directory structure, and Python environment (`requirements.txt`).
2. Connect to the Discogs REST API and test album search queries.
3. Write the metadata parser to extract artist, title, year, genre, style, and tracklist.
4. Implement `load_crate()` and `save_crate()` for local `crate.json` storage.
5. Build `wax add` and `wax remove` CLI commands.
6. Build `wax list` and `wax show` commands with `Rich` tables and filter flags.
7. Implement the `wax pick` random/filtered selector command.
8. Add the Bash installer and `wax sync` Git backup script.