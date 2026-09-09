# GitHub Copilot Instructions — InfrastructureIntent

Follow the canonical InfrastructureIntent architecture and standards in the `InfrastructureIntent/documentation` repository.

## Architecture rules

- Treat InfrastructureIntent as an infrastructure intent compiler, not a Terraform-first transformation tool.
- Keep Adapter concerns limited to source translation into Parsed Intent.
- Keep infrastructure-domain semantics in Integrations and their Domain Abstractions.
- Keep graph mechanics, identity registration, canonical reference resolution, graph validation, and deterministic managed ordering in Engine contracts/implementation.
- Keep Backend responsibility limited to lowering valid domain semantics into one Target contract and validating Target representability.
- Keep Target responsibility limited to Target-model validation and emission.
- Do not let Backends reopen raw or Parsed Intent to recover accepted domain meaning.
- Keep `CompilationContext` scoped to one Intent/compilation; never introduce global compilation state.
- Keep domain semantic type independent from managed/existing lifecycle.
- Typed references target domain contracts, not lifecycle-specific implementation classes.
- Integrations own canonical ResourceKey construction and dependency semantics/direction.
- Relationships express domain meaning; dependencies express prerequisites/order. Do not invent synthetic relationships solely for ordering.
- Treat public contract generations as immutable in required shape/semantics once published; breaking changes require a new generation.

## Repository rules

- Root solution only.
- Production projects under `src/`.
- Test projects under `test/`.
- Shared MSBuild policy belongs in `Directory.Build.props` / `Directory.Build.targets`.
- Central package versions belong in `Directory.Packages.props`.
- `.csproj` files contain project-specific metadata and dependencies, not duplicated repository defaults.
- Production projects that generate API documentation explicitly reference `Xml2Doc.MSBuild`; test projects do not.
- Public production APIs require XML documentation; Xml2Doc-generated Markdown must stay current.
- Test projects do not require public XML/Xml2Doc documentation.
- Add/update tests for behavioral changes.
- Prefer deterministic output and stable diagnostics.
- Aggregate independent user-correctable validation failures where safe; fail fast only when continuation would be invalid or misleading.

## Milestone and release rules

- Planned implementation work requires a governing GitHub issue assigned to an established milestone before coding begins.
- Milestones represent coherent release scope rather than schedules; due dates are optional.
- Keep changes bounded to the governing issue. Do not opportunistically implement unrelated TODO items.
- `TODO.md` is an orientation aid, not an authoritative backlog. GitHub Issues and Milestones are authoritative.
- Keep `CHANGELOG.md` current for notable changes and retain `[Unreleased]` at the top.
- Keep `RELEASE_NOTES.md` aligned with the active milestone and actual release state.
- Update the repository-local iteration log for substantive work.
- Do not move or recreate an existing release tag to make documentation match; correct post-release documentation on `main` instead.

## Review behavior

When suggesting or reviewing a change:

- call out architecture-boundary violations even when the code compiles and tests pass;
- verify the change is bounded to the governing milestone-assigned issue;
- call out stale documentation, release metadata, tests, or generated API documentation caused by the change;
- distinguish actionable findings from informational observations;
- prefer the smallest change that preserves published contracts and ownership boundaries;
- do not recommend suppressing diagnostics or weakening tests merely to make CI green;
- do not invent architecture or public contracts that are not required by the active issue.
