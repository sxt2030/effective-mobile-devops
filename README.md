# Effective Mobile — DevOps Test Task

A simple web application deployed in Docker containers with Nginx as a reverse proxy.

## Architecture

Client (curl) --> [Port 80] --> Nginx (proxy) --> [Docker Network :8080] --> Backend (Python)

The client sends a request to Nginx on port 80. Nginx proxies the request to the backend inside an isolated Docker network. The backend listens on port 8080 and is not directly accessible from the host.

## Tech Stack

- **Backend** — Python 3.12 (built-in `http.server` module)
- **Reverse Proxy** — Nginx (official Alpine image)
- **Containerization** — Docker + Docker Compose

## Project Structure
.
├── backend/
│ ├── Dockerfile # Python image build
│ └── app.py # HTTP server in Python
├── nginx/
│ └── nginx.conf # Reverse proxy configuration
├── docker-compose.yml # Service orchestration
└── README.md

## Quick Start

### Prerequisites

- Docker 20.10+
- Docker Compose 2.0+

### Run

```bash
git clone https://gitlab.com/maxpack2030/effective-mobile-devops.git
cd effective-mobile-devops
docker compose up -d

### Verify

docker compose ps
curl http://localhost

#Expected output:
Hello from Effective Mobile!


### Stop

docker compose down


### How It Works

Backend — a minimal Python HTTP server. Listens on port 8080 and responds with "Hello from Effective Mobile!" to any GET request to /. Runs as non-root user "appuser".

Nginx — accepts incoming connections on port 80 and proxies them to the backend via proxy_pass http://backend. Forwards standard proxy headers (Host, X-Real-IP, X-Forwarded-For, X-Forwarded-Proto). Server tokens are disabled for security.

Docker Compose — starts both services on an isolated bridge network "app-network". Only port 80 (Nginx) is exposed to the host. The backend remains accessible exclusively within the Docker network.

### Security

No secrets — no passwords, tokens, or keys in the repository

Non-root backend — backend runs as appuser

Minimal exposure — only port 80 exposed to the host, backend is network-isolated

Read-only mounts — Nginx config mounted as :ro

Server tokens off — Nginx version hidden from error pages and headers
