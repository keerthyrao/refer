# Governance: Feature 1 with Bob and GitLab Duo, no scripts

Feature 1 builds the specification baseline of iln using the **RAG approach inside the tools the team already has**:

- **Retrieve:** Bob and Duo search and read `iln` themselves (exact search, context mentions, Duo semantic code search).
  The specs they write become the first place both tools look next time.
- **Augment and generate:** each agent works from its instructions (`agents/`), a template (`agents/templates/`) and the code it retrieved.
- **Ground:** every statement cites lines the agent opened; inventory parts and *Code coverage* are counted, so nothing is silently missed.
- **Govern:** checklists, Duo Code Review, Bob mode limits, CODEOWNERS approvals, protected `main` and baseline tags.

There is nothing to install or run: no Python, no pipeline, no commit hook.

## Feature 1 at a glance

| Story | Definition of Done | How it is met | People needed |
|---|---|---|---|
| [1.1 Environment](story-1.1-environment.md) | A spec stored, versioned, retrieved by an agent, processed and traced | GitLab settings, Bob and Duo configuration in the repository, `generated_by` / `based_on` in every output, baseline tags | GitLab admin, DevOps, lead |
| [1.2 Spec standard](story-1.2-spec-standard.md) | A real capability fully described without COBOL | Templates + `agents/checklists.md`, applied by agents, Duo Code Review and reviewers | Product Owner, SME, lead |
| [1.3 Inventory](story-1.3-inventory.md) | The pilot source estate in a machine-consumable inventory | Folder-by-folder inventory parts, each counted against the folder | SME (scope, review) |
| [1.4 Dependency graph](story-1.4-dependency-graph.md) | Impact of changing a program, copybook, VSAM file, transaction, job | Connections tables + impact analysis confirmed by code search, one per kind | SME (review) |
| [1.5 Business behaviour](story-1.5-business-behaviour.md) | PO and SME confirm behaviour from a readable spec | Program and module specs with evidence, confidence and *Code coverage* | Product Owner, SME |

## Set-up, in order

1. Decide where the folders live: inside `iln` or a separate repository ([where-it-lives.md](where-it-lives.md)).
2. Do the [Story 1.1 runbook](story-1.1-environment.md#set-up-runbook): protected `main`, CODEOWNERS, Duo settings, context exclusions,
   secret push protection, Duo Code Review, Bob modes.
3. Fill in [environment.yaml](environment.yaml) with the admin and DevOps.
4. Agree the pilot scope with the lead and write it in `specs/iln/index.md` (*Inventory coverage*).
5. Start: in Bob, mode *iln Specification*, `/inventory-folder` for the first folder part; or the same request in Duo Agentic Chat.

## Moving from the earlier package (the one with `tooling/`)

In the specification repository, on the feature branch, with the new package unzipped somewhere else (for example `C:\Downloads\iln-specs`).
These are one-off Git and copy commands typed by a person, not scripts in the environment.

```powershell
cd C:\work\<spec-repository>
git switch fe/<FEATURE-KEY>
git pull

# 1. Remove the old commit hook first: it runs a script that is about to be deleted, and would block every commit
Remove-Item .git\hooks\pre-commit -ErrorAction SilentlyContinue

# 2. Keep your edited environment register; rename tests to test-cases and the island folders to iln; remove the tooling
New-Item -ItemType Directory -Force governance | Out-Null
git mv tooling/config/environment.yaml governance/environment.yaml
git mv tests test-cases
git mv specs/island specs/iln                 # the folders are now named after iln
git mv architecture/island architecture/iln
git mv test-cases/island test-cases/iln
git rm -r -q tooling
Remove-Item -Recurse -Force tooling -ErrorAction SilentlyContinue      # leftovers from practice runs

# 3. Copy the new package in, except the environment register
robocopy C:\Downloads\iln-specs . /E /XF environment.yaml /XD .git   # exit code 1 means files were copied
git add -A
git status
```

Then, by hand:

- In `governance/environment.yaml` (yours), copy across the sections marked **CHANGED (no scripts)** from the package's copy.
  If yours has `used_for: TBD (later features: build, test, deploy)`, put quotes around the value: the colon inside makes the file invalid YAML.
- In `.gitlab/CODEOWNERS` (yours), replace the `[Tooling]` section, and `/tests/` with `/test-cases/`, using [CODEOWNERS.template](CODEOWNERS.template).
- Your old root `README.md` still describes the tooling: replace its text with [overview.md](overview.md).
- Check `git status` lists no `.py` file and nothing under `tooling/`.
- Commit: `<JIRA-KEY> specification: move Feature 1 to Bob and Duo, no scripts`.

If you move into `iln` instead, follow [where-it-lives.md](where-it-lives.md), option A; the old spec repository can be archived.

## Files here

| File | What |
|---|---|
| `overview.md` | What the folders are, how the work runs, conventions |
| `story-1.1-environment.md` ... `story-1.5-business-behaviour.md` | How each story is met, its acceptance criteria, and what to show in the review |
| `where-it-lives.md` | Inside `iln` or a separate repository: trade-offs and steps |
| `environment.yaml` | Environment register (Story 1.1) |
| `duo-context-exclusions.txt` | Paste into the Duo settings (Story 1.1, AC 9); `.bobignore` holds the same list |
| `CODEOWNERS.template` | Who approves what (Story 1.1, AC 6) |

## Honest limits

- **Completeness is shown by counting, not by a program.** Each inventory part and each program spec is counted against the code
  and checked by the Verification agent and the SME. Small batches keep the counts trustworthy.
- **Effort.** Agents reading 12 million lines member by member takes many sessions. Measure the first module and plan the rest from it.
- **Tool features vary by version.** Duo semantic code search and the Knowledge Graph are beta; Bob for Z is a separate package.
  The approach works with plain search and file reading; the extras make retrieval faster, not more trustworthy.
- **Duo Code Review instructions are guidance,** not a hard gate. The required approvals (CODEOWNERS) are the gate.
