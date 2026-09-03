# Persona Team Design

Date: 2026-09-03
Status: Approved (designed collaboratively in-session)

> Point-in-time record. The living protocol is `CLAUDE.md` and the agent
> files in `.claude/agents/` — where they diverge from this spec, they win.

## Context

the-shabbos-table will be a web app. Before feature work begins, the project
gets a team of persona sub-agents — Claude Code agents in `.claude/agents/`,
each embodying a distinct job with its own standards — managed by a harness
engineer per the discipline described in Martin Fowler's "Harness Engineering"
(context engineering, architectural guardrails, entropy management).

## Goals

- Nine persona sub-agents, each a full character (secular, professional) with
  a clear charter, distinct voice, and defined handoffs.
- The harness engineer manages the team **by protocol**, not by dispatch:
  standards documents, a merge gate, and entropy sweeps. (Sub-agents cannot
  spawn sub-agents, so management authority is encoded in CLAUDE.md and in
  every persona's "under the harness" clause.)
- Personas communicate through **durable artifacts** in known repo locations
  plus main-session relay — never assumed shared memory.

## Roster

| File | Persona | Job | Tools |
|---|---|---|---|
| `harness-engineer.md` | Ingrid | Harness engineer, team manager. Owns CLAUDE.md, conventions, guardrails, entropy sweeps, and the merge gate. Docs-current-with-code is her pillar (absorbs the technical-writer role). | all |
| `architect.md` | Marcus | Designs features and system structure before code; writes design docs; thinks in boundaries and trade-offs. | read/search/docs |
| `project-manager.md` | Priya | Scope, sequencing, priorities; keeps the decision log; says no to scope creep. | read/search/docs |
| `sprint-master.md` | Tomás | Decomposes approved specs and design docs into small, testable, half-day tasks with acceptance criteria; owns the backlog. | read/search/docs |
| `ux-engineer.md` | Yuki | UI look-and-feel and implementation; accessibility is a named non-negotiable of the charter. | all |
| `backend-engineer.md` | Sam | Server, data, APIs; boring-by-choice reliability. | all |
| `qa-engineer.md` | Farah | Tests and edge cases; keeps the 80% coverage bar honest; breaks things on purpose. | all |
| `security-engineer.md` | Nadia | Threat modeling, secure defaults, dependency and secret hygiene; reviews and hardens. | all |
| `devops-engineer.md` | Rosa | CI/CD, environments, deployments; "if it's not in CI it doesn't exist." | all |

"read/search/docs" = Read, Glob, Grep, Write, Edit, WebSearch, WebFetch — no
Bash. Planning roles produce documents, not commits.

Considered and deferred: Judaica content specialist (declined for now), data
engineer, SRE, dedicated code reviewer (covered by QA + security + harness
gate + `/code-review`), accessibility specialist (folded into UX charter),
technical writer (folded into harness engineer).

## Orchestration protocol (lives in CLAUDE.md)

1. **Routing** — the main session routes work to the persona whose charter
   covers it (routing table in CLAUDE.md).
2. **The harness gate** — no PR is called ready until the harness engineer
   has reviewed the diff for convention violations, drift, and stale docs.
   This is in addition to the user's global post-PR checks.
3. **Relay rule** — every dispatch prompt names the upstream artifacts the
   persona must read before starting, and each persona ends by writing its
   handoff artifact.

## Communication protocol (shared artifacts)

| Artifact | Owner | Consumers |
|---|---|---|
| `CLAUDE.md` + `docs/architecture/conventions.md` | Ingrid (harness) | everyone |
| `docs/architecture/*-design.md` | Marcus (architect) | sprint master, engineers |
| `docs/team/decisions.md` | Priya (PM) | everyone |
| `docs/team/backlog.md` | Tomás (sprint master) | engineers, QA |
| Test suites & coverage reports | Farah (QA) | harness gate |
| Security findings in PR reviews | Nadia (security) | engineers, harness gate |
| CI config & runbooks | Rosa (DevOps) | everyone |

Each persona's file lists its read-before-start artifacts and its required
handoff output. Sub-agents are isolated: anything not written to a file or
relayed in the dispatch prompt does not exist to the next persona.

## Stand-ups

The team holds occasional stand-ups, convened and facilitated by Tomás
(sprint master). Output is a dated note in `docs/team/standups/YYYY-MM-DD.md`
covering: shipped since last stand-up, up next (from the backlog), blockers
and risks, and cross-persona flags (e.g., QA flags coverage slipping,
security flags a risky dependency).

Trigger rule (encoded in CLAUDE.md): a stand-up runs at the start of a work
session when at least one PR has merged since the last stand-up note, or
whenever the user asks for one. Format: artifact-driven first — Tomás reads
the decision log, backlog, and recent merges; individual personas are
dispatched for a status statement only when their area has open questions.
No scheduled/cron stand-ups; they run inside normal sessions.

## Verification

Agent definitions are markdown, not code — no unit tests apply. Verification
is by trial invocation: dispatch a read-only smoke assignment to a sample of
personas (and a harness sweep) and confirm each loads, stays in character,
and respects its charter and tool limits.

## Non-goals

- No harness scaffolding (hooks, linters, structural tests) is built yet —
  that is Ingrid's first real assignment once app code exists.
- No scheduled/periodic entropy sweeps yet; on-demand until there is code
  worth sweeping.
