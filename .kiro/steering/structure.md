# Project Structure

## Root Layout
```
/
├── ui-next/              # Next.js web application (main codebase)
├── prometheus/           # Prometheus config, alert rules, recording rules, probe targets
├── grafana/              # Dashboard and datasource provisioning
├── loki/                 # Loki + Promtail config
├── alertmanager/         # Alertmanager routing config
├── exporters/blackbox/   # Blackbox Exporter probe modules
├── scripts/              # Management scripts (bash + PowerShell)
├── docs/                 # Markdown documentation
├── docker-compose.yml    # Production stack
├── docker-compose.dev.yml
├── docker-compose.logging.yml
├── config.json           # Monitoring config
├── site-config.json      # Branding / SEO config
└── devops-monitor.sh     # Linux/macOS management wrapper
```

## ui-next/ Structure
```
ui-next/
├── app/                  # Next.js App Router pages and API routes
│   ├── api/              # API routes
│   │   ├── auth/         # NextAuth handlers + registration
│   │   ├── health/       # Stack health endpoint
│   │   ├── notifications/# Notification send/test/webhook routes
│   │   ├── proxy/[service]/[...path]/  # Reverse proxy to backends
│   │   └── ssl/          # SSL certificate inspection
│   ├── dashboard/        # Main dashboard page
│   ├── alerts/           # Alert management
│   ├── logs/             # Loki log viewer
│   ├── metrics/          # Prometheus metrics explorer
│   ├── services/         # Service health + SLA + SSL tracker
│   ├── settings/         # Notification and system settings
│   ├── admin/            # Admin panel
│   ├── auth/             # Sign in / sign up / error pages
│   └── plugins/          # Plugin management UI
├── components/           # Shared React components
│   ├── ui/               # shadcn/ui primitives (button, card, input, etc.)
│   └── _custom/          # Custom toast system
├── lib/                  # Core logic and utilities
│   ├── config/           # Config manager, parser, multi-tenant, types
│   ├── hooks/            # React hooks (polling, metrics, alerts, logs, etc.)
│   ├── plugins/          # Plugin system (registry, manager, AWS/GitHub/Jenkins)
│   └── server/           # Server-only modules (API auth, upstream allowlist, health, redaction)
├── hooks/                # Top-level app hooks
├── prisma/               # Prisma schema, migrations, seed
├── tests/                # Vitest unit tests
└── public/               # Static assets
```

## Key Architectural Patterns

### API Proxy
All backend traffic flows through `/api/proxy/[service]/[...path]`. Every request requires an authenticated session. Allowed paths are defined in `ui-next/lib/server/upstream.ts` — edit this file to expose new backend endpoints.

### Server vs Client Code
- `ui-next/lib/server/` — server-only modules (never imported in client components)
- `ui-next/lib/hooks/` — client-side React hooks for data fetching
- `ui-next/app/api/` — Next.js Route Handlers (server-side)

### Config System
- `ui-next/lib/config/manager.ts` — runtime config loading
- `ui-next/lib/config/parser.ts` — config parsing (has a pretty-printer for round-trip testing)
- `ui-next/lib/config/multi-tenant-manager.ts` — per-tenant config isolation
- `ui-next/lib/config/types.ts` — shared TypeScript types

### Plugin System
Plugins live in `ui-next/lib/plugins/`. Each plugin implements the interface in `types.ts` and registers via `registry.ts`. The manager in `manager.ts` handles lifecycle.

### Notifications
Secret masking is enforced server-side in `ui-next/lib/server/notifications-redact.ts`. Notification channels are never returned with plain secrets to the client.

### Prometheus Probe Targets
`prometheus/targets/*.yml` files are hot-reloaded every 30 seconds — add new uptime probe targets here without restarting the stack.

## Test Location
All tests are in `ui-next/tests/` with `.test.ts` suffix. They test server-side logic only (no browser/DOM). Co-locating tests next to source files with `.test.ts` is acceptable for new tests.
