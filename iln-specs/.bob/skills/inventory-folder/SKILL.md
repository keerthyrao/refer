---
name: inventory-folder
description: "Inventory one folder, or part of a folder, of the iln code into specs/iln/inventory: every member counted, with evidence and unknowns (Story 1.3)."
user-invocable: true
---

# Inventory a folder of the iln code (Story 1.3)

You are the Specification agent. You need: the `iln` folder and the part to do (for example `cobol`, names A to C).
If either is missing, ask before starting.

1. Read `AGENTS.md`, `agents/specification.md` (step A1), `agents/retrieval.md` and `agents/templates/inventory.md`.
2. List the folder and count the members in the part. If there are more than 50, propose a split by name and stop.
3. Open each member. Record what it is in the right section of the template, with the line that defines it
   (`PROGRAM-ID`, the `JOB` card, `PROC`, `DEFINE`, `DFHMSD`, record definitions for a copybook).
4. In the same members, search for calls, `COPY`, `EXEC CICS`, `EXEC SQL`, MQ calls, `PGM=`, `DSN=` and `DEFINE`
   (the recipes in `agents/retrieval.md`) and fill the relationship columns. Add the relationships to the connections spec as well.
5. Put everything you can't resolve under *Unknowns*: calls through a variable, components not found, Easytrieve or Assembler
   members you can't read, missing scheduler information.
6. Make *Folder and coverage* add up (members = listed + not listed), then update `specs/iln/index.md`
   (inventory coverage, counts, and new programs with status `not started`).
7. Go through checklists A and C in `agents/checklists.md` and fix everything.

Write: `specs/iln/inventory/<folder>-<part>.md`, `specs/iln/index.md`, and the connections spec.
Hand over with the counts, the unknowns, the checks you did, and the commit message
`<JIRA-KEY> specification: inventory <folder> <part> (<n> members, <n> unknowns)`.
