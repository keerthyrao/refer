# Instructions for agents (IBM Bob and GitLab Duo)

IBM Bob and GitLab Duo both read this file automatically. Read it first, then the agent file for your task.

## What this is

The specification of the `iln` mainframe application, pulled from its code, and the agents that build and use it. **There are no scripts.** You find code with your own search and file-reading tools (retrieval, the "R" in RAG),
write Markdown from the templates, and check your work against the checklists. People review and approve in merge requests.

## Pick your agent

| Task | Agent file | Bob mode | Skill |
|---|---|---|---|
| Inventory the code, write and update the specification | `agents/specification.md` | iln Specification | `inventory-folder`, `specify-program`, `map-connections` |
| Pull design patterns, or design a story | `agents/architect.md` | iln Architect | |
| Change the application code for a story | `agents/coding.md` | iln Coding | |
| Write and run tests from the specs | `agents/testing.md` | iln Testing | |
| Check that every agent did its job | `agents/verification.md` | iln Verification | `verify-spec` |
| Run a Jira story end to end, starting with impact analysis | `agents/jira.md` | iln Jira | `impact-analysis` |

How to find code: `agents/retrieval.md`. Checks before you hand over: `agents/checklists.md`.
Overview of the folders: `governance/overview.md`. Templates: `agents/templates/`. Use them exactly; don't add, drop or rename their sections.

## Rules for every agent

1. **The spec is the source of truth.** Code is changed to match an approved spec, never the other way round.
   If code and spec disagree, report it; don't silently "fix" either. Build only on specs that are `approved` (for a story)
   or `reviewed` (for the Feature 1 test suite), read from `main` or a baseline tag, never on a draft.
2. **Specs first, then code.** Look in `specs/iln/` before searching the code. What is already specified and reviewed is your best context.
3. **Evidence or it didn't happen.** Every statement taken from the code cites `path:line` or `path:from-to` in `iln`,
   and you have opened and read those lines. Search hits, summaries and drafts from any tool or workflow are pointers, never evidence.
4. **Say where you looked.** "None found" is always followed by the folders and searches you used, so a reviewer can repeat them.
5. **Report unknowns, don't fill gaps.** Anything the code doesn't prove gets confidence `medium` or `low` and an open question.
6. **Small batches, counted.** One program, one folder part (at most 50 members) or one story per session.
   Never claim a list is complete without counting it against the folder (`agents/retrieval.md`, *Completeness*).
7. **Impact analysis before any change** for a story, committed first.
8. **One branch per feature, one Jira key per commit.** Work on `fe/<FEATURE-KEY>`. Every commit message starts
   `<JIRA-KEY> <agent>: <what changed>`. Never push to `main`; never merge, delete or rename branches.
9. **Stay in your lane.** Write only your own outputs (the Bob modes allow nothing else). Report problems in other agents' outputs.
10. **No scripts.** Don't write or run scripts, programs or terminal commands to do this work. Search, read and write files only.
11. **No secrets, no production.** No passwords, user ids, account or card numbers, customer data or production data in prompts,
    files or commits. Don't open or `@`-mention files excluded by `.bobignore` or the Duo context exclusions:
    a direct mention bypasses the exclusion. Development and test environments only.
12. **Record who made it.** Fill `generated_by` in the header of everything you write (agent, tool, mode, model, session),
    and `based_on` (spec ids, versions and the baseline tag) in anything built from a spec.
13. **Check before you hand over.** Go through your output's checklist in `agents/checklists.md`, fix everything,
    and list the checks you did in your handover and the merge request.
14. **Never approve.** Don't set `reviewed` or `approved` or record approvals yourself: the Mainframe SME and the Product Owner do that.

## Where outputs go

| Output | Folder |
|---|---|
| Inventory, program, data, module and application specs, connections, index | `specs/iln/` |
| Impact analysis and spec change for a story | `specs/iln/stories/<JIRA-KEY>/` |
| Patterns and architecture overview | `architecture/iln/` |
| Test cases written from the specs | `test-cases/iln/` |
| Story design, code-change record, test results, verification, status | `delivery/<JIRA-KEY>/` |
| Application code changes | the iln source folders (`cobol/`, `job/`, `procs/`, `ctl/`, `bms/` ...), Coding agent only |

When you finish, give the person the commit message to use (`agents/README.md`, *Commit messages*). People commit and push.
