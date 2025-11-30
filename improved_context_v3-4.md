Unified Context — Gemini + Qwen (Full-Stack & CLI-Optimized)
Version: 3.4
Last Updated: November 3, 2025

0. Overview
This unified context empowers both Gemini and Qwen3-Coder to:
Generate, analyze, and optimize full-stack code (React + Python).
Support asynchronous FastAPI backends, TypeScript frontends, and CLI automation.
Enforce clean code, testing standards, and LLM-friendly structure.
Integrate seamlessly with VS Code Continue Plugin and Qwen CLI.
Designed for developers who build, test, and deploy across ecosystems.
Optimized for cross-language reasoning, test-driven workflows, and secure production automation.

⚠️ Critical Safety Rule:
The Qwen CLI scans all Markdown files for patterns it mistakes as import paths.
This file contains no executable Python syntax—only plain-English descriptions.
Never include decorator syntax (e.g., "at-app-dot-get"), function calls, or import statements anywhere in this file.

0.1 Dependency Management & Updates
Regularly review dependencies:
Python: `pip list --outdated`
Node.js: `npm outdated`
Use Dependabot or Renovate. Test updates in staging before production.

1. Build & Validation Workflow
1.1 Node / TypeScript
npm run preflight  # runs build, test, typecheck, lint
1.2 Python
pytest -v
mypy .
black --check .

2. Environment & Configuration
Python: Use `venv` + `requirements.txt`
Node.js: Use `package-lock.json` with exact versions
Load `.env` via `python-dotenv` (Python) or `dotenv` (Node.js)
Commit a template:
# .env.example
DATABASE_URL=postgresql://user:pass@localhost/db
API_KEY=your_api_key_here

3. Qwen CLI Integration
3.1 Correct Usage
✅ Good:
qwen run --model qwen2.5-coder --input "Generate a FastAPI CRUD app with async database support and Pydantic validation."
❌ Never do this:
Do not paste code into `--input`
Do not include any Python-like syntax in Markdown files

3.2 Best Practices
Store configs in project root (`.env`, `qwen_config.yaml`)
Use `Makefile` for multi-step workflows
Capture logs: `qwen run ... > output.log 2>&1`
Work in isolated environments

3.3 Docker
Python: `python:3.11-slim`, non-root user, `--no-cache-dir`
Node.js: `node:20-alpine`, copy `package-lock.json` before `package.json`
Never embed secrets in images

4. Testing and Quality Control
TypeScript: Use Vitest; co-locate `.test.ts` files
Python: Use `pytest` with `TestClient` for FastAPI
E2E: Use Playwright or Cypress in isolated environments
Target ≥90% coverage for critical logic

5. JavaScript / TypeScript Guidelines
Prefer interfaces over classes for data
Avoid `any`; use `unknown` with type guards
Use Zustand or Jotai for global state (not Redux unless necessary)
Keep components pure and Concurrent React–compatible

6. Python Development Guidelines
6.1 Core Principles
Use functions over classes
Add type hints and Google-style docstrings
Follow PEP 8 and PEP 257
Handle specific exceptions (e.g., `ValueError`, not `Exception`)

6.2 Recommended Libraries
| Category       | Library         | Purpose                |
|----------------|------------------|------------------------|
| Web            | FastAPI          | Async web framework    |
| ORM            | SQLAlchemy 2.x   | Database modeling      |
| Validation     | Pydantic v2      | Schema & serialization |
| HTTP Client    | httpx            | Async-first requests   |
| Config         | python-dotenv    | Environment loading    |

6.3 Async & Validation (Descriptive Only)
To manage resource lifecycle in FastAPI:
Define an async context manager that connects to the database on startup and disconnects on shutdown
Pass this context manager as the `lifespan` argument when creating the FastAPI app
For data validation:
Define a Pydantic model with an email field (validated as email format), a username (3–50 chars), and an optional active flag
🔒 No actual code is shown here to prevent CLI parsing errors.

7. Logging & Error Handling
Use Python `logging` module (not `print`)
Implement structured logging in production
Add retry logic with exponential backoff for network calls
Include a `/health` endpoint in all services

8. Security & Reliability
8.1 Key Practices
Never hardcode secrets; use environment variables or secret managers
Validate and sanitize all external inputs
Enforce HTTPS with certificate verification
Pin all dependency versions for reproducible builds

8.2 API Protection (Descriptive Only)
To protect FastAPI endpoints from abuse:
Use a rate-limiting middleware that tracks requests per client IP
Configure a limit of 5 requests per minute on sensitive routes (e.g., `/items`)
Return a 429 JSON response when the limit is exceeded
Register a custom error handler for rate-limit exceeded events
🔒 No decorator syntax (such as "at-app-dot-get" or "at-limiter-dot-limit") or route definitions are included.

9. Style & Naming
| Element                | Convention     | Example          |
|------------------------|----------------|------------------|
| Python vars/functions  | snake_case     | calculate_total  |
| JS/TS vars/functions   | camelCase      | calculateTotal   |
| React components       | PascalCase     | UserProfile      |
| CLI flags              | --kebab-case   | --input-file     |

Use `isort` (Python) and ESLint `import/order` (JS/TS) for consistent imports.

10. Summary
Unified support for Gemini + Qwen3-Coder
Production-ready, async-safe, testable design
Works in VS Code and Qwen CLI
Covers full-stack (React + Python) with security and reliability
When generating code, always use natural-language prompts like:
"Create a FastAPI application that sets up a database connection during startup, applies rate limiting of 5 requests per minute to the /items route, handles rate limit exceeded errors with a 429 JSON response, and uses a Pydantic model for user creation with email validation."
