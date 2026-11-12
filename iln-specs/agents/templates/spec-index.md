---
spec_id: INDEX-iln
type: index
source:
  repository: iln
  commit: <commit id of the source the lists were built from>
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: specification
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
last_updated: <yyyy-mm-dd>
---

# iln specification index and progress

<!-- The Specification agent updates this file every time an inventory part or a spec is written, and the SME's review changes a status.
     It is the progress measure for Feature 1: done when every folder is inventoried and every program is specified and reviewed. -->

## 1. Progress

| Item | Total | Not started | Partial | Specified | Reviewed |
|---|---|---|---|---|---|
| Programs | <n> | <n> | <n> | <n> | <n> |
| Copybooks / data | <n> | <n> | <n> | <n> | <n> |
| Modules | <n> | <n> | <n> | <n> | <n> |

## 2. Inventory coverage (Story 1.3)

| Folder | Members | Inventoried | Not listed (with reason) | Parts | Status |
|---|---|---|---|---|---|
| [<folder>](inventory/<folder>.md) | <n> | <n> | <n> | <cobol-a-c, cobol-d-f ...> | <not started / partial / done / reviewed> |

<!-- One row per folder of iln. "Members" is counted by listing the folder. Members = inventoried + not listed. -->

## 3. What iln contains

| Kind | Count | Where listed |
|---|---|---|
| Programs | <n> | inventory section 2 |
| Copybooks | <n> | inventory section 3 |
| CICS transactions | <n> | inventory section 4 |
| Jobs / PROCs | <n> / <n> | inventory section 5 |
| VSAM files | <n> | inventory section 6 |
| External interfaces | <n> | inventory section 8 |
| Existing tests and documents | <n> | inventory section 9 |
| Open unknowns | <n> | inventory section 10 |

## 4. Programs

| Program | Type | Module | Status | Spec version | Rules | Open questions | Confidence | Last updated |
|---|---|---|---|---|---|---|---|---|
| [<PROGRAM>](programs/<PROGRAM>.md) | <batch / online / called> | <module> | <not started / partial / specified / reviewed / approved> | <0.1.0> | <n> | <n> | <lowest confidence in the spec> | <yyyy-mm-dd> |

## 5. Data

| Data | Kind | Status | Spec version | Used by (count) |
|---|---|---|---|---|
| [<NAME>](data/<NAME>.md) | <record / VSAM / table> | <status> | <0.1.0> | <n> |

## 6. Modules

| Module | Programs | Status | Spec version |
|---|---|---|---|
| [<module>](modules/<module>.md) | <n> | <status> | <0.1.0> |

## 7. Transactions and jobs

| Kind | Name | Module | First program / steps |
|---|---|---|---|
| <transaction / job> | <NAME> | <module> | <PROGRAM / n steps> |

## 8. Use of the specs (adoption)

<!-- Progress is measured by results and by people using the specs. Add a row whenever a story, impact analysis,
     test suite or question was answered from the specs. -->

| Date | Used for | By | Specs used | Saved or found |
|---|---|---|---|---|
| <yyyy-mm-dd> | <impact analysis for JIRA-KEY / test cases / SME question> | <team or role> | <PGM-... v1.0.0> | <for example: impact found 2 jobs missed before> |
