---
name: add-media
description: >
  Add a movie, TV show, or audiobook to the media watchlist. Use this skill whenever the user
  wants to add, track, save, queue, or remember a movie, show, series, audiobook, or book to
  watch/listen to. Also trigger when the user says things like "add X to my list", "I want to
  watch X", "remember to watch X", "queue up X", "save X for later", or names a title and
  implies they want to track it. This skill handles the full workflow: identifying the media type,
  searching the web for metadata (IMDB, trailers, Goodreads, narrators), formatting the table
  row, appending it to the correct watchlist file, and committing the change.
---

# Add Media to Watchlist

This skill adds entries to the media watchlist repo at `/home/adminuser/projects/media-watchlist/`.
Each media type has its own folder and `watchlist.md` file with a markdown table.

## Workflow

### 1. Identify the media type and title

If the user already specified both (e.g., "add the movie Dune"), skip straight to the search step.
If ambiguous, ask — but only what's missing:

- **Type unclear**: "Is that a movie, TV show, or audiobook?"
- **Title unclear**: "What's the title?"

Don't ask both if one is obvious from context. If the user says "add Severance" you can infer TV show.
Use your judgment — if genuinely ambiguous (e.g., a title that exists as both a movie and a show), ask.

### 2. Web search for metadata

Search the web to fill in **every** required field. Never guess URLs or leave placeholders.

**For movies** — search for:
- IMDB page → extract: year, genre, director, full IMDB URL
- YouTube official trailer → `"<Title> <Year> official trailer"` → full YouTube URL
- If multiple versions exist (original vs remake), confirm which one the user means

**For TV shows** — search for:
- IMDB page (include "TV series" in query) → extract: first air year, seasons, genre, network/platform, full IMDB URL
- YouTube trailer → `"<Title> TV series official trailer"` → prefer Season 1 trailer, full YouTube URL

**For audiobooks** — search for:
- Goodreads page → extract: author, publication year, genre, series info, full Goodreads URL
- Audible listing → extract: narrator, runtime (format as `XXh YYm`)
- If multiple audiobook editions exist, prefer the most popular narration

### 3. Check for duplicates

Before adding, read the target `watchlist.md` and check if the title already exists.
If it does, tell the user instead of adding a duplicate.

### 4. Format and append the row

Add the new row at the **top** of the table (below the header row). Use today's date for Date Added.
Default Status to `Queued` unless the user specified otherwise.

**Movies** — append to `movies/watchlist.md`:
```
| YYYY-MM-DD | Title | Year | Genre | Director | [IMDB](url) | [Trailer](url) | Queued | | |
```

**TV Shows** — append to `shows/watchlist.md`:
```
| YYYY-MM-DD | Title | Year | Seasons | Genre | Network | [IMDB](url) | [Trailer](url) | Queued | | |
```

**Audiobooks** — append to `audiobooks/watchlist.md`:
```
| YYYY-MM-DD | Title | Author | Year | Narrator | Genre | Series | Length | [Goodreads](url) | Queued | | |
```

Use markdown link syntax `[IMDB](url)` for URLs to keep the table readable on GitHub.

### 5. Commit and push

After appending the entry:

```bash
cd /home/adminuser/projects/media-watchlist
git add <path-to-watchlist.md>
git commit -m "add: <Title> (<Year>) to <type> watchlist"
git push
```

### 6. Confirm to the user

Show a brief confirmation with the key details:
- Title, year, and type
- Links (IMDB/trailer or Goodreads) so they can click through
- The status (Queued unless specified otherwise)

## Batch additions

If the user gives multiple titles at once (e.g., "add Dune and Interstellar to movies"), process
them all in sequence — search, format, append each one — then do a single commit with all entries:

```
git commit -m "add: <Title1>, <Title2>, ... to <type> watchlist"
```

## Updating existing entries

If the user says they watched/finished/dropped something, or wants to rate it, find the row in
the appropriate `watchlist.md` and update the Status, Rating, or Notes columns. Commit with:

```
git commit -m "update: <Title> — <what changed>"
```
