# Cortex — Arka Labs

**Version** : v3.5.0  
**Statut** : Production active  
**Dernière mise à jour** : avril 2026

---

## Bienvenue

Vous accédez à **Cortex** — une plateforme de **mémoire organisationnelle augmentée** conçue pour orchestrer des agents LLM dans des environnements demandant structure, gouvernance et traçabilité.

Ce document est votre point d'entrée. Il décrit ce que Cortex fait, comment y accéder, et comment commencer.

---

## Qu'est-ce que Cortex ?

**En trois phrases :**

Cortex est un backend d'intelligence opérationnelle qui maintient un graphe de connaissance structuré (SurrealDB), gouverné par des règles métier, enrichi par une mémoire d'apprentissage multi-strate. Il expose deux surfaces complémentaires — une **interface utilisateur React** pour les humains et un **serveur MCP** pour les agents LLM — permettant une collaboration homme-machine sur du travail structuré (projets, missions, livrables, analyses).

**Caractéristiques clés :**
- Mémoire structurée et versionnable : chaque artefact conserve son histoire sémantique
- Gouvernance déclarative : phases, règles de signature, profils composables
- Auto-hébergement : stack 100% locale (air-gappable si besoin), zéro cloud obligatoire
- Orchestration d'agents : interface MCP standardisée + API REST complète

---

## Ce que vous pouvez faire avec Cortex

### Pour les humains

**Interface web** (`https://cortex.arkalabs.app` ou instance locale) :
- 📊 **Tableau de bord** : vue synthétique de projets, agents, statuts
- 📁 **Catalogue** : parcourir les blocs de connaissance (skills, expertises, outils, méthodes)
- 👥 **Profils** : consulter et composer des profils HYOS (assemblages dynamiques de blocs)
- 🗂️ **Projets** : créer et suivre des missions (MC), livrables (ODM), tâches (TASK)
- 💾 **Mémoire** : lire notes structurées, patterns détectés, décisions documentées
- 🔍 **Recherche** : requête full-text sur tous les artefacts + filtrages
- 📊 **Graphe 3D** : naviguer les relations du graphe de connaissance

### Pour les agents LLM

**Serveur MCP** (port 3001, variante `claude-code` disponible) :
- **Workspace** : créer/lire projets, missions, ODM, tâches
- **Mémoire** : déposer notes, lire résumés, accéder historique
- **Gouvernance** : consulter phases, règles, templates
- **Catalogue** : lister blocs, modèles, ingérer du contenu
- **Recherche** : requêtes transversales sur le graphe
- **Profils HYOS** : consulter et composer des profils

### Pour l'infrastructure

**REST API** (`/api/*`, port 9096) :
- 234 endpoints organisés en 24 modules
- Authentification JWT Supabase + API Key
- RBAC 4 niveaux (admin, producteur, contributeur, lecteur)
- Documentation OpenAPI disponible
- Webhooks temps réel (SSE)

---

## Architecture en un coup d'œil

### Stack d'infrastructure

| Composant | Technologie | Port | Rôle |
|-----------|-------------|------|------|
| **Graphe de connaissance** | SurrealDB v2.1.7 | privé | Nœuds, arêtes, historique |
| **Documents & audit** | PostgreSQL 16 | privé | Artefacts, log, OAuth, clés |
| **Recherche** | Meilisearch v1.12 | privé | Index full-text + filtres |
| **Stockage blob** | MinIO | privé | Fichiers, exports |
| **IA local** | Ollama | privé | Embeddings (bge-m3), LLM (mistral) |
| **API backend** | Node.js 22 / Express 5 | **9096** | REST, logique métier |
| **MCP serveur** | Node.js 22 / Express 5 | **3001** | Outils pour agents LLM |
| **UI web** | React 19 / Vite | **80** | Interface utilisateur |

### Architecture logicielle

```
┌─────────────────────────────────────────────────────────┐
│ Interfaces                                              │
│  • UI React 19 (port 80)                               │
│  • REST API (port 9096)                                 │
│  • MCP Server (port 3001)                              │
├─────────────────────────────────────────────────────────┤
│ Services domaine (31 services)                         │
│  • governance-builder, memory-service, versioning      │
│  • snapshot, divergence, resonance, spawn-agent        │
│  • template-builder, encryption-service, ...           │
├─────────────────────────────────────────────────────────┤
│ Stockage & recherche                                    │
│  • SurrealDB (graphe)                                   │
│  • PostgreSQL (documents, audit)                        │
│  • Meilisearch (recherche)                             │
│  • MinIO (blob storage)                                │
│  • Ollama (IA local)                                    │
└─────────────────────────────────────────────────────────┘
```

---

## Accès à Cortex

### Prérequis

- **Instance déployée** avec tous les 9 services actifs (docker-compose v3)
- **URL d'accès** : fournie par votre administrateur (ex: `https://cortex.arkalabs.app` ou `http://localhost`)
- **Credentials** : compte utilisateur + clé API (si accès programmatique)

### Authentification

| Interface | Méthode | Format |
|-----------|---------|--------|
| **UI web** | OAuth 2.0 Supabase | URL de login standard |
| **REST API** | JWT Token + API Key | Header `Authorization: Bearer <JWT>` + `X-API-Key` |
| **MCP (claudeai)** | OAuth 2.0 StreamableHTTP | Connexion transparente via Claude |
| **Hub local** | Token PG (5 min TTL) | Auto-généré, auto-renouvelé |

### RBAC — 4 niveaux de permission

| Rôle | Permissions |
|------|------------|
| **admin** | Toutes opérations, gestion infra, accès audit |
| **producteur** | Créer projets, templater, signer gouvernance |
| **contributeur** | Créer missions/ODM/tâches, déposer notes, exécuter workflows |
| **lecteur** | Consulter, chercher, lire mémoire |

Chaque projet applique une ACL stricte : seuls les contributeurs assignés au projet peuvent y accéder.

### Premières actions selon votre rôle

**Si vous êtes administrateur :**
1. Vérifier tous les 9 services actifs : `/api/cortex/status`
2. Configurer les providers GenAI (Google Gemini, etc.) : `/account/providers`
3. Créer les profils HYOS de base via le Catalogue

**Si vous êtes producteur :**
1. Consulter les profils HYOS disponibles : menu Catalogue
2. Créer un nouveau projet : `/projects` → "Nouveau projet"
3. Définir la gouvernance du projet (phases, règles)

**Si vous êtes contributeur :**
1. Rejoindre un projet assigné
2. Consulter le tableau de bord du projet
3. Créer une première mission (MC) ou tâche (TASK)

**Si vous êtes agent LLM (Claude Code) :**
1. Activer la variante MCP `claude-code` : `mcp.claude.ai` → ajouter serveur `cortex.arkalabs.app:3001`
2. Utiliser les outils MCP pour créer/lire/analyser dans le workspace
3. Consulter les capacités : `/api/cortex/status`

---

## Interfaces disponibles

### 🖥️ Interface utilisateur web

**URL :** `https://cortex.arkalabs.app` (ou instance locale)  
**Navigateur :** Chrome, Firefox, Safari (v15+)  
**Connexion :** OAuth Supabase (email)

**Modules clés :**
- **Dashboard** : synthèse projets + agents actifs
- **Catalogue** : parcourir blocs HYOS (skills, expertises, outils, méthodes)
- **Profils** : consulter et composer profils
- **Projets** : créer/éditer missions, ODM, tâches avec gouvernance
- **Mémoire** : notes structurées, checkpoints, résumés par projet
- **Explorer** : navigation graphe 3D
- **Recherche** : requête full-text unifiée
- **Gouvernance** : visualiser règles, phases, flowmaps

### 🔌 Serveur MCP pour agents

**Endpoint :** `cortex.arkalabs.app:3001` (ou localhost:3001)  
**Transport :** stdio, HTTP/REST avec OAuth 2.0  
**Variantes disponibles :**
- `main` : tous les ~89 outils
- `claude-code` : sous-ensemble optimisé pour Claude Code (read-heavy, workspace + memory + gov + search)
- `cloud` : variant cloud Anthropic
- `nemesis` : variant workflows
- `praxis` : variant mémoire (write-heavy)

**Exemple : connexion Claude Code**
```
mcp:
  servers:
    cortex:
      command: curl
      args:
        - http://cortex.arkalabs.app:3001
      env:
        CORTEX_TOKEN: <votre-token>
```

**Outils disponibles (89 au total) :**
- Workspace (10) : `cortex_workspace_draft`, `cortex_workspace_scope`, `cortex_workspace_sign`, ...
- Memory (6) : `cortex_memory_deposit`, `cortex_memory_resume`, `cortex_memory_checkpoint`, ...
- Governance (3) : `cortex_gov_flowmap`, `cortex_gov_addons`, ...
- Catalog (4) : `cortex_catalogue_search`, `cortex_catalogue_modeles`, ...
- Profil (5) : `cortex_profil_list`, `cortex_profil_pack_hyos`, ...
- Search (1) : `cortex_search`
- Status (1) : `cortex_status`

### 📡 REST API

**Base URL :** `/api` (port 9096)  
**Auth :** JWT Supabase + API Key (header `X-API-Key`)  
**Format :** JSON

**Modules (24) :**
`atoms`, `catalogue`, `deposit`, `divergence`, `documents`, `events`, `governance`, `ingest`, `kairos`, `library`, `memory`, `navigation`, `search`, `templates`, `versioning`, `workspace`, `account`, `public`, `unversioned`, etc.

**Documentation :** lien OpenAPI disponible en développement (`/api/docs`)

---

## Concepts clés

### Cycle de vie : MC → ODM → TASK

Chaque projet suit une progression structurée :

1. **MC (Mission Contract)** : engagement, cadre, validation
2. **ODM (Ordre de Mission)** : sous-objectif, exécutable
3. **TASK** : unité atomique assignée
4. **CR (Compte-rendu)** : résultat, verdict, rework si nécessaire

Chaque étape a des règles de signature cascade : une TASK ne peut être signée que si tous les critères sont remplis, puis l'ODM parental, puis le MC.

### Mémoire structurée

Chaque agent accumule de la mémoire par projet :
- **Notes Praxis** : blocages, besoins, signaux faibles, clarifications
- **Checkpoints** : verdicts, protocoles de résilience (Continuer / Pause / Degrade / Handoff / Skip)
- **Patterns** : récurrences détectées
- **Décisions** : choix documentés

La mémoire est versionable et interrogeable via MCP.

### Profils HYOS

Un profil n'est plus un modèle figé — c'est un **assemblage dynamique** de blocs :
- `skill` : compétence transverse (TDD, Kanban, RGPD, CI/CD)
- `expertise` : savoir métier (pharmacovigilance, urbanisme, growth)
- `tool` : technologie (React, Docker, Kubernetes, PostgreSQL)
- `method` : approche structurée (OWASP Top 10, audit RGPD)
- `scope` : permission/mode (can_assign, read_only, mode_projet)

Un profil se compose à la demande selon projet et contexte.

### Versioning sémantique

Chaque artefact porte un **semver** (`major.minor.patch`) + **content hash** (SHA-256). Quand un projet instancie un artefact, il crée un **snapshot** immuable. Le drift entre la version consommée et la version courante est calculé en temps réel.

---

## Roadmap — Ce qui arrive

**Q2 2026 :**
- ✅ Workers autonomes (debrief, nlp_cleanup, reconciliation)
- 🔄 Activation embeddings bge-m3 en production
- 📦 Bloc `squads.ts` + `depot.ts` actifs (services instantiation agents)

**Q3 2026 :**
- 🤖 9 agents HCM auto-déployables (Extracteur, Cartographe, Forgeron, Gardien, Diffuseur, etc.)
- 🌐 Cross-pollination automatique inter-projets
- 📊 Divergence inter-instances entièrement mesurable + UI

**Q4 2026 & au-delà :**
- Intégration providers GenAI supplémentaires (OpenAI, Anthropic, etc.)
- Time-travel complet (VERSION clause SurrealDB + diffing)
- UI collaboration temps réel

---

## Support & liens

### Ressources
- **Documentation technique** : `/docs` ou `DOCS-CORTEX/`
- **API OpenAPI** : `/api/docs` (en dev)
- **Code source** : `/hcm` (monorepo npm)
- **Logs système** : `/var/log/cortex.log` ou Docker logs

### Contact
- **Administrateur système** : équipe infrastructure
- **Support technique** : canal interne Slack / email support
- **Feedback produit** : issues sur `github.com/arkalabs/cortex`

---

## Notes de version

**v3.5.0 (avril 2026) — Production active**

- ✅ Mémoire multi-strate + LangGraph agent autonome
- ✅ Profils HYOS modulaires
- ✅ Versioning sémantique + snapshots
- ✅ Gouvernance par phases + cascade de signatures
- ✅ 234 endpoints REST + 89 outils MCP
- ✅ RBAC 4 niveaux + anti-IDOR (SEC-022)
- ✅ Providers GenAI configurables (Google Gemini)
- ✅ Résonance sémantique (embeddings bge-m3, Ollama local)
- ⏳ Workers autonomes (en phase déploiement)
- ⏳ Agents HCM 9 auto-constructeurs (roadmap Q3)

**Dettes mineures documentées :**
- `tools/depot.ts`, `tools/squads.ts`, `tools/praxis.ts` : coquilles vides (0 outil)
- Embeddings bge-m3 présent mais non activé en prod index

---

**Prêt à démarrer ?** Connectez-vous via le lien d'accès ci-dessus ou contactez votre administrateur pour les identifiants.
