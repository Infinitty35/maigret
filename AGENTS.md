# AGENTS.md

## Cursor Cloud specific instructions

Maigret is a standalone Python (>=3.10) OSINT CLI tool managed with **Poetry**. There is no database or backing service; the "sites database" is a bundled JSON file (`maigret/resources/data.json`). Dependencies are installed into an in-project virtualenv (`.venv`) by the startup update script, so run everything through `poetry run`.

- Poetry is installed to `~/.local/bin`. That directory is on `PATH` via `~/.bashrc`; if a non-login shell can't find `poetry`, run `export PATH="$HOME/.local/bin:$PATH"` first.
- Standard commands live in the `Makefile`: `make test` (coverage + pytest), `make lint` (flake8 + mypy), `make format` (black). Prefix with `poetry run` when invoking the underlying tools directly, e.g. `poetry run pytest tests`.
- `tests/test_executors.py::test_asyncio_progressbar_executor` is timing-sensitive and can fail flakily on a loaded VM (it asserts an upper bound on execution time). Re-run failing tests with `poetry run pytest --lf -vv` (aka `make rerun-tests`); `pytest-rerunfailures` is installed.
- Run the CLI with `poetry run maigret <username>`. Real searches hit live external sites, so they need internet access and results/error rates vary by site availability. Use `--top-sites N`, `--timeout`, and `--no-progressbar` to keep runs fast and quiet.
- Web interface: `poetry run maigret --web 5000`, then open `http://127.0.0.1:5000`. It binds to `127.0.0.1` by default (override with `FLASK_HOST`). It writes reports to `/tmp/maigret_reports` and uploads to `uploads/`. The web search also queries live sites and can take a while depending on `top_sites`.
- The repo ships a `.githooks/pre-commit` that runs `poetry run update_sitesmd` (regenerates `sites.md`). Git hooks are not enabled by default; enable with `git config core.hooksPath .githooks` if you need it.
