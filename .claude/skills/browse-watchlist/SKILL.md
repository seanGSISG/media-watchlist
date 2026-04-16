---
name: browse-watchlist
description: >
  Browse, search, filter, and get stats from the media watchlist. Use this skill whenever the
  user asks what's on their list, wants to search for something they added, asks for stats or
  counts, filters by status/genre/year, or asks questions like "what movies are queued?",
  "have I added X?", "what did I watch last month?", "how many audiobooks have I finished?",
  "show me all sci-fi", "what's next to watch?", or any query about the contents of their
  watchlist. Also trigger when the user says "what's on my list", "show my watchlist", or
  "browse my list".
---

# Browse Watchlist

This skill queries the media watchlist at `/home/adminuser/projects/media-watchlist/`.

## Data Locations

```
movies/watchlist.md      — Movie entries
shows/watchlist.md       — TV show entries
audiobooks/watchlist.md  — Audiobook entries
```

Each file contains a markdown table with entries sorted newest-first.

## How to Respond to Queries

### "What's on my list?" / "Show my watchlist"

Read all three `watchlist.md` files and present a summary:
- Count per category and status (e.g., "12 movies: 8 queued, 3 watched, 1 skipped")
- List the most recently added entries (last 5 per category)

### "What's queued?" / "What should I watch next?"

Filter for `Queued` status across the requested category (or all categories if unspecified).
Present as a clean list with title, year, and the relevant link (IMDB or Goodreads).

### Searching — "Have I added X?" / "Find X"

Search all three watchlist files for the title. Report:
- Found: show the full row with all metadata
- Not found: say so, and offer to add it

### Filtering — "Show me all sci-fi" / "What did I add in March?"

Parse the markdown tables and filter by the requested column(s). Present matches as a
clean formatted list. Support filtering by:
- **Genre**: match against the Genre column
- **Status**: match against Status column
- **Year**: match against Year column (release year)
- **Date Added**: match against Date Added column (when it was added to the list)
- **Director/Author/Narrator**: match against the relevant column
- **Type**: limit to movies, shows, or audiobooks

Multiple filters can combine (e.g., "queued sci-fi movies").

### Stats — "How many X?" / "Watchlist stats"

Parse all tables and compute:
- Total entries per category
- Breakdown by status per category
- Most common genres
- Recently added (last 7 days)
- Rating distribution (if any rated entries exist)

Present as a concise summary, not raw data.

## Formatting

- Use tables for structured results (5+ items)
- Use bullet lists for short results (< 5 items)
- Always include clickable links (IMDB, trailer, Goodreads) in results
- When showing a single entry, display all columns in a readable format rather than a wide table
