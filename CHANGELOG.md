# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2026-04-02

### Added

- Initial release extracted from [ClaudioOS](https://github.com/suhteevah/claudio-os)
- Cranelift-based x86_64 code generation (no LLVM)
- `no_std` + `alloc` compatible for bare-metal targets
- JIT proof-of-concept: compile `fn add(a: i64, b: i64) -> i64` and execute it
- `test()` function to verify Cranelift initialization
- `test_jit()` function for full compile-and-execute round trip
