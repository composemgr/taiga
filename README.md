## 👋 Welcome to taiga 🚀

Project management platform for agile teams

## 📋 Description

Project management platform for agile teams

## 🚀 Services

- **taiga**: taigaio/taiga:latest

### Infrastructure Components

- **taiga-db**: Postgres database


## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/taiga/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/taiga" ~/.local/srv/docker/taiga
cd ~/.local/srv/docker/taiga
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install taiga
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
```

See `docker-compose.yaml` for complete list of configurable options.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:8076

## 📂 Volumes

- `./rootfs/data/taiga` - Data storage
- `./rootfs/config/taiga` - Data storage
- `./rootfs/data/db/postgres/taiga` - Data storage

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose logs -f taiga
```

## 🛠️ Management

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# Update to latest images
docker compose pull && docker compose up -d

# View logs
docker compose logs -f

# Restart services
docker compose restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
