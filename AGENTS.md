# Base44 Dev Environment — Maigret

## What this is
Maigret is a Python OSINT tool that searches for a username across ~5,900 sites.
It has a built-in Flask web UI (`maigret --web <port>`).

## Running in the sandbox
```bash
docker compose -f docker-compose.base44.yml up -d --build
```
The web UI is served on **port 3000**.

## Key facts
- **No external database** — the sites database is a bundled JSON file at `maigret/resources/data.json` (~2.4 MB). Job results are kept in-memory.
- **No API keys required** for the web UI or basic username search. The optional `--ai` feature needs `OPENAI_API_KEY`, but the web UI does not use it.
- **Flask debug mode** is enabled in the sandbox for auto-reload on file changes.
- The Dockerfile.base44 installs system build deps (libcairo, libxml2, libxslt) and all Python deps during the build. At runtime the source is bind-mounted and an editable install link is created so edits are picked up without a rebuild.
- `YARL_NO_EXTENSIONS=1` is set during `pip install` in the Dockerfile to avoid building yarl C extensions.

## Verifying it works
```bash
curl -s http://localhost:3000/ | head -5   # should return HTML
```
