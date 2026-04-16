# Audiobooks — `audiobooks/watchlist.md`

When adding an audiobook, you MUST web search to fill in every required field. Never use placeholders or leave blanks.

## Required Columns

| Column | Format | How to populate |
|--------|--------|-----------------|
| Date Added | `YYYY-MM-DD` | Today's date |
| Title | Text | Confirm exact title via search |
| Author | Text | From Goodreads or publisher listing |
| Year | `YYYY` | Original publication year |
| Narrator | Text | **Web search** `<Title> <Author> audiobook narrator` — from Audible or Goodreads |
| Genre | Text | Primary genre (Fantasy, Sci-Fi, Memoir, etc.) |
| Length | `XXh YYm` | **Web search** audiobook runtime from Audible listing |
| Goodreads | URL | **Web search** `<Title> <Author> Goodreads` — use full `https://www.goodreads.com/book/show/XXXXX` URL |
| Status | Enum | Default `Queued`. Options: `Queued`, `Listening`, `Completed`, `DNF` |

## Optional Columns

| Column | Format | When to fill |
|--------|--------|-------------|
| Series | Text | Series name + book number (e.g. `Stormlight Archive #1`). Fill if the book is part of a series |
| Rating | `1-10` | Only after user has finished or DNF'd it |
| Notes | Text | Narrator quality, user's thoughts, who recommended it |

## Rules

- Always search the web for the Goodreads link, narrator, and runtime — do not guess
- If a book has multiple audiobook editions (different narrators), prefer the most popular/highest-rated narration
- For series entries, always include the series name and book number in the Series column
- Append new rows at the top of the table (newest first)
- Check for duplicates before adding — if the title already exists, tell the user instead of adding again
