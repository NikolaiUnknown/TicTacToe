# TicTacToe Application

> **Clone with submodules**

> ```bash
> git clone --recurse-submodules https://github.com/NikolaiUnknown/TicTacToe.git
> ```


This repository contains two independent services that together power a real‑time
multiplayer Tic‑Tac‑Toe game:

* **TicTacToe-Server** – a Java/Spring Boot server implementing user management, game
  logic and WebSocket messaging.
* **TicTacToe-Node** – a Node.js/Express static server delivering the browser client
  and managing JWT tokens for authentication.

---

## Backend (Spring Boot)

### Tech & Dependencies

* Java 21, Gradle 8.14.2
* Spring Boot 3.5, Security, Data JPA, WebSocket (STOMP)
* MySQL + Hibernate, MapStruct, JWT (access/refresh)
* Testing with JUnit 5 & Mockito; Lombok for boilerplate

### Core Responsibilities

* **User authentication** – signup, signin, JWT tokens with refresh support.
* **Game management** – create/cancel games, invite opponents, track moves.
* **Real‑time play** – STOMP/WebSocket endpoints for sending/receiving moves.
* **Player statistics** – history, ELO rating system, profiles.
* **Fault tolerance** – automatic cancellation on disconnect, exception handlers.

### API Highlights

```text
POST   /api/v1/auth/signin       → obtain tokens
POST   /api/v1/auth/signup       → register
POST   /api/v1/auth/refresh      → refresh JWT
GET    /api/v1/games/            → list user’s games
POST   /api/v1/games/            → create game
DELETE /api/v1/games/{id}        → cancel
GET    /api/v1/players/{id}/games → history
... (board state, profiles, etc.)
```

WebSocket endpoint: `/ws/connect`; send moves to `/app/game/move`.

### Configuration

Key values are stored in `backend/src/main/resources/application.yaml`:

```yaml
game:
  rating_increase: 25
  acceptable_disconnect_time: 20000
security:
  jwt:
    lifetime: 900000
```

### Running Locally

```bash
cd backend
./gradlew bootRun
```

---

## Frontend (Node/Express)

### Tech & Dependencies

* Node 22 (Alpine), Express 4.x, STOMP client (@stomp/stompjs)
* Static HTML/CSS/vanilla JS application
* Cookies/persistent tokens via localStorage

### Responsibilities

* Serve the browser UI and assets (login, matchmaking, game board, profile,
  leaderboard, FAQ, etc.).
* Manage JWT tokens and forwards API/WebSocket requests to the backend.
* Establish STOMP/WebSocket connections to receive game proposals and
  exchange moves in real time.
* Perform client‑side reconnection logic and token refresh.

### Quick Start

```bash
cd frontend
npm install        # for local development
BACKEND=localhost:8080 npm start
```

Docker container can be built with `docker build -t tictactoe-node .` and run
with `-e BACKEND=…`.

---

## Docker Compose Quick Start

The included `docker-compose.yaml` launches the backend, frontend and a MySQL
database in one command. Simply run:

```bash
docker compose up --build
```

Frontend is exposed on **http://localhost:3000**; the backend listens on
**http://localhost:8080**. Database credentials and other settings are defined in
`docker-compose.yaml`.

Use `docker compose down` to stop and remove the containers.

---

## Project Structure

```
backend/   – Spring Boot server (see backend/README.md for full details)
frontend/  – Node/Express client (see frontend/README.md for full details)
init/mysql/tables.sql – database schema
``` 

---

## License

Developed for educational purposes. All rights reserved.
