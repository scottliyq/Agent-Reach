# AGENTS.md

## Project
Agent Reach — Python CLI + library that gives AI agents read/search access to 14+ internet platforms.
Positioning: installer + doctor + config tool. NOT a wrapper — after install, agents call upstream tools directly.
Repo: github.com/Panniantong/Agent-Reach | License: MIT | Version: 1.4.0

## Environment
- Always use the Conda environment `py312aiproxy` for this project
- Do not create or use `venv` / `.venv` for this repo
- Preferred command prefix: `conda run -n py312aiproxy`
- Python interpreter: `conda run -n py312aiproxy python`
- Pip installer: `conda run -n py312aiproxy python -m pip`
- Pytest runner: `conda run -n py312aiproxy python -m pytest`

## Commands
- `conda run -n py312aiproxy python -m pip install -c constraints.txt -e .[dev]` — Dev install
- `conda run -n py312aiproxy python -m pytest tests/ -v` — All tests
- `conda run -n py312aiproxy python -m pytest tests/test_cli.py -v` — CLI tests only
- `bash test.sh` — Legacy integration script; it creates its own venv, so do not use it for normal local workflow
- `conda run -n py312aiproxy python -m agent_reach.cli doctor` — Run diagnostics
- `conda run -n py312aiproxy python -m agent_reach.cli install --env=auto` — Auto-configure

## Structure
- `agent_reach/cli.py` — CLI entry point (argparse)
- `agent_reach/core.py` — Core read/search routing logic
- `agent_reach/config.py` — Config management (YAML, env vars)
- `agent_reach/doctor.py` — Diagnostics engine
- `agent_reach/channels/` — One file per platform (twitter.py, reddit.py, youtube.py, etc.)
- `agent_reach/channels/base.py` — Base channel class (all channels inherit from this)
- `agent_reach/integrations/mcp_server.py` — MCP server integration
- `agent_reach/skill/` — OpenClaw skill files
- `agent_reach/guides/` — Usage guides
- `tests/` — pytest tests
- `config/mcporter.json` — MCP tool config

## Conventions
- Python 3.10+ with type hints
- Each channel is a single file in `channels/`, inherits from `BaseChannel`
- Channel contract: must implement `can_handle(url)`, `read(url)`, `search(query)`, `check()` methods
- Use `loguru` for logging, `rich` for CLI output
- Commit format: `type(scope): message` (one commit = one thing)
- All upstream tool calls go through public API/CLI, never hack internals

## Rules
- NEVER modify upstream open source projects' source code
- Agent Reach is a "glue layer" — only route and call, don't reimagine
- Version in THREE places must match: `pyproject.toml`, `__init__.py`, `tests/test_cli.py`
- Always new branch for changes, PR to main, never push to main directly
- Run `conda run -n py312aiproxy python -m pytest tests/ -v` before committing — all tests must pass
- Cookie-based auth (Twitter, XHS): use Cookie-Editor export method only, no QR scan
- XHS login: Cookie-Editor browser export only (QR will hang)
- Do not create new virtualenvs for troubleshooting or local setup; keep all project work inside `py312aiproxy`
