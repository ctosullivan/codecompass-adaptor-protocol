# codecompass-adaptor-protocol

A small, language-neutral JSON protocol for **external CodeCompass
adapters** — independent processes that analyze a project in some
package ecosystem and report structured results back to
[CodeCompass](https://github.com/ctosullivan/codecompass) over stdin/stdout.

This repository is the **contract only**: schemas, human-readable
documentation, worked examples, and conformance test vectors. It ships
**no code in any language**, and depends on nothing from CodeCompass or
from any adapter. That is deliberate — the protocol is licensed
permissively (MIT) precisely so that any adapter, in any language, under
any license (including a proprietary one), can implement it without
inheriting license obligations from the contract itself. See
[`SCHEMA.md`](SCHEMA.md) for the full specification.

## Why this exists

CodeCompass's usual adapters (npm, Python, Cargo) are in-process Python
classes. That works when an adapter's implementation can safely live as
importable GPL-licensed Python code inside CodeCompass's own process. It
does not work for every future case — a hypothetical COBOL/mainframe
adapter suite, for example, might need a different license or a
different language runtime entirely. This protocol is the reference
contract for that second kind of adapter: a separate OS process,
speaking a small, versioned JSON protocol, that CodeCompass invokes as
an opaque subprocess and never imports code from.

The first real implementation of this protocol is
[`codecompass-adaptor-haskell`](https://github.com/ctosullivan/codecompass-adaptor-haskell),
built for CodeCompass's Phase 60.

## What's in this repository

- [`SCHEMA.md`](SCHEMA.md) — the canonical, human-readable protocol
  specification: message framing, methods, capabilities, error shape.
- [`schemas/`](schemas/) — JSON Schema (2020-12) documents for every
  request/response/error shape `SCHEMA.md` describes.
- [`examples/`](examples/) — worked, valid example messages, one per
  schema.
- [`conformance/`](conformance/) — test vectors (`valid/` and `invalid/`
  message fixtures) any implementation can validate its own messages
  against, using an off-the-shelf JSON Schema validator in whatever
  language it's written in. This repository intentionally does **not**
  bundle an executable validation harness — that would tie the contract
  to one language's tooling. See [`conformance/README.md`](conformance/README.md).

## Versioning

This repository's own release version (semver, e.g. `0.1.0`) is
**distinct** from the wire-level `protocol_version` integer negotiated
at runtime inside `initialize` (see `SCHEMA.md`). A release here can fix
documentation, add examples, or add conformance vectors without bumping
`protocol_version` — that integer changes only when the wire contract
itself changes in a way that isn't backward compatible.

## Design principles

- **Deliberately minimal.** Three methods (`initialize`,
  `analyze_project`, `shutdown`), a closed capability list, a closed
  error-code set. Not gRPC, not a network service, not a plugin
  marketplace, not a generalized SDK — see `SCHEMA.md`'s own "what this
  is not" section.
- **Language-neutral.** No field or shape here references any specific
  programming language, package ecosystem, or CodeCompass-internal data
  structure.
- **License-neutral.** MIT, and no code, so depending on this repository
  never triggers copyleft obligations for an adapter under any license.

## Relationship to CodeCompass

This repository has no dependency, in either direction, on the
CodeCompass repository. CodeCompass consumes this contract by checking
it out as a git submodule (see CodeCompass's own
`docs/external-adapters.md`) purely for local development convenience —
not as a code or build dependency.

## License

MIT — see [`LICENSE`](LICENSE).
