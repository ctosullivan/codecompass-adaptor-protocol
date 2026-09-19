# Changelog

All notable changes to this project are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), versioning
follows [SemVer](https://semver.org/).

## [Unreleased]

## [0.1.0] - 2026-09-19

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
- `analyze_project-response.json`'s `symbols` items gained two optional
  fields, `kind` (`"export"` | `"reexport"` | `"undetermined"`, default
  `"export"`) and `note` (nullable free text) — needed by the reference
  Haskell adapter to represent a module re-export entry or a
  build-conditional-gated name it cannot confidently resolve, without
  overstating confidence as a plain `"export"` entry would. Additive and
  backward compatible — no `protocol_version` change.
