# Stack Profile: Python + FastAPI

Copy these values into your pipeline when prompted, or use:
`/dev-pipeline start "..." --stack python`

| Variable | Value |
|----------|-------|
| `{{TECH_STACK}}` | Python + FastAPI + SQLAlchemy |
| `{{BUILD_COMMAND}}` | `uv run python -m pytest --collect-only -q` |
| `{{TEST_COMMAND}}` | `uv run pytest` |
| `{{LINT_COMMAND}}` | `uv run ruff check .` |
| `{{EXT}}` | `py` |

## Recommended packages
- **Runtime**: `fastapi`, `uvicorn`
- **DB**: `sqlalchemy`, `alembic`
- **Validation**: `pydantic`
- **Auth**: `python-jose`, `passlib`
- **Testing**: `pytest`, `httpx`, `pytest-asyncio`
