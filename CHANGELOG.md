# Changelog

All notable changes to this project are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), versioning
follows [SemVer](https://semver.org/).

## [Unreleased]

## [0.1.0] - TBD

Initial protocol version, built as CodeCompass's Phase 60 (minimal
external Haskell adapter).

### Added

- `SCHEMA.md` — protocol specification: JSON Lines framing over stdio,
  `initialize`/`analyze_project`/`shutdown` methods, a closed
  capability list (`dependencies`, `symbols`, `observations`,
  `diagnostics`), a closed error-code set.
- `schemas/` — JSON Schema (2020-12) documents for every message shape.
- `examples/` — worked example messages for every schema.
- `conformance/` — valid/invalid test vectors for schema validation by
  any implementation, in any language.
