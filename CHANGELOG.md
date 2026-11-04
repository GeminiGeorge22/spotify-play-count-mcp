# Changelog

All notable changes to this project.

## [0.1.0] - 2026-11-03

- Open-source stdio MCP server (Node.js, plus a matching Python package `spotify-play-count-mcp` in `python/`) for the Spotify Play Count & Monthly Listeners Scraper Actor.
- `initialize` and `tools/list` work without a token: the tool is served from a checked-in `tools.json` built from the Actor's public input schema (`npm run build:tools`).
- `tools/call` goes to Apify's hosted MCP gateway with your own `APIFY_TOKEN`; without a token it returns a clear error.
- Tests run with no token (an env guard fails the suite if `APIFY_TOKEN` is set); CI and a daily workflow fail on drift between `tools.json` and the live public input schema.
- `server.json` 1.0.0 for the official MCP Registry (Streamable HTTP remote); PyPI trusted-publishing workflow; Dockerfile, glama.json, SECURITY.md and Dependabot config.
