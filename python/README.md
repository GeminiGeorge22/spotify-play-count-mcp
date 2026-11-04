# spotify-play-count-mcp (Python)

<!-- mcp-name: io.github.GeminiGeorge22/spotify-play-count-mcp -->

Python version of the [spotify-play-count-mcp](https://github.com/GeminiGeorge22/spotify-play-count-mcp) stdio MCP server, built on the official [`mcp`](https://pypi.org/project/mcp/) Python SDK. Same tool, same `APIFY_TOKEN` env var, same hosted endpoint as the Node.js server.

Spotify play counts for any track, album, artist or playlist link, plus an artist's monthly listeners and followers. Optional streams over the last 1, 7 and 30 days for tracks in daily tracking; those fields are empty on a track's first day and fill in after the first day of history.

It answers `initialize` and `tools/list` locally from the bundled `tools.json` (no token, no network). `tools/call` is forwarded to Apify's hosted MCP endpoint (`mcp.apify.com`) with **your own** `APIFY_TOKEN`, so each run uses your Apify account. Without a token a call returns a clear error instead of running anything. The token is only sent in the `Authorization` header and is never logged.

## Install

Requires Python 3.10+. With [uv](https://docs.astral.sh/uv/):

```bash
uvx spotify-play-count-mcp
```

With pip:

```bash
pip install spotify-play-count-mcp
spotify-play-count-mcp
```

To run the latest unreleased code from GitHub: `uvx --from "git+https://github.com/GeminiGeorge22/spotify-play-count-mcp#subdirectory=python" spotify-play-count-mcp`.

MCP client config:

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

Optional env: `APIFY_ACTORS` (comma-separated subset of the Actors) and `APIFY_MCP_URL` (full endpoint override).

## Tests

Tests use an in-process mock server, with no network and no token. They fail if `APIFY_TOKEN` is set.

```bash
cd python && pip install -e ".[test]"
env -u APIFY_TOKEN python -m pytest
```

`python scripts/build_tools.py --check` compares the bundled `tools.json` with each Actor's live public input schema and exits 1 on any mismatch.
