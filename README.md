# go-web-infrastructure

Shared Docker-oriented infrastructure for Go Web stacks: nginx reverse proxy configs and an observability bundle (Grafana, Loki, Alloy for Docker logs).

## Nginx proxy (`proxy/`)

- `nginx.conf` — sample TLS + upstream routing (`server_name` and upstream hostnames are placeholders).
- `nginx-dev.conf` — local `/api` and `/swagger` to the gateway service name `go-gateway-template`.
- `Dockerfile` — generates self-signed material under `/etc/nginx/certs` so the container starts with no volumes.

Standalone compose:

```bash
docker compose -f docker-compose.proxy.yml up -d --build
```

Requires external network `go-network` (same convention as gateway compose files).

## Observability (`observability/`)

Grafana + Loki + Alloy scraping Docker logs from containers on `go-network`. Copied from `lmk-service-website`; tailor retention, ports, and `GF_*` as needed.

```bash
cp .env.sample .env
docker network create go-network  # if it does not exist yet
docker compose --env-file ./.env -f observability/docker-compose.yml up -d
```

Grafana defaults to `http://localhost:${GRAFANA_PORT:-4001}`. To expose Grafana via this proxy, align `server_name` and upstreams in `proxy/nginx.conf`.
