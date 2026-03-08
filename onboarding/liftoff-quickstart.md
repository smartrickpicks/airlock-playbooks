# Liftoff Quickstart Guide

**Purpose**: Get your Airlock running and ask Otto your first question in under 30 minutes.
**Audience**: Solo founder or first-time Controller starting a fresh Airlock instance.
**Prerequisites**: Node.js >= 20, pnpm 9.x, Python 3.11+, Docker Desktop installed and running.

---

## What You'll Have at the End

A running Airlock on localhost with: a workspace you own, modules active, your first Vault created, and Otto answering questions about it. Everything local. Everything yours.

---

## Step 1: Clone and Install

```bash
git clone https://github.com/smartrickpicks/airlock-app.git
cd airlock-app
pnpm install
cd apps/api && pip install -e ".[dev]"
cd ../..
```

This installs the full monorepo: Next.js frontend, FastAPI backend, shared types, and all dependencies.

---

## Step 2: Start Infrastructure

```bash
docker compose up -d postgres redis
```

This spins up PostgreSQL 16 and Redis 7 in Docker containers. Wait a few seconds for them to be healthy before proceeding.

Verify they're running:
```bash
docker compose ps
```

Both `postgres` and `redis` should show `Up` status.

---

## Step 3: Configure Environment

Copy the example env and fill in your values:

```bash
cp .env.example .env
```

Open `.env` and set these required values:

```env
# Database (Docker defaults work out of the box)
DATABASE_URL=postgresql://airlock:airlock@localhost:5432/airlock
REDIS_URL=redis://localhost:6379

# Auth (required — create a Google OAuth app at console.cloud.google.com)
JWT_SECRET=<generate-a-strong-random-string>
GOOGLE_CLIENT_ID=<your-google-oauth-client-id>
GOOGLE_CLIENT_SECRET=<your-google-oauth-client-secret>

# AI (required for Otto — get a key at openrouter.ai)
OPENROUTER_API_KEY=<your-openrouter-api-key>
```

**Generate a JWT secret** (run this in your terminal):
```bash
openssl rand -hex 32
```

**Google OAuth setup**: Go to [console.cloud.google.com](https://console.cloud.google.com), create an OAuth 2.0 Client ID. Set the authorized redirect URI to `http://localhost:3000/api/auth/callback/google`. Copy the client ID and secret into your `.env`.

**OpenRouter setup**: Go to [openrouter.ai](https://openrouter.ai), create an account, generate an API key. This gives Otto access to Claude Sonnet 4, GPT-4o, and other models through a single endpoint. Free tier is available for testing.

---

## Step 4: Run Database Migrations

```bash
cd apps/api
alembic upgrade head
cd ../..
```

This creates all the tables Airlock needs: workspaces, vaults, channels, roles, permissions, otto sessions, audit events, and more.

---

## Step 5: Start the Application

Open two terminal windows:

**Terminal 1 — API server:**
```bash
cd apps/api
uvicorn src.main:app --reload
```
API is now running at `http://localhost:8000`.

**Terminal 2 — Frontend:**
```bash
cd apps/web
pnpm dev
```
Frontend is now running at `http://localhost:3000`.

---

## Step 6: Sign In and Create Your Workspace

1. Open `http://localhost:3000` in your browser
2. Click **Sign in with Google**
3. Authorize with your Google account
4. You'll be prompted to create a workspace:
   - **Workspace name**: Your company or project name (e.g., "Airlock HQ")
   - You're automatically assigned the **Executive** org role — full access to everything

You're now inside Dispatch — the global homepage of your Airlock.

---

## Step 7: Explore the Layout

Airlock uses the **Triptych** layout — three panels that give you different perspectives on your work:

- **Signal** (left): Incoming data, Otto chat, notifications. This is where intelligence flows in.
- **Orchestrate** (center): The main workspace. Vault content, documents, task boards. Where you do the work.
- **Control** (right): Settings, audit trail, member management, AI context. Where you configure and monitor.

The top-level navigation shows your active **Modules**: CRM, Contracts, Triage, Calendar, Documents, and Admin (system overlay).

---

## Step 8: Create Your First Vault

Navigate to the **CRM** module (or whichever module fits your first use case).

1. Click **New Vault**
2. Give it a name — use something real. A company you want to prospect, a deal you're working, a project you're tracking.
3. The Vault starts in the **Discover** chamber — the first stage of the lifecycle
4. You're automatically added as a member with full permissions

A Vault is a workflow instance. It moves through four Chambers: **Discover** (research and assess) > **Build** (create deliverables) > **Review** (verify and approve) > **Ship** (execute and close). Gates between Chambers ensure nothing moves forward until it's ready.

---

## Step 9: Ask Otto Your First Question

In the Signal panel of your new Vault, find the Otto chat interface.

Try one of these:

> What do we know about this vault so far?

> Research [the company you named the vault after]. What do they do, who runs it, and what's their latest news?

> Help me figure out the best angle to approach [company name] about [what you're selling or proposing].

Otto will use the enrichment pipeline to research, pull data, and respond. Everything Otto produces is a **draft** — it assists, you approve.

If Otto doesn't respond, check:
- Is your `OPENROUTER_API_KEY` set correctly in `.env`?
- Is the API server running without errors?
- Check the Admin module > AI Runtime settings

---

## Step 10: You're Live

You now have a running Airlock. From here:

- **Add more Vaults** as you identify prospects, deals, or projects
- **Move Vaults through Chambers** as work progresses (Discover > Build > Review > Ship)
- **Use Otto** to research, draft, and analyze — it gets more useful the more context your Vaults have
- **Explore the modules**: Triage for task management, Documents for rich text editing and PDFs, Calendar for timeline views
- **Read the playbooks** for guided workflows: `prospecting/otto-research.md` for full prospect research, `contracts/vault-lifecycle.md` for understanding Chamber transitions

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `docker compose up` fails | Make sure Docker Desktop is running. Check `docker --version`. |
| `alembic upgrade head` fails | Check `DATABASE_URL` in `.env`. Make sure Postgres container is healthy. |
| Google OAuth error | Verify redirect URI is exactly `http://localhost:3000/api/auth/callback/google` in Google Console. |
| Otto not responding | Check `OPENROUTER_API_KEY` in `.env`. Check API server logs for errors. |
| Frontend won't start | Run `pnpm install` from the repo root. Check Node version (`node --version` >= 20). |
| Port already in use | Kill existing processes on 3000/8000: `lsof -ti:3000 | xargs kill` |

---

## What's Next

You're at zero-to-one. Your Airlock is running. Now make it yours:

1. **Run the prospecting playbook** — use Otto to research your first real prospect
2. **Invite a team member** — test multi-user with roles and permissions
3. **Customize your workspace** — configure feature flags, adjust modules, set up your workflow
4. **Build your first playbook** — document a workflow that's specific to your business

Welcome to your Airlock.
