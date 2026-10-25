# Agentic AI for IT and Business Operations

Hackathon prototype: a multi-agent platform that routes employee support tickets to specialized AI agents (Network, Application, Finance), troubleshoots with organizational knowledge, chats with the employee in-app, and documents resolution in a mock ITSM portal.

**Team:** Aquil · Vybhavi · Divyansh  
**Hackathon:** AI Applications Hackathon (deliverables due Dec 1)

## Ownership

| Person | Focus |
|--------|--------|
| **Vybhavi** | LangGraph agents (orchestrator, Network agent, tools, chat resume) |
| **Divyansh** | Mock ITSM portal UI (tickets, chat panel, work notes) |
| **Aquil** | FastAPI backend, RAG/KB, mock tools, evaluation, docs & deliverables |

## Mock ITSM portal

Divyansh's slice is in `frontend/`. It is a ServiceNow-shaped desk, not a real ServiceNow connection.

- List, search, and filter incidents by state and assignment group
- Open a new incident
- Chat with the caller on the incident (in-app stand-in for Teams)
- Keep internal work notes on the service desk record
- Resolve, escalate, reopen, and reassign from the desk
- Switch between **Service desk** and **Caller**. The caller view is signed in as Alex Rivera and hides work notes

The portal ships with sample incidents. `INC0010042` is the VPN demo: the Network agent has already retrieved a runbook, run a mock diagnostic, and asked the caller to refresh a certificate.

With no API configured, tickets, chat, and notes stay in the browser (`localStorage`). **Reset demo** in the header restores the sample incidents.

### Run it

Requires Node.js 20 or newer.

```powershell
cd frontend
npm install
npm run dev
```

Open http://localhost:5173.

```powershell
npm run build
```

### Point it at the API

Copy `frontend/.env.example` to `frontend/.env` and set:

```
VITE_API_URL=http://localhost:8000
```

Restart the dev server after changing that file. The same screens then call FastAPI. If the API does not respond, the portal falls back to the local mock and says so.

Expected routes:

| Method | Path | Body | Response |
|--------|------|------|----------|
| `GET` | `/api/tickets` | | `Ticket[]` |
| `POST` | `/api/tickets` | caller, email, category, priority, short description, description | `{ ticket, work_notes, messages }` |
| `GET` | `/api/tickets/:id` | | `{ ticket, work_notes, messages }` |
| `PATCH` | `/api/tickets/:id` | `state`, `assignment_group`, and/or `priority` | `{ ticket, work_notes, messages }` with only the records created by that change |
| `POST` | `/api/tickets/:id/messages` | `{ body, role, author }` | `ChatMessage`, or the wrapped shape above |
| `POST` | `/api/tickets/:id/work-notes` | `{ body, author }` | `WorkNote`, or the wrapped shape above |

`role` on a message is `employee` or `agent`. A bare ticket, message, or work note is also accepted. Field names match `frontend/src/types.ts`, and the client lives in `frontend/src/api.ts`.

## Architecture (MVP)

- **Mock ITSM portal** — create/list tickets, per-ticket chat, work notes (ServiceNow-shaped; not real ServiceNow)
- **Orchestrator agent** — classify and route to a fixed specialist
- **Network agent** (demo core) — RAG + mock diagnostics + ask user + resolve/escalate
- **Application / Finance agents** — stubs with team KB
- **Fixed agents per team** — new *session* per ticket, not a new agent definition per ticket

## Repo layout

```
frontend/         # Mock ITSM portal (React + Vite)
backend/          # FastAPI, agents, tools, RAG (Aquil)
data/kb/          # Sample runbooks
docs/             # Architecture, evaluation
tests/
```

## Status

The mock ITSM portal is ready to demo on its own. Agents, the API, and the knowledge base are still in progress. New incidents stay unassigned until the orchestrator routes them.

## GitHub workflow

- Branch per feature (`feature/...`), open pull requests into `master`
- Never commit secrets. Use `.env` (see `frontend/.env.example`)
- If the repo is private for submission, add collaborator `TinyZhen`
