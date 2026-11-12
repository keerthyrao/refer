# Story 1.1: Establish the specification-driven agentic development environment

**Summary:** Configure GitLab Duo Agent Platform, repositories, identities and controls for SDD.
**Definition of Done:** A sample specification can be stored, versioned, retrieved by an agent, processed, and traced through an agent execution.

## What the environment is

No scripts: every control is a GitLab setting, a Bob or Duo setting, or a file in this repository that people review.

| Layer | Where | What it does |
|---|---|---|
| Controlled specification folders | `specs/` (in `iln` or a spec repository: [where-it-lives.md](where-it-lives.md)) | Specifications only, version controlled. No application code. |
| Agent instructions | `AGENTS.md`, `agents/`, `.gitlab/duo/chat-rules.md`, `.bob/`, `.agents/skills/` | How every agent behaves. Bob and Duo read `AGENTS.md` by themselves. Reviewed like code. |
| Edit limits | `.bob/custom_modes.yaml` | Each Bob mode can edit only its agent's folders, and has no terminal |
| Approval control | CODEOWNERS + protected `main` | Spec changes need Mainframe SME and Product Owner approval. Agents can't merge. |
| Approved baselines | Git tags `spec/iln/v<n>.<n>`, created in GitLab after a merge | Downstream agents build only on approved specs, and record which baseline they used |
| Review checks | `.gitlab/duo/mr-review-instructions.yaml`, `.gitlab/merge_request_templates/`, `agents/checklists.md` | Duo Code Review and the reviewers check every merge request against the standard |
| Traceability | `generated_by` / `based_on` in every output, commit messages, merge requests, GitLab audit events | Who or what made each file, from which spec version, in which session |
| Secret control | Duo context exclusions, `.bobignore`, secret push protection, review checklists | Keeps restricted files out of the models' context and secrets out of the repository |
| Environment register | `governance/environment.yaml` | Repositories, identities, permissions, models and non-production integration points on record |

## Set-up runbook

Do these once, with the GitLab admin and the DevOps SME. All are settings in GitLab, Bob or the repository.

1. **Branch.** Work on the Feature 1 branch `fe/<FEATURE-KEY>`; push and raise the merge request per feature.
2. **Protect `main`.** Settings > Repository > Protected branches: Maintainers can merge, no one can push.
   Settings > Merge requests: turn on *Require approval from code owners*; put the CODEOWNERS sections from
   [CODEOWNERS.template](CODEOWNERS.template) in place, with real users.
3. **GitLab Duo.** Confirm Duo is on for the group and the project (Settings > General > GitLab Duo). Ask the admin whether
   **semantic code search** (beta) is on and which models are selected; record both in `environment.yaml`.
4. **Context exclusions.** Paste [duo-context-exclusions.txt](duo-context-exclusions.txt) into Settings > General > GitLab Duo >
   *GitLab Duo context exclusions*. `.bobignore` holds the same list for Bob.
5. **Secret push protection** (GitLab Ultimate): Secure > Security configuration > *Secret push protection*. It blocks pushes
   that contain keys and tokens, without any pipeline. Without Ultimate, the review checklists are the control.
6. **Duo Code Review.** Add Duo as a reviewer on merge requests (or turn on automatic reviews); it uses `.gitlab/duo/mr-review-instructions.yaml`.
7. **Bob.** Open the repository in Bob; check the six *iln* modes appear and `AGENTS.md` is read
   (setting `bob-code.useAgentRules`, on by default). Start a new chat after any change to these files.
8. **Identities and access.** Agents run as the person using Bob or Duo (or a service account for Duo flows, if the admin sets one up):
   Developer here (push branches, not merge), Reporter on the iln source if it is a separate repository. Record them in `environment.yaml`.
9. **Audit.** Ask the admin to confirm GitLab audit events are kept for the project (merges, approvals, setting changes, push-protection skips).

## Definition of Done: show it in the review

Use the **first real program spec** the Specification agent writes (Story 1.2 / 1.5), not made-up data.

| Step | What to show | Where |
|---|---|---|
| **Store** | The spec in its merge request: file in `specs/iln/programs/`, Duo Code Review comments, SME and Product Owner approvals | GitLab merge request |
| **Version** | Header `version` and `status`, the change log, the merge to protected `main`, then the tag `spec/iln/v0.1` | File header; Code > Tags |
| **Retrieve by an agent** | In Bob (*iln Testing* mode) or Duo Chat: *"Write test cases for PGM-&lt;PROGRAM&gt; from baseline spec/iln/v0.1."* Then ask the same for a spec that is still `draft`: the agent must refuse (`AGENTS.md` rule 1, checklist E) | Bob or Duo chat |
| **Process** | The test cases it wrote: `test-cases/iln/programs/<PROGRAM>.md`, each case naming the rule it verifies | The new file |
| **Trace** | `based_on` (spec, version, tag) and `generated_by` (tool, mode, model, session) in its header; the commit `<JIRA-KEY> testing: ...`; the merge request; the audit events | File header, commit list, Secure > Audit events |

## Acceptance criteria

| AC | Criterion | How it is met | Evidence / owner |
|---|---|---|---|
| 1 | GitLab Duo Agent Platform configured for the MVP | Duo on for group and project; `AGENTS.md`, chat rules, Code Review instructions and skills in the repository | Admin confirmation, `environment.yaml` |
| 2 | Selected mainframe repositories accessible | Read access to `iln` (or the specs live inside it); semantic code search indexed if enabled | `environment.yaml` `source_repositories` |
| 3 | Controlled repository for authoritative specs | `specs/` holds only specifications; protected `main`; CODEOWNERS | Project URL, protected branch settings |
| 4 | Specification artifacts version controlled | Git, a version and change log in every spec, baseline tags | Git history, Code > Tags |
| 5 | Agent/service identities established | Agents run as the named person (Bob, Duo) or a Duo service account; recorded in `generated_by` | `environment.yaml` `identities` |
| 6 | Repository read/write permissions defined | Roles, protected `main`, CODEOWNERS, Bob mode edit limits | `environment.yaml` `permissions`, CODEOWNERS, `.bob/custom_modes.yaml` |
| 7 | Approved models accessible | Models selected by the admin for Duo; Bob's models as provided by IBM under the company's agreement; recorded per output | `environment.yaml` `models`, `generated_by.model` |
| 8 | Agent execution logging enabled | `generated_by.session` in every output; commits and merge requests; GitLab audit events; Duo and Bob session history | Admin (audit events), output headers |
| 9 | Secrets and restricted information excluded from model context | Duo context exclusions, `.bobignore`, never `@`-mention restricted files (`AGENTS.md` rule 11), secret push protection | Settings screenshots, `duo-context-exclusions.txt` |
| 10 | Production access prohibited | No production identities or data; Bob modes have no terminal; rule 11 | `environment.yaml` `production`, `AGENTS.md` |
| 11 | Non-production integration points documented | Register section, filled in with DevOps / Mainframe SME | `environment.yaml` |
| 12 | Generated artifacts identify the spec version they came from | `based_on` (spec, version, baseline tag) in every output built from a spec; `source.commit` in every spec | Output headers |
| 13 | Agent instructions version controlled | `AGENTS.md`, `agents/`, `.bob/`, `.agents/`, `.gitlab/duo/` in Git, lead approval through CODEOWNERS | Git history |
| 14 | Agent actions traceable to an execution/session | `generated_by` (tool, mode, model, session); commit messages carry the Jira key and agent; merge requests | Output headers, commit list |
| DoD | Store, version, retrieve, process, trace | The review walk-through above | Review recording, the merge request |

## What still needs other people

- **GitLab admin:** Duo settings and models, semantic code search, context exclusions, secret push protection, audit events, service account.
- **DevOps / Mainframe SME:** non-production integration points (AC 11).
- **Lead:** real names in CODEOWNERS; where the folders live ([where-it-lives.md](where-it-lives.md)).
