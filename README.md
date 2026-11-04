# Spotify Play Count MCP: Plays & Monthly Listeners

<!-- mcp-name: io.github.GeminiGeorge22/spotify-play-count-mcp -->

An MCP server that gives your AI assistant Spotify play counts. Paste a track, album, artist or playlist link (or a `spotify:` URI) and it returns **every track's play count**, and for artists their **monthly listeners**, **followers**, world rank and top cities. Optionally it adds streams over the last 1, 7 and 30 days for tracks in daily tracking. No Spotify login and no proxy setup.

Built on the public Spotify web pages, via the Apify Actor [Spotify Play Count & Monthly Listeners Scraper](https://apify.com/publicrecords/spotify-play-count-scraper). Not affiliated with Spotify.

- Apify Store: https://apify.com/publicrecords/spotify-play-count-scraper
- MCP server: https://mcp.apify.com?tools=publicrecords/spotify-play-count-scraper

## Example prompts

- "How many plays does each track on this album have? https://open.spotify.com/album/4aawyAB9vmqN3uQ7FjRGTy"
- "Compare monthly listeners and followers for these three artists."
- "Which tracks on this playlist have the most plays?"
- "Track my artist page weekly and show streams per day for every song."

## Tool

| Tool | Input | Output |
| --- | --- | --- |
| `publicrecords--spotify-play-count-scraper` | `urls` (track, album, artist or playlist links or URIs), `addDailyStreams`, `includeArtistAlbums`, `maxTracksPerTarget` | One row per track (track, artists, album, play count, release date, links) and one per artist (monthly listeners, followers, world rank, top cities) |

About the daily stream fields (`streamsLast1d`, `streamsLast7d`, `streamsLast30d`, with `addDailyStreams` on): a track seen for the first time gets empty values on that day. The fields fill in after the first day of history, so a run a day later shows the last day and a week later the last 7 days.

The full input schema is in [`tools.json`](tools.json), generated from the Actor's public input schema and checked for drift daily.

## MCP endpoint

```
https://mcp.apify.com?tools=publicrecords/spotify-play-count-scraper
```

Transport: Streamable HTTP (hosted by Apify, see https://docs.apify.com/mcp).

## Auth

Sign in with Apify OAuth when your client prompts you, or send the header
`Authorization: Bearer <APIFY_TOKEN>` where `<APIFY_TOKEN>` is your own Apify API token.
Never commit a real token.

## Client setup

### Claude

Claude Code:

```
claude mcp add --transport http spotify-play-count "https://mcp.apify.com?tools=publicrecords/spotify-play-count-scraper"
```

Claude Desktop / claude.ai: Settings → Connectors → Add custom connector, and paste the MCP endpoint URL above.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "spotify-play-count": {
      "url": "https://mcp.apify.com?tools=publicrecords/spotify-play-count-scraper"
    }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "spotify-play-count": {
      "type": "http",
      "url": "https://mcp.apify.com?tools=publicrecords/spotify-play-count-scraper"
    }
  }
}
```

To use a token instead of OAuth, add `"headers": { "Authorization": "Bearer <APIFY_TOKEN>" }` next to `"url"`.

---

Disclosure: I maintain this Actor.

## Local stdio server (open source)

This repo also ships a small MCP server in [`src/`](src/) for clients that only run local stdio servers. It answers `initialize` and `tools/list` locally from [`tools.json`](tools.json), which is generated from the Actor's public input schema and checked for drift in CI and daily, so any client or directory can inspect the tools with no token. `tools/call` is forwarded to the hosted Apify MCP endpoint above with **your own** `APIFY_TOKEN`; without a token a call returns a clear error instead of running anything. The token is only sent in the `Authorization` header and is never logged.

```bash
npx -y github:GeminiGeorge22/spotify-play-count-mcp
```

```json
{
  "mcpServers": {
    "spotify-play-count": {
      "command": "npx",
      "args": ["-y", "github:GeminiGeorge22/spotify-play-count-mcp"],
      "env": { "APIFY_TOKEN": "YOUR_APIFY_TOKEN" }
    }
  }
}
```

Or from a clone:

```bash
git clone https://github.com/GeminiGeorge22/spotify-play-count-mcp.git
cd spotify-play-count-mcp && npm install
APIFY_TOKEN=YOUR_APIFY_TOKEN node src/server.js
```

Optional env: `APIFY_ACTORS` (comma-separated subset of the Actors above) and `APIFY_MCP_URL` (full endpoint override).

Tests run against an in-memory mock server, with no network and no token: `npm test`. Requires Node.js 18+.

### Python (uvx/pip)

A matching Python version of this server lives in [`python/`](python/), built on the official `mcp` Python SDK: same tool, same `APIFY_TOKEN`, same hosted endpoint. Requires Python 3.10+.

```bash
uvx spotify-play-count-mcp
```

```json
{
  "mcpServers": {
    "spotify-play-count": {
      "command": "uvx",
      "args": ["spotify-play-count-mcp"],
      "env": { "APIFY_TOKEN": "YOUR_APIFY_TOKEN" }
    }
  }
}
```

Or with pip: `pip install spotify-play-count-mcp`, then run `spotify-play-count-mcp`. Until the first PyPI release, run it from GitHub: `uvx --from "git+https://github.com/GeminiGeorge22/spotify-play-count-mcp#subdirectory=python" spotify-play-count-mcp`. Python tests: `cd python && pip install -e ".[test]" && env -u APIFY_TOKEN python -m pytest`.

## See also

- [google-ads-transparency-mcp](https://github.com/GeminiGeorge22/google-ads-transparency-mcp): Competitor Google ads from the Ads Transparency Center: new, running and stopped.
- [etsy-sales-mcp](https://github.com/GeminiGeorge22/etsy-sales-mcp): Etsy search results and Etsy shop sales.
- [ecommerce-sales-mcp](https://github.com/GeminiGeorge22/ecommerce-sales-mcp): E-commerce sales signals: Amazon bestsellers and reviews, eBay sold prices, TikTok Shop and Etsy shop sales.
- [sports-odds-mcp](https://github.com/GeminiGeorge22/sports-odds-mcp): Sports odds from Kambi sportsbooks.
