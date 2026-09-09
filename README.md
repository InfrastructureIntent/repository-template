# InfrastructureIntent C# Repository Template

This repository defines the standard C# repository layout and engineering defaults for InfrastructureIntent repositories.

## Standard layout

```text
/
├── <RepositoryName>.sln
├── Directory.Build.props
├── Directory.Build.targets
├── Directory.Packages.props
├── global.json
├── .editorconfig
├── AGENTS.md
├── README.md
├── CHANGELOG.md
├── TODO.md
├── RELEASE_NOTES.md
├── LICENSE                       # selected explicitly for the generated repository
├── src/
│   └── <production projects>
├── test/
│   └── <test projects>
├── docs/
│   └── iterations/
└── .github/
    ├── copilot-instructions.md
    ├── pull_request_template.md
    ├── ISSUE_TEMPLATE/
    └── workflows/
        ├── build.yml
        ├── test.yml
        ├── package.yml
        └── architecture-check.yml
```

## Project-file policy

Solution-wide defaults belong in `Directory.Build.props`; late-evaluated enforcement belongs in `Directory.Build.targets`; package versions belong in `Directory.Packages.props`.

A project file should contain only metadata, dependencies, or build behavior specific to that project. Do not duplicate repository-wide target framework, language, documentation, deterministic-build, analyzer, or package-version policy into every `.csproj`.

Typical production project:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <AssemblyName>InfrastructureIntent.Example</AssemblyName>
    <RootNamespace>InfrastructureIntent.Example</RootNamespace>
    <PackageId>InfrastructureIntent.Example</PackageId>
    <Description>Project-specific description.</Description>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Xml2Doc.MSBuild" PrivateAssets="all" />
  </ItemGroup>
</Project>
```

A production project that intentionally does not generate Xml2Doc API documentation must make that decision explicit in the project file:

```xml
<PropertyGroup>
  <Xml2Doc_Enabled>false</Xml2Doc_Enabled>
</PropertyGroup>
```

Typical test project:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <IsTestProject>true</IsTestProject>
    <IsPackable>false</IsPackable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" />
    <PackageReference Include="xunit.v3" />
    <PackageReference Include="xunit.runner.visualstudio" PrivateAssets="all" />
  </ItemGroup>
</Project>
```

`global.json` selects `Microsoft.Testing.Platform`, and `xunit.v3` is the Microsoft.Testing.Platform-compatible xUnit v3 test framework used by that runner. The template also includes `Microsoft.NET.Test.Sdk` and `xunit.runner.visualstudio` as part of its standard VSTest/IDE compatibility baseline; they should not be interpreted as requirements imposed by Microsoft.Testing.Platform itself. A repository may deliberately standardize on a different MTP-compatible test framework or a narrower runner/tooling surface, but that is an explicit repository-level deviation from this template baseline. A newly initialized test project should include at least one real smoke test so CI proves test discovery/execution and does not fail with a zero-tests result.

## Build defaults

`Directory.Build.props` is authoritative for common project behavior. It establishes:

- .NET 10 / modern C# defaults;
- nullable reference types;
- implicit usings;
- deterministic and continuous-integration builds;
- current recommended .NET analyzers;
- warnings as errors;
- XML documentation generation for production projects;
- shared Xml2Doc Markdown API-generation defaults for production projects;
- common package metadata that is safe across repository types.

`Directory.Build.targets` handles rules that require the fully evaluated project. Test projects automatically opt out of public XML/Xml2Doc documentation and suppress `CS1591`.

Xml2Doc uses an explicit production-project intent model. Each production `.csproj` must either reference `Xml2Doc.MSBuild` with `PrivateAssets="all"` to generate API documentation, or explicitly set `<Xml2Doc_Enabled>false</Xml2Doc_Enabled>` when that project intentionally does not generate Xml2Doc output. Test projects must not reference `Xml2Doc.MSBuild`. The version is owned centrally by `Directory.Packages.props`, while the shared Xml2Doc behavior is owned by `Directory.Build.props`.

## Package versions

Use Central Package Management through `Directory.Packages.props`. Individual project files declare package dependencies without versions.

The baseline currently pins `Microsoft.NET.Test.Sdk` 18.9.0, `xunit.v3` 4.0.0, `xunit.runner.visualstudio` 4.0.0, and `Xml2Doc.MSBuild` 2.4.0. Add other dependency versions centrally when the repository introduces those dependencies.

## Workflows

- `build.yml` restores and performs a Release build for pull requests and pushes to `main`.
- `test.yml` builds and runs the repository test suite for pull requests and pushes to `main`.
- `package.yml` proves package production on pull requests and `main` without publishing.
- `architecture-check.yml` enforces repository structure and provides the disabled hook for the future advisory AI architecture reviewer.

Actual publication is intentionally separate from package validation until feed, signing, versioning, and release-trigger policy are established.

## Milestones and release documentation

GitHub Issues and Milestones are the authoritative planning system for planned implementation work.

- Planned implementation SHALL have a GitHub issue assigned to an established milestone before implementation begins.
- Milestones represent coherent release scope rather than schedules; due dates are optional.
- Implementation PRs should correspond to milestone-assigned issues.
- `CHANGELOG.md` follows Keep a Changelog 1.1.0 and records curated notable changes as work lands. Keep `[Unreleased]` at the top and add version/date/link information only when the corresponding release artifact exists.
- `TODO.md` is a lightweight repository-facing view of future work. It is not a second backlog and does not authorize implementation by itself.
- `RELEASE_NOTES.md` is the evolving human-readable narrative for the active milestone and may seed a GitHub Release description at closeout.
- Package validation and package publication are separate concerns; each repository defines its publication policy explicitly.

At release closeout, update the changelog and release notes to reflect what actually shipped, record validation/publication evidence, and keep existing release tags immutable.

The source template is a special case because `CHANGELOG.md`, `TODO.md`, and `RELEASE_NOTES.md` are seed files copied into generated repositories. They remain clean generated-repository starting state. The repository-template's own version history is recorded in its GitHub Releases instead of being written into those seed files.

## Repository-local agent state

Each repository keeps its own `AGENTS.md` and `docs/iterations/YYYY/YYYY-MM-DD.md` execution history. Cross-repository architecture and standards remain canonical in `InfrastructureIntent/documentation`.

## Using this template

When creating a new repository from this template:

1. Rename `RepositoryName.sln` to the repository/product solution name.
2. Replace `REPOSITORY_NAME` in root build metadata.
3. Choose the repository's license **before substantive code is committed**. Do not inherit a license merely because the repository came from this template.
4. Establish the first release milestone and create issues for planned implementation work before coding begins.
5. Initialize `TODO.md` and `RELEASE_NOTES.md` for the active milestone; keep `CHANGELOG.md` current as changes land.
6. Add production projects under `src/`; each production project must either explicitly reference `Xml2Doc.MSBuild` for API documentation or explicitly set `Xml2Doc_Enabled=false`.
7. Add test projects under `test/`; use the approved Microsoft.Testing.Platform-compatible framework plus the repository's intended VSTest/IDE compatibility tooling, do not reference Xml2Doc, and include at least one real test so the test pipeline proves discovery/execution.
8. Add all projects to the root solution.
9. Add package versions centrally in `Directory.Packages.props`.
10. Keep project-specific values in each `.csproj`; keep shared policy in root build files.
11. Ensure Build, Test, Package, and Architecture Check workflows are green before substantive implementation proceeds.

For InfrastructureIntent-owned repositories, licensing must follow the canonical IP/licensing policy in `InfrastructureIntent/documentation`—in particular, the controlled Engine implementation and public extension ecosystem do not use the same license.

## Template license

The files in **this template repository** are licensed under the Apache License 2.0; see `TEMPLATE-LICENSE`.

That license applies to the reusable template material itself. A repository generated from this template must establish its own `LICENSE` and package-license metadata according to that repository's ownership boundary. The InfrastructureIntent Engine is not automatically Apache-licensed because it began from this template.
