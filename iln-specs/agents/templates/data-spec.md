---
spec_id: DATA-<NAME>
type: data
name: <COPYBOOK, record or table name>
kind: <record | VSAM file | sequential file | DB2 table | COMMAREA | container | screen | message>
feature: <JIRA-FEATURE-KEY>
version: 0.1.0
status: draft
source:
  repository: iln
  commit: <commit id>
  files:
    - <cobol/COPYBOOK or DDL member>     # path in iln
approvals: []                          # added on approval: {role: product_owner, name: <name>, date: <yyyy-mm-dd>, version: <version>}
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: specification
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
last_updated: <yyyy-mm-dd>
---

# <NAME>: <business name>

## 1. What it holds

<One or two sentences in business words.>

## 2. Storage

| Item | Value | Evidence |
|---|---|---|
| Dataset / table | <dataset name pattern or table name> | <file:line> |
| Organisation | <KSDS / ESDS / RRDS / sequential / DB2> | <file:line> |
| Key | <field(s), offset, length> | <file:line> |
| Record length | <fixed n / variable min-max> | <file:line> |
| Defined by | <IDCAMS job / DDL member> | <file:line> |

## 3. Layout

| Level | Field | Meaning | Type | Length | Offset | Valid values | Evidence |
|---|---|---|---|---|---|---|---|
| 05 | <FIELD-NAME> | <business meaning> | <character / zoned decimal / packed decimal / binary / date> | <bytes> | <offset> | <values, or 88-level names and values> | <file:line> |

<!-- Field lengths must add up to the record length. Note REDEFINES and OCCURS explicitly. -->

## 4. Used by

| Program | Access | Evidence |
|---|---|---|
| [<PROGRAM>](../programs/<PROGRAM>.md) | <read / write / update / delete> | <file:line> |

## 5. Open questions

| ID | Question | Owner | Status |
|---|---|---|---|
| Q-<NAME>-01 | <question> | <owner> | open |

## 6. Change log

| Version | Date | Story | Change |
|---|---|---|---|
| 0.1.0 | <yyyy-mm-dd> | <JIRA-KEY> | First extraction |
