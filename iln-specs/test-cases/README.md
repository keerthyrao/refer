# Test cases

The lasting test suite for the iln application, written **from the specifications** by the Testing agent (`agents/testing.md`).
Expected results come from the spec, so a difference between code and spec shows up as a failed test.
(The existing tests that are already in `iln` live in its `test/` folder; the inventory lists them.)

```
test-cases/
  iln/
    programs/<PROGRAM>.md     test cases per program: one or more per business rule and error condition
    modules/<module>.md       end-to-end cases per module flow
```

Test case ids: `TC-<PROGRAM or MODULE>-<nnn>`. Types: smoke, integration, regression (with unit, boundary and restart as sub-types).
Template: `agents/templates/test-cases.md`. Results for a story go in `delivery/<JIRA-KEY>/test-results.md`.

Test data is made up for the purpose. Never use production data or copies of it.
