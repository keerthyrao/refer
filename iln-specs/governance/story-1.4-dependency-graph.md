# Story 1.4: Generate the legacy application dependency graph

**Summary:** Map dependencies across COBOL, CICS, JCL, copybooks, VSAM, batch and interfaces.
**User story:** As a Mainframe SME, I want application dependencies automatically mapped, so that agents can calculate change impact.
**Definition of Done:** The graph can identify known components affected by changing a selected program, copybook, VSAM record, CICS transaction, or JCL job.

## How it works

The dependency graph is written as tables in the **connections spec** (`specs/iln/connections.md`, or one part per module in
`specs/iln/connections/`; template `agents/templates/connections.md`). Every row is one relationship, "A relates to B", with its kind,
`path:line` evidence and a confidence. The Specification agent adds rows while inventorying (Story 1.3) and specifying programs (Story 1.5),
and with the `map-connections` skill. GitLab draws the small Mermaid diagram in each part.

**Impact analysis** is the Jira agent following those tables in both directions, and confirming every step with a code search
(`agents/jira.md`, skill `impact-analysis`):

- **Affected by a change to X:** everything that depends on X, directly or indirectly, including data flow: if a program writes a file,
  the jobs and programs that read that file are affected.
- **X relies on:** what X runs and touches; that is the test scope.
- **Possibly affected:** calls through a variable, components referenced but not found, job order that is only inferred. An SME confirms.

Three rules keep the results honest:

1. **Record layouts change with structure, not content.** A copybook is affected when the file it describes changes structure,
   not when a program writes different data into it.
2. **Changing a job or PROC changes its own steps.** Changing one program does not change the other steps in the same job.
3. **Unknowns stay visible,** and every connection found in a spec is confirmed by a search before it is called affected.

Bob and Duo already do impact analysis for the team; this makes it **repeatable and checkable**: the same tables, the same searches
listed in the result, the same evidence, whichever tool or person runs it.

## Running it

| In | Do |
|---|---|
| **Bob** | Mode *iln Jira*, then `/impact-analysis program <NAME>, Jira <JIRA-KEY>` (or copybook, vsam, transaction, job) |
| **Duo** | Agentic Chat: *"Act as the Jira agent. Do the impact analysis for changing copybook `<NAME>`, Jira <JIRA-KEY>."* |

## Acceptance criteria

| AC | Relationship | Where (connections spec section) |
|---|---|---|
| 37 | Program to calling and called programs | 3. Program calls (static, through a variable, CICS LINK/XCTL) |
| 38 | Program to copybook, VSAM file and input/output datasets | 4. Programs to copybooks, files and datasets; 2. the DDs of the job steps |
| 39 | CICS transaction to program, and linked programs | 1. Online: transaction, first program, LINK, XCTL |
| 40 | CICS program to VSAM | 1. Online: files (CICS FILE to dataset) |
| 41 | Job to step, step to PROC, step to program | 2. Batch: jobs to steps to programs (including `RUN PROGRAM` under IKJEFT01) |
| 42 | Job to dataset and job to VSAM | 2. Batch: reads and writes |
| 43 | Upstream and downstream jobs | 5. Job order (inferred from shared datasets unless a scheduler export exists) |
| 44 | Copybook to consumer, record layout to VSAM | 4. Programs to copybooks; inventory section 6 (layout of each VSAM file) |
| 45 | Program to external interface | 6. Interfaces with other systems |
| 46 | Source evidence for every relationship | Every row's Evidence column; checklist D; Verification agent sample |
| DoD | Impact of changing a program, copybook, VSAM record, CICS transaction or JCL job | Five impact analyses, one per kind (below) |

## Definition of Done: show it in the review

1. With the SME, pick one real component of each kind: a program, a copybook, a VSAM file, a CICS transaction and a JCL job.
2. Run the impact analysis for each (Bob `/impact-analysis` or Duo), under the Story 1.4 Jira key:
   `specs/iln/stories/<JIRA-KEY>/impact-<kind>-<NAME>.md`.
3. In the review, open each one: affected, possibly affected, relied on, the searches used. Click two connections through to the code.
4. Let the SME compare with what they know, or with an impact analysis the team did before. Anything missed is a gap to fix in the
   connections spec, and a finding for the review.
5. Show one module part's Mermaid diagram in GitLab.

## Limits (tell the SME)

- **Only as complete as the connections spec and the searches.** Programs not yet inventoried, or called through a variable, limit how far
  impact can be traced, and the result says so under *Possibly affected*.
- **Job order is inferred** from shared datasets until a scheduler export is added.
- **Diagrams stay small** (about 40 boxes). The tables are the record.
