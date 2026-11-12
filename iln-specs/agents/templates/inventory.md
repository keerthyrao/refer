---
spec_id: INV-<folder>-<part>                # for example INV-cobol-a-c
type: inventory
folder: <folder in iln>                     # for example cobol
part: <which members>                       # for example names starting A to C, or all
version: 0.1.0
status: draft                               # draft | in-review | reviewed
source:
  repository: iln
  commit: <commit id of the source that was read>
generated_by:
  agent: specification
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
last_updated: <yyyy-mm-dd>
---

# Inventory: <folder> (<part>)

<!-- Story 1.3. One file per folder, or per part of at most 50 members. Every row has evidence: the line that defines it.
     Sections that don't apply to this folder read "None found in <folder>". Relationships between components also go
     into the connections spec (Story 1.4). -->

## 1. Folder and coverage

| Item | Count | How counted |
|---|---|---|
| Members in the folder part | <n> | <listed the folder in Bob / GitLab repository view, names A to C> |
| Members listed below | <n> | <rows in sections 2 to 9> |
| Members not listed | <n> | <see the table below> |

| Member not listed | Why |
|---|---|
| <MEMBER> | <empty / duplicate of / not source / excluded as restricted> |

<!-- Members in the folder part = members listed + members not listed. -->

## 2. Programs (AC 27)

| Program | Member | Kind | Calls (static) | Calls through a variable (target) | CICS LINK / XCTL | Evidence | Confidence |
|---|---|---|---|---|---|---|---|
| <PROGRAM> | <member> | <batch / online / called routine> | <PROGRAM, ...> | <variable → PROGRAM, or unknown> | <PROGRAM, ...> | <path:line of PROGRAM-ID> | <high> |

## 3. Copybooks (AC 28)

| Copybook | Member | Describes | Record length | Used by | Evidence |
|---|---|---|---|---|---|
| <COPYBOOK> | <member> | <record or COMMAREA, in business words> | <bytes> | <PROGRAM, ... or "none found"> | <path:line> |

## 4. CICS resources (AC 29)

| Kind | Name | Program or dataset | Group | COMMAREA, channel or container | Evidence |
|---|---|---|---|---|---|
| <transaction / program / file / mapset> | <NAME> | <PROGRAM or DSNAME> | <CSD group> | <name and length, or none> | <path:line> |

## 5. JCL jobs and PROCs (AC 30)

| Job or PROC | Step | Runs (program or PROC) | Condition | Evidence |
|---|---|---|---|---|
| <JOB> | <STEP> | <PROGRAM / PROC> | <COND / IF, or none> | <path:line> |

## 6. VSAM files (AC 31)

| Cluster | Organisation | Key (offset, length) | Record size | Defined by | Layout (copybook) | Layout length matches? | Evidence |
|---|---|---|---|---|---|---|---|
| <CLUSTER> | <KSDS / ESDS / RRDS / AIX> | <offset, length> | <average, maximum> | <member and step> | <COPYBOOK> | <yes / no / not checked> | <path:line> |

## 7. Datasets and utilities (AC 32)

| Dataset | Created by | Updated by | Read by | Kind | Evidence |
|---|---|---|---|---|---|
| <DATASET> | <JOB.STEP> | <JOB.STEP> | <JOB.STEP> | <permanent / GDG / temporary> | <path:line> |

| Utility | Used in | For | Evidence |
|---|---|---|---|
| <SORT / IDCAMS / IEBGENER / IKJEFT01 / FTP> | <JOB.STEP> | <what it does there> | <path:line> |

## 8. External interfaces (AC 33)

| Kind | Name | Direction | Used by | Evidence | Confidence |
|---|---|---|---|---|---|
| <DB2 table / MQ queue / file transfer / CICS web / inbound or outbound file> | <NAME> | <in / out / both> | <PROGRAM, JOB> | <path:line> | <high> |

## 9. Existing tests, specifications and documentation (AC 35)

| Item | Kind | What it covers | Evidence |
|---|---|---|---|
| <file or folder> | <test case / test data description / document> | <programs or functions> | <path> |

## 10. Unknowns (AC 36)

| ID | Category | Detail | Evidence | Suggested action | Owner |
|---|---|---|---|---|---|
| U-<folder>-01 | <call through a variable / not found in the source / unreadable language / no schedule information / other> | <what is unknown> | <path:line> | <ask the SME / get the scheduler export / get the CSD extract> | <SME> |

## 11. Change log

| Version | Date | Story | Change |
|---|---|---|---|
| 0.1.0 | <yyyy-mm-dd> | <JIRA-KEY> | First inventory of this part |
