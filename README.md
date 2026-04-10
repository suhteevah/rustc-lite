# rustc-lite

[![Crates.io](https://img.shields.io/crates/v/rustc-lite.svg)](https://crates.io/crates/rustc-lite)
[![docs.rs](https://img.shields.io/docsrs/rustc-lite)](https://docs.rs/rustc-lite)
[![License](https://img.shields.io/crates/l/rustc-lite.svg)](https://github.com/suhteevah/rustc-lite#license)

Minimal `no_std` Rust compiler using the **Cranelift** backend -- no LLVM required.

Compiles a subset of Rust to x86_64 machine code directly, suitable for bare-metal
and embedded JIT compilation scenarios. Originally built as part of
[ClaudioOS](https://github.com/suhteevah/claudio-os), a bare-metal Rust operating
system that runs AI coding agents on raw hardware.

## What it does

- **Cranelift code generation** -- uses the Cranelift compiler backend to produce
  native x86_64 machine code without any LLVM dependency
- **`no_std` compatible** -- works with only `alloc`, no standard library required
- **JIT execution** -- compile functions at runtime and call them immediately
- **Bare-metal ready** -- designed to run on bare metal with no OS underneath

## Supported Rust features

This is an early-stage compiler. Currently it provides:

- Function creation with Cranelift IR
- Integer arithmetic (`i64` operations: add, sub, mul, etc.)
- SystemV calling convention for x86_64
- Direct machine code emission and execution

The compiler is being actively developed to support more Rust constructs including
variables, control flow, structs, and basic type checking.

## Usage

Add to your `Cargo.toml`:

```toml
[dependencies]
rustc-lite = "0.1"
```

### Verify Cranelift initialization

```rust
assert!(rustc_lite::test());
```

### JIT compile and execute a function

```rust
// Compiles: fn add(a: i64, b: i64) -> i64 { a + b }
// Then calls add(3, 4) and verifies the result is 7
assert!(rustc_lite::test_jit());
```

### Building Cranelift IR manually

The crate re-exports nothing from Cranelift directly, but you can use the same
pattern shown in `test_jit()` to build your own functions:

1. Create an x86_64 ISA via `cranelift_codegen::isa::lookup_by_name`
2. Define a `Signature` with `AbiParam` types
3. Build IR using `FunctionBuilder`
4. Compile with `Context::compile`
5. Copy the resulting machine code to executable memory and call it

## Cranelift `no_std` forks

**Important:** The upstream `cranelift-codegen` and `cranelift-frontend` crates
depend on `std`. To use this crate in a `no_std` / bare-metal environment, you
need patched forks of the Cranelift crates.

The ClaudioOS project maintains these forks:

| Crate | Fork |
|-------|------|
| `cranelift-codegen` | `cranelift-codegen-nostd` |
| `cranelift-frontend` | `cranelift-frontend-nostd` |
| `cranelift-codegen-shared` | `cranelift-codegen-shared-nostd` |
| `cranelift-control` | `cranelift-control-nostd` |
| `rustc-hash` | `rustc-hash-nostd` |
| `arbitrary` | `arbitrary-stub` |

To use them, add `[patch.crates-io]` entries in your workspace `Cargo.toml`:

```toml
[patch.crates-io]
cranelift-codegen = { path = "path/to/cranelift-codegen-nostd" }
cranelift-frontend = { path = "path/to/cranelift-frontend-nostd" }
cranelift-codegen-shared = { path = "path/to/cranelift-codegen-shared-nostd" }
cranelift-control = { path = "path/to/cranelift-control-nostd" }
rustc-hash = { path = "path/to/rustc-hash-nostd" }
arbitrary = { path = "path/to/arbitrary-stub" }
```

If you are targeting a hosted platform (Linux, macOS, Windows), the upstream
Cranelift crates work as-is and no patches are needed.

## Origin

This crate was extracted from the [ClaudioOS](https://github.com/suhteevah/claudio-os)
project -- a bare-metal Rust OS that boots via UEFI and runs multiple AI coding
agents (Claude) simultaneously with no Linux kernel, no POSIX, and no JavaScript
runtime. `rustc-lite` provides the self-hosting compilation foundation, allowing
the OS to compile Rust code on bare metal using Cranelift.

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or <http://www.apache.org/licenses/LICENSE-2.0>)
- MIT License ([LICENSE-MIT](LICENSE-MIT) or <http://opensource.org/licenses/MIT>)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.

---

---

---

---

---

---

## Support This Project

If you find this project useful, consider buying me a coffee! Your support helps me keep building and sharing open-source tools.

[![Donate via PayPal](https://img.shields.io/badge/Donate-PayPal-blue.svg?logo=paypal)](https://www.paypal.me/baal_hosting)

**PayPal:** [baal_hosting@live.com](https://paypal.me/baal_hosting)

Every donation, no matter how small, is greatly appreciated and motivates continued development. Thank you!
