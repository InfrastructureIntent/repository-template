# Repository Iteration Logs

This directory contains repository-local execution history.

Create one log per active work day:

```text
docs/iterations/YYYY/YYYY-MM-DD.md
```

Use the following structure as a minimum:

```markdown
# YYYY-MM-DD — <short focus>

## Scope

- Governing issue/milestone
- Bounded objective for the day

## Work completed

- ...

## Decisions / observations

- ...

## Validation evidence

- Restore/build/test results
- Architecture/conformance checks
- Relevant PR/commit/package evidence

## Risks / blockers

- ...

## Architecture implications

- None, or describe evidence that may require a canonical documentation/ADR update

## Next

- ...
```

Iteration logs are not canonical architecture documents. Cross-repository weekly reporting may aggregate these logs with GitHub issue/PR/milestone activity.
