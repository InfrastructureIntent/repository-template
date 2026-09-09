# Changelog

All notable changes to this repository should be documented in this file.

The format is based on [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/), and this repository should use Semantic Versioning where versioned artifacts are produced.

## [Unreleased]

### Added

- Initial repository baseline.
- Architecture validation for root-solution membership, production Xml2Doc opt-in, test-project Xml2Doc exclusion, and required release-governance files.

### Changed

- Architecture Check now provisions the SDK from `global.json` so solution membership validation uses the repository's declared .NET baseline.

### Deprecated

### Removed

### Fixed

### Security

<!--
At release time:

1. Move released entries out of [Unreleased] into a versioned section.
2. Use an ISO date, for example: ## [0.1.0] - 2026-09-08
3. Add compare/release links only after the referenced tag exists.
4. Keep [Unreleased] at the top for subsequent work.
-->
