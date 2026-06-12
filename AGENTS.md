# AGENTS.md

Multi-arch Ghost blog image: pinned `FROM` over official `ghost`, deployed as the `ghost` swarm stack (two sites behind traefik).

## Commands

```bash
make build                                  # build jahrik/arm-ghost:latest
docker run -d -p 2368:2368 -e url=http://localhost:2368 jahrik/arm-ghost:latest
make deploy                                 # swarm stack deploy (stack: ghost)
```

## CI

`build.yml`: Test (build + HTTP poll on :2368) on PR; Release (buildx amd64/arm64/armv7 push to Docker Hub) on merge to main. Needs `DOCKERHUB_USERNAME`/`DOCKERHUB_TOKEN` secrets.

## Quirks

- Bump Ghost via the `FROM` tag.
- Compose expects external `traefik` overlay network and `/mnt/g1/ghost` gluster paths — keep that wiring. Traefik labels are 1.x syntax, updated when the traefik stack is.
- CI smoke test runs SQLite; the stack uses MySQL via `DB_*` env vars.
