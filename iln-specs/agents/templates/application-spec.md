---
spec_id: APP-iln
type: application
name: iln
feature: <JIRA-FEATURE-KEY>
version: 0.1.0
status: draft
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

# iln application specification

## 1. Purpose and scope

<What the application does for the business, who uses it, and what is in and out of scope.>

## 2. Business capabilities

| Capability | Description | Modules |
|---|---|---|
| <capability> | <one line> | [<module>](modules/<module>.md) |

## 3. Modules

| Module | Purpose | Online / batch | Programs | Spec |
|---|---|---|---|---|
| <module> | <one line> | <both> | <count> | [<module>](modules/<module>.md) |

## 4. External interfaces

| Direction | Partner system | Through | Frequency | Spec |
|---|---|---|---|---|
| <in / out> | <system> | <file transfer / MQ / DB2 / API> | <daily / online> | <link> |

## 5. Shared data

<The main files and tables, which module owns each, and who else reads or writes them. Link the data specs.>

## 6. Batch schedule overview

<The nightly / daily / monthly cycles, in order, and their critical dependencies.>

## 7. Cross-cutting behaviour

| Topic | How the application does it | Spec or pattern |
|---|---|---|
| Security | <signon, transaction security, resource checks> | <link> |
| Audit | <what is logged, where> | <link> |
| Error handling | <conventions> | <architecture pattern link> |
| Restart and recovery | <batch restart, online recovery> | <link> |
| Dates and calendars | <business dates, holidays> | <link> |

## 8. Size and coverage

| Item | Count | Specified | Reviewed |
|---|---|---|---|
| Programs | <n> | <n> | <n> |
| Copybooks | <n> | <n> | <n> |
| Transactions | <n> | – | – |
| Jobs | <n> | – | – |

<Taken from index.md. Feature 1 is done when every program is specified and reviewed.>

## 9. Open questions

| ID | Question | Owner | Status |
|---|---|---|---|
| Q-APP-01 | <question> | <owner> | open |

## 10. Change log

| Version | Date | Story | Change |
|---|---|---|---|
| 0.1.0 | <yyyy-mm-dd> | <JIRA-KEY> | First version |
