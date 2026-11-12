---
spec_id: PGM-<PROGRAM>
type: program
name: <PROGRAM>
feature: <JIRA-FEATURE-KEY>            # the Jira feature this spec was produced under
capability: <business capability>      # as named in the application spec
module: <module-name>                  # filled in once modules are defined
program_type: <batch | online | called routine>
language: <COBOL | COBOL with DB2 | COBOL with CICS | Assembler>
version: 0.1.0                         # 1.0.0 = first reviewed version
status: draft                          # draft | in-review | reviewed | approved
source:
  repository: iln
  commit: <commit id of the source that was read>
  files:
    - <cobol/PROGRAM>                  # path in iln; members have no extension
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

# <PROGRAM>: <business name>

<!-- Evidence format everywhere: path in iln, then :line or :from-to (cobol/<MEMBER>:120-134). Cite only lines you have opened and read.
     Confidence: confirmed | high | medium | low. Anything below high needs an open question.
     Write Purpose, Rules, Flow and State changes so a Product Owner can read them without knowing COBOL:
     no data names or COBOL verbs there. Code names go in evidence, in brackets, and in the glossary.
     A section with nothing to say reads "None found", followed by where you looked. -->

## 1. Purpose

<One or two sentences: what this program does for the business.>

## 2. How it is started

| Started by | Name | Evidence |
|---|---|---|
| <CICS transaction / JCL job and step / called by program / linked from program> | <name> | <file:line> |

## 3. Inputs and outputs

| Direction | Kind | Name (DD, file, table, queue, map) | Access | Record or layout | Evidence |
|---|---|---|---|---|---|
| <in / out / in-out> | <VSAM KSDS / sequential / DB2 table / MQ / COMMAREA / container / BMS map / report> | <name> | <read / write / update / delete / browse> | <copybook or data spec link> | <file:line> |

## 4. Data used

| Data | Description | Spec | Evidence |
|---|---|---|---|
| <copybook / record / table> | <what it holds, in business words> | [<NAME>](../data/<NAME>.md) | <file:line> |

## 5. Business rules

### BR-<PROGRAM>-001: <short business name>

- **Given** <the starting situation>
- **When** <the event or condition>
- **Then** <the outcome>
- **Otherwise** <what happens instead, if the code says>
- **Evidence:** <file:from-to>
- **Confidence:** <high>
- **Status:** current

<!-- One rule per decision or calculation. Copy the block for each rule: BR-<PROGRAM>-002, -003 ... -->

## 6. Processing flow

1. <Step in business words> (<paragraph name>, <file:line>)

## 7. State changes

| What changes | From | To | When | Evidence | Confidence |
|---|---|---|---|---|---|
| <the status or state, in business words> | <old value> | <new value> | <condition> | <file:line> | <high> |

## 8. Errors and exceptions

| Condition | How it is detected | What happens | Evidence | Confidence |
|---|---|---|---|---|
| <record not found / file error / DB2 error / invalid input> | <INVALID KEY / file status / SQLCODE / RESP> | <message / skip / abend / return code n> | <file:line> | <high> |

## 9. Restart and recovery

<Commit points, checkpoints, what a rerun after failure does, what is or isn't rolled back.>

## 10. Connections

| Relationship | Component | Static or dynamic | Evidence |
|---|---|---|---|
| <Calls / Called by / Shares data with> | <PROGRAM, transaction, job step, or file> | <static CALL / dynamic CALL / CICS LINK / XCTL> | <file:line> |

## 11. Non-functional: security, performance, audit

<Security checks, audit records written, commit frequency, large sorts, response-time sensitive paths.>

## 12. Architecture constraints

| Pattern | How it applies to this program |
|---|---|
| [<ONL-01>](../../../architecture/iln/patterns/<ONL-01>.md) | <how> |

## 13. Acceptance criteria

| ID | Testable statement | Verifies |
|---|---|---|
| AC-<PROGRAM>-01 | <a check a tester can run, in business words> | <BR-PROGRAM-001> |

## 14. Code coverage

<!-- Story 1.5: proof that nothing in the program was missed. One row per paragraph or section of the procedure division,
     in source order. Together the rows cover the procedure division with no gaps. "Technical" rows say why the code is not
     business behaviour. -->

| Paragraph or section | Lines | Explained in | Note |
|---|---|---|---|
| <paragraph name> | <file:from-to> | <BR-PROGRAM-001 / flow step 2 / errors row 1 / restart / technical> | <for technical: opens files, initialises counters> |

**Procedure division:** <file:from-to>, <n> paragraphs and sections, all listed above.

## 15. Open questions

| ID | Question | Why it matters | Owner | Status |
|---|---|---|---|---|
| Q-<PROGRAM>-01 | <question> | <what is uncertain without the answer> | <SME / Product Owner> | open |

## 16. Glossary

| In the code | Means |
|---|---|
| <CODE-NAME> | <business meaning> |

## 17. Change log

| Version | Date | Story | Change |
|---|---|---|---|
| 0.1.0 | <yyyy-mm-dd> | <JIRA-KEY> | First extraction |
