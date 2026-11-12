# Story 1.3: Reverse-engineer the legacy application structure

**Summary:** Discover and inventory the complete relevant source estate for the pilot application.
**Definition of Done:** The pilot application source estate is represented in a machine-consumable inventory.

## How it works

The Specification agent goes through `iln` **folder by folder, in parts of at most 50 members**, opens every member, and writes what
it is into `specs/iln/inventory/<folder>-<part>.md` (template `agents/templates/inventory.md`), each item with the line that defines it.
Each part starts with a count: members in the folder part = members listed + members not listed, each with a reason.
That count is how completeness is shown without a scanner. The index (`specs/iln/index.md`) adds the parts up per folder.

The inventory is **machine-consumable**: a YAML header and fixed tables with the same columns in every file, which Bob, Duo
(and any tool) can read, search and count. It is also the agents' fact base: the program list, the connections (Story 1.4)
and every program spec (Story 1.5) start from it.

## Agree the scope first

"The complete relevant source estate for the pilot application." Agree with the lead which folders and members belong to the pilot,
and write them in the index's *Inventory coverage* table before starting. iln is around 12 million lines; without scripts the
inventory is the slowest story, so do the pilot scope first and plan the rest in batches.

## Running it

| In | Do |
|---|---|
| **Bob** | Mode *iln Specification*, then `/inventory-folder` with the folder and part, for example: *"/inventory-folder cobol, names A to C, Jira <JIRA-KEY>"* |
| **Duo** | Agentic Chat: *"Act as the Specification agent. Do step A1 for folder `job`, members A to F, Jira <JIRA-KEY>."* |

Then: the Verification agent checks the part (`/verify-spec`), the person commits
(`<JIRA-KEY> specification: inventory cobol A-C (48 members, 3 unknowns)`), and the SME reviews the merge request.

## The `iln` folders

| Folder | Read as | Gives the inventory |
|---|---|---|
| `cobol/` | COBOL programs, and copybooks kept with them (data definitions, no `PROGRAM-ID`) | Programs, calls, copybooks and their record lengths, file and DB2 access, CICS commands |
| `job/` | JCL jobs | Jobs, steps, programs run, datasets, step conditions |
| `procs/` | JCL PROCs | PROCs and which jobs use them |
| `ctl/` | Control cards | IDCAMS `DEFINE CLUSTER`/`AIX` (VSAM keys and record sizes), DFHCSDUP `DEFINE` (CICS transactions, programs, files), utility input |
| `bms/` | BMS maps | Screens |
| `ezt/`, `assembler/` | Easytrieve, Assembler | Listed; behaviour the agent can't read reliably goes under *Unknowns* |
| `test/` | Existing tests | AC 35 |
| `application-conf/`, `pipeline-properties/`, `ci-cd/`, `change-control/`, `simpleLogger/` | Build and pipeline files | Listed as not application source |

If no CICS definitions turn up, ask the CICS team for a DFHCSDUP extract of the application's CSD group; until then transactions
are unknowns. If there is no scheduler export (TWS, Control-M), job order is inferred from shared datasets and marked so.

## Acceptance criteria

| AC | Criterion | How it is met (template section) |
|---|---|---|
| 27 | COBOL programs, calling and called programs | 2. Programs: `PROGRAM-ID`; static calls; calls through a variable with the target if the code shows it; CICS `LINK`/`XCTL`. Called-by is in the connections |
| 28 | Copybooks and their consumers | 3. Copybooks: record, length, used by (from `COPY` and `EXEC SQL INCLUDE` searches) |
| 29 | CICS transactions, programs, COMMAREA, channels/containers | 4. CICS resources, from the CSD `DEFINE`s and `EXEC CICS` commands |
| 30 | JCL and PROCs | 5. JCL jobs and PROCs: steps, programs run, conditions |
| 31 | VSAM files, access, layouts, keys | 6. VSAM files: organisation, key, record size, defining job, layout copybook, and whether the layout length matches |
| 32 | Input/output datasets and utilities | 7. Datasets (created, updated, read by) and utilities |
| 33 | External interfaces | 8. External interfaces: DB2, MQ, file transfer, CICS web, inbound and outbound files, with confidence |
| 34 | Batch jobs and upstream/downstream relationships | Connections spec, 5. Job order (inferred from shared datasets unless a scheduler export exists) |
| 35 | Existing tests, specs, documentation | 9. Existing tests, specifications and documentation; "None found" with where the agent looked |
| 36 | Unknowns reported, not presented as facts | 10. Unknowns: category, detail, evidence, suggested action, owner |
| DoD | Machine-consumable inventory | Fixed-column Markdown with a YAML header; counts reconciled in every part and in the index |

## Definition of Done: show it in the review

1. Open `specs/iln/index.md`: the *Inventory coverage* table (every pilot folder, members counted and accounted for) and the totals.
2. Open one inventory part. Pick two rows and click their evidence into the code.
3. **Repeat a count live:** in Bob, list the same folder part and count; or search `PROGRAM-ID` in it. It must match the part's first table.
4. Show the *Unknowns* with their owners, and the Verification agent's report for the part.

## Limits (tell the SME)

- **The agents read; they don't parse.** Each part is checked by counting, by the Verification agent and by the SME, not by a program.
  Keep parts small; a part that doesn't add up fails.
- **Values known only at run time** (program names read from tables or parameters) stay unknown until an SME answers.
- **COPY REPLACING**, `OCCURS DEPENDING ON` and Easytrieve or Assembler logic are noted, and flagged when they matter.
- **Job order** from datasets is a hint; the scheduler export is the source of truth.
- **Time.** Measure the first few parts and use them to plan the rest with the lead.
