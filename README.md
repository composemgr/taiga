## 👋 Welcome to taiga 🚀

Project management platform for agile teams

## 📋 Description

Project management platform for agile teams

One all-in-one image, `casjaysdevdocker/taiga`, serves the whole of Taiga behind a
single nginx gateway on port 80: the front end, the API (`/api`, `/admin`), the
async worker, the events websocket (`/events`) and signed protected media
(`/media`). Only PostgreSQL and RabbitMQ run as separate containers. The image
source is <https://github.com/casjaysdevdocker/taiga>. Migrations and the first
administrator run automatically on first start.

## 🚀 Services

- **app**: casjaysdevdocker/taiga:latest (port 80)
- **db**: postgres:latest
- **rabbitmq**: rabbitmq:3-alpine

RabbitMQ is pinned to 3.x on purpose: 4.x rejects the transient queues that
Taiga events and Celery declare, so the workers cannot connect.

## 📦 Installation

### Option 1: Quick Install

```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/taiga/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone

```bash
git clone "https://github.com/composemgr/taiga" ~/.local/srv/docker/taiga
cd ~/.local/srv/docker/taiga
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr

```bash
composemgr install taiga
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
BASE_HOST_NAME=taiga.example.com
BASE_DOMAIN_NAME=example.com
APP_SECRET_TOKEN_64=replace-with-a-random-64-character-hex-string
APP_ADMIN_USER=admin
APP_ADMIN_EMAIL=admin@example.com
APP_ADMIN_PASS=replace-with-a-random-password
APP_RABBITMQ_PASS=replace-with-a-random-password
DB_USER_NAME=taiga
DB_USER_PASS=replace-with-a-random-password
DB_CREATE_DATABASE_NAME=taiga
```

The site is served as `https://${BASE_HOST_NAME}`, so put an HTTPS reverse proxy
in front of the published port. `APP_SECRET_TOKEN_64` is the Django secret key;
when it is empty the image generates one and keeps it in `./volumes/config/taiga`.
`APP_ADMIN_USER`, `APP_ADMIN_EMAIL` and `APP_ADMIN_PASS` create the first
administrator, since Taiga has no default account.

See `docker-compose.yaml` for the complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

The inline defaults in `docker-compose.yaml` boot the stack, but they use
placeholder passwords, so set the passwords above before first use.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64361` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The proxy variants publish no host ports; the app is reached only through the
proxy network.

## 🌐 Access

- **Web Interface**: <http://172.17.0.1:64361>

## 📂 Volumes

- `./volumes/config/taiga` - Generated configuration and the persisted secret key
- `./volumes/data/taiga` - Uploaded media, exports and logs
- `./volumes/data/rabbitmq/taiga` - RabbitMQ queue data
- `./volumes/db/postgres/taiga` - Postgres database

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
