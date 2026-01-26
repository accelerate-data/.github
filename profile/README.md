## VibeData Overview

> **"Every data practitioner ships trusted silver and gold pipelines in hours instead of weeks, and keeps them running—with institutional knowledge saved alongside the code in git, the 'why' never lost."**

VibeData is an agentic platform that turns business questions into reliable silver and gold tables in hours. It captures institutional knowledge in skills and specifications, so domain expertise scales across your organization instead of staying trapped in people's heads.

### **The Platform**

VibeData consists of three core services:

#### **1. Foundation**
Foundation Service is the control plane for VibeData Azure Managed Applications — a publisher-side orchestrator combined with managed-app-side platform services that handle instance lifecycle, customer onboarding, licensing, and telemetry.

**Key Capabilities:**
- Azure Managed Application deployment in customer tenant
- Complete data sovereignty and security isolation
- Instance lifecycle management and updates
- Customer-controlled AI endpoints (Azure OpenAI)

#### **2. Studio**
Turn business intent into data products.  
Studio captures business questions and automatically generates dbt transformations, semantic models, and data contracts — guiding users from **Intent → Plan → Validation → Deploy**.

**Key Capabilities:**
- Intent-to-production workflow with full transparency
- Skills-powered domain expertise (source, domain, technical patterns)
- MCP integration for live context (Notion, Fabric, GitHub)
- GitHub-centric development (branches, PRs, code ownership)
- Comprehensive artifact generation (dbt models, tests, documentation, Fabric notebooks)

#### **3. Monitor**
Deliver autonomous observability and self-healing for pipelines, data quality, and infrastructure.

**Key Capabilities:**
- Built on Elementary + AI-powered enhancements
- Context-aware incident detection and root cause analysis
- Automated remediation with user-controlled autonomy
- Alert intelligence (correlation, business-context grouping)
- GitHub-integrated incident management

### **Core Differentiators**

| Differentiator | Description |
| -------------- | ----------- |
| **Fabric-Native** | Deep Azure Fabric integration vs warehouse-agnostic tools |
| **Skills Ecosystem** | Institutional knowledge captured in reusable skills and specifications |
| **Intent-to-Production** | Complete workflow from business question to deployed pipeline |
| **Day 2 Operations** | AI-powered monitoring, triage, and resolution—not just detection |

### **Target Users**

- **Full-Stack Analyst**: Data practitioners who bridge business context with technical execution (primary persona)
- **Data Reliability Engineer**: Operational excellence specialists ensuring data systems run reliably (co-primary persona)
- **Teams**: Early/growth-stage companies (1-20 data practitioners) using Azure Fabric

### **Technology Stack**

- **Data Platform**: Azure Fabric + OneLake
- **Transformation**: dbt Core
- **Ingestion**: dlt (production connectors + OpenAPI scaffolding)
- **Code Management**: GitHub (all artifacts, CI/CD, issues)
- **Deployment**: Azure Managed Application (customer tenant)
- **Observability**: Elementary + VibeData Monitor

### **Strategic Positioning**

**Complementary to Fabric IQ:**
> "Fabric IQ assumes your data is available, accurate, and complete. VibeData solves those three problems."

VibeData provides the data foundation (pipelines, quality, transformations) that powers Fabric IQ components (Data Agents, Operations Agent, Ontology, Semantic Models).

**vs Status Quo (ChatGPT/Claude):** End-to-end workflow vs fragmented copy-paste  
**vs dbt Cloud:** Intent-to-production vs code generation only  
**vs Fabric Copilot:** Proactive requirements crystallization vs reactive code help  
