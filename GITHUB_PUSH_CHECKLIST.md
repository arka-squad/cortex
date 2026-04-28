# GitHub Push Checklist — Cortex v3.5.0

**Status**: Ready for first release beta push  
**Target Repo**: git@github.com:arka-squad/cortex.git  
**Branch**: main

---

## Files Ready for Push

### 📄 Documentation
- ✅ `README.md` — Comprehensive public onboarding (vision, quickstart, architecture, features, roadmap)
- ✅ `GITHUB_DESCRIPTION.md` — Repository "About" section + long description (this file)
- ✅ `LICENSE` (MIT) — Copy from parent repo or create if needed

### 📸 Screenshots (10x)
Located in `screenshots/` directory:
- ✅ Dashboard.png — Active projects + agent status dashboard
- ✅ Missions.png — Mission listing with statuses
- ✅ ProjectDetail.png — Full project detail view
- ✅ odm.png — ODM detail with priority/assignee
- ✅ Cataloue.png — Catalogue of 268+ knowledge blocks
- ✅ Profils_HYOS_listing.png — Profile listing view
- ✅ Profil_HYOS.png — Profile composition detail
- ✅ FlowmapNemesis.png — Governance flowmap visualization
- ✅ CRQA.png — CR (Compte-Rendu) & QA workflow
- ✅ adminLibUpload.png — Admin library management

### 🔐 Optional: Configuration Templates
- `.gitignore` — Exclude .env, docker-compose.local.yml, dist/, node_modules/
- `.github/workflows/` — (Optional for this first push)

---

## Files NOT for GitHub

❌ `.env` — Secret credentials (stays in private repo only)  
❌ `docker-compose.yml` — Infrastructure config (may vary per deployment)  
❌ `hcm/` directory — Application source code (stays in private repo)  
❌ `docs/DEPLOY.md` — Internal deployment procedures  
❌ `.claude/` — Internal development hooks

---

## Short Description (GitHub "About" field)

```
Organizational memory platform for LLM agent orchestration, with governance, versioning & semantic search.
```

**Character count**: 119 / 160 ✅

---

## Topics for GitHub

Add these topics to the repository:
- `cortex`
- `llm`
- `agent-orchestration`
- `organizational-memory`
- `typescript`
- `react`
- `nodejs`
- `governance`
- `semantic-search`

---

## Instructions for Push

1. **Create repo on GitHub** (if not exists)
   ```
   Repository: cortex
   Description: (use GITHUB_DESCRIPTION.md short version)
   Visibility: Public
   Topics: cortex, llm, agent-orchestration, ...
   ```

2. **Initialize cortex repo locally** (if needed)
   ```bash
   mkdir cortex && cd cortex
   git init
   git add README.md GITHUB_DESCRIPTION.md LICENSE screenshots/
   git commit -m "feat: init cortex beta v3.5.0 public release"
   git remote add origin git@github.com:arka-squad/cortex.git
   git branch -M main
   git push -u origin main
   ```

3. **Or: Push from CORTEX_READY folder**
   ```bash
   cd CORTEX_READY
   git init
   git add .
   git commit -m "feat: cortex beta v3.5.0 public release"
   git remote add origin git@github.com:arka-squad/cortex.git
   git branch -M main
   git push -u origin main
   ```

4. **Update GitHub repo settings**
   - Main branch protection: Require reviews before merge (optional for now)
   - Visibility: Public
   - Description: Copy from `GITHUB_DESCRIPTION.md`

---

## First Release Notes (v3.5.0-beta)

```markdown
## Cortex v3.5.0-beta

**First public release** 🚀

Organizational memory platform for LLM agent orchestration.

### Status
- Beta production-ready
- 234 REST endpoints + ~89 MCP tools
- 1000+ tests, full CI/CD
- Self-hosted, air-gappable

### What's Included
- React 19 UI (dashboard, governance, explorer)
- Node.js 22 API + MCP server
- SurrealDB graph database + PostgreSQL
- Docker Compose setup (5 min)
- Full documentation + 10 screenshots

### Getting Started
1. See [README.md](./README.md) for quickstart
2. Run `docker-compose up -d`
3. Visit http://localhost:80

### License
MIT

---

### Next
- v3.5.1: Bug fixes & stability
- v3.6: Workers auto-execution + expanded GenAI providers
```

---

## Version History (for internal reference)

- **v3.5.0** (2026-04-28) — Initial public beta release
- **v3.4.x** — Internal production versions
- **v3.0-v3.3** — Alpha development cycle

---

## Contact & Support (once public)

Add to README footer:
- **Issues & Discussions**: GitHub Issues
- **Email**: support@arkalabs.app (or team email)
- **Community**: #cortex-users (Slack or Discord)

---

## Post-Push Tasks

- [ ] Update main HCM repo with link to cortex public repo
- [ ] Announce v3.5.0-beta in internal channels
- [ ] Set up automated syncing from private repo to public (if needed)
- [ ] Monitor GitHub issues for early feedback
