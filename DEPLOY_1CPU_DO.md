# RLCC Assistant: 1 vCPU DigitalOcean Deployment

This repo now includes production-tuned files for a 1 vCPU droplet.

## What to run

1. Set required secrets in `rlcc-assistant-api-service/.env`:
- `JWT_SECRET` (required)
- DB credentials
- any mail/PCO secrets you use
- `DB_HOST` should point to your managed DB or database container hostname

2. Add Cloudflare Origin Certificate files:
- Place cert at `nginx/ssl/cloudflare-origin.crt`
- Place private key at `nginx/ssl/cloudflare-origin.key`
- Ensure file permissions are readable by Docker (recommended `600` for key, `644` for cert)

3. Start production stack:

```bash
docker compose -f docker-compose.prod.yml up -d --build
```

4. Check service health/logs:

```bash
docker compose -f docker-compose.prod.yml ps
docker compose -f docker-compose.prod.yml logs -f --tail=100
```

## Tunings applied for 1 vCPU

- API runs in production mode (`node`, no nodemon)
- Angular client prebuilt and served by nginx (no dev server)
- Conservative CPU/memory limits per service
- Reduced API cache TTL and request body limit defaults
- Lower global rate-limit defaults for burst control
- Redis configured in memory-only mode with LRU maxmemory policy
- Log file rotation on all containers
- nginx gzip enabled for smaller payloads

## Files added/updated

- `docker-compose.prod.yml`
- `nginx/conf.d/prod.conf`
- `nginx/conf.d/dev.conf`
- `rlcc-assistant-api-service/Dockerfile.prod`
- `rlcc-assistant-client/Dockerfile.prod`
- `rlcc-assistant-client/nginx.prod.conf`
- `rlcc-assistant-client/.dockerignore`
- `rlcc-assistant-api-service/package.json` (`start`, `start:boot`)

## Recommended droplet baseline

- 1 vCPU / 1 GB RAM minimum
- Ubuntu 22.04+
- Swap enabled (1-2 GB)
- Docker + Docker Compose plugin
