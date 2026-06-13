# arm-ghost

[![Build](https://github.com/jahrik/arm-ghost/actions/workflows/build.yml/badge.svg)](https://github.com/jahrik/arm-ghost/actions/workflows/build.yml)

Multi-arch [Ghost](https://ghost.org/) blog image for a Pi swarm cluster. Originally per-arch builds for 2018 ARM nodes; now a pinned layer over the official `ghost` image.

## Run

```bash
docker run -d -p 2368:2368 -e url=http://localhost:2368 jahrik/arm-ghost:latest
```

## Deploy (swarm)

```bash
docker network create -d overlay traefik   # once
make deploy                                # stack: ghost (two sites behind traefik)
```

DB and mail settings come from `DB_HOST`/`DB_USER`/`DB_PASS`/`DATABASE` and `MAIL_*` env vars.

## Build

```bash
make build
make push
```

CI: PR builds + HTTP smoke test; merge to main pushes multi-arch (amd64/arm64/armv7) to Docker Hub.
