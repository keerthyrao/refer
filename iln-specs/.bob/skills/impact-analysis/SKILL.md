---
name: impact-analysis
description: "Work out what changing an iln program, copybook, VSAM file, transaction or job affects, for a Jira story or the Story 1.4 demonstration."
user-invocable: true
---

# Impact analysis for a change (Story 1.4 and every Jira story)

You are the Jira agent. You need: the Jira key and either the story text, or the component to analyse
(`program`, `copybook`, `vsam`, `transaction` or `job`, and its name). If they are missing, ask before starting.

1. Read `AGENTS.md`, `agents/jira.md` (step 2), `agents/retrieval.md` and `agents/templates/story-impact.md`.
2. Find the starting points: in the specs first, then in the code (semantic search for business words, exact search for names).
3. Follow the connections in both directions from the connections spec and the program specs, and confirm every step with
   a code search, for example `PGM=<PROGRAM>`, `COPY +<COPYBOOK>`, `CALL +'<PROGRAM>'`, `DSN=<DATASET>`, `TRANSACTION *\( *<TRAN>`.
   - Affected: must be changed or re-tested.
   - Relied on: test scope, not changed.
   - Possibly affected: calls through a variable, components not found, inferred job order. An SME must confirm.
4. Record every search in *How it was found*, cite evidence for every connection, list the specs and versions affected,
   and rate the risk with the reason.
5. Go through checklists A and D in `agents/checklists.md` and fix everything.

Write: `specs/iln/stories/<JIRA-KEY>/impact.md`, or for the Story 1.4 demonstration
`specs/iln/stories/<JIRA-KEY>/impact-<kind>-<NAME>.md`.
Hand over with the counts (affected, possibly affected, relied on), the risk, and the commit message
`<JIRA-KEY> jira: impact analysis for <kind> <NAME>, <n> affected, <n> possibly affected`.
