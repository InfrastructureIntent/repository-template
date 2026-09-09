# InfrastructureIntent Repository Agent Contract

This file defines the repository-local operating contract for AI coding agents.

## Authority

Organization-wide architecture and standards are canonical in `InfrastructureIntent/documentation`.

Repository-local implementation details may refine those standards but must not contradict accepted architecture decisions. If implementation pressure exposes a conflict, record the evidence and propose an architecture/documentation change rather than silently diverging.

## Required working behavior

Before substantive work:

1. Read this repository's `README.md`.
2. Read applicable canonical architecture/standards from `InfrastructureIntent/documentation`.
3. Review the current GitHub milestone and governing issue.
4. Confirm the planned implementation issue is assigned to an established milestone before coding begins.
5. Review `CHANGELOG.md`, `TODO.md`, and `RELEASE_NOTES.md` for the active release context.
6. Create or update today's repository-local iteration log under `docs/iterations/YYYY/YYYY-MM-DD.md`.

During work:

- keep changes bounded to the active issue/slice;
- preserve architecture ownership boundaries;
- add or update tests with behavioral changes;
- keep deterministic behavior where output ordering or generation is observable;
- do not suppress diagnostics merely to make tests green;
- do not put repository-wide build policy into individual `.csproj` files;
- keep package versions in `Directory.Packages.props`;
- keep production projects under `src/` and test projects under `test/`;
- update documentation when public behavior or contracts change;
- update `CHANGELOG.md` as notable changes land;
- keep `TODO.md` as an orientation aid only—GitHub Issues/Milestones remain authoritative;
- keep `RELEASE_NOTES.md` aligned with the active milestone and actual release state.

Before completion:

- run restore/build/test in Release configuration;
- run repository architecture checks;
- verify generated documentation is current when public APIs changed;
- record validation evidence, decisions, blockers, and next work in the iteration log;
- ensure the PR links the governing milestone-assigned issue;
- ensure release documentation reflects the change when it affects the active release scope.

## Milestones and release management

Milestones represent coherent release scope rather than schedules; due dates are optional.

Planned implementation must not begin without an issue assigned to an established milestone. `TODO.md` does not authorize implementation by itself.

Use the release files as follows:

- `CHANGELOG.md` — curated notable changes, following Keep a Changelog 1.1.0 with `[Unreleased]` retained at the top;
- `TODO.md` — lightweight repository-facing candidate/current-work orientation only;
- `RELEASE_NOTES.md` — evolving human-readable release narrative for the active milestone.

At release closeout, update release documentation to reflect what actually shipped, record validation/publication evidence, and do not move or recreate an existing release tag.

## Iteration logs

Iteration logs are execution records, not canonical architecture documents. Record:

- work attempted/completed;
- decisions made or questions raised;
- tests and evidence;
- issue/PR references;
- blockers/risks;
- architecture implications;
- next work.

Cross-repository weekly summaries are generated from these local logs and GitHub activity.

## Commit and PR discipline

Commits should be small enough to explain and review. PR descriptions should state the problem, architectural impact, implementation, and validation evidence.

Do not claim architecture compliance solely because code compiles. Changes must conform to the documented ownership and dependency boundaries as well as tests.