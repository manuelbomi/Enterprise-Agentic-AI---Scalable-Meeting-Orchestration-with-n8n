# Enterprise Agentic AI: Scalable Meeting Orchestration with n8n 

#### An Agentic AI–powered workflow built with n8n, designed to automate meeting scheduling across an enterprise using OpenAI + Google Calendar.

#### This workflow demonstrates how Agentic AI can seamlessly integrate with enterprise tools to provide scalable, intelligent, and context-aware scheduling assistance. It can be adapted for broader enterprise use cases beyond just calendar automation.

#### Here is a figure of the Agent:

<img width="995" height="360" alt="Image" src="https://github.com/user-attachments/assets/148ba70b-0b80-462f-b414-55142ae3e711" />


---

#### For AI Architects that are interest in step-by-step detail of how the Agent is created in n8n, it is given here:

#### Instructions regarding how the Agent can be cloned and deployed using its json file (available here: https://github.com/manuelbomi/Enterprise-Agentic-AI---Scalable-Meeting-Orchestration-with-n8n/blob/main/workflows/agentic_ai_enterprise_scheduling.json) is given under the deployment section in the Read Me. 

#### Further details regarding how the Agent's deployment can be scaled for other applications in an enterprise setting are also given in later segments of the Read Me. They could also be obtained on files included in the repository. 



---

## Introduction 📖

#### In enterprise environments, scheduling meetings across teams can be repetitive and time-consuming. This repository provides a ready-to-use Agentic AI workflow in n8n that:

* Acts as an AI-powered scheduling assistant

* Reads availability from Google Calendar

* Confirms timeslots interactively with users

* Creates events automatically once finalized

* It showcases how Agentic AI principles can be applied to real-world enterprise workflows.

---

## Workflow Overview 🛠️ 

#### The workflow is composed of the following components:

* Chat Input – Entry point for the user to interact with the assistant.

* AI Agent – Orchestrates the conversation, manages tools, and ensures correct action flow.

* OpenAI Chat Model – Provides reasoning and natural language capabilities (powered by gpt-4o-mini).

* Simple Memory – Stores context across interactions.

* Google Calendar Tools

* Get Availability: Searches for free time slots.

* Create Event: Books the meeting once a slot is confirmed.

* This is the n8n Agentic AI workflow that powers the scheduling assistant:

<img width="1100" height="344" alt="Image" src="https://github.com/user-attachments/assets/1361cf67-feed-43fe-bcc3-5a89505dec0c" />
  
---

#### Below is a step-by-step interaction diagram showing how the AI Agent interact with the uset to help schedule a meeting:
  * participant U as User
    
  * participant A as AI Agent
    
  * participant G as Google Calendar



  sequencediagram

    U->>A: "Can you schedule a 30-min meeting with John tomorrow?"
    A->>G: Check availability for tomorrow
    G-->>A: Free slots: 2:00 PM, 3:30 PM, 4:15 PM
    A-->>U: "You are free at 2:00, 3:30, and 4:15. Which works best?"
    U->>A: "Let’s go with 3:30 PM."
    A->>U: "Should I add a description?"
    U->>A: "Yes, 'Project Roadmap Discussion'."
    A->>G: Create event at 3:30 PM with description
    G-->>A: Event created
    A-->>U: "Meeting scheduled with John at 3:30 PM tomorrow."

---

#### Below are some figures of how the Agent interact with users via the n8n UI
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/0d2b2e3e-8580-47cf-9db2-954faa2bba46" />

#### Result of the Agent's effort on Google Calendar

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/792d2cf7-2235-4c8b-b8f6-1c91f80403f7" />


---

## Workflow Components Details 🧩 :
#### 1. Chat Input

  ##### &emsp; Node Type: chatTrigger

  ##### &emsp; Purpose: Entry point for user interaction.

  ##### &emsp; Notes:

  ##### &emsp; Captures user queries (e.g., “Schedule a meeting with John tomorrow”).

  ##### &emsp; Provides a conversational interface for the assistant.

#### 2. AI Agent

  ##### &emsp; Node Type: langchain.agent

  ##### &emsp; Purpose: Core reasoning engine of the workflow.

  ##### &emsp; Features:

  ##### &emsp; Takes user input.

  ##### &emsp; Uses tools (Google Calendar actions).

  ##### &emsp; Delegates tasks like checking availability or creating events.

  ##### &emsp; Configured With:

  ##### &emsp; System Message → Defines assistant role as a “calendar assistant.”

  ##### &emsp; Memory & LLM → Keeps track of context across the conversation.

#### 3. OpenAI Chat Model

  ##### &emsp; Node Type: lmChatOpenAi

  ##### &emsp; Model Used: gpt-4o-mini

  ##### &emsp; Purpose: Provides natural language reasoning and response generation.

  #### &emsp;  Why GPT-4o-mini?

  ##### &emsp; &emsp; Optimized for speed and cost.

  ##### &emsp; &emsp; Provides sufficient intelligence for scheduling tasks.
  

#### 4. Simple Memory

  ##### &emsp; Node Type: memoryBufferWindow

  ##### &emsp; Purpose: Stores the history of recent messages.

  ##### &emsp; * Benefit:

  ##### &emsp; &ensp; Helps the AI maintain context (e.g., remembering the meeting details during confirmation).


####  5. Google Calendar Tools
     
##### &emsp;  a) Get Availability

##### &emsp; &emsp;  Node Type: googleCalendarTool

##### &emsp; &emsp;  Purpose: Checks the user’s Google Calendar for free time slots.

##### &emsp; &emsp;  Inputs:

##### &emsp; &emsp;  start_time and end_time from AI reasoning.

##### &emsp; &emsp; Outputs:

##### &emsp; &emsp; List of free blocks that the AI can propose to the user.

##### &emsp; b) Create Event

##### &emsp; &emsp; Node Type: googleCalendarTool

##### &emsp; &emsp; Purpose: Books the confirmed meeting.

##### &emsp; &emsp; Inputs:

##### &emsp; &emsp; start_date, end_date, summary, and description.

##### &emsp; &emsp; Outputs:

##### &emsp; &emsp; Confirmation that an event has been created.


#### Key Highlights of the Design 💡  :

##### Agentic AI Pattern: Combines reasoning (LLM), context (Memory), and action execution (Calendar Tools).

##### Enterprise-Ready: Can scale to multiple calendars, integrate with SSO, or extend to HR/CRM use cases.

##### Reusable Design: The same structure can be repurposed for other enterprise workflows (ticketing, approvals, task automation).

---

 
## How to Deploy the Current Workflow 🚀
* 1. Clone the Repository
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

* 2. Import Workflow into n8n

Open your n8n instance
.

Go to Workflows → Import from File.

Upload Agentic_AI_workflow.json from this repo.

* 3. Set Up Credentials

Configure your OpenAI API key under Credentials → OpenAI.

Configure your Google Calendar OAuth2 credentials under Credentials → Google Calendar.

* 4. Run the Workflow

Activate the workflow.

Start interacting with the chat input node.

The AI assistant will:

Fetch availability

Suggest slots

Book events automatically

---


## Production Grade Deployment Guide 🚀 

This segment explains how to deploy the Enterprise Agentic AI Scheduler workflow in a production-grade environments including :

##### &emsp; Local / small infra — Docker Compose

##### &emsp; Managed — n8n Cloud or single-instance UI import

##### &emsp; Production-grade — Kubernetes with Helm (recommended for scale)

🔁 Before you deploy

##### &emsp; Put the workflow JSON in your repo under workflow/Agentic_AI_workflow.json.

##### &emsp; Add any visual assets under images/.

##### &emsp; Keep secrets out of your repo — use environment variables or secret stores.

---


####  ⚙️ 1) Quick (UI) deploy — easiest for demos & small teams

##### &emsp; Steps

##### &emsp; Start an n8n instance (see Docker Compose below or n8n Cloud).

##### &emsp; Open n8n in the browser and sign in as an admin.

##### &emsp; From the Editor, open the top-right menu (three dots) → Import from File → choose workflow/Agentic_AI_workflow.json.

##### &emsp; Configure credentials in n8n (OpenAI, Google Calendar OAuth2) under Credentials.

##### &emsp; Activate the workflow.

Note that the n8n Editor supports straightforward export/import via the top-right menu (Import/Export). 
n8n Docs

---

#### 🐳 2) Docker Compose — recommended for single-server self-hosting

##### &emsp; Use Docker Compose for a simple repeatable deployment. For production, use a managed Postgres DB and consider Redis for queue mode.
---
```ruby

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

```
---

#### Notes & recommendations

* Use managed Postgres (Cloud SQL / AWS RDS / Azure DB) for production.

* Run n8n in queue mode (with Redis) for scalable execution across worker nodes.

* Use a reverse proxy (nginx / Traefik) with TLS for secure access.

* Persist ./n8n and DB volumes.

* See n8n’s Docker Compose / server setup docs for more details. 

---

####  ☁️ 3) n8n Cloud / Managed Hosting

##### &emsp; If you prefer no-ops hosting, use n8n Cloud (Enterprise) — it offers managed scaling, backups, and enterprise SLAs. 

##### &emsp; This is the fastest path for enterprises that want a managed experience (SSO, team management, etc.). Contact n8n sales for enterprise features.

---

#### ☸️ 4) Kubernetes — production-grade, highly available

* For large enterprises, use Kubernetes + Helm chart to support high availability (HA), autoscaling, GitOps and/or MLOps.

##### &emsp; Options

##### &emsp; &emsp; Use the official/community Helm chart (artifact/packaged charts exist). 
##### &emsp; &emsp; Artifact Hub
##### &emsp; &emsp; +1

* Use external managed DB (Postgres), external Redis (cloud provider), and object storage (S3/MinIO) for binary data.

##### &emsp; High-level steps

* Create namespace and configure persistent volumes or S3 for storage.

* Install Postgres (managed) and Redis (managed) or provide access to existing instances.

* Install n8n Helm chart with values:

* db.type=postgres

* externalDb.host=..., externalDb.user=..., externalDb.password=...

* redis.enabled=false (if using managed Redis) and configure queue.mode=true

* Configure ingress for TLS and hostnames.

* HA configuration

* Run n8n in queue mode with multiple worker pods.

* Run a small number of web pods behind a load balancer for editor/API traffic.

* Use readiness/liveness probes and configure resource requests/limits.

* Integrate Prometheus/Grafana for metrics & alerting.

##### &emsp; &emsp; Community Helm charts and guides provide detailed values.yaml samples and best practices. 
##### &emsp; &emsp; Artifact Hub
##### &emsp; &emsp; +1

---

#### 🔐 Credentials & Secrets

* NEVER store API keys or OAuth tokens in the repo. Use environment variables, Kubernetes Secrets, or a vault (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault).

* For Google Calendar: set up OAuth2 client credentials in Google Cloud Console and add them as n8n credentials (via UI or secrets).

* For OpenAI: store OPENAI_API_KEY in secrets; configure the n8n OpenAI credential to read it.

---

#### 📥 Importing the workflow (recommended ways)

* A) Editor UI (recommended for most users)

##### &emsp;  Editor → Top-right three dots → Import from File → select workflow/Agentic_AI_workflow.json. This is the simplest and most reliable method. 
##### &emsp; n8n Docs

* B) CLI import (automation / CI)

##### &emsp; You can import workflows programmatically using n8n CLI tools on the same host as n8n (useful for CI pipelines / provisioning):

##### &emsp;  Example (run where n8n binary/cli is available)

##### &emsp;  n8n import:workflow --input=workflow/Agentic_AI_workflow.json


##### &emsp;  Use the CLI when provisioning many workflows or seeding an instance. The CLI supports importing credentials as well; be careful with IDs — exported JSON contains IDs that may need to be changed to avoid overwriting existing items. 

##### &emsp;  n8n Docs
##### &emsp;  +1


* C) API import (programmatic, advanced)

##### &emsp;  n8n exposes REST endpoints allowing creation of workflows via API — but note some exported JSON fields (IDs, state, credentials) may need removal or normalization before an API import to avoid errors. Community dicsussions show you sometimes must strip fields that conflict with the target instance.    Test on staging first. 

##### &emsp;  n8n Community
##### &emsp;  +1

---

#### 🧪 CI/CD & Automated Provisioning

* Keep workflow/ JSON in Git.

* Use a CI job to run n8n import:workflow (or call API) into staging after changes are merged.

* For secrets, use your CI secrets store and avoid writing keys to disk.

* Add smoke tests that run a simple test message through the chat trigger (or run a synthetic job) to validate the workflow imported correctly.

---

#### 📦 Backups & Recovery

* Backup Postgres DB regularly (daily, with WAL streaming for point-in-time recovery).

* Backup ~/.n8n or your binary storage (if using local file storage).

* Periodically export workflows and credentials using CLI for an extra copy.

---

#### 📈 Monitoring & Observability

* Capture metrics (Prometheus + Grafana) for:

* Workflow executions (success/failure rates)

* Queue lengths, worker lag (Redis metrics)

* HTTP error rates & latency for webhook nodes

* Centralized logging (ELK / Loki) for troubleshooting.

* Configure alerts for failed executions or large queue backlogs.

* (see example in: )

---
  

#### ✅ Enterprise checklist (pre-launch)

* Use managed Postgres or HA Postgres cluster.

* Enable queue mode with Redis for worker scaling.

* Configure TLS via ingress / reverse proxy.

* Put n8n behind SSO (Okta/AzureAD) or enable basic auth for the Editor.

* Store credentials in secure secret store.

* Set resource limits and HPA for K8s deployments.

* Configure backup and restore procedures.

* Test importing workflow/Agentic_AI_workflow.json in staging.

* Run security review on OAuth scopes for Google Calendar credentials.

---

#### ✅ Useful links & references

* n8n — Export & import workflows (Editor UI). 
* n8n Docs

* n8n — Docker Compose / Server setups. 
* n8n Docs
* +1

* n8n — CLI commands & import tips. 
* n8n Docs

* Community notes about API import caveats and tips. 
* n8n Community
* +1

* Helm / Kubernetes charts for n8n (community/official charts). 
* Artifact Hub
* +1


---



## Scalability for Enterprise Applications ⚙️ 

#### While this workflow focuses on meeting scheduling, the same architecture can be extended to other enterprise use cases.

#### This is the current workflow

<img width="1713" height="133" alt="Image" src="https://github.com/user-attachments/assets/d02b28b6-c452-466e-8a5a-813f7b95a4b3" />

---


#### This is an example of how the workflow may be scaled to other enterprise use cases

<img width="1858" height="283" alt="Image" src="https://github.com/user-attachments/assets/f3d1bccd-2a3f-47a7-80fb-6631206572d1" />

---

#### Examples of how the workflow may be scaled to other use cases include:

##### &emsp; Human Resources: Automate interview scheduling & reminders

##### &emsp; Project Management: Book sprint reviews, stand-ups, and cross-team syncs

##### &emsp; Customer Support: Schedule client calls, follow-ups, and escalations

##### &emsp; Sales & Marketing: Auto-schedule demos, campaigns, and recurring check-ins


#### Scalability Features:

##### &emsp; Plug-and-play integration with multiple calendars or services (Google Workspace, Microsoft Outlook, etc.)

##### &emsp; Extendable with enterprise authentication (SSO, OAuth2)

##### &emsp; Reusable Agentic AI pattern: chat input → memory → reasoning → tool execution





