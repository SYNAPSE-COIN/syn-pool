# CRUSH.md

Build / Run
- uv sync --extra all; uv pip install flash-attn --no-build-isolation
- uv run main                      # CLI entry; see AGENT.md for modes
- uv run main --dry --n 1 --debug  # Dry run with mock client
- uv run main <ware|loom> <dry|train|dump> [n] [opts]

Test
- uv run testslide syn_pool/test_SYNAPSeware_parse_testslide.py  # pre-commit hook target
- uv run testslide tests/                                       # run all TestSlide suites
- uv run pytest tests -q                                        # for pytest-based checks
- Single test: uv run pytest tests/test_SYNAPSeware.py::test_basic -q

Docs
- (Sphinx under docs/) make -C docs html

Lint / Typecheck
- If configured, use:
  - uv run ruff check syn_pool
  - uv run mypy syn_pool

Style
- Python 3.11–3.12 via uv. No comments unless requested. Avoid kwargs unless defining a public API surface.
- Favor mutable structs/classes over heavy functional patterns. Strong typing; explicit imports only; don’t assume undeclared deps.
- Security: never log secrets; no plaintext keys.
- Logging: use syn_pool.lib.log (RichHandler + color helpers). Prefer `getLogger/setup_logging`. Console logs concise; file logs detailed.
- Errors: raise specific exceptions; in CLI paths, show help on bad args; follow `log_design` principles from AGENT docs.
- Naming: snake_case for vars/fns, PascalCase for classes, UPPER_SNAKE for consts.
- Imports: absolute within package (`from syn_pool.*`) per existing layout.
- Formatting: black-like; keep lines readable (~120 cols for log blocks).
- CLI: prefer `uv run main`; flags in `argp.py`.
- Testing: prefer TestSlide where it fits; mock network boundaries.

Cursor/Copilot guidelines
- Include `.cursor/rules/base.mdc` guidance (coding, logging, commands). Follow Pair Programming + Work Standards from AGENT docs.

Notes
- Project scripts: `[project.scripts]` expose `main/errl` in `pyproject.toml`. vLLM/flash-attn are heavy; uv toolchain recommended.
- Uses `runs/` and `logs/` directories created at runtime.
