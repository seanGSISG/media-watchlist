# Media Watchlist

Personal media tracking repo. Three categories: movies, shows, audiobooks.

## Quick Reference

<progressive_disclosure>
<behavior>Read referenced files ONLY when the task requires that domain knowledge.</behavior>

| Domain | File | When to Read |
|--------|------|-------------|
| Repo-wide conventions | `.claude/rules/general.md` | Any watchlist operation |
| Movie column specs | `.claude/rules/movies.md` | Adding/editing movies |
| TV show column specs | `.claude/rules/shows.md` | Adding/editing shows |
| Audiobook column specs | `.claude/rules/audiobooks.md` | Adding/editing audiobooks |
| Add media workflow | `.claude/skills/add-media/SKILL.md` | Adding new entries |
| Browse/search/stats | `.claude/skills/browse-watchlist/SKILL.md` | Querying the lists |
| Download movie/TV | `.claude/skills/download-movie-tv/SKILL.md` | IPT/PTP → qBittorrent → Jellyfin |
| Download audiobook | `.claude/skills/download-audiobook/SKILL.md` | MAM → qBittorrent → Audiobookshelf |

</progressive_disclosure>

## Adding Entries

When the user asks to add a movie, show, or audiobook, follow the rules for that media type below. Always set **Date Added** to today's date (`YYYY-MM-DD`). Default **Status** to `Queued` unless told otherwise.

If the user gives a vague title, search to confirm the correct title, year, and metadata before adding.

<progressive_disclosure>

### Movies — `movies/watchlist.md`

| Column | Required | Notes |
|--------|----------|-------|
| Date Added | Yes | `YYYY-MM-DD` |
| Title | Yes | |
| Year | Yes | Release year |
| Genre | Yes | Primary genre (e.g. Sci-Fi, Thriller, Drama) |
| Director | Yes | |
| IMDB | Yes | Full URL: `https://www.imdb.com/title/ttXXXXXXX/` |
| Trailer | Yes | YouTube link to official trailer. Search for `<Title> <Year> official trailer` |
| Status | Yes | `Queued`, `Watched`, `Skipped` |
| DL | Yes | `✅` when downloaded, blank otherwise |
| Rating | No | User's rating after watching (1-10) |
| Notes | No | Brief thoughts or who recommended it |

### TV Shows — `shows/watchlist.md`

| Column | Required | Notes |
|--------|----------|-------|
| Date Added | Yes | `YYYY-MM-DD` |
| Title | Yes | |
| Year | Yes | First air year |
| Seasons | Yes | Total season count |
| Genre | Yes | Primary genre |
| Network | Yes | Original network or streaming platform |
| IMDB | Yes | Full URL: `https://www.imdb.com/title/ttXXXXXXX/` |
| Trailer | Yes | YouTube link to official/season 1 trailer |
| Status | Yes | `Queued`, `Watching`, `Completed`, `Dropped` |
| DL | Yes | `✅` when downloaded, blank otherwise |
| Rating | No | User's rating (1-10) |
| Notes | No | Brief thoughts, current season, who recommended it |

### Audiobooks — `audiobooks/watchlist.md`

| Column | Required | Notes |
|--------|----------|-------|
| Date Added | Yes | `YYYY-MM-DD` |
| Title | Yes | |
| Author | Yes | |
| Year | Yes | Publication year |
| Narrator | Yes | Audiobook narrator |
| Genre | Yes | Primary genre |
| Series | No | Series name + book number if applicable (e.g. `Stormlight Archive #1`) |
| Length | Yes | Runtime in hours (e.g. `45h 30m`) |
| Goodreads | Yes | Full URL to Goodreads page |
| Status | Yes | `Queued`, `Listening`, `Completed`, `DNF` |
| DL | Yes | `✅` when downloaded, blank otherwise |
| Rating | No | User's rating (1-10) |
| Notes | No | Brief thoughts, narrator quality, who recommended it |

</progressive_disclosure>

## Updating Status

When the user says they watched/finished/dropped something, update the **Status** column. If they give a rating or notes, fill those in too.

## General Rules

- Keep tables sorted by Date Added (newest first)
- One entry per row — no duplicate titles within a category
- If a title exists and the user asks to add it again, flag the duplicate instead of adding
- Use web search to fill in metadata (IMDB links, trailer URLs, narrator, etc.) — don't leave blanks or placeholders
