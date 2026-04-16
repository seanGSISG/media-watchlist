# TV Shows — `shows/watchlist.md`

When adding a TV show, you MUST web search to fill in every required field. Never use placeholders or leave blanks.

## Required Columns

| Column | Format | How to populate |
|--------|--------|-----------------|
| Date Added | `YYYY-MM-DD` | Today's date |
| Title | Text | Confirm exact title via search |
| Year | `YYYY` | First air date year |
| Seasons | Number | Total seasons aired (update if show is ongoing) |
| Genre | Text | Primary genre (Sci-Fi, Drama, Comedy, etc.) |
| Network | Text | Original network or streaming platform (HBO, Netflix, Apple TV+, etc.) |
| IMDB | URL | **Web search** `<Title> TV series IMDB` — use full `https://www.imdb.com/title/ttXXXXXXX/` URL |
| Trailer | URL | **Web search** `<Title> official trailer YouTube` — prefer the Season 1 / series premiere trailer |
| Status | Enum | Default `Queued`. Options: `Queued`, `Watching`, `Completed`, `Dropped` |
| DL | Emoji | `✅` when downloaded via torrent, blank otherwise. Set by the download-movie-tv skill |

## Optional Columns

| Column | Format | When to fill |
|--------|--------|-------------|
| Rating | `1-10` | Only after user has watched it |
| Notes | Text | Current season, user's thoughts, who recommended it |

## Rules

- Always search the web for the IMDB link and YouTube trailer — do not guess URLs
- Include "TV series" in IMDB searches to avoid matching same-name movies
- For ongoing shows, note total available seasons at time of adding
- Append new rows at the top of the table (newest first)
- Check for duplicates before adding — if the title already exists, tell the user instead of adding again
