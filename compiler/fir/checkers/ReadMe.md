# FIR: Checkers

This module is the final and most user-facing stage of the FIR frontend pipeline. After the [resolve](../resolve/ReadMe.md) phase has enriched the FIR tree with full semantic information, the checkers are responsible for traversing the tree to find and report any diagnostics, such as errors (e.g., type mismatches, unresolved references) and warnings (e.g., deprecated usages, redundant casts).

## Key Concepts

- **Diagnostic Reporting:** The primary purpose of this module is to populate a list of diagnostics that will be presented to the user. Each diagnostic includes an error or warning message, the location in the source code, and a severity level.
- **Visitor-Based Analysis:** Like other FIR phases, checkers are implemented as visitors that traverse the resolved FIR tree. Each visitor is typically responsible for a specific set of checks on a particular type of FIR element (e.g., a `FirFunctionCallChecker`, `FirClassChecker`).
- **Platform-Specific Diagnostics:** A key feature of the Kotlin compiler is its ability to target multiple platforms. Some language rules are universal, while others are specific to a particular target (e.g., JVM, JS, Native). This module is structured to support both common and platform-specific checks.

## Sub-modules

The `checkers` module is organized to handle the separation between common and platform-specific diagnostics:

- **`/src`:** This directory contains the core checker infrastructure and the implementations of all **common checkers**. These are diagnostics that apply to Kotlin code regardless of its final compilation target.
- **`/checkers.jvm`:** Contains checkers for diagnostics that are specific to the JVM platform.
- **`/checkers.js`:** Contains checkers for diagnostics that are specific to the JavaScript platform.
- **`/checkers.native`:** Contains checkers for diagnostics that are specific to the Kotlin/Native platform.
- **`/checkers.wasm`:** Contains checkers for diagnostics that are specific to the WebAssembly platform.
- **`/checkers.web.common`:** Contains checkers for diagnostics that are common to all web-based platforms (JS and Wasm).
- **`/checkers-component-generator`:** A source generator tool that helps to automatically wire up the various checker components, reducing boilerplate and ensuring consistency.
