# GitHub Repository Description

## Short Description (160 chars max for "About" field)
```
Organizational memory platform for LLM agent orchestration, with governance, versioning & semantic search.
```

## Long Description (for README pinning / repository description)

**Cortex v3.5.0** — Open-source organizational memory and LLM agent orchestration platform.

Enable teams and autonomous agents to:
- 🧠 Build shared memory (notes, checkpoints, patterns, decisions)
- 🔄 Collaborate on structured work with integrated governance (validation phases, cascade signatures)
- 🎯 Orchestrate multi-agent missions with version control and semantic versioning
- 🔐 Track every action with complete audit trail and snapshot-based instancing

**Stack:** SurrealDB + PostgreSQL + React 19 + Node.js 22 + MCP (Model Context Protocol)

---

## Key Features

✅ **For Teams**
- Interactive dashboard (React 19)
- Project lifecycle: Project → Mission → ODM → Task with built-in governance
- Full-text search + 3D graph explorer
- RBAC (4-level: admin, producteur, contributeur, lecteur)

✅ **For LLM Agents**
- ~89 MCP tools for workspace, memory, governance, search
- API-first design (234 REST endpoints)
- OAuth 2.0 StreamableHTTP for secure agent authentication
- Signal-based orchestration for autonomous workers

✅ **For Infrastructure**
- Self-hosted Docker Compose (5 min setup)
- Air-gappable (works offline)
- Configurable GenAI providers (Google Gemini, extensible to OpenAI, Anthropic)
- Real-time SSE events + webhooks

---

## Quick Start

**Option 1: Demo**
```
URL: https://cortex.arkalabs.app
(credentials provided by admin)
```

**Option 2: Local Install**
```bash
git clone https://github.com/arka-squad/cortex.git
cd cortex
docker-compose up -d
# UI at http://localhost:80, API at http://localhost:9096
```

---

## Architecture

```
┌─────────────────────────────────────┐
│  UI (React 19)                      │
│  REST API (234 endpoints)           │
│  MCP Server (~89 tools)             │
├─────────────────────────────────────┤
│  31 Domain Services                 │
│  • Governance, Memory, Versioning   │
│  • Agent Orchestration, Snapshot    │
├─────────────────────────────────────┤
│  SurrealDB v2.1.7                   │
│  PostgreSQL 16 + Meilisearch        │
│  Ollama (embeddings) + MinIO        │
└─────────────────────────────────────┘
```

---

## Use Cases

- **LLM Agent Orchestration** — Multi-agent coordination on complex missions
- **Organizational Memory** — Persistent knowledge base, pattern detection
- **Business Governance** — Validation workflows, audit trails, compliance
- **Project Instantiation** — Semantic versioning, divergence tracking, cross-pollination

---

## Status

- **v3.5.0** — Beta, production-active
- **234 endpoints**, **31 services**, **~89 MCP tools**
- **1000+ tests**, full CI/CD
- MIT License

---

## Documentation

- **Full Guide**: See [README.md](./README.md)
- **API OpenAPI**: http://localhost:9096/api/docs (in dev)
- **Community**: GitHub Issues, Email support@arkalabs.app

---

## License

MIT — Free to use, modify, and deploy.
