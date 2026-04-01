# SaaS Stack — Ready to Use

## Stack
| Container      | Rolle                        | Erreichbar (lokal)       |
|----------------|------------------------------|--------------------------|
| Nginx          | Reverse Proxy / Türsteher    | http://localhost         |
| Frontend       | Vite React App               | http://localhost/        |
| Backend        | FastAPI Python API           | http://localhost/api/    |
| Supabase       | Postgres DB + Auth           | http://localhost/supabase/ |
| n8n            | Automations                  | http://localhost/n8n/    |
| Redis          | Cache & Queue                | intern                   |
| Mailpit        | E-Mail (Dev UI)              | http://localhost/mail/   |

---

## Setup (einmalig)

### 1. Secrets generieren
```bash
cp .env.example .env
```
Dann `.env` öffnen und ausfüllen:
```bash
# JWT Secret generieren
openssl rand -base64 32

# Backend Secret generieren
openssl rand -hex 32

# n8n Key generieren
openssl rand -hex 24
```

### 2. Starten
```bash
docker compose up -d
```

### 3. Fertig
- Frontend:  http://localhost
- API Docs:  http://localhost/api/docs
- n8n:       http://localhost/n8n/
- Mailpit:   http://localhost/mail/

---

## Deinen Code reinlegen

### Frontend (Vite/React)
Einfach deinen kompletten Vite-Projektinhalt in `/frontend` legen.
Der Container lädt automatisch neu bei Änderungen.

### Backend (FastAPI)
Deinen Python Code in `/backend` legen.
`main.py` ist der Einstiegspunkt — einfach Routes ergänzen.
Der Container lädt automatisch neu bei Änderungen.

---

## Deployment mit Cloudflare

1. Server mit öffentlicher IP aufsetzen (Hetzner, DigitalOcean etc.)
2. Stack auf dem Server starten (`docker compose up -d`)
3. In Cloudflare: Domain hinzufügen → DNS A-Record auf Server-IP zeigen
4. In `.env`: `SITE_URL=https://deine-domain.com` setzen
5. Cloudflare Proxy aktivieren (oranges Wolken-Icon) → HTTPS automatisch

### Mailpit in Produktion ersetzen
In `docker-compose.yml` die Mailpit-Umgebungsvariablen in `supabase-auth`
auf deinen externen SMTP-Dienst (z.B. Resend, Postmark) umstellen:
```yaml
- GOTRUE_SMTP_HOST=smtp.resend.com
- GOTRUE_SMTP_PORT=465
- GOTRUE_SMTP_USER=resend
- GOTRUE_SMTP_PASS=dein-api-key
```
Danach den Mailpit-Block in `/nginx/conf.d/default.conf` entfernen.

---

## Nützliche Befehle

```bash
# Alles starten
docker compose up -d

# Logs anschauen
docker compose logs -f backend
docker compose logs -f frontend

# Einzelnen Container neu starten
docker compose restart backend

# Alles stoppen
docker compose down

# Alles stoppen + Volumes löschen (Achtung: Daten weg!)
docker compose down -v
```
