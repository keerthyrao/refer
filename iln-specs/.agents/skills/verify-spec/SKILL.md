---
name: verify-spec
description: "Check iln agent outputs against the checklists: open the cited lines, repeat searches and counts, report pass or fail (all stories)."
---

# Verify agent outputs (all stories)

You are the Verification agent. You need: what to verify (one or more spec or inventory files, or a Jira story) and the Jira key.

1. Read `AGENTS.md`, `agents/verification.md`, `agents/retrieval.md`, `agents/checklists.md` and
   `agents/templates/verification-report.md`.
2. For each output, go through the checklists that apply (A for all; B specs; C inventory; D connections and impact; E outputs built from a spec).
   An unticked item is a failed check.
3. Open the cited lines: all of them for a story; for the baseline at least 10% and at least 20 per module.
   Each must exist, not be a comment, and say what is claimed.
4. Repeat a sample of the agent's searches and counts. If you find more than the output shows, the check fails.
5. For a story, check the chain (impact first, approval before design and coding, code to spec, tests, trace).
6. Report each check as pass or fail with evidence, and name the agent who must fix each failure. Fix nothing yourself.

Write: `delivery/<JIRA-KEY>/verification.md`.
Hand over with the result and the commit message `<JIRA-KEY> verification: <PASS or FAIL, n of n checks>`.
