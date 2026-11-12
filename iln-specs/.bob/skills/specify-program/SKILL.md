---
name: specify-program
description: "Write or update the specification of one iln program from its code, with evidence, confidence, code coverage and open questions (Stories 1.2 and 1.5)."
user-invocable: true
---

# Specify one iln program (Stories 1.2 and 1.5)

You are the Specification agent. You need: the program name and the Jira key. If either is missing, ask before starting.

1. Read `AGENTS.md`, `agents/specification.md` (step A3), `agents/retrieval.md`, `agents/templates/program-spec.md`
   and, for shared record layouts, `agents/templates/data-spec.md`.
2. Find the program in `specs/iln/index.md` and the inventory. If it isn't there, say so: it becomes an open question.
3. Retrieve everything it needs, as *Retrieving everything one program needs* in `agents/retrieval.md` describes:
   existing specs, the program, its copybooks, what starts it, its files and datasets, its tables and queues, what it calls.
   Open every file you will cite.
4. Read the procedure division paragraph by paragraph and draft the *Code coverage* table: for each paragraph, what it does.
5. Fill every section of the template in business words, with evidence and confidence on every rule and row.
   Anything the code doesn't prove becomes an open question.
6. Search the program for the decision, calculation, error and restart keywords in `agents/retrieval.md`.
   Every hit must fall in a paragraph you have explained.
7. Go through checklists A and B in `agents/checklists.md` and fix everything. Add new relationships to the connections spec,
   and set the program's status in `specs/iln/index.md` to `specified` (or `partial`).

Write: `specs/iln/programs/<PROGRAM>.md`, any new `specs/iln/data/<NAME>.md`, the connections spec, `specs/iln/index.md`.
Hand over with the number of rules and open questions, the checks you did, and the commit message
`<JIRA-KEY> specification: <PROGRAM> spec v<version> (<n> rules, <n> open questions)`.
