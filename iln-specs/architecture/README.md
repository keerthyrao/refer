# Architecture

How the iln application is built, pulled from the code by the Architect agent (`agents/architect.md`).
The Coding agent follows these patterns so changes are built the way the rest of the application is.

```
architecture/
  iln/
    architecture.md                 overview: components, online, batch, data, interfaces, list of patterns
    patterns/<PATTERN-ID>.md        one per pattern, with at least three examples from the code
```

Pattern ids: `ONL` online (CICS), `BAT` batch (JCL), `DAT` data access, `STR` program structure, `NAM` naming, `XCT` cross-cutting.
Template: `agents/templates/architecture-pattern.md`.
