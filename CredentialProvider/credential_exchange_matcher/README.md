# Credential Manager WASM Adapter (Rust)

This project provides a Rust implementation of a WebAssembly (WASM) module designed to interface with a host **Credential Manager** system.

It acts as a secure, logic layer that parses incoming credential requests, processes a custom binary/JSON credential store format, and returns matching exporter to the host environment via the `credman` import module.

## Overview

This library offers a **Rust** implementation for:
* **Memory Safety**: Eliminates manual pointer arithmetic errors common in the C version.
* **Robust Parsing**: Uses `serde` and `serde_json` for type-safe JSON handling.
* **WASM-First**: Built specifically for the `wasm32-unknown-unknown` target with optimized binary sizes.

### The Use Case
This WASM binary is not a standalone application. It is designed to be loaded by a specific host application (e.g., an Android Credential Provider or a browser extension) that implements the `credman` import module.

**The Workflow:**
1.  **Input:** The host provides a Request JSON and a binary blob containing stored credentials (custom header layout + JSON).
2.  **Processing:** The WASM parses the binary headers to locate icons and JSON data, then filters exporters based on the `credentialTypes` requested.
3.  **Output:** Matches are returned to the host via the `AddExportEntry` FFI boundary.

## 🛠 Prerequisites

* **Rust**: Stable release (install via [rustup](https://rustup.rs/)).
* **Make**: For running the build scripts.
* **WASM Target**: You need the 32-bit WASM target installed:
    ```bash
    rustup target add wasm32-unknown-unknown
    ```

## 📦 Building the Project

We provide a `Makefile` to handle building, optimization, and license attribution in a single step.

For convenience, the wasm binary hex that is produced from this library is copied to `credential_exchange_matcher.hex`. This can be used directly to interact with Credential Manager.

### 1. Build and Generate Licenses
Run the default command to build the release binary and generate the dependency license file:

```bash
make

## 📄 Licensing & Compliance

This project is configured to be **Apache-2.0** compliant.

* **Dependencies**: All Rust dependencies (`serde`, `serde_json`) are used under their Apache-2.0 license option.
* **Attribution**: The `make` command automatically generates a `THIRD_PARTY_LICENSES.txt` file using `cargo vendor`. This file contains the full license text for all static dependencies linked into the final WASM binary.

## 🤝 Contributing

1.  Ensure you have the WASM target installed (`make setup`).
2.  Make changes to `src/lib.rs`.
3.  Ensure `serde` structs match any changes to the Host JSON schema.
4.  Run `make` to verify compilation and license generation.