# iln-specs: Feature 1

Feature 1 understands the `iln` code and extracts its functional specification, using the RAG approach in IBM Bob or GitLab Duo.
There are no scripts: one agent is one Markdown file, and Bob or Duo does the retrieval and the writing.

## Files

| File | For |
|---|---|
| `AGENTS.md` | The rules every agent follows; Bob and Duo read it automatically |
| `feature-1/specification.md` | The Feature 1 agent, in the standard sections: agent identity, input contract, probing question rules with the completion trigger, output rules with coverage, format and constraints |
| `.bob/custom_modes.yaml` | The Bob mode *iln Specification*: it can write only in `specs/functional/` and can't run commands |
| `.bobignore` | Files Bob must never read; the same list goes into the Duo context exclusions |
| `.gitlab/CODEOWNERS` | Specs need the Mainframe SME's and Product Owner's approval |
| `iln-specs.code-workspace` | Opens this repository and the `iln` code together in Bob |
| `specs/functional/` | The output only: index, inventory, connections, program, data, module and application specs, saved impact answers |

## Set-up (Story 1.1)

Tick each item when it is done; together they meet Story 1.1's acceptance criteria.

- [ ] Clone `iln-specs` and `iln` side by side, for example `C:\work\iln-specs` and `C:\work\iln` (AC 2)
- [ ] `iln-specs` in GitLab: protect `main` so only merge requests change it (AC 3, 6)
- [ ] Turn on *Require approval from code owners*, and put real names in `.gitlab/CODEOWNERS` (AC 6)
- [ ] Admin: GitLab Duo on for `iln-specs` and `iln` (AC 1, 2)
- [ ] Admin: paste the `.bobignore` list into Settings > General > GitLab Duo > *GitLab Duo context exclusions* (AC 9)
- [ ] Admin: secret push protection on, if you have GitLab Ultimate (AC 9)
- [ ] Admin: semantic code search on for `iln`, if available (AC 2)
- [ ] Admin: confirm GitLab audit events are kept for both projects (AC 8)
- [ ] Bob: **File > Open Workspace from File** > `iln-specs.code-workspace`; the mode *iln Specification* appears (AC 1, 6)
- [ ] Fill in the record below (AC 5, 7, 11)

### Story 1.1 record

| Item | Value | AC |
|---|---|---|
| `iln-specs` project | TBD (GitLab path) | 3 |
| `iln` project, read access for the people running the agent | TBD | 2 |
| Who runs the agent (named people; or a Duo service account) | TBD | 5 |
| Roles: who can push branches, who merges, who approves | TBD | 6 |
| Approved models: Duo (chosen by the admin) and Bob | TBD | 7 |
| Production access | None: no production identities, datasets or data | 10 |
| Non-production integration points (test LPAR, CICS region, test data) | TBD, with DevOps | 11 |

Where the rest of Story 1.1 lives: the agent and its rules are in Git and changed only through approved merge requests (AC 4, 13);
every file the agent writes records the tool, model and session in its header, and the source commit it read (AC 8, 12, 14);
every commit names the Jira story (AC 14).

## Running Feature 1

Agree the pilot scope with the lead first; the first request gives it to the agent. Then work on the branch `fe/<FEATURE-KEY>`, in this order.
Every request names the Jira key and the `iln` commit you are reading (`git -C <your iln folder> log -1 --format=%H`).
If anything is vague or missing, the agent asks all its questions in one reply before it starts: answer them in one reply too.

| Order | Request to the agent | Gives |
|---|---|---|
| 1 | `Inventory folder <folder>, members <from> to <to>. Jira <JIRA-KEY>. iln commit <id>.` The first time, add `Pilot scope: <folders and ranges>.` Suggested folder order: `ctl`, `job`, `procs`, `cobol` (and its program subfolders), `bms`, `ezt`, `assembler`, `test`; at most 50 members a part | Inventory parts, connections, the program list in `index.md` |
| 2 | `Specify program <PROGRAM>. Jira <JIRA-KEY>. iln commit <id>.` (repeat per program) | Program and data specs |
| 3 | `Specify module <module> (transactions or jobs ...). Jira <JIRA-KEY>. iln commit <id>.` | Module specs |
| 4 | `Specify the application. Jira <JIRA-KEY>. iln commit <id>.` | The application spec |
| Any time | `What is affected by changing <kind> <NAME>?` (add `Save it. Jira <JIRA-KEY>. iln commit <id>.` to keep it) | An impact answer |

The build folders (`application-conf`, `pipeline-properties`, `ci-cd`, `change-control`, `simpleLogger`) aren't inventoried; the agent reads
`application-conf` only for build facts, such as which programs use SQL, CICS or Hogan MQ.

After each request: read the completion message, commit with the message it gives, and open a merge request. The SME reviews
(and sets `status: reviewed`); the Product Owner approves. Progress is in `specs/functional/index.md`.

## Feature 1 stories: what meets them and how to show it

| Story | Acceptance criteria met by | Definition of Done: how to show it |
|---|---|---|
| 1.1 Environment (AC 1 to 14) | The set-up checklist and record above | Take one reviewed program spec: show its merge request (stored), its Git history (versioned), then ask the agent for a module spec or an impact answer that reads it (retrieved and processed), and show the header and commit (traced) |
| 1.2 Specification standard (AC 15 to 26) | The fixed formats and the completion trigger in `feature-1/specification.md` | The Product Owner picks one small capability; its program, data and module specs are written; the Product Owner explains it from the module spec without COBOL |
| 1.3 Inventory (AC 27 to 36) | Task A | `index.md` shows every folder in scope inventoried, with counts that add up and unknowns owned |
| 1.4 Dependency graph (AC 37 to 46) | The connections spec, and task E | Five saved impact answers (a program, a copybook, a VSAM file, a transaction, a job), checked by the SME |
| 1.5 Business behaviour (AC 47 to 54) | Task B | A program spec the SME has reviewed and the Product Owner has approved, read without COBOL |

Feature 1 is done when every folder in the pilot scope is inventoried, every program in it is specified and reviewed, and its
module specs and the application spec exist. Version history is Git's history: specs carry no version numbers or change logs.
