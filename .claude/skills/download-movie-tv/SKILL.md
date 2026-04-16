---
name: download-movie-tv
description: >
  Search IPTorrents and PassThePopcorn for movies or TV shows, grab torrents to qBittorrent,
  and update the watchlist. Use when the user wants to download, grab, torrent, or pirate a
  movie or TV show. Triggers include "download X", "grab X", "torrent X", "search ipt",
  "find on iptorrents", "search ptp", or when a movie/show is on the watchlist and the user
  says "get it", "download it", or "grab that one". Do NOT use for audiobooks — use
  download-audiobook instead.
---

# Download Movie / TV Show

Search IPTorrents + PassThePopcorn via Prowlarr, download via qBittorrent, and track status in the watchlist.

This skill requires the `litellm-homelab-ops` MCP server for SSH access to docker-host-01 where Prowlarr and qBittorrent run.

## Connection Details

All commands run on **docker-host-01** via `ssh-ssh_execute_command` (server: `docker-host-01`).

| Service | URL (from docker-host-01) | Auth |
|---------|---------------------------|------|
| Prowlarr | `http://localhost:9696` | API Key: `1899574556fe403a89969ff5aa53efa7` |
| qBittorrent | `http://localhost:1337` | `admin` / `adminadmin` |
| Jellyfin | `http://localhost:8096` | Token: `5bbf0fd575894eae8cfbe9eb96d47b84` |

### Prowlarr Indexer IDs

| ID | Tracker | Use For |
|----|---------|---------|
| 4 | IPTorrents | Movies, TV, Music, general |
| 6 | PassThePopcorn (PTP) | Movies/films (superior quality, rare titles) |
| 5 | BroadcasTheNet | TV |

**For movie searches, always search both IPT (4) and PTP (6):** `indexerIds=4,6`

## Workflow

### 1. SEARCH — Find torrents via Prowlarr

Run via SSH on docker-host-01:
```bash
curl -s -H "X-Api-Key: 1899574556fe403a89969ff5aa53efa7" \
  "http://localhost:9696/api/v1/search?query=QUERY&indexerIds=4,6&limit=20"
```

**Always search 4K/2160p first.** Only fall back to 1080p if no 4K release exists.

Parse results and present as a table sorted by seeders (highest first):
```
Title | Size (GB) | Seeders | Quality | Tracker
```

**Quality preference order:**
1. REMUX (untouched BluRay, best quality)
2. BluRay x265/HEVC (great quality, smaller)
3. BluRay x264 (good quality)
4. WEB-DL (good quality, reasonable size)
5. WEBRip (acceptable)

### 2. GRAB — Download selected torrent

All via SSH on docker-host-01:

```bash
# Download .torrent file
curl -s -o /tmp/grab.torrent "DOWNLOAD_URL"

# Login to qBittorrent
curl -s -c /tmp/qbt_cookie -X POST "http://localhost:1337/api/v2/auth/login" \
  -d "username=admin&password=adminadmin"

# Add to qBittorrent
curl -s -b /tmp/qbt_cookie -X POST "http://localhost:1337/api/v2/torrents/add" \
  -F "torrents=@/tmp/grab.torrent" \
  -F "savepath=SAVE_PATH" \
  -F "category=CATEGORY"
```

### 3. SORT — Save paths by content type

| Content Type | Save Path | Category |
|-------------|-----------|----------|
| Movies | `/data/media/movies` | `movies` |
| TV Shows | `/data/media/tv` | `tv` |
| Cartoons | `/data/media/cartoons` | `cartoons` |

### 4. UPDATE WATCHLIST

After grabbing a torrent, check if the title exists in the watchlist:

- **If found in `movies/watchlist.md` or `shows/watchlist.md`**: set the DL column to `✅` and add a note like "Downloaded 2160p REMUX"
- **If not in watchlist**: offer to add it using the add-media skill workflow (web search for IMDB, trailer, etc.), and set DL to `✅` since it's being downloaded

Commit and push the watchlist update:
```bash
git add movies/watchlist.md shows/watchlist.md
git commit -m "update: <Title> — downloading"
git push
```

### 5. MONITOR — Check download progress

Via SSH on docker-host-01:
```bash
curl -s -b /tmp/qbt_cookie "http://localhost:1337/api/v2/torrents/info?category=CATEGORY"
```

### 6. SCAN — Trigger Jellyfin library refresh

Via SSH on docker-host-01:
```bash
curl -s -X POST "http://localhost:8096/Library/Refresh" \
  -H 'Authorization: MediaBrowser Token="5bbf0fd575894eae8cfbe9eb96d47b84"'
```

## Error Handling

- **No results**: Try broader terms, remove year/quality filters
- **Download fails**: Re-login to qBittorrent (cookie expired)
- **Prowlarr 401**: Check indexer health at `/api/v1/health`
