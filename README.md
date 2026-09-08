# InfrastructureIntent C# Repository Template

This repository defines the standard C# repository layout and engineering defaults for InfrastructureIntent repositories.

## Standard layout

```text
/
├── <RepositoryName>.sln
├── Directory.Build.props
├── Directory.Packages.props
├── global.json
├── .editorconfig
├── AGENTS.md
├── README.md
├── LICENSE
├── src/
│   └── <production projects>
├── test/
│   └── <test projects>
├── docs/
│   └── iterations/
└── .github/
    ├── copilot-instructions.md
    ├── pull_request_template.md
    └── workflows/
        ├── ci.yml
        ├── package.yml
        └── architecture-check.yml
```

## Project-file policy

Solution-wide defaults belong in `Directory.Build.props` and package versions belong in `Directory.Packages.props`.

A project file should contain only metadata or build behavior that is specific to that project. Do not duplicate repository-wide target framework, language, documentation, deterministic-build, analyzer, or package defaults into every `.csproj`.

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

  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" />
    <PackageReference Include="xunit.v3" />
    <PackageReference Include="xunit.runner.visualstudio" />
  </ItemGroup>
</Project>
```

## Build defaults

`Directory.Build.props` is authoritative for common project behavior. It currently establishes:

- modern C# language defaults;
- nullable reference types;
- implicit usings;
- deterministic and continuous-integration builds;
- XML documentation generation for production projects;
- Xml2Doc Markdown API generation for production projects;
- test-project opt-out from public XML/Markdown API documentation;
- package metadata defaults that can be overridden by a project when genuinely project-specific.

## Package versions

Use Central Package Management through `Directory.Packages.props`. Individual project files declare package dependencies without versions.

## Workflows

- `ci.yml` runs restore, build, and tests for pull requests and pushes to `main`.
- `package.yml` proves package production on `main` and supports release packaging without publishing by default.
- `architecture-check.yml` provides the standard hook for deterministic architecture rules and the future advisory AI architecture review.

## Repository-local agent state

Each repository keeps its own `AGENTS.md` and `docs/iterations/YYYY/YYYY-MM-DD.md` execution history. Cross-repository architecture and standards remain canonical in `InfrastructureIntent/documentation`.

## Using this template

When creating a new repository from this template:

1. Rename `<RepositoryName>.sln` to the repository/product solution name.
2. Add production projects under `src/`.
3. Add test projects under `test/`.
4. Add all projects to the root solution.
5. Replace template placeholders in package/repository metadata.
6. Keep project-specific values in each `.csproj`; keep shared policy in root build files.
7. Ensure CI is green before beginning substantive implementation.

## License

This repository template is licensed under the Apache License 2.0. Generated repositories must apply the license appropriate to that repository's ownership boundary; the InfrastructureIntent Engine implementation is not automatically Apache-licensed merely because its repository began from this template.