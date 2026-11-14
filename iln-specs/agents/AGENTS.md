# Instructions for agents (IBM Bob and GitLab Duo)

Bob and Duo read this file automatically. It holds the rules every agent follows. Each agent is one Markdown file.

## Agents

| Feature | Agent | File | Status |
|---|---|---|---|
| 1. Understand the code and extract the spec | Specification | `feature-1/specification.md` | Active |
| 2. Put the spec together and plan solutions | Jira and impact analysis | (to come) | Not active |
| 3. Design, build and verify | Design, architecture, coding, testing | (to come) | Not active |

Only Feature 1 is active. For any other request, say that it is not available yet.

To run the Specification agent: in Bob, choose the mode *iln Specification*. In Duo, start with
*"Act as the agent in `feature-1/specification.md`"*, then the request in the form its input contract shows.

## Rules for every agent

1. **Check the input first.** Validate every field of the agent's input contract. If anything is vague or missing, ask all clarifying
   questions in one grouped reply, and start only when the agent's completion trigger passes.
2. **Evidence or it didn't happen.** Every statement taken from the code cites `path:line` or `path:from-to` inside `iln`, from lines you opened.
3. **Unknowns are questions, not facts.** What the code doesn't prove gets confidence `medium` or `low` and an open question.
4. **Small batches.** One folder part (at most 50 members), one program or one module per session.
5. **Verify before output.** Run the agent's coverage checks and fix what you can; mark anything you can't fix `partial` and say what failed.
6. **`iln` is read only.** Write only where your agent file says (`specs/functional/` for Feature 1). If `iln` has its own `AGENTS.md`
   (the Bob-made map of the repository), use it as a map of folders and build settings, never as evidence.
7. **No scripts.** Don't write or run scripts, programs or terminal commands. Search, read and write files only.
8. **No secrets, no production.** No passwords, user ids, account or card numbers, customer or production data anywhere.
   Don't open or `@`-mention files excluded by `.bobignore` or the Duo context exclusions.
9. **Never approve.** Don't set `reviewed`; the Mainframe SME does that when approving the merge request.
10. **People commit.** End with the completion message and the commit message `<JIRA-KEY> <agent>: <what changed>`.
    Work on the feature branch `fe/<FEATURE-KEY>`; never on `main`.
