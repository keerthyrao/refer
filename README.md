# SDD Platform for ILN

This repository holds the approved specifications of the ILN mainframe programs and the instructions of the agent that writes
them. The specifications on `main` are the authoritative ones: a specification gets there only through a merge request
approved by its code owners.

## Layout

| Folder | What it holds | Who writes it |
|---|---|---|
| `codebase/` | A link (submodule) to `ILN/shaw/iln`, the ILN mainframe source. Read-only | Nobody in this project |
| `agentic-pipeline/` | The agent's instructions. `specification.md` is the system prompt of the iln Specification Agent | The lead, by merge request |
| `specs/programs/<PROGRAM>/` | `functional-requirements.md` and `non-functional-requirements.md` for each program | The agent, on a branch; merged after approval |
| `specs/impact-analysis/` | Saved answers to "What depends on …?", one file per component | The agent, on a branch; merged after approval |
| `specs/modules/`, `specs/applications/`, `specs/design-specifications/` | Empty in the MVP | Later phases |
| `tests/` | Empty in the MVP | Later phases |

Full GitLab paths:
- `ILN/SDD`: `mandtbank/mtb/enterprise-operations/consumer-loan-servicing/iln/sdd`
- `ILN/shaw/iln`: `mandtbank/mtb/enterprise-operations/consumer-loan-servicing/iln/shaw/iln`

## The agent

One custom agent on the GitLab Duo Agent Platform, the **iln Specification Agent**, used from GitLab Duo Chat. Its system
prompt is `agentic-pipeline/specification.md` from `main`. It does three jobs: it specifies one program from the code, it
answers what depends on a component, and it turns a new requirement into a change that the Product Owner approves in a merge
request.

## Version and lifecycle

Each specification carries `version` and `status` in its header:
- `version: 1` for a program's first specification; a change takes the version on `main` plus 1.
- `status`: the agent sets `draft`, `partial` (some checks failed) or, for a change, `proposed`. The approver changes it to
  `approved` in the merge request, as its last commit before merging. Git history and the `baseline/` tags keep every version.
- Only specifications on `main` with `status: approved` are the development baseline. Every agent in `agentic-pipeline/`,
  today and in later phases, reads specifications only from `main`, and the change job refuses a specification that is not
  approved.

## Identities

- **The agent** is the custom agent named above. GitLab keeps each saved version of its system prompt as a fixed version.
- **The person** who opens the chat is the identity that acts. The agent reads and commits with that person's GitLab account
  and permissions, so every commit and merge request shows who ran it.
- The MVP uses no service account: the agent runs only in chat. Flows, which run under a service account, are a later phase.

## Permissions

| Role | `ILN/shaw/iln` | `ILN/SDD` |
|---|---|---|
| Person using the agent | Reporter: read the code | Developer: push `spec/`, `impact/` and `change/` branches, open merge requests |
| Mainframe SME | Reporter | Developer, and code owner of `specs/` |
| Product Owner | None needed | Developer, and code owner of `specs/`: approves merge requests |
| Lead | Reporter | Maintainer: merges, creates `baseline/` tags, code owner of `agentic-pipeline/` and `codebase/` |

- `main` is protected: nobody pushes to it directly, and a merge needs code-owner approval (`.gitlab/CODEOWNERS`).
- The agent's instructions forbid it to write in `codebase/`, `agentic-pipeline/` or `tests/`, to commit to `main` or to
  `ILN/shaw/iln`, and to merge.

## Models

The GitLab Duo administrator for the `mandtbank` group chooses the model that GitLab Duo Chat uses. Use only that model, and
run the agent from the GitLab web UI so that every run uses the same one. Each specification records its model in its
`generated_by` line.

## Logging and traceability

Every run can be traced through:
- the header of each file it writes: `source`, `members_read`, `based_on` (the specification version it started from) and
  `generated_by` (agent, instructions version, model, date and time)
- the commit message, which names the agent, instructions version, model, and date and time; the commit author is the person
- the merge request: review comments, the validation result for changes, and the approval
- the `baseline/<name>` tag on each approved change
- the person's GitLab Duo Chat history, which holds the chat that produced the commit

## Secrets and restricted information

- The agent's instructions forbid it to open credential or secret files, and to write passwords, user ids, account or card
  numbers, customer or production data.
- A Maintainer of each project sets GitLab Duo context exclusions for secret files, for example `*.env`, `*password*`,
  `*secret*`, `*credential*` and key stores, so they are never sent to the model.

## No production access

The agent reaches only the two GitLab repositories above. It has no connection to the mainframe, test or production systems,
databases or datasets; it runs no commands, jobs or pipelines; and it changes no code. Do not give it tools that run commands.

## Integration points

All are non-production.

| Integration point | Direction | Used for |
|---|---|---|
| GitLab project `ILN/shaw/iln`, branch `main` | Read | COBOL programs, copybooks, JCL jobs and PROCs, CICS definitions, build settings |
| GitLab project `ILN/SDD` | Read, and write through branches and merge requests | Specifications, impact analyses, agent instructions |
| GitLab Duo Agent Platform, with the model chosen by the administrator | Runs inside GitLab | Runs the agent |

Not connected in the MVP: the mainframe, test systems, the job scheduler, databases, file transfer and Jira.

## Changing the agent's instructions

1. Change `agentic-pipeline/specification.md` on a branch, and raise the version line at its top.
2. The lead approves the merge request.
3. After the merge, paste the file into the agent's system prompt in GitLab. GitLab saves it as a new agent version.
