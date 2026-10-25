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

## Architecture (MVP)

- **Mock ITSM portal** — create/list tickets, per-ticket chat, work notes (ServiceNow-shaped; not real ServiceNow)
- **Orchestrator agent** — classify and route to a fixed specialist
- **Network agent** (demo core) — RAG + mock diagnostics + ask user + resolve/escalate
- **Application / Finance agents** — stubs with team KB
- **Fixed agents per team** — new *session* per ticket, not a new agent definition per ticket

## Repo layout (planned)

```
backend/          # FastAPI, agents, tools, RAG
frontend/         # Mock ITSM portal
data/kb/          # Sample runbooks
docs/             # Architecture, evaluation
tests/
```

## Status

Initial repository setup. Implementation in progress.

## GitHub workflow

- Branch per feature (`feature/...`), open PRs into `main`
- Never commit secrets — use `.env` (see `.env.example` when added)
- If the repo is private for submission, add collaborator `TinyZhen`
