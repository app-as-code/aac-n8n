<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/icon-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/icon-light.svg">
    <img src="assets/icon.svg" alt="n8n logo" width="128">
  </picture>
</p>

<h1 align="center">aac-n8n</h1>

<p align="center">Self-hosted deployment definition for n8n, a low-code workflow automation platform.</p>

<p align="center">
  <a href="https://github.com/n8n-io/n8n"><img alt="Upstream" src="https://img.shields.io/badge/upstream-n8n--io%2Fn8n-blue?logo=github"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-n8nio%2Fn8n%3A2.28.2-2496ED?logo=docker&logoColor=white">
  <a href="https://github.com/n8n-io/n8n/blob/master/LICENSE.md"><img alt="License" src="https://img.shields.io/badge/license-Sustainable%20Use-orange"></a>
</p>

## About

n8n is a workflow automation platform that combines a visual, node-based editor with the
option to write custom JavaScript or Python code. Workflows connect hundreds of services
and APIs, and can be triggered by schedules, webhooks or external events. This repository
describes how n8n is deployed as a service together with a Redis instance and a PostgreSQL
database.

Key features (as provided by upstream):

- Visual workflow editor with a large catalog of integration nodes
- Custom code steps in JavaScript and Python
- Webhook and schedule triggers
- Self-hosted operation with a PostgreSQL database and optional Redis queue mode
- Task runners for executing code nodes

## Upstream

- Project: <https://github.com/n8n-io/n8n>
- Website: <https://n8n.io>
- Documentation: <https://docs.n8n.io>
- Container image: `n8nio/n8n:2.28.2`
- Dependencies: `redis` 8.6.1 (deployed as `aac-n8n-redis`), `postgres` 18 (deployed as `aac-n8n-db`)
- License: Sustainable Use License (fair-code, upstream project)

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine.
For a manual run, provide the values listed under "Configuration" in an environment file
and start the stack:

```bash
# Provide the values listed under "Configuration" in .env / stack.env
docker compose up -d
```

Secrets are not stored in this repository. Provide them through the deployment pipeline
or create the environment files yourself. Do not commit them.

## Configuration

Environment variables (defaults from `service.yml`; `CHANGE_ME` must be set per deployment):

| Variable | Default | Description |
|---|---|---|
| `N8N_HOST` | `CHANGE_ME` | Public hostname of the n8n instance |
| `N8N_PORT` | `5678` | Port n8n listens on inside the container |
| `N8N_PROTOCOL` | `https` | Protocol used in public URLs |
| `WEBHOOK_URL` | `CHANGE_ME` | Public base URL used for webhook addresses |
| `WEBHOOK_TUNNEL_URL` | `CHANGE_ME` | Tunnel URL for webhooks, if a tunnel is used |
| `N8N_PROXY_HOPS` | `1` | Number of reverse proxies in front of n8n |
| `N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS` | `true` | Enforce strict permissions on the n8n settings file |
| `N8N_RUNNERS_ENABLED` | `true` | Enable task runners for code execution |
| `DB_TYPE` | `postgresdb` | Database type used by n8n |
| `DB_POSTGRESDB_HOST` | `CHANGE_ME` | Hostname of the PostgreSQL database (`aac-n8n-db`) |
| `DB_POSTGRESDB_PORT` | `5432` | PostgreSQL port |
| `DB_POSTGRESDB_DATABASE` | `n8n` | PostgreSQL database name |
| `DB_POSTGRESDB_USER` | `n8n` | PostgreSQL user |
| `DB_POSTGRESDB_PASSWORD` | `CHANGE_ME` (secret) | PostgreSQL password for n8n |
| `N8N_REDIS_HOST` | `CHANGE_ME` | Hostname of the Redis instance (`aac-n8n-redis`) |
| `N8N_REDIS_PORT` | `6379` | Redis port |
| `POSTGRES_DB` | `n8n` (secret) | Database name for the PostgreSQL dependency |
| `POSTGRES_USER` | `n8n` (secret) | Database user for the PostgreSQL dependency |
| `POSTGRES_PASSWORD` | `CHANGE_ME` (secret) | Password for the PostgreSQL dependency |
| `TZ` | `Europe/Berlin` | Time zone |
| `PUID` / `PGID` | `1000` | User and group ID the container runs as |

Ports:

| Port | Protocol | Purpose |
|---|---|---|
| `5678` | TCP | Web interface and webhooks (`web`) |

Volumes:

| Mount | Description |
|---|---|
| `config` -> `/home/node/.n8n` | n8n configuration, database and workflows |
| `netl_feser` -> `/mnt/n8n` | Bind mount from the host for n8n file access |
| `redis` -> `/data` | Persistent storage for Redis |
| `db_data` -> `/var/lib/postgresql` | Persistent storage for PostgreSQL |

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, environment,
   volumes, dependencies and per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` (or a configured stage branch) runs the service pipeline, which renders
   the manifests with `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-n8n>.

## Credits and license

n8n is developed by n8n GmbH and licensed under the Sustainable Use License (with separate
Enterprise License terms for enterprise features). This repository only contains the
deployment definition for it and is not a fork of the application. The logo is provided
by the n8n project; see [`assets/ICON_SOURCE.md`](assets/ICON_SOURCE.md) for icon sources
and licenses.
