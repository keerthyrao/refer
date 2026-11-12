# Story 1.5: Reverse engineer business behaviour

**Summary:** Extract business behaviour from legacy COBOL, CICS, VSAM and batch code.
**User story:** As a Product Owner, I want current business behaviour extracted from legacy code, so that existing behaviour can be preserved or intentionally changed.
**Definition of Done:** The Product Owner and Mainframe SME can confirm the extracted behaviour from a readable specification without needing to read the COBOL source directly.

## How it works

The **Specification agent** writes the behaviour into the program and module specs (`agents/specification.md`, step A3; skill `specify-program`):
business rules, flow, state changes, errors, restart behaviour and data access, each with evidence and a confidence level.

Three things make it complete and trustworthy without scripts:

1. **Retrieval of everything the program needs** (`agents/retrieval.md`): its existing specs, the program, its copybooks, what starts it,
   its files, datasets, tables and queues, what it calls. Every file the agent cites, it opens.
2. **Code coverage.** The agent reads the procedure division paragraph by paragraph and lists **every** paragraph in the spec's
   *Code coverage* section, with its lines and what explains it (a rule, a flow step, an error, restart, or technical with a reason).
   Then it searches the program for decision, calculation, error and restart keywords: every hit must fall in an explained paragraph.
   This replaces the old worklist and coverage tools.
3. **Checks and review.** The agent goes through `agents/checklists.md` B; the Verification agent opens a sample of cited lines and
   repeats the paragraph count; Duo Code Review applies the same checklist; the SME and Product Owner approve in the merge request.

If your Bob has the **Premium Package for Z**, its *Extract business rules* and *Generate program documentation* workflows can be used
as a first draft. They are pointers, not evidence: every item goes into our template with lines the agent opened, or becomes an open question.

## Acceptance criteria

| AC | What | How it is met |
|---|---|---|
| 47 | Business rules, calculations and decision logic | Spec 5. Business rules, Given/When/Then; keyword search for `IF`, `EVALUATE`, `COMPUTE`, arithmetic, condition names |
| 48 | Preconditions, postconditions, scenarios, state transitions | Spec 6. Processing flow, 7. State changes; module 3. Flows |
| 49 | Errors, exceptions and data transformations | Spec 8. Errors and exceptions; keyword search for `INVALID KEY`, `AT END`, file status, `SQLCODE`, `RESP`, abends |
| 50 | CICS transaction and batch-processing behaviour | Spec 2. How it is started, 6. Processing flow; module flows; connections |
| 51 | VSAM read, write, update, delete, key and record-layout behaviour | Spec 3. Inputs and outputs, 4. Data used; data specs (key, layout, lengths) |
| 52 | Restart and recovery | Spec 9. Restart and recovery, which must not be empty; keyword search for `SYNCPOINT`, `COMMIT`, checkpoints |
| 53 | Source evidence for extracted behaviour | Every rule and row cites `path:line`; the agent opened it; Verification and SME open a sample |
| 54 | Confidence for uncertain behaviour | Every rule and row has a confidence; medium or low needs an open question (checklist B) |
| DoD | PO and SME confirm without reading COBOL | Plain-language check; the spec read in GitLab; *Code coverage* shows nothing was skipped; recorded approval |

## How confirmation works (Definition of Done)

1. The agent writes the spec and goes through the checklist; the Verification agent checks it; the person commits
   (`<JIRA-KEY> specification: <PROGRAM> spec v0.1.0 ...`) and opens the merge request with the *Specification* template.
2. The **Mainframe SME** reviews against the code, starting with medium and low items and *Code coverage*, and sets `status: reviewed`
   (version `1.0.0`).
3. The **Product Owner** reads the spec in GitLab (no COBOL needed), answers the open questions, and approves: the approval goes in the
   header for that version and `status: approved`. Approval waits while any question is open.
4. After the merge, a maintainer tags the baseline (`spec/iln/v<n>.<n>`).

## Show it in the review

1. Open a reviewed program spec in GitLab. Read the purpose and two rules aloud: business words only.
2. Click a rule's evidence into the code; the SME confirms it.
3. Show *Code coverage*: every paragraph accounted for, the technical ones with reasons.
4. Show an open question and how the Product Owner answered it in the merge request.

## Limits (tell the Product Owner and SME)

- **The agent reads the code; the SME confirms it.** Coverage shows nothing was skipped; it doesn't prove each sentence is right.
  That is what the review is for.
- **Static reading only.** Values set at run time, programs not in the source and scheduler rules show up as uncertain, never guessed.
- **Large programs** take several sessions; the agent marks the spec `partial` until every paragraph is covered.
