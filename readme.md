creedengo-rust
================

_Creedengo_ is a collective project aiming to reduce environmental footprint of software at the code level. The goal of the project is to provide a list of static code analyzers to highlight code structures that may have a negative ecological impact: energy and resources over-consumption, "fatware", shortening terminals' lifespan, etc.

_Creedengo_ is based on evolving catalogs of [good practices](https://github.com/green-code-initiative/creedengo-rules-specifications/blob/main/docs/rules), for various technologies.

This set of [Clippy](https://github.com/rust-lang/rust-clippy) linters implements these catalogs as rules for scanning your Rust projects using [DyLint](https://github.com/trailofbits/dylint).

> ⚠️ 🚧 This is still a very early stage project 🚧. Any feedback or contribution will be highly appreciated. Please refer to the contribution section.

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0) [![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg)](https://github.com/green-code-initiative/creedengo-common/blob/main/doc/CODE_OF_CONDUCT.md)

🚀 Getting Started
------------------

[Follow the get started file](getstarted.md)

🧩 Compatibility
-----------------
 
This project requires:
- Rust nightly toolchain (for dylint development)
- cargo-dylint and dylint-link tools
- VS Code with rust-analyzer extension (for IDE integration)

🌿 Rules
-------------------

|Id|Description|Severity|Enabled|
|--|-----------|:------:|:--------:|
|[GCI2](https://github.com/green-code-initiative/creedengo-rules-specifications/blob/main/src/main/rules/GCI2/GCI2.json)|Avoid multiple if-else statement|⚠️|✅|

🤝 Contribution
---------------

See [contribution](https://github.com/green-code-initiative/creedengo-rules-specifications#-contribution) on the central Creedengo repository.

🤓 Main contributors
--------------------

See [main contributors](https://github.com/green-code-initiative/creedengo-rules-specifications#-main-contributors) on the central Creedengo repository.
