# General Rules — Media Watchlist

This is a personal media tracking repository. Every interaction likely involves adding, updating, or browsing watchlist entries.

## Core Conventions

- **URLs are mandatory** — every entry must have real, verified links (IMDB, YouTube, Goodreads). Web search for them; never fabricate or leave as placeholders.
- **Markdown link syntax** — always use `[Label](url)` in table cells to keep tables readable on GitHub.
- **Newest first** — new rows go at the top of the table, directly below the header row.
- **No duplicates** — before adding, scan the target `watchlist.md` for the title. If it exists, tell the user.
- **Auto commit and push** — after every add, update, or remove, automatically `git commit` and `git push` to main without asking. Never wait for confirmation.

## Status Values

Each media type has its own valid statuses:

| Type | Valid Statuses |
|------|---------------|
| Movies | `Queued`, `Watched`, `Skipped` |
| TV Shows | `Queued`, `Watching`, `Completed`, `Dropped` |
| Audiobooks | `Queued`, `Listening`, `Completed`, `DNF` |

## Commit Message Format

- Adding: `add: <Title> (<Year>) to <type> watchlist`
- Updating: `update: <Title> — <what changed>`
- Batch: `add: <Title1>, <Title2> to <type> watchlist`
- Removing: `remove: <Title> from <type> watchlist`

## File Locations

All data lives in markdown tables — one per category:

```
movies/watchlist.md
shows/watchlist.md
audiobooks/watchlist.md
```

Do not create additional files for individual entries. Everything goes in the table.
