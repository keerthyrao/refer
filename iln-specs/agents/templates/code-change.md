---
story: <JIRA-KEY>
type: code-change
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: coding
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
based_on:                              # the approved specs this was built from (Story 1.1 AC 12)
  - spec: <SPEC-ID>
    version: <version>
    baseline: <tag spec/iln/vN.N, or main commit id>
date: <yyyy-mm-dd>
application_repository: iln
branch: fe/<FEATURE-KEY>
commits: [<commit id>, <commit id>]
build: <passed | failed>          # compile, link, bind, map assembly
---

# Code change: <JIRA-KEY> <story title>

## 1. Members changed

| Member | Kind | What changed | Spec items | Pattern followed | Commit |
|---|---|---|---|---|---|
| <PROGRAM> | <COBOL / copybook / JCL / map / CSD / DDL> | <short description> | <BR-...-004> | <ONL-01> | <commit id> |

## 2. Spec items to code

| Spec item | Implemented in | Lines (after change) |
|---|---|---|
| <BR-PROGRAM-004> | <PROGRAM> | <file:from-to> |

<!-- Every spec item in the spec change appears here. Every member in section 1 maps to at least one spec item. -->

## 3. Build

| Step | Result | Evidence |
|---|---|---|
| Compile | <RC 0 / 4 (warnings listed)> | <listing or job output reference> |
| Link-edit | <RC> | <reference> |
| DB2 precompile / bind | <RC or n/a> | <reference> |
| CICS map assembly / CSD | <RC or n/a> | <reference> |

## 4. Not changed

<Components in the impact analysis marked "re-test" only, confirming they were not modified.>

## 5. Notes for testing

<Anything the Testing agent should know: new error paths, boundary values, changed messages.>
