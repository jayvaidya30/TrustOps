## 📌 Project Name: TrustOps

**Zero-Trust Autonomous SRE Commander**

---

## 🧠 Project Vision

We are building an AI-powered Site Reliability Engineering (SRE) system that:

- Automatically detects production incidents
- Investigates root cause
- Suggests or generates fixes
- While enforcing **Zero-Trust security for AI agents**

The key differentiator:
👉 Even AI agents are continuously monitored and restricted
👉 Compromised or unsafe agents are blocked or quarantined

---

## 💡 One-Line Pitch

AI SRE agents that fix production incidents automatically — while a zero-trust layer ensures no agent can go rogue or leak secrets.

---

# 🧱 System Architecture

## Core Layers

### 1️⃣ SRE Agent Layer (Automation Brain)

Agents perform operational tasks:

- Incident Detection Agent
- Investigation Agent
- Fix Generation Agent

---

### 2️⃣ Zero-Trust Security Layer (Safety Brain)

Monitors agents and enforces policies:

- Trust scoring per agent
- Tool access guard
- Quarantine system

---

### 3️⃣ Archestra Platform (Orchestration Layer)

Handles:

- Agent orchestration
- MCP tool connectivity
- Secrets management
- Observability

---

### 4️⃣ MCP Tool Layer (External Integrations)

Agents interact with systems via MCP:

- Logs MCP
- GitHub MCP
- Slack MCP
- Metrics MCP (Optional)

---

# 🔄 End-to-End Workflow

## Step 1 — Logs Enter System

Logs arrive via Logs MCP server.

---

## Step 2 — Detection Agent

Detects anomalies and creates incident.

Example:

```
Incident: Memory spike
Service: Auth Service
Severity: Critical
```

---

## Step 3 — Investigation Agent

Analyzes logs and determines:

- Root cause
- Confidence score
- Blast radius

---

## Step 4 — Fix Agent

Suggests or creates fix:

- Config fix
- Code patch
- GitHub PR

---

## Step 5 — Zero Trust Security Validation

Before any tool access:

```
Agent → Requests Tool
↓
Security Layer Checks:
Trust Score
Tool Risk Level
Context Risk
↓
Allow OR Block
```

---

## Step 6 — Quarantine (If Needed)

If suspicious:

- Trust score drops
- Tool access blocked
- Agent quarantined

---

# 🧑‍🤝‍🧑 Team Structure (3 People)

---

## 👨‍💻 AI / Agent Engineer (Siddharth)

Owns:

- Detection Agent
- Investigation Agent
- Fix Agent
- Agent orchestration logic

---

## 🛡 Security Engineer (Jay)

Owns:

- Trust scoring engine
- Tool guard middleware
- Quarantine system
- Policy rules

---

## ⚙ Platform + MCP + Frontend Engineer (Shubham)

Owns:

- Archestra setup
- MCP servers
- Dashboard UI
- Fake incident generator
- Demo environment

---

# 🧰 Tech Stack

---

## 🧠 AI + Backend

- Python
- FastAPI
- LLM SDK (OpenAI / Claude / etc)

---

## 🤖 Agent Architecture

- Tool calling pattern
- MCP client integration

---

## 🏢 Platform

- Archestra Platform
- MCP Protocol

---

## 📊 Observability

- Logs monitoring
- Archestra observability
- Optional: Prometheus / Grafana

---

## 💻 Frontend

- Next.js
- React
- TailwindCSS

---

## 🗄 Database

- PostgreSQL (Incidents + Trust + Audit Logs)

---

## 🐳 Infra

- Docker
- Optional: Kubernetes

---

# 📦 Repository Structure

```
backend/
  agents/
  security/
  api/
  services/

frontend/

mcp/
  logs_mcp/
  fake_incident_generator/

infra/
  docker/
  archestra-config/

docs/
```

---

# 🤖 Agents Specification

---

## Detection Agent

Input:

- Logs

Output:

- Incident object

---

## Investigation Agent

Input:

- Incident
- Logs

Output:

- Root cause
- Confidence score
- Blast radius

---

## Fix Agent

Input:

- Root cause analysis

Output:

- Fix suggestion
- PR creation request

---

# 🛡 Security System Specification

---

## Trust Score Rules

Default:

```
100 = Fully trusted
```

---

### Penalties

| Event                     | Penalty |
| ------------------------- | ------- |
| Unauthorized tool attempt | -25     |
| Sensitive tool request    | -20     |
| Suspicious behavior       | -10     |

---

### Rewards (Optional)

| Event                  | Reward |
| ---------------------- | ------ |
| Safe successful action | +2     |

---

## Quarantine Rule

```
Trust < 20 → Quarantine Agent
```

---

# 🔒 Tool Guard Logic

Example Rules:

- High Risk Tool → Requires Trust > 70
- Medium Risk Tool → Requires Trust > 40

---

# 🧪 Demo Requirements

Must Demonstrate:

✅ Incident auto detection
✅ Root cause investigation
✅ Fix suggestion or PR generation
✅ Tool access blocked by security
✅ Trust score changes live
✅ Agent quarantine scenario

# 🚨 Out Of Scope (Do NOT Build)

- Complex ML anomaly models
- Too many MCP servers
- Microservice over-engineering
- Fancy UI animations

---

# 🧠 System Mental Model

```
Agents = Employees
Security Layer = Security Team
Archestra = Office Building
MCP Tools = Company Systems
```

---

# 🧪 Demo Scenario

1️⃣ System running normally
2️⃣ Fake outage injected
3️⃣ Detection agent triggers
4️⃣ Investigation agent analyzes
5️⃣ Fix agent proposes patch
6️⃣ Compromised agent attempts secret access
7️⃣ Security blocks tool
8️⃣ Trust score drops
9️⃣ Agent quarantined

---

# 🎯 Success Criteria

Project is successful if:

- Multi-agent orchestration works
- Security layer actively enforces policies
- MCP tools are used realistically
- Demo shows real operational workflow

---

# 🚀 Immediate Next Steps

Team must:

1. Setup repo structure
2. Run Archestra locally
3. Build Fake Logs MCP
4. Build Detection Agent first
5. Add Security Layer second
6. Build UI last

---

# 📌 Key Philosophy

Working Demo > Perfect Architecture

Security + Observability + Orchestration = Winning Project

