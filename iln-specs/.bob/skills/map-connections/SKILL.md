---
name: map-connections
description: "Record how iln programs, transactions, jobs, copybooks, files and other systems connect, with evidence, in the connections spec (Story 1.4)."
user-invocable: true
---

# Map how iln components connect (Story 1.4)

You are the Specification agent. You need: what to map (a program, job, transaction, copybook, file, or a whole module) and the Jira key.

1. Read `AGENTS.md`, `agents/retrieval.md` and `agents/templates/connections.md`.
2. Start from the specs: the program specs' *Connections* sections and the inventory.
3. Confirm and complete each relationship with the search recipes in `agents/retrieval.md`, in both directions:
   who starts or calls it, what it calls, which copybooks, files, datasets and tables it uses, which jobs run before and after it,
   which other systems it exchanges data with. Open each hit and cite the line.
4. Add the rows to the right sections of the connections spec, each with evidence and a confidence. Calls through a variable
   and job order inferred from shared datasets are `medium` at most. Anything you can't resolve goes under *Gaps*.
5. For a module, draw its main flow in the *Diagram* section (at most about 40 boxes).
6. Go through checklists A and D in `agents/checklists.md` and fix everything.

Write: `specs/iln/connections.md` or its part in `specs/iln/connections/`.
Hand over with the number of relationships and gaps, the checks you did, and the commit message
`<JIRA-KEY> specification: connections for <what> (<n> relationships, <n> gaps)`.
