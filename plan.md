## Plan: TrustOps Full Implementation & Task Assignment

A phased implementation plan to build the Zero-Trust Autonomous SRE Commander from the current empty scaffold. Work is split across 3 team members — **Siddharth** (AI/Agents), **Jay** (Security), **Shubham** (Platform/MCP/Frontend) — with dependencies clearly ordered so parallel work is possible from day one.

---

### Phase 1 — Foundation & Scaffolding (Days 1–3) `[All Members]`

1. **Shubham** — Initialize project configs: create `pyproject.toml` + `requirements.txt` in `backend/`, `package.json` in `frontend/`, root `docker-compose.yaml` in `infra/`, and PostgreSQL database schema (incidents, trust_scores, audit_logs tables) in a new `backend/db/` module.
2. **Shubham** — Set up Archestra Platform locally, write config in `infra/archestra-config/`, and verify agent orchestration harness boots.
3. **Siddharth** — Create the base `Agent` abstract class in `backend/agents/` (Python) with a common interface: `run()`, `get_status()`, `request_tool()`. Wire LLM SDK (OpenAI/Claude) client initialization in a shared `backend/services/llm_client.py`.
4. **Jay** — Design and implement the FastAPI app skeleton in `backend/api/` with placeholder routers (`/incidents`, `/agents`, `/security`, `/tools`). This becomes the shared API surface everyone integrates into.

---

### Phase 2 — MCP Tools & Fake Data (Days 3–5) `[Shubham]`

5. **Shubham** — Build the **Logs MCP Server** in `mcp/logs_mcp/` that serves structured log data over MCP protocol.
6. **Shubham** — Build the **Fake Incident Generator** (`mcp/fake_incident_generator/`) that injects realistic outage scenarios (memory spikes, latency, error bursts) into the Logs MCP.
7. **Shubham** — (Stretch) Stub out **GitHub MCP** and **Slack MCP** connectors for the Fix Agent to use later.

---

### Phase 3 — SRE Agents (Days 4–8) `[Siddharth]`

8. **Siddharth** — Implement **Detection Agent** in `backend/agents/`: consumes logs from Logs MCP → runs LLM-based anomaly detection → creates an `Incident` object (service, severity, timestamp) → persists to PostgreSQL via `backend/services/incident_service.py`.
9. **Siddharth** — Implement **Investigation Agent**: takes an `Incident` + raw logs → produces root cause analysis, confidence score (0–1), and blast radius → stores findings on the incident record.
10. **Siddharth** — Implement **Fix Agent**: takes root cause analysis → generates a fix suggestion (config change or code patch) → optionally creates a GitHub PR via GitHub MCP. Integrate with the Tool Guard (from Jay) for every tool call.
11. **Siddharth** — Wire the **agent orchestration pipeline**: Detection → Investigation → Fix, with each step calling into Jay's security layer before using any tool.

---

### Phase 4 — Zero-Trust Security Layer (Days 4–8) `[Jay]`

12. **Jay** — Build the **Trust Scoring Engine** in `backend/security/`: maintain per-agent trust scores in PostgreSQL (default 100), apply penalty rules (unauthorized tool attempt → −25, sensitive tool request → −20, suspicious behavior → −10), and optional reward (+2 for safe action).
13. **Jay** — Build the **Tool Guard Middleware** in `backend/security/`: intercept every agent `request_tool()` call, check trust score against tool risk level (high-risk → trust > 70, medium-risk → trust > 40), and return allow/block decision. Log every decision to an `audit_logs` table.
14. **Jay** — Build the **Quarantine System**: when trust score < 20, automatically block all tool access for the agent, mark it quarantined, and emit an event to the dashboard.
15. **Jay** — Expose security endpoints in the FastAPI app: `GET /security/trust-scores`, `GET /security/audit-log`, `POST /security/quarantine/{agent_id}`, `POST /security/reset/{agent_id}`.

---

### Phase 5 — Frontend Dashboard (Days 8–11) `[Shubham]`

16. **Shubham** — Initialize Next.js + React + TailwindCSS project in `frontend/`.
17. **Shubham** — Build dashboard pages: **Incidents list** (status, severity), **Agent status** (running/quarantined + live trust scores), **Audit log** (tool access decisions), and **Incident detail** (root cause + fix suggestion).
18. **Shubham** — Add real-time trust score updates (polling or WebSocket) and visual quarantine alerts.

---

### Phase 6 — Integration, Docker & Demo (Days 11–14) `[All Members]`

19. **Shubham** — Write `Dockerfile` for backend, frontend, and PostgreSQL; compose everything in `docker-compose.yaml` under `infra/`. Ensure one-command `docker compose up` boots the full stack.
20. **Siddharth** — End-to-end integration test: inject fake outage → verify detection → investigation → fix pipeline runs fully.
21. **Jay** — Demo the security scenario: trigger a compromised agent attempting secret access → tool blocked → trust score drops live → agent quarantined.
22. **All** — Rehearse the 9-step demo scenario from `team.md` end-to-end.

---

### Task Assignment Summary

| Member | Owns | Key Deliverables |
|---|---|---|
| **Siddharth** (AI/Agent) | `backend/agents/`, `backend/services/` | Base Agent class, Detection/Investigation/Fix agents, orchestration pipeline, LLM integration |
| **Jay** (Security) | `backend/security/`, `backend/api/` | Trust scoring engine, Tool Guard middleware, Quarantine system, FastAPI skeleton + security endpoints, audit logging |
| **Shubham** (Platform/MCP/FE) | `mcp/`, `frontend/`, `infra/` | Project configs, Archestra setup, Logs MCP, Fake Incident Generator, GitHub/Slack MCP stubs, Next.js dashboard, Docker compose |

---

### Further Considerations

1. **LLM Provider** — Which LLM SDK to start with? OpenAI (cheaper/faster for dev) vs Claude (better reasoning). Recommend starting with OpenAI and abstracting the client so it's swappable.
2. **Archestra dependency** — If Archestra setup is complex, consider a lightweight custom orchestrator first and swap in Archestra later to avoid blocking Siddharth's agent work.
3. **Database migrations** — Use Alembic for PostgreSQL schema migrations from day one to avoid manual SQL drift across the team.
