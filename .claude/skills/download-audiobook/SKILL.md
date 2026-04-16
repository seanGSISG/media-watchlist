---
name: download-audiobook
description: >
  Search MyAnonamouse (MAM) for audiobooks and download via qBittorrent to Audiobookshelf.
  Use when the user wants to download, grab, or find an audiobook, or mentions MAM,
  MyAnonamouse, or Audiobookshelf. Also trigger when an audiobook is on the watchlist and
  the user says "get it", "download it", or "grab that one". Do NOT use for movies or TV
  shows — use download-movie-tv instead.
---

# Download Audiobook

Search MyAnonamouse, select the best torrent, download to Audiobookshelf via qBittorrent, and track in the watchlist.

This skill uses the `litellm-media` MCP server which provides MAM tools directly.

## MCP Tools Available

| Tool | Purpose |
|------|---------|
| `mam-mam_search` | Search MAM by title/author |
| `mam-mam_get_torrent` | Get torrent details by ID |
| `mam-qbt_add_from_mam` | Add MAM torrent to qBittorrent |
| `mam-qbt_list_downloads` | List current qBittorrent downloads |
| `mam-qbt_get_status` | Check specific download status |

## Selection Criteria

**Format priority:** m4b > m4a > mp3 > other

**Within same format, rank by:**
1. Seeders (highest first — proxy for quality and availability)
2. Snatched count (most grabbed = community-vetted)
3. Freeleech preferred when seeder counts are close

## Save Path

qBittorrent `audiobooks` category saves to `/data/media/books` (container path), which maps to `/home/adminuser/data/media/books` on host — the Audiobookshelf library root.

## Workflow

### 1. SEARCH

Use `mam-mam_search` with the book title and/or author.

### 2. SELECT — Apply format + seeder criteria

Present the best option with reasoning:

```
**Recommended:** Title Name (m4b, 885 MB)
  Author: Name | Narrator: Name | Seeders: 149
  Reason: m4b format, highest seeders

  Also available:
  - Same title (mp3, 894 MB) - 81 seeders
```

### 3. CONFIRM — Ask user before downloading

Show pick with title, author, narrator, format, size, seeders. Wait for confirmation.

### 4. DOWNLOAD

Use `mam-qbt_add_from_mam` with `torrent_id` as an **integer** (not string).

### 5. UPDATE WATCHLIST

After downloading, check if the title exists in `audiobooks/watchlist.md`:

- **If found**: set the DL column to `✅` and add a note with the format (e.g., "Downloaded m4b, narrated by X")
- **If not in watchlist**: add it using the full metadata workflow — web search for Goodreads URL, narrator, runtime, etc., append to `audiobooks/watchlist.md`, and set DL to `✅` since it's being downloaded

Also update the audiobook recommender library:
`~/.claude/skills/audiobook-recommender/library.md`
- Add to the appropriate genre section
- Set Source to `Audiobookshelf`, Status to `Queued`

Commit and push:
```bash
git add audiobooks/watchlist.md
git commit -m "update: <Title> — downloading"
git push
```

### 6. MONITOR

Use `mam-qbt_list_downloads` or `mam-qbt_get_status` to check progress.

## Tool Parameter Notes

- `mam-qbt_add_from_mam` requires `torrent_id` as an **integer** — pass the raw number, no quotes
- `mam-mam_search` returns torrent IDs as numbers — use them directly

## MAM Cookie Refresh

If `mam-mam_search` returns an authentication error, the `mam_id` cookie has expired.
Update it in `/home/adminuser/apps/mcp-stack/.env` on docker-host-01, then restart:
```bash
ssh docker-host-01 "cd /home/adminuser/apps/mcp-stack && docker compose restart mcp-mam"
```

## Edge Cases

- **No m4b available**: Fall back to mp3, note the compromise
- **No results**: Try broader search (title only, fewer keywords)
- **Series vs single book**: Prefer single-book torrent unless user requests the series
- **Multiple authors**: First author is primary
