🚀 Deployment Guide

This document explains how to deploy the Enterprise Agentic AI Scheduler workflow in production-grade environments:

Local / small infra — Docker Compose

Managed — n8n Cloud or single-instance UI import

Production-grade — Kubernetes with Helm (recommended for scale)

🔁 Before you deploy

Put the workflow JSON in the repo under workflow/Agentic_AI_workflow.json.

Add any visual assets under images/.

Keep secrets out of your repo — use environment variables or secret stores.

⚙️ 1) Quick (UI) deploy — easiest for demos & small teams

Steps

Start an n8n instance (see Docker Compose below or n8n Cloud).

Open n8n in the browser and sign in as an admin.

From the Editor, open the top-right menu (three dots) → Import from File → choose workflow/Agentic_AI_workflow.json.

Configure credentials in n8n (OpenAI, Google Calendar OAuth2) under Credentials.

Activate the workflow.

The n8n Editor supports straightforward export/import via the top-right menu (Import/Export). 
n8n Docs

🐳 2) Docker Compose — recommended for single-server self-hosting

Use Docker Compose for a simple repeatable deployment. For production, use a managed Postgres DB and consider Redis for queue mode.

Minimal docker-compose.yml (example)
version: "3.8"
services:
  postgres:
    image: postgres:15
    restart: unless-stopped
    environment:
      POSTGRES_USER: n8n
      POSTGRES_PASSWORD: n8n_pass
      POSTGRES_DB: n8n
    volumes:
      - ./pgdata:/var/lib/postgresql/data

  redis:
    image: redis:7
    restart: unless-stopped

  n8n:
    image: n8nio/n8n:latest
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      # Basic required envs - set to secure vals in production
      DB_TYPE: postgresdb
      DB_POSTGRESDB_HOST: postgres
      DB_POSTGRESDB_PORT: 5432
      DB_POSTGRESDB_DATABASE: n8n
      DB_POSTGRESDB_USER: n8n
      DB_POSTGRESDB_PASSWORD: n8n_pass

      # Use redis for queue mode
      QUEUE_MODE: "true"
      EXECUTIONS_PROCESS: "main"
      REDIS_HOST: redis
      REDIS_PORT: 6379

      # Admin user (or use OAuth/SSO)
      N8N_BASIC_AUTH_ACTIVE: "true"
      N8N_BASIC_AUTH_USER: "admin"
      N8N_BASIC_AUTH_PASSWORD: "${N8N_BASIC_AUTH_PASSWORD}" # set in .env
    volumes:
      - ./n8n:/home/node/.n8n
    depends_on:
      - postgres
      - redis


Notes & recommendations

Use managed Postgres (Cloud SQL / AWS RDS / Azure DB) for production.

Run n8n in queue mode (with Redis) for scalable execution across worker nodes.

Use a reverse proxy (nginx / Traefik) with TLS for secure access.

Persist ./n8n and DB volumes.

See n8n’s Docker Compose / server setup docs for more details. 
n8n Docs
+1

☁️ 3) n8n Cloud / Managed Hosting

If you prefer no-ops hosting, use n8n Cloud (Enterprise) — it offers managed scaling, backups, and enterprise SLAs. This is the fastest path for enterprises that want a managed experience (SSO, team management, etc.). Contact n8n sales for enterprise features.

☸️ 4) Kubernetes — production-grade, highly available

For large enterprises, use Kubernetes + Helm chart to support HA, autoscaling, and GitOps.

Options

Use the official/community Helm chart (artifact/packaged charts exist). 
Artifact Hub
+1

Use external managed DB (Postgres), external Redis (cloud provider), and object storage (S3/MinIO) for binary data.

High-level steps

Create namespace and configure persistent volumes or S3 for storage.

Install Postgres (managed) and Redis (managed) or provide access to existing instances.

Install n8n Helm chart with values:

db.type=postgres

externalDb.host=..., externalDb.user=..., externalDb.password=...

redis.enabled=false (if using managed Redis) and configure queue.mode=true

Configure ingress for TLS and hostnames.

HA configuration

Run n8n in queue mode with multiple worker pods.

Run a small number of web pods behind a load balancer for editor/API traffic.

Use readiness/liveness probes and configure resource requests/limits.

Integrate Prometheus/Grafana for metrics & alerting.

Community Helm charts and guides provide detailed values.yaml samples and best practices. 
Artifact Hub
+1

🔐 Credentials & Secrets

NEVER store API keys or OAuth tokens in the repo. Use environment variables, Kubernetes Secrets, or a vault (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault).

For Google Calendar: set up OAuth2 client credentials in Google Cloud Console and add them as n8n credentials (via UI or secrets).

For OpenAI: store OPENAI_API_KEY in secrets; configure the n8n OpenAI credential to read it.

📥 Importing the workflow (recommended ways)
A) Editor UI (recommended for most users)

Editor → Top-right three dots → Import from File → select workflow/Agentic_AI_workflow.json. This is the simplest and most reliable method. 
n8n Docs

B) CLI import (automation / CI)

You can import workflows programmatically using n8n CLI tools on the same host as n8n (useful for CI pipelines / provisioning):

# Example (run where n8n binary/cli is available)
n8n import:workflow --input=workflow/Agentic_AI_workflow.json


Use the CLI when provisioning many workflows or seeding an instance. The CLI supports importing credentials as well; be careful with IDs — exported JSON contains IDs that may need to be changed to avoid overwriting existing items. 
n8n Docs
+1

C) API import (programmatic, advanced)

n8n exposes REST endpoints allowing creation of workflows via API — but note some exported JSON fields (IDs, state, credentials) may need removal or normalization before an API import to avoid errors. Community discussions show you sometimes must strip fields that conflict with the target instance. Test on staging first. 
n8n Community
+1

🧪 CI/CD & Automated Provisioning

Keep workflow/ JSON in Git.

Use a CI job to run n8n import:workflow (or call API) into staging after changes are merged.

For secrets, use your CI secrets store and avoid writing keys to disk.

Add smoke tests that run a simple test message through the chat trigger (or run a synthetic job) to validate the workflow imported correctly.

📦 Backups & Recovery

Backup Postgres DB regularly (daily, with WAL streaming for point-in-time recovery).

Backup ~/.n8n or your binary storage (if using local file storage).

Periodically export workflows and credentials using CLI for an extra copy.

📈 Monitoring & Observability

Capture metrics (Prometheus + Grafana) for:

Workflow executions (success/failure rates)

Queue lengths, worker lag (Redis metrics)

HTTP error rates & latency for webhook nodes

Centralized logging (ELK / Loki) for troubleshooting.

Configure alerts for failed executions or large queue backlogs.

✅ Enterprise checklist (pre-launch)

 Use managed Postgres or HA Postgres cluster.

 Enable queue mode with Redis for worker scaling.

 Configure TLS via ingress / reverse proxy.

 Put n8n behind SSO (Okta/AzureAD) or enable basic auth for the Editor.

 Store credentials in secure secret store.

 Set resource limits and HPA for K8s deployments.

 Configure backup and restore procedures.

 Test importing workflow/Agentic_AI_workflow.json in staging.

 Run security review on OAuth scopes for Google Calendar credentials.

✅ Useful links & references

n8n — Export & import workflows (Editor UI). 
n8n Docs

n8n — Docker Compose / Server setups. 
n8n Docs
+1

n8n — CLI commands & import tips. 
n8n Docs

Community notes about API import caveats and tips. 
n8n Community
+1

Helm / Kubernetes charts for n8n (community/official charts). 
Artifact Hub
+1