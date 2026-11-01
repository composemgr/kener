## 👋 Welcome to kener 🚀

Modern, self-hosted status page and monitoring platform

## 📋 Description

Kener is a modern, self-hosted status page that lets you monitor uptime and
incidents for your services from a single dashboard.

## 🚀 Services

- **app**: rajnandan1/kener:latest

### Infrastructure Components

- **db**: Postgres database
- **redis**: Redis cache and job queue

## 📦 Installation

### Option 1: Quick Install

```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/kener/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone

```bash
git clone "https://github.com/composemgr/kener" ~/.local/srv/docker/kener
cd ~/.local/srv/docker/kener
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr

```bash
composemgr install kener
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ORG_NAME=Kener
APP_SECRET_KEY=replace-with-a-random-16-byte-hex-string
DB_USER_NAME=kener
DB_USER_PASS=replace-with-a-random-password
DB_CREATE_DATABASE_NAME=kener
```

See `docker-compose.yaml` for complete list of configurable options.

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

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64221` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The proxy variants publish no host ports. The app is reachable only through the
proxy network, and the Postgres and Redis services stay internal to the project
network in every variant.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64221

## 📂 Volumes

- `./volumes/data/kener` - Uploaded monitor images and assets
- `./volumes/data/kener/database` - Kener database directory
- `./volumes/db/postgres/kener` - Postgres database data
- `./volumes/db/redis/kener` - Redis data

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
