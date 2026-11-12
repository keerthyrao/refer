# Specifications

This folder holds **only specifications** of the iln application, pulled from the code, and their impact analyses per story.
Git keeps the version history. Nothing else goes here: no agent instructions, test results or code.

## Structure

```
specs/
  iln/
    index.md                  progress: every folder inventoried, every program, copybook, module and its status; use of the specs
    inventory/<folder>.md     what each folder of iln contains, counted, with evidence and unknowns (Story 1.3)
    connections.md            how transactions, jobs, programs, data and other systems connect (Story 1.4);
    connections/<part>.md       split into parts when it grows
    programs/<PROGRAM>.md     one per program (Stories 1.2 and 1.5)
    data/<NAME>.md            shared record layouts, VSAM files, DB2 tables
    modules/<module>.md       one per business function (online function or batch flow)
    application.md            the whole application
    stories/<JIRA-KEY>/
      impact.md               impact analysis (Jira agent, written first)
      spec-change.md          what the story changes in the specs (Specification agent)
```

Templates: `agents/templates/`. How specs are written: `agents/specification.md`. Checks: `agents/checklists.md`.

## Levels

| Level | One per | Says |
|---|---|---|
| Inventory | Folder of `iln` (or part of one) | What exists, counted against the folder, with the line that defines each item, and what is unknown |
| Connections | iln (or a module) | Every relationship between components, with evidence and confidence; the dependency graph as tables |
| Program | `PROGRAM-ID` | Purpose, how it starts, inputs and outputs, data, business rules, flow, errors, restart, connections, code coverage |
| Data | Shared copybook, file or table | Layout field by field, key, organisation, who reads and writes it |
| Module | Business function | End-to-end flows across programs, module data, interfaces, batch schedule |
| Application | iln | Capabilities, modules, external interfaces, shared data, cross-cutting behaviour, coverage |

## Status

| Status | Meaning | Set by |
|---|---|---|
| `draft` | Written by the agent, not reviewed | Agent |
| `in-review` | In a merge request with the Mainframe SME | Agent |
| `reviewed` | SME agrees it matches the code | Mainframe SME |
| `approved` | Product Owner approves it as the baseline for change, recorded in `approvals` for that version | Product Owner |

## Versions and baselines

`MAJOR.MINOR.PATCH` in each spec's header: patch for wording or evidence fixes, minor for a changed or added rule, major for a change
of purpose or interface. Each change adds a line to the spec's change log with the Jira key.

When a merge request with approved specs is merged to `main`, a maintainer tags `main` in GitLab (**Code > Tags > New tag**)
as `spec/iln/v<n>.<n>`, with a message listing the specs and versions. Agents that build on specs record that tag in `based_on`.
