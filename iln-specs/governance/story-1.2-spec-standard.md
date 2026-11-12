# Story 1.2: Canonical executable specification standard

**Summary:** Define the standard specification structure that becomes authoritative for business behaviour.
**Definition of Done:** A representative business capability can be completely represented without requiring COBOL source code to explain expected behaviour.

## The standard is the templates and the checklist

Specifications are **Markdown files with a short header**, so people read them in GitLab and Bob and Duo read the same files.
The shape of every spec is its template in `agents/templates/`; the rules every spec must meet are `agents/checklists.md`.
Changing either (through a merge request the lead approves) changes the standard.

| Level | Template | One per | File |
|---|---|---|---|
| Inventory | `inventory.md` | Folder of `iln` or part of one | `specs/iln/inventory/<folder>.md` |
| Program | `program-spec.md` | `PROGRAM-ID` | `specs/iln/programs/<PROGRAM>.md` |
| Data | `data-spec.md` | Shared copybook, file or table | `specs/iln/data/<NAME>.md` |
| Module | `module-spec.md` | Business function (online function or batch flow) | `specs/iln/modules/<module>.md` |
| Application | `application-spec.md` | iln | `specs/iln/application.md` |
| Connections | `connections.md` | iln, or one part per module | `specs/iln/connections.md` |
| Progress index | `spec-index.md` | iln | `specs/iln/index.md` |

This follows the lead's hierarchy: **program, then module, then the whole application**, with the connections between them.

## Principles

1. **Business language.** Purpose, rules, flow, state changes and acceptance criteria are written so a Product Owner understands them
   without COBOL. Code names appear only as evidence, in brackets, and in the glossary.
2. **Every rule is a test.** Business rules are written Given / When / Then, and acceptance criteria point at the rules they verify.
3. **Evidence for everything pulled from the code:** `path:line` or `path:from-to` in `iln`, opened by the agent, plus a confidence level.
   Unknowns become open questions, never facts.
4. **Nothing missed.** A program spec's *Code coverage* accounts for every paragraph of the procedure division.
5. **Plain text in Git.** Every change is a reviewed diff and a new version.

## How the standard is enforced, without scripts

| Who | When | How |
|---|---|---|
| The writing agent | Before handing over | Goes through `agents/checklists.md` A and B, and lists the checks in the merge request |
| GitLab Duo Code Review | On every merge request | `.gitlab/duo/mr-review-instructions.yaml` gives Duo the same checklist for `specs/**` |
| Verification agent | Each batch | Repeats the checklist, opens a sample of cited lines, repeats searches |
| Mainframe SME, Product Owner | Approving the merge request | The *Specification* merge request template; CODEOWNERS makes their approval required |

## How the template sections meet the acceptance criteria

| AC | Requirement | Program spec | Module / application / data spec |
|---|---|---|---|
| 15 | Business feature identifier and capability | Header `feature`, `capability` | Header `feature`, `capability` |
| 16 | Requirement and traceability identifiers | `spec_id`, rule ids `BR-`, acceptance ids `AC-`, change log with Jira keys, `source.commit`, `generated_by` | Same; flows `FLOW-` |
| 17 | Functional requirements and business rules | 5. Business rules (Given / When / Then) | Module 4. Business rules |
| 18 | Preconditions, postconditions, scenarios, state transitions | 6. Processing flow, 7. State changes | Module 3. Flows (before / steps / after) |
| 19 | Input and output contracts | 3. Inputs and outputs | Module 6. Interfaces; application 4. External interfaces |
| 20 | Data structures and VSAM record contracts | 4. Data used | Data spec: storage (organisation, key, record length) and field-by-field layout |
| 21 | Error conditions and exception behaviour | 8. Errors and exceptions | Module 3. "If it fails" |
| 22 | Acceptance criteria | 13. Acceptance criteria | Module 9. Acceptance criteria |
| 23 | Non-functional, security, performance, resilience, restart/recovery | 9. Restart and recovery, 11. Non-functional | Module 8; application 7. Cross-cutting behaviour |
| 24 | Architecture constraints | 12. Architecture constraints (links to `architecture/` patterns) | Module 8 |
| 25 | Version and lifecycle status | Header `version`, `status`, `approvals`; 17. Change log | Same |
| 26 | Assumptions, open questions, source evidence | Evidence on every rule and row; 14. Code coverage; 15. Open questions | Same |

## Writing a business rule

```markdown
### BR-<PROGRAM>-001: <short business name>

- **Given** <the starting situation, in business words>
- **When** <the event or condition>
- **Then** <the outcome>
- **Otherwise** <what happens instead, if the code says>
- **Evidence:** <cobol/<MEMBER>:from-to>
- **Confidence:** high
- **Status:** current
```

One rule per decision or calculation. `Status` is `current` for the baseline; a later change uses `new`, `modified` or `removed`.

## Lifecycle and versions

`draft` (written by the agent) → `in-review` (merge request open) → `reviewed` (Mainframe SME agrees it matches the code)
→ `approved` (the Product Owner's approval recorded in the header for that exact version, and no open question still open).

Versions are `MAJOR.MINOR.PATCH`: patch for wording or evidence, minor for an added or changed rule, major for a change of purpose
or interface. Every version has a change-log line. Approved baselines are tagged `spec/iln/v<n>.<n>` on `main`.

## Definition of Done: show it in the review

1. Agree with the Product Owner one **real business capability** of iln that is small but complete (one online function or one batch flow).
2. The Specification agent writes its program specs, data specs and module spec (`/specify-program` in Bob, or the Specification agent in Duo).
3. In the review, open the **module spec in GitLab** and let the Product Owner explain the capability from it: flows, rules, errors, restart.
   No COBOL on screen.
4. Open two or three rules and click through their evidence into the code, so the SME confirms the spec says what the code does.
5. Show the Duo Code Review comments and the checklist ticks in the merge request.

## What still needs other people

- **Product Owner:** picks the capability, reads the spec, answers open questions, approves.
- **Mainframe SME:** reviews the evidence and sets `reviewed`.
- **Lead:** approves any change to the templates or checklists.
