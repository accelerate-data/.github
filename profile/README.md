## VibeData Overview

> **"Every data practitioner turns business questions into trusted silver and gold pipelines in hours—and keeps them running with institutional knowledge captured in git."**

VibeData is an agentic data engineering platform with three core functions:

- **Ingest** data from source systems into the lakehouse
- **Transform** raw data into curated silver and gold tables
- **Operate** the resulting pipelines in production

Most data tools address one of these in isolation — ingestion (Fivetran, Airbyte), transformation (dbt Cloud), or observability (Elementary, Monte Carlo). VibeData owns the full lifecycle and connects them through an improvement flywheel that makes the platform smarter with every pipeline built and every incident resolved.

The platform is built on four integrated pillars — LLM, Agentic Workflow, Skills, and MCPs — which together form a purpose-built platform for agentic data engineering. Users maintain control throughout: all generated code is visible, all decisions are logged in GitHub, and users approve deployments.

### **The Four Pillars**

| Pillar | Role | What It Enables |
| ------ | ---- | --------------- |
| **LLM** | The brain | Reasoning, code generation, and decision-making that powers every agent |
| **Agentic Workflow** | The spine | End-to-end automation from intent → deployed objects → observability → improvement |
| **Skills** | The domain memory | Domain expertise that makes agents expert BAs, data engineers, and DREs for the technology stack, sources, and domains |
| **MCP** | The hands | Open connectivity to live systems (lakehouse, specs, tickets) |

> An LLM alone is generic. Skills alone can be copied. MCP alone is a protocol. The agentic workflow alone is a feature. Together, they create a platform that takes significant effort to build and maintain.

### **The Improvement Flywheel**

Every action makes the platform smarter through two reinforcing loops:

| Loop | Trigger | What Improves |
| ---- | ------- | ------------- |
| **Build** | User intent → deployed pipeline | Skills: organizational knowledge, source knowledge, validation patterns, edge cases discovered |
| **Operate** | Production alert → issue resolved | Skills + Runbooks + Code + Tests: root causes documented, fix patterns recorded, new tests prevent recurrence |

Cross-loop reinforcement: Operate issues reveal gaps → Skills updated → Build avoids the same issue. Build context (intent, plan) preserved → Operate diagnosis understands original requirements.

---

### **Modules**

VibeData consists of three modules:

#### **1. Foundation (Control Plane)**
Manages the customer instance lifecycle. Deployed as an Azure Managed Application in the customer's Azure tenant.

**Key Capabilities:**
- Azure Managed Application deployment in customer tenant (complete data sovereignty)
- Instance lifecycle: bootstrapping, monitoring, credential rotation, upgrades, deletion
- Registry: configuration APIs for component versions, policies, feature flags, and entitlements
- Zero-trust networking: all PaaS services use private endpoints, VNet-integrated compute, Front Door → APIM → backend services

#### **2. Studio (UI)**
Main user interface for data practitioners. Covers ingestion, transformation, monitoring, skills management, and configuration.

**Components:**

| Component | Description |
| --------- | ----------- |
| **Source** | Configure and manage source data ingestion (API sources via dlt, database sources via Fabric Mirroring, Open Mirroring) |
| **Transform** | Build dbt models to transform data into silver and gold tables |
| **Monitor** | Data and pipeline observability — telemetry collection (Azure Log Analytics + Lakehouse ops schema), Elementary anomaly detection, alert routing via GitHub Issues |
| **Skills** | Browse, create, and manage the skills library; invoke the Retro Agent to analyze chat history and recommend skill updates |
| **Settings** | Data domains, sources, MCP servers, system prompts, usage & logs, profile, users/RBAC |

**Key Capabilities:**
- Intent-to-production workflow with phase transitions (Plan → Execute → Validate → Deploy)
- Branch-based development with ephemeral Fabric workspaces for isolated testing
- GitHub-centric CI/CD (linting, test coverage, performance analysis pre-merge; deployment post-merge)
- SSE streaming for real-time agent responses
- Engine layer: dlt, dbt, Git Integration, Fabric Integration, Alert, Git Webhook

#### **3. Agents**
AI agents that automate data engineering, analytics engineering, and DRE workflows. Organized into three groups:

| Group | Purpose | Persona |
| ----- | ------- | ------- |
| **Builders** | Create and modify pipelines (intent → production) | Full-Stack Analyst |
| **Operators** | Monitor, triage, diagnose, and remediate production issues | Data Reliability Engineer |
| **Advisors** | Provide on-demand recommendations to improve quality, performance, and skills | Both |

**Agent Inventory:**

| Agent | Group | Purpose |
| ----- | ----- | ------- |
| Requirements Agent | Builder | Gather requirements and design for new or modified silver and gold tables |
| Build Agent | Builder | Generate or reverse engineer dbt artifacts — models, tests, contracts, semantic models |
| Validation Agent | Builder | Perform golden data validation |
| Triage Agent | Operator | Prioritize and categorize GitHub issues; alert suppression |
| Diagnose Agent | Operator | Analyze telemetry, identify root cause |
| Remediation Agent | Operator | Execute approved runbooks, propose fixes |
| Test Recommender | Advisor | Suggest tests based on dbt model, chat history, and issue log |
| Performance Analyzer | Advisor | Review dbt code and data model for performance tuning |
| Retro Agent | Advisor | Analyze chat history and issues; suggest skill updates |

**Agent Knowledge Artifacts:**

| Artifact | Scope | Purpose |
| -------- | ----- | ------- |
| Skills (`.skill` files) | Reusable | Source, domain, and pattern knowledge consumed by all agents |
| `claude.md` | Per repo | Coding standards, naming conventions, business rules |
| `intent.md` | Per intent | Business context, requirements, success criteria |
| `plan.md` | Per intent | Transformation plan and technical decisions |

---

### **Data Platform Concepts**

VibeData implements data mesh principles through two foundational concepts:

**Source** — A logical grouping representing a replica of a source system. Defines the bronze data layer. Owned by source system experts (e.g., Salesforce admin owns Salesforce source). API sources use dlt pipelines; database sources use Fabric Mirroring; Open Mirroring sources are consumed directly.

**Data Domain** — Represents silver and gold tables owned by the functional team closest to that data (e.g., Marketing, Sales, Product). Each Data Domain publishes data products that are governed (dbt contracts, CI quality gates), discoverable (Fabric SQL endpoint, dbt docs), observable (Elementary anomaly detection), trustworthy (continuous production DQ checks), contract-backed, and semantically defined (dbt semantic models).

Each source or data domain is linked to a GitHub repository and a Fabric workspace.

---

### **Core Differentiators**

| Differentiator | Description |
| -------------- | ----------- |
| **Full Lifecycle** | Ingest + Transform + Operate in one platform, connected through an improvement flywheel |
| **Fabric-Native** | Deep Azure Fabric integration — workspaces, Spark compute, lakehouse, scheduling |
| **Skills Ecosystem** | Institutional knowledge captured in reusable skills (platform, data engineering, domain, source) |
| **Intent-to-Production** | Complete workflow from business question to deployed pipeline with full transparency |
| **Day 2 Operations** | AI-powered triage, diagnosis, and remediation — not just detection |
| **Improvement Flywheel** | Every pipeline built and every incident resolved makes the platform smarter; competitors start at zero |

---

### **Target Users**

| Persona | Role | Description |
| ------- | ---- | ----------- |
| **Full-Stack Analyst** | Primary | Mid/senior data practitioners bridging business context with technical execution. Target: 4 pipelines/user/month (8x improvement) |
| **Data Reliability Engineer** | Co-Primary | Operational excellence specialists ensuring data systems run reliably. Target: MTTR <30 min |
| **Head of Data / VP Analytics** | Secondary (Buyer) | Purchase decision maker |

**Anti-Personas:** Enterprise teams (10+ engineers), non-Fabric users, real-time/streaming teams, large company data engineers.

---

### **Strategic Positioning**

**Complementary to Fabric IQ:**
> "Fabric IQ assumes your data is available, accurate, and complete. VibeData solves those three problems."

VibeData provides the data foundation (pipelines, quality, transformations) that powers Fabric IQ components (Data Agents, Operations Agent, Ontology, Semantic Models).

**vs ChatGPT/Claude:** Context resets each session; no codebase awareness; no Day 2 operations
**vs AI Coding Assistants (Claude Code, Cursor):** Capable platform, but requires building system prompts, MCP tools, skills, and orchestration for data engineering. VibeData is this assembly, purpose-built and maintained.
**vs dbt Cloud + Elementary:** No agentic model building; specs outside code; no closed loop from issues to tests
**vs Fabric Copilot:** Schema-only data access; no intent/specs capture; no closed loop from issues to tests
**vs Fivetran + dbt:** Unifies tooling but not effort — still manual development, no AI/agentic capabilities, no knowledge capture

---

### **Technology Stack**

| Category | Technology | Customer Interaction |
| -------- | ---------- | -------------------- |
| Lakehouse | Microsoft Fabric | Customer-provisioned capacity and workspaces |
| Ingestion | dlt | Python pipelines in Fabric notebooks |
| Transformation | dbt | SQL models executed via Spark in Fabric notebooks |
| Data Quality | Elementary | Anomaly detection, freshness, volume checks |
| Testing | dbt tests (unit + data), dbt-coverage | Included in CI; visible in PR checks |
| Orchestration | Fabric pipelines | Customer-managed scheduling |
| Version Control | GitHub | Repos, PRs, Issues, CI/CD via GitHub Actions |
| AI | Azure AI Foundry | Customer-provisioned LLM endpoints |
| Deployment | Azure Managed Application | Customer tenant; Front Door → APIM → App Service/Function App |

---

### **Deployment Model**

VibeData is deployed as an Azure Managed Application in the customer's Azure tenant.

| Principle | Description |
| --------- | ----------- |
| **Data residency** | All customer data stays in customer tenant; no customer data sent to publisher |
| **Customer ownership** | Customer owns all traffic logs, audit trails, Fabric capacity, AI endpoints, and data |
| **Zero-trust networking** | All PaaS services use private endpoints; all compute is VNet-integrated |
| **Identity** | User's Entra ID for Fabric; user's GitHub OAuth for code operations; all actions attributed to the user |

---

## Repository Naming Convention

All repositories follow a consistent naming pattern to organize code, specifications, infrastructure, and experimental work:

| Prefix | Purpose | Example |
|--------|---------|---------|
| `vd-specs-<name>` | Specifications and requirements | `vd-specs-foundation` |
| `vd-mockup-<name>` | UI/UX mockups and designs | `vd-mockup-studio` |
| `vd-docs-<name>` | Documentation sites and knowledge bases | `vd-docs-community`, `vd-docs-internal` |
| `vd-<name>` | Production code repositories for applications (API, Backend, UI, Data platform) | `vd-studio`, `vd-dbt-fabric-spark` |
| `vd-infra-<name>` | Infrastructure as Code (Terraform, Bicep, etc.) | `vd-infra-azure` |
| `vd-skills-<name>` | Domain skills and capabilities | `vd-skills-community`, `vd-skills-proprietary` |
| `vd-agent-<name>` | Individual agent implementations | `vd-agent-orchestrator` |
| `vd-template-<name>` | Standards, templates, and scaffolding | `vd-template-service` |
| `scratch-<name>` | Experimental and ephemeral work | `scratch-llm-evaluation` |

### Public vs Private Repositories

- **Default:** All repositories are private unless explicitly made public
- **Semantic naming:** When both public and private versions exist, use descriptive suffixes:
  - `vd-docs-community` (public documentation)
  - `vd-docs-internal` (internal documentation)
  - `vd-skills-community` (open-source skills)
  - `vd-skills-proprietary` (proprietary skills)
