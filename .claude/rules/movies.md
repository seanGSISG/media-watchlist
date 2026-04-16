# Movies — `movies/README.md`

When adding a movie, you MUST web search to fill in every required field. Never use placeholders or leave blanks.

## Required Columns

| Column | Format | How to populate |
|--------|--------|-----------------|
| Date Added | `YYYY-MM-DD` | Today's date |
| Title | Text | Confirm exact title via search |
| Year | `YYYY` | Release year from IMDB |
| Genre | Text | Primary genre (Sci-Fi, Thriller, Drama, etc.) |
| Director | Text | From IMDB page |
| IMDB | URL | **Web search** `<Title> <Year> IMDB` — use full `https://www.imdb.com/title/ttXXXXXXX/` URL |
| Trailer | URL | **Web search** `<Title> <Year> official trailer YouTube` — use the official trailer, not fan edits or teasers |
| Status | Enum | Default `Queued`. Options: `Queued`, `Watched`, `Skipped` |

## Optional Columns

| Column | Format | When to fill |
|--------|--------|-------------|
| Rating | `1-10` | Only after user has watched it |
| Notes | Text | User's thoughts, who recommended it, etc. |

## Rules

- Always search the web for the IMDB link and YouTube trailer — do not guess URLs
- If multiple versions exist (original vs remake), confirm which one the user means
- Append new rows at the top of the table (newest first)
- Check for duplicates before adding — if the title already exists, tell the user instead of adding again
