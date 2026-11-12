---
spec_id: MOD-<module-name>
type: module
name: <module-name>
feature: <JIRA-FEATURE-KEY>
capability: <business capability>
version: 0.1.0
status: draft
programs: [<PROGRAM>, <PROGRAM>]
transactions: [<TRAN>]
jobs: [<JOB>]
source:
  repository: iln
  commit: <commit id>
owners:
  mainframe_sme: <name>
  product_owner: <name>
approvals: []                          # added on approval: {role: product_owner, name: <name>, date: <yyyy-mm-dd>, version: <version>}
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: specification
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
last_updated: <yyyy-mm-dd>
---

# <Module business name>

## 1. Purpose

<What business function this module delivers, in two or three sentences.>

## 2. Components

| Kind | Name | Role in the module | Spec |
|---|---|---|---|
| Program | <PROGRAM> | <role> | [<PROGRAM>](../programs/<PROGRAM>.md) |
| Transaction | <TRAN> | <screen or function> | – |
| Job | <JOB> | <batch flow> | – |
| Data | <NAME> | <what it holds> | [<NAME>](../data/<NAME>.md) |

## 3. Flows

### FLOW-<module>-01: <business name> (<online | batch>)

- **Starts when:** <trigger: user enters transaction, schedule, file arrives>
- **Before:** <preconditions>
- **Steps:**
  1. <step> (<PROGRAM>, rule BR-<PROGRAM>-00n)
  2. <...>
- **After:** <postconditions>
- **If it fails:** <errors, restart behaviour>
- **Evidence:** <file:line for the trigger and each step>

## 4. Business rules

<Rules that span programs, plus a list of the most important program rules by id. Don't copy program rules; link them.>

| Rule | Summary | Where |
|---|---|---|
| BR-<PROGRAM>-00n | <one line> | [<PROGRAM>](../programs/<PROGRAM>.md) |

## 5. Data

<The files and tables this module owns, reads from other modules, and writes for other modules.>

## 6. Interfaces with other modules and systems

| Direction | With | Through | Spec |
|---|---|---|---|
| <in / out> | <module or external system> | <file / table / queue / call> | <link> |

## 7. Batch schedule

<Jobs in order, their dependencies and frequency, as far as the code and schedule definitions show. Mark anything inferred.>

## 8. Non-functional and architecture constraints

<Batch window, response times, volumes, security and audit for the module; the architecture patterns its programs follow.>

## 9. Acceptance criteria

| ID | Testable statement | Verifies |
|---|---|---|
| AC-<module>-01 | <an end-to-end check, in business words> | <FLOW-module-01> |

## 10. Open questions

| ID | Question | Owner | Status |
|---|---|---|---|
| Q-<module>-01 | <question> | <owner> | open |

## 11. Change log

| Version | Date | Story | Change |
|---|---|---|---|
| 0.1.0 | <yyyy-mm-dd> | <JIRA-KEY> | First version |
