# quiz-cli

Lightweight, terminal‑friendly quiz tools inspired by **Kahoot**. Built as a set of small, focused Python projects.

This GitHub organization currently contains these repositories:

- **Server** – FastAPI WebSocket backend: see [`quiz-server` README](https://github.com/quiz-cli/quiz-server#readme)
- **Player client** – CLI quiz participant: see [`quiz-client` README](https://github.com/quiz-cli/quiz-client#readme)
- **Admin client** – CLI quiz controller: see [`quiz-admin` README](https://github.com/quiz-cli/quiz-admin#readme)
- **Shared models** – common dataclasses for quizzes: see [`quiz-common` README](https://github.com/quiz-cli/quiz-common#readme)

## Architecture overview

The system is split into independent components:

- **Shared models** (`quiz-common`)
  - Defines dataclasses for quiz structure:
    - [`quiz_common.models.Quiz`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py)
    - [`quiz_common.models.Question`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py)
    - [`quiz_common.models.Option`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py)
  - Used by server and both clients to ensure a consistent quiz format (YAML / JSON).

- **Server** (`quiz-server`)
  - FastAPI app exposing WebSocket endpoints:
    - `/admin` – managed by the admin client
    - `/connect/{player_name}` – used by player clients
  - Manages connected players and their permissions via:
    - [`models.Player`](https://github.com/quiz-cli/quiz-server/blob/main/src/models.py)
    - [`models.Players`](https://github.com/quiz-cli/quiz-server/blob/main/src/models.py)
    - [`models.Results`](https://github.com/quiz-cli/quiz-server/blob/main/src/models.py)
  - Drives quiz progression using [`quiz_common.models.Quiz`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py).

- **Player client** (`quiz-client`)
  - Simple CLI application for quiz participants.
  - Connects to the server over WebSocket, receives questions, and sends answers.
  - Entry point: [`quiz_client.__main__.main`](https://github.com/quiz-cli/quiz-client/blob/main/src/quiz_client/__main__.py).

- **Admin client** (`quiz-admin`)
  - CLI tool for lecturers / hosts to:
    - Load quizzes from YAML files.
    - Validate them with [`quiz_common.models.Quiz`](https://github.com/quiz-cli/quiz-common/blob/main/src/quiz_common/models.py).
    - Send the quiz to the server.
    - Step through questions interactively via WebSocket.
  - Entry point: [`quiz_admin.__main__.main`](https://github.com/quiz-cli/quiz-admin/blob/main/src/quiz_admin/__main__.py).
  - Example quiz files: [`quiz_example.yaml`](https://github.com/quiz-cli/quiz-admin/blob/main/data/quiz_example.yaml)


## Features

Across the organization, the projects together provide:

- **Real‑time WebSocket quiz flow** via FastAPI and `websockets`
- **Multiple player support** with simple connection management
- **Admin‑driven progression** through quiz questions
- **YAML‑defined quizzes** validated against shared dataclasses
- **Terminal‑based UX** for both admin and players
- Clean, typed Python code with `ruff` and `uv` tooling


## Getting started

### 1. Run the server

See [`quiz-server` README](https://github.com/quiz-cli/quiz-server#readme) for full details. In short:

```bash
$ cd quiz-server
$ uv tool install . --editable
$ uv run fastapi dev src/main.py
```

The server exposes WebSocket endpoints on the configured host/port.

### 2. Run the admin client

See [`quiz-admin` README](https://github.com/quiz-cli/quiz-admin#readme). Example:

```bash
$ cd quiz-admin
$ uv tool install . --editable
$ quiz-admin localhost:8000 data/quiz_example.yaml
```

The admin sends the quiz definition to the server and controls when to move to the next question.

### 3. Run one or more player clients

See [`quiz-client` README](https://github.com/quiz-cli/quiz-client#readme). Example:

```bash
$ cd quiz-client
$ uv tool install . --editable
$ quiz-client localhost:8000 Alice
```

Each player connects to `/connect/{player_name}`, receives questions, and answers via the CLI.


## Development

- Python >=3.13
- Code style and linting via `ruff` (`ruff format`, `ruff check`)
- Modern packaging (`pyproject.toml`) and dependency management (`uv`)
