# Tech Stack & Infrastructure

## Stack
- **Language:** Python / Flask (single-file apps)
- **Database:** JSON files (produce app) + SQLite (WatcherHQ)
- **Hosting:** Hostinger KVM 2 VPS, Boston (187.124.248.154)
- **Containers:** Docker (no Compose for updates — docker cp + restart only)
- **Reverse proxy:** Nginx (assumed, based on domain routing)
- **Email:** Hostinger SMTP (smtp.hostinger.com:465)

## Key Services
| Service | URL | Container |
|---------|-----|-----------|
| Produce order app | orders.everblack.cloud | produceorderapp-produceorderapp-1 |
| WatcherHQ | watcherhq.net | watcherhq |
| Phone agent | phone.everblack.cloud | Vapi-hosted |

## GitHub
- Everblack repo: `finiteg35/everblack` (private)
- Agent Brain: `finiteg35/agent-brain` (autonomous)
- George Brain: `finiteg35/george-brain` (private, this repo)
- Bishop Brain: `finiteg35/bishop-brain` (private)
- Token: `/root/produceorderapp/.env` or `/data/.openclaw/workspace/.github_token`
- git config: user.email finiteg35@gmail.com | user.name George Walker

## Deploy Pattern
```bash
# Edit host file → syntax check → docker cp → restart → commit to GitHub
python3 -m py_compile app.py
docker cp app.py produceorderapp-produceorderapp-1:/app/app.py
docker restart produceorderapp-produceorderapp-1
```
⚠️ NEVER `docker compose up -d` for app updates

## DNS / Email (watcherhq.net)
- SPF: `v=spf1 include:_spf.mail.hostinger.com ~all`
- DKIM: hostingermail1._domainkey + CNAMEs → Hostinger
- DMARC: `v=DMARC1; p=none; rua=mailto:everblack@watcherhq.net`
- A `@` → 187.124.248.154

## Critical Technical Rules
- Never use contractions in single-quoted JS strings inside Python templates (apostrophe closes string)
- Avoid `new RegExp('\\b' + word + '\\b')` in JS inside Python templates — escaping mangled
- Never use innerHTML with nested quotes — use createElement + addEventListener
- Always grep FULL template for vendor AND admin blocks — they're separate
- Voice SR: `continuous=false` is correct. `continuous=true` breaks recognition.
