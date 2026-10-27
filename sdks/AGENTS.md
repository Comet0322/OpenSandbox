# SDKs AGENTS

You are working on OpenSandbox SDKs. Keep generated and handwritten code separate. Only the Python SDKs live here.

## Scope

- `sandbox/**`
- `code-interpreter/**`
- `mcp/**`
- workspace-level SDK build and release metadata

If the task is driven by spec changes, also read `../specs/AGENTS.md`.

## Key Areas

- `sandbox/python`
- `code-interpreter/python`
- `mcp/sandbox/python`

## Generated Code

Do not manually edit generated code as the only fix.

Generator-owned paths include:

- `sandbox/python/src/opensandbox/api/**`
- OpenAPI outputs produced by local generator scripts

Handwritten logic belongs in adapters, services, facades, converters, and stable SDK models.

## Commands

Python sandbox SDK:

```bash
cd sdks/sandbox/python
uv sync
uv run python scripts/generate_api.py
uv run ruff check
uv run pyright
uv run pytest tests/ -v
uv build
```

Python code-interpreter SDK:

```bash
cd sdks/code-interpreter/python
uv sync
uv run ruff check
uv run pyright
uv run pytest
uv build
```

## Guardrails

Always:

- For spec-driven changes, regenerate affected SDK code, update handwritten layers, then run the affected package checks.
- For MCP changes, keep tool schemas, client setup docs, and sandbox SDK dependency behavior aligned.
- Add a regression test for every bug fix.
- Prefer tests for request mapping, response conversion, error mapping, streaming behavior, and resource cleanup.
- Keep package-local validation fast before widening verification.
- Keep wire-format units and public SDK units separate. Public SDK interfaces should expose time durations as `timedelta` or as explicitly second-based fields such as `timeout_seconds`.

Ask first:

- Public breaking changes
- Large refactors

Never:

- Patch generated output as the only fix.
- Change SDK public behavior without tests.
- Mix unrelated non-SDK work into an SDK change.

## Good Patterns

- Generated clients for normal request/response APIs
- Handwritten transport only for streaming or protocol-specific paths such as SSE
