---
type: pattern
pattern_id: <ONL-01>               # ONL online, BAT batch, DAT data access, STR structure, NAM naming, XCT cross-cutting
name: <short name>
version: 0.1.0
status: draft                      # draft | reviewed | approved
confidence: <high | medium | low>
source:
  repository: iln
  commit: <commit id>
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: architect
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
last_updated: <yyyy-mm-dd>
---

# <ONL-01>: <pattern name>

## 1. The rule

<One or two sentences that the Coding agent can follow. Example shape: "A <kind of component> is built as <structure>,
uses <mechanism> for <purpose>, and handles <errors> by <convention>.">

## 2. Applies to

<Which components: all CICS screen programs / all batch update programs / all programs reading VSAM KSDS files.>

## 3. How it looks in the code

<The structure in steps or a short outline (paragraph roles, the order of commands). No full code listings.>

## 4. Examples

| Program | Where | Evidence |
|---|---|---|
| <PROGRAM> | <paragraph or section> | <file:from-to> |
| <PROGRAM> | <...> | <file:from-to> |
| <PROGRAM> | <...> | <file:from-to> |

<!-- At least three, from different programs. -->

## 5. Exceptions

| Program | How it differs | Evidence | Deliberate? |
|---|---|---|---|
| <PROGRAM> | <difference> | <file:line> | <yes / no / unknown> |

## 6. Rules for new or changed code

- <Do ...>
- <Don't ...>

## 7. Open questions

| ID | Question | Owner | Status |
|---|---|---|---|
| Q-<ONL-01>-01 | <question> | <owner> | open |

## 8. Change log

| Version | Date | Story | Change |
|---|---|---|---|
| 0.1.0 | <yyyy-mm-dd> | <JIRA-KEY> | First version |
