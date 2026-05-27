# Tech Stack & Infrastructure

## Servers
- Produce app runs in Docker container on remote SSH server
- Container: `produceorderapp-produceorderapp-1`
- Config via docker-compose.yml

## Services
- **orders.everblack.cloud** — produce order API
- **phone.everblack.cloud** — Vapi phone agent
- **Vapi** — phone agent platform
- **Telegram** — primary comms channel with Bishop

## Tools Bishop Uses
- `gh` CLI for GitHub
- `docker` for container management
- SSH terminal backend
