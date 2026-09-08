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
</Project>
```

Typical test project:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <IsTestProject>true</IsTestProject>
    <IsPackable>false</IsPackable>
  </PropertyGroup>

  <!-- Add the repository's approved test framework dependencies here. -->
</Project>
```

## Build defaults

`Directory.Build.props` is authoritative for common project behavior. It establishes:

- .NET 10 / modern C# defaults;
- nullable reference types;
- implicit usings;
- deterministic and continuous-integration builds;
- current recommended .NET analyzers;
- warnings as errors;
- XML documentation generation for production projects;
- Xml2Doc Markdown API generation for production projects;
- common package metadata that is safe across repository types.

`Directory.Build.targets` handles rules that require the fully evaluated project. In particular, test projects automatically opt out of public XML/Xml2Doc documentation while production projects receive `Xml2Doc.MSBuild` as private build tooling.

## Package versions

Use Central Package Management through `Directory.Packages.props`. Individual project files declare package dependencies without versions.

The baseline currently pins `Xml2Doc.MSBuild` 2.4.0. Test-framework and other dependency versions should be added centrally when the repository introduces those dependencies.

## Workflows

- `build.yml` restores and performs a Release build for pull requests and pushes to `main`.
- `test.yml` builds and runs the repository test suite for pull requests and pushes to `main`.
- `package.yml` proves package production on pull requests and `main` without publishing.
- `architecture-check.yml` enforces repository structure and provides the disabled hook for the future advisory AI architecture reviewer.

Actual publication is intentionally separate from package validation until feed, signing, versioning, and release-trigger policy are established.

## Repository-local agent state

Each repository keeps its own `AGENTS.md` and `docs/iterations/YYYY/YYYY-MM-DD.md` execution history. Cross-repository architecture and standards remain canonical in `InfrastructureIntent/documentation`.

## Using this template

When creating a new repository from this template:

1. Rename `RepositoryName.sln` to the repository/product solution name.
2. Replace `REPOSITORY_NAME` in root build metadata.
3. Choose the repository's license **before substantive code is committed**. Do not inherit a license merely because the repository came from this template.
4. Add production projects under `src/`.
5. Add test projects under `test/`.
6. Add all projects to the root solution.
7. Add package versions centrally in `Directory.Packages.props`.
8. Keep project-specific values in each `.csproj`; keep shared policy in root build files.
9. Ensure Build, Test, Package, and Architecture Check workflows are green before substantive implementation proceeds.

For InfrastructureIntent-owned repositories, licensing must follow the canonical IP/licensing policy in `InfrastructureIntent/documentation`—in particular, the controlled Engine implementation and public extension ecosystem do not use the same license.

## Template license

The files in **this template repository** are licensed under the Apache License 2.0; see `TEMPLATE-LICENSE`.

That license applies to the reusable template material itself. A repository generated from this template must establish its own `LICENSE` and package-license metadata according to that repository's ownership boundary. The InfrastructureIntent Engine is not automatically Apache-licensed because it began from this template.