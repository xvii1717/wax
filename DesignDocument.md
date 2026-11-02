As your record collection grows, it can get hard to remember all the albums in your collection.

This makes it difficult to pick the right music for your specific mood.

Wax is a simple set of terminal commands that tracks your record collection.

It can also help the user select music in some capacity.

Design Diagram: ![wax Architecture Diagram](.docs/wax.png)


1. PROGRAMMING LANGUAGES

- Primary Language: Python 3

  - Used for the core CLI logic, searching/filtering the record collection, and talking to the Discogs API.

  - Why: Python handles JSON data and API requests in just a few lines of code and runs instantly as a terminal 
    command without needing to compile JAR files like Java.

- Secondary Language: Bash (Shell Scripting)

  - Used for a simple installation script (install.sh) to set up the `wax` command alias and an optional Git 
    backup wrapper.



2. LIBRARIES, PACKAGES & EXTERNAL TOOLS

- Python Libraries:

  - `requests`: To send HTTP GET requests to the Discogs API and fetch album metadata.

  - `typer` (or Python's built-in `argparse`): To handle CLI commands (`wax add`, `wax list`, `wax pick`) and 
    filter flags (`--genre`, `--decade`).

  - `rich`: To print clean, readable tables and formatted Side A / Side B tracklists in the terminal instead of 
    messy plain text.

  - `json` & `random` (Python standard library): For reading/writing the local collection file and picking a 
    random record to spin.

- Bash Utilities & Commands (for setup and V2.0 Git sync):

  - `git`: Used if we implement the V2.0 `wax sync` command to automatically commit and push the collection file 
    to GitHub (`git add`, `git commit`, `git push`).

  - `chmod` / `ln`: Used in the setup script to make the CLI executable from anywhere in the terminal.



3. DATA STORAGE

- Format: Local JSON file (`crate.json`)

- How it works:

  - Every record added to the collection is saved as a JSON object inside a single local file (`~/.wax/crate.json`).

  - Each entry stores only the essentials needed for browsing and picking: Record ID, Artist, Album Title, 
    Release Year, Genres, Styles, and Tracklist.

- Why JSON:

  - No database server setup is needed for the MVP.

  - Once an album's info is saved locally to `crate.json`, browsing (`wax list`) and picking a record (`wax pick`)
    work 100% offline and load instantly.

  - Because JSON is plain text, it is easy to inspect, edit manually if needed, and track with Git.



4. EXTERNAL DATA SOURCES & API ACCESS

- External Source: Discogs REST API (https://api.discogs.com)

- What data we pull:

  - When a user runs `wax add "<Artist - Album>"`, we query the Discogs database search endpoint 
    (`/database/search`) and release endpoint (`/releases/{id}`) to automatically pull the album's official 
    title, artist, release year, genre, style, and tracklist.

- How we access it:

  - Accessed over HTTPS using Python's `requests` library with a free Discogs Personal Access Token passed in 
    the request header (`Authorization: Discogs token=...`) and a custom `User-Agent` header as required by 
    Discogs API rules.



5. OTHER TECHNICAL DETAILS (ENVIRONMENT & WORKFLOW)

- Version Control: Git & GitHub for managing source code, branches, and pull requests across the team.

- Environment Management: Python `venv` and a `requirements.txt` file so anyone grading or testing the project 
can install the dependencies (`requests`, `typer`, `rich`) with a single `pip install -r requirements.txt` command.
