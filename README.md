# RLCC Assistant Workspace

Monorepo for RLCC Assistant services:
- `rlcc-assistant-api-service` (Node.js + Express + Sequelize)
- `rlcc-assistant-client` (Angular)

## 1. Clone With Submodules

```bash
git clone <repo-url>
cd rlcc-assistant-workspace
git submodule update --init --recursive
```

If the repo is already cloned:

```bash
git submodule sync --recursive
git submodule update --init --recursive
```

## 2. Run With Docker Compose

From the workspace root:

```bash
docker compose up --build
```

Main endpoints:
- Client (via Nginx): `http://localhost:80`
- Angular dev server container: `http://localhost:4200`
- API: `http://localhost:3000`

## 3. API Startup Behavior (Important)

On container start, API now runs:
1. `db:create` (safe if DB already exists)
2. `db:migrate`
3. API dev server

This is wired through:
- `docker-compose.yml` -> API command `npm run dev:boot`
- API script `db:init` in `rlcc-assistant-api-service/package.json`
- bootstrap script `rlcc-assistant-api-service/src/scripts/dbInit.js`

## 4. Manual Service Development

### API
```bash
cd rlcc-assistant-api-service
npm install
npm run db:init
npm run dev
```

### Client
```bash
cd rlcc-assistant-client
npm install
npm start
```

## 5. Notes

- API and Client are submodules; commit and push inside each submodule first.
- After submodule commits, commit/push updated pointers in this workspace repo.
- See API details and endpoints in:
  - `rlcc-assistant-api-service/README.md`
