# Product: DevOps Monitoring in a Box

A production-ready, self-hosted observability stack that bundles Prometheus, Grafana, Loki, Alertmanager, Node Exporter, cAdvisor, and Blackbox Exporter behind a unified Next.js web UI. The goal is a single `docker compose up -d` that gives any DevOps engineer or SRE a fully functional monitoring platform.

## Core Value
- One command to start the entire stack
- A modern, responsive web dashboard (port 4000) that proxies all backends — the browser never calls Prometheus/Loki/Alertmanager directly
- Real data everywhere: no mock numbers, honest empty/error states when backends are unavailable

## Key Features
- SSL/TLS certificate expiry tracking
- SLI/SLA uptime windows (24h / 7d / 30d) from Blackbox probe data
- Alert silence management (create, list, expire)
- Live log tail with URL-synced filters
- Multi-channel notifications (Slack, Teams, Discord, Email, Webhooks)
- Multi-tenant auth with role-based access (Admin, Editor, Viewer)
- Plugin system for external integrations (AWS CloudWatch, GitHub Actions, Jenkins)
- Dashboard template marketplace

## Target Users
DevOps engineers, SREs, homelab enthusiasts, and teams learning observability tooling.

## Repository
https://github.com/Ayush-Singh986/Devops-Monitoring-Box
