# Agents

Each agent is one Markdown file with four parts:

| Part | What it says |
|---|---|
| **Identity** | Who the agent is, what it is responsible for, what it reads |
| **What to do** | The steps, in order |
| **What not to do** | Hard limits. Breaking one is a failure, even if the output looks good |
| **Output format** | Exactly which files it writes, where, and from which template in `agents/templates/` |

Shared by all of them: `AGENTS.md` (the rules), `retrieval.md` (how to find code), `checklists.md` (the checks before handing over).

## Running an agent

There are no scripts. The agents run inside the assistants the team already uses, which do the retrieval themselves.

| In | How |
|---|---|
| **IBM Bob** | Bob reads `AGENTS.md` by itself. Pick the agent's mode (*iln Specification*, *iln Jira* ...; defined in `.bob/custom_modes.yaml`). Each mode reads its agent file and can edit only that agent's folders. Or type a skill as a slash command, for example `/specify-program`. |
| **GitLab Duo** (Agentic Chat) | Duo reads `AGENTS.md` and `.gitlab/duo/chat-rules.md` by itself. Ask, for example: *"Act as the Specification agent in `agents/specification.md`. Do step A3 for program `<PROGRAM>`."* Duo also uses the skills in `.agents/skills/` when a request matches them. |
| **GitLab Duo Code Review** | Reviews every merge request against `agents/checklists.md` (`.gitlab/duo/mr-review-instructions.yaml`). |

Start a new chat for each program, folder part or story step, so each session's context holds only what that step needs.

## The agents

| Agent | File | Job | Writes to |
|---|---|---|---|
| Specification | `specification.md` | Inventories the code, maps the connections, and writes the specification: program, then module, then application | `specs/` |
| Architect | `architect.md` | Pulls the design patterns and architecture out of the code, and says which patterns a story must follow | `architecture/`, `delivery/<JIRA-KEY>/design.md` |
| Coding | `coding.md` | Changes the code from the approved spec plus the architecture | iln source (feature branch), `delivery/<JIRA-KEY>/code-change.md` |
| Testing | `testing.md` | Picks and writes smoke, integration and regression tests from the spec, and records results | `test-cases/`, `delivery/<JIRA-KEY>/test-results.md` |
| Verification | `verification.md` | Checks that every agent did its job, with evidence | `delivery/<JIRA-KEY>/verification.md` |
| Jira | `jira.md` | Runs one Jira story end to end: impact analysis first, then the other agents in order | `specs/iln/stories/<JIRA-KEY>/`, `delivery/<JIRA-KEY>/status.md` |

## Two ways the agents are used

**1. Building the baseline (Feature 1).** The Specification agent inventories the code folder by folder, maps the connections,
then specifies every program, then the modules and the application. The Architect agent pulls the patterns. The Verification agent
checks each batch. Progress is tracked in `specs/iln/index.md`: Feature 1 is done when every program is specified and reviewed.

**2. Delivering a Jira story (after the baseline).** The Jira agent runs the chain:

```
Jira story
   │
   ▼
1. Jira agent ─────────── impact analysis ──────────► specs/iln/stories/<KEY>/impact.md
   ▼
2. Specification agent ── update the spec ──────────► specs/... (new version) + stories/<KEY>/spec-change.md
   ▼
   ■ GATE: Product Owner approves the spec change (merge request approval)
   ▼
3. Architect agent ────── patterns for this story ──► delivery/<KEY>/design.md
   ▼
4. Coding agent ───────── code change ──────────────► iln source (feature branch) + delivery/<KEY>/code-change.md
   ▼
5. Testing agent ──────── tests and results ────────► test-cases/... + delivery/<KEY>/test-results.md
   ▼
6. Verification agent ─── did everyone do their job? ► delivery/<KEY>/verification.md
   ▼
   ■ GATE: reviewers approve the merge requests
```

Every step is a separate commit. Nothing moves to the next step until the previous step's output exists.

## What replaced the scripts

| Before (tooling) | Now |
|---|---|
| Inventory scanner | Specification agent, step A1: folder by folder, counted (`templates/inventory.md`) |
| Code index and search | Bob's workspace search and context mentions; Duo's file search and semantic code search (`retrieval.md`) |
| Dependency graph and impact tool | Connections specs, followed both ways and confirmed by code search (`jira.md`) |
| Behaviour worklist and coverage | The *Code coverage* section of every program spec, and keyword searches (`specification.md`, A3) |
| Spec checker | `checklists.md`, applied by the agent, the Verification agent, Duo Code Review and the reviewers |
| Spec store (release, fetch, verify) | Merge to protected `main` with approvals, then a Git tag; downstream outputs record `based_on` |
| Run log | `generated_by` in every output header, commit messages, merge requests, GitLab audit events |
| Secret check and commit hook | Duo context exclusions, `.bobignore`, secret push protection (GitLab Ultimate), the review checklists |

## Commit messages

The agent proposes the message; the person commits.

```
<JIRA-KEY> <agent>: <what changed>

<why, in one or two lines>
Spec: <spec id> v<version>          (when a spec is involved)
Generated by: <Bob or Duo>, <mode>, session <id>
```

Examples of the first line:

```
<JIRA-KEY> specification: inventory cobol A-C (48 members, 3 unknowns)
<JIRA-KEY> jira: impact analysis, 6 programs and 2 copybooks affected
<JIRA-KEY> specification: <PROGRAM> spec v1.3.0, rule BR-<PROGRAM>-004 changed
<JIRA-KEY> coding: <PROGRAM> applies rule BR-<PROGRAM>-004
<JIRA-KEY> testing: 12 regression cases added, 12 passed
<JIRA-KEY> verification: all checks passed
```

For the Feature 1 baseline (no story yet), use the Jira key of the Feature 1 story you are working on.

## Confidence levels (used by every agent)

| Level | Meaning |
|---|---|
| `confirmed` | A Mainframe SME or the Product Owner has confirmed it |
| `high` | The code alone proves it, and the evidence is cited |
| `medium` | Very likely, but depends on something not fully visible (a value set at run time, a program not found) |
| `low` | An interpretation. Must have an open question |
