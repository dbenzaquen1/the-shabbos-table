# the-shabbos-table

A web app (early stage). The project is built by a team of persona
sub-agents defined in `.claude/agents/`, managed by the harness engineer.
Full design: `docs/superpowers/specs/2026-09-03-persona-team-design.md`.

## The team and routing

Route work to the persona whose charter covers it:

| Work | Persona (agent) |
|---|---|
| Standards, conventions, drift audits, docs currency, merge-gate review | Ingrid (`harness-engineer`) |
| Feature/system design, boundaries, trade-offs | Marcus (`architect`) |
| Scope, priorities, sequencing, decision log | Priya (`project-manager`) |
| Breaking designs into tasks, backlog, stand-ups | Tomás (`sprint-master`) |
| UI design and implementation, accessibility | Yuki (`ux-engineer`) |
| APIs, data, server logic | Sam (`backend-engineer`) |
| Tests, edge cases, coverage verification | Farah (`qa-engineer`) |
| Threat modeling, security review, dependency/secret hygiene | Nadia (`security-engineer`) |
| CI/CD, environments, deployments | Rosa (`devops-engineer`) |

## Orchestration protocol

1. **Flow for a feature:** Priya scopes it → Marcus designs it → Tomás
   decomposes it → engineers (Yuki/Sam/Rosa) build it test-first → Farah
   verifies it → Nadia reviews security-sensitive surfaces → Ingrid's
   harness gate.
2. **The harness gate:** no PR is called ready until `harness-engineer`
   has reviewed the diff for convention violations, drift, and stale docs.
   This is in addition to the global post-PR checks.
3. **Relay rule:** sub-agents are isolated — anything not in a file or the
   dispatch prompt does not exist to them. Every dispatch prompt must name
   the upstream artifacts the persona reads first (see their file's "Read
   before starting" list) and each persona ends by writing its handoff
   artifact.
4. **Missing artifacts:** early in the project, some shared artifacts will
   not exist yet. A missing artifact reads as empty — note its absence and
   proceed; never invent its contents. The persona that owns it creates it
   on first need.

## Shared artifacts

- `docs/architecture/conventions.md` — Ingrid's conventions (binding on all)
- `docs/architecture/*-design.md` — Marcus's design docs
- `docs/team/decisions.md` — Priya's decision log
- `docs/team/backlog.md` — Tomás's task backlog
- `docs/team/standups/YYYY-MM-DD.md` — stand-up notes

## Stand-ups

At the start of a work session, if at least one PR has merged since the
last note in `docs/team/standups/` (or when the user asks), dispatch Tomás
to run a stand-up. If no stand-up note exists yet, the first one is due
after the first merged feature PR. Artifact-driven: he reads the decision log, backlog, and
recent merges, and requests a persona's status only when their area has
open questions.
