# Tech Stack

## Frontend (ui-next/)
- **Framework**: Next.js 16 (App Router)
- **Language**: TypeScript 5 (strict mode)
- **Styling**: Tailwind CSS v4 + shadcn/ui components (Radix UI primitives)
- **Icons**: Hugeicons (`@hugeicons/react`)
- **Animations**: Framer Motion
- **Charts**: Recharts
- **State/Data fetching**: TanStack React Query v5
- **Auth**: NextAuth v4 + `@auth/prisma-adapter`
- **ORM**: Prisma 6 with SQLite (dev) — schema at `ui-next/prisma/schema.prisma`
- **HTTP client**: Axios

## Backend Services (Docker)
| Service | Image | Port |
|---|---|---|
| Prometheus | `prom/prometheus:v2.45.0` | 9090 |
| Grafana | `grafana/grafana:10.0.0` | 3000 |
| Loki | `grafana/loki:2.8.0` | 3100 |
| Promtail | `grafana/promtail:2.8.0` | — |
| Alertmanager | `prom/alertmanager:v0.25.0` | 9093 |
| Node Exporter | `prom/node-exporter:v1.6.0` | 9100 |
| cAdvisor | `gcr.io/cadvisor/cadvisor:v0.47.0` | 8080 |
| Blackbox Exporter | `prom/blackbox-exporter:v0.24.0` | 9115 |
| DevOps Monitor UI | Custom Dockerfile | 4000→3000 |

## Testing
- **Runner**: Vitest 3
- **Config**: `ui-next/vitest.config.ts`
- **Test location**: `ui-next/tests/**/*.test.ts` (node environment, no browser)
- Tests cover API surface logic: upstream allowlist, URL resolution, secret redaction, health aggregation

## Common Commands (run from `ui-next/`)

```bash
# Install dependencies + generate Prisma client
npm install

# Local dev server (http://localhost:3000)
npm run dev

# Create/migrate local SQLite DB
npm run db:push

# Run tests (single pass)
npm test

# Type-check only
npm run type-check

# Lint
npm run lint
npm run lint:fix

# Full verification (lint + type-check + test + build)
npm run verify

# Production build
npm run build

# Prisma studio (DB browser)
npm run db:studio
```

## Docker Commands (from repo root)

```bash
# Generate .env with secrets
./scripts/setup-env.sh              # Linux/macOS
.\scripts\devops-monitor.ps1 init-env  # Windows

# Start full stack
docker compose up -d

# Check status
docker compose ps

# View logs for a service
docker compose logs -f prometheus

# Stop and clean
docker compose down
docker compose down -v  # also removes volumes
```

## Path Alias
`@/*` maps to `ui-next/*` in TypeScript and Vitest.

## Environment Variables
- Copy `ui-next/env.example` to `ui-next/.env.local` for local dev
- `NEXTAUTH_SECRET` and `ALERT_WEBHOOK_TOKEN` are **required** in production
- Backend URLs default to container-network hostnames when `DOCKER_ENV=true`
