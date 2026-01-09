# quiz-cli

Lightweight, terminal‑friendly quiz service inspired by **Kahoot**. Built as a set of small, focused Python projects.


## Features

- **Real‑time WebSocket quiz flow** via FastAPI and `websockets`
- **Multiple player support** with simple connection management
- **Admin‑driven progression** through quiz questions
- **YAML‑defined quizzes** validated against shared dataclasses
- **Terminal‑based UX** for both admin and players
- Clean, typed Python code with `ruff`, `ty` and managed by `uv` tooling


## Architecture overview

This GitHub organization currently contains these repositories:

- **Server** – FastAPI WebSocket backend: see [`quiz-server` README](https://github.com/quiz-cli/quiz-server#readme)
  - FastAPI app exposing WebSocket endpoints:
    - `/admin` – managed by the admin client
    - `/connect/{player_name}` – used by player clients
  - Manages connected players and their permissions via:
    - [`models.Player`](https://github.com/quiz-cli/quiz-server/blob/main/src/models.py)
    - [`models.Players`](https://github.com/quiz-cli/quiz-server/blob/main/src/models.py)
    - [`models.Results`](https://github.com/quiz-cli/quiz-server/blob/main/src/models.py)
  - Drives quiz progression using [`quiz_common.models.Quiz`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py).
- **Player client** – CLI quiz participant: see [`quiz-client` README](https://github.com/quiz-cli/quiz-client#readme)
  - Simple CLI application for quiz participants.
  - Connects to the server over WebSocket, receives questions, and sends answers.
  - Entry point: [`quiz_client.__main__.main`](https://github.com/quiz-cli/quiz-client/blob/main/src/quiz_client/__main__.py).
- **Admin client** – CLI quiz controller: see [`quiz-admin` README](https://github.com/quiz-cli/quiz-admin#readme)
  - CLI tool for lecturers / hosts to:
    - Load quizzes from YAML files.
    - Validate them with [`quiz_common.models.Quiz`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py).
    - Send the quiz to the server.
    - Step through questions interactively via WebSocket.
  - Entry point: [`quiz_admin.__main__.main`](https://github.com/quiz-cli/quiz-admin/blob/main/src/quiz_admin/__main__.py).
  - Example quiz files: [`quiz_example.yaml`](https://github.com/quiz-cli/quiz-admin/blob/main/data/quiz_example.yaml)

- **Shared models** – common dataclasses for quizzes: see [`quiz-common` README](https://github.com/quiz-cli/quiz-common#readme)
  - Defines dataclasses for quiz structure:
    - [`quiz_common.models.Quiz`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py)
    - [`quiz_common.models.Question`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py)
    - [`quiz_common.models.Option`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py)
  - Used by server and both clients to ensure a consistent quiz format (YAML / JSON).


## Development

- Python >=3.13
- Code style and linting via `ruff` (`ruff format`, `ruff check`)
- Type checking enforced via `ty` (`ty check`)
- Modern packaging (`pyproject.toml`) and dependency management (`uv`)
- All development artifacts must be in English
  - Source code (variable names, function names, class names)
  - Comments and docstrings
  - Commit messages
  - Issues and pull requests
  - Code review comments and discussions

  This ensures consistency and accessibility for all contributors.

- Branch naming convention for developers with write access:
  - Format: `<initials>-<issue-number>-<short-description>`
  - Example: `mz-1-short-desc`
