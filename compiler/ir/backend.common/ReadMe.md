# IR Backend: Common

This module is the heart of the unified backend system. It contains the shared logic, data structures, and a library of transformation phases that are common to all Kotlin compiler backends (JVM, JS, Native, Wasm). Its primary purpose is to take the "raw" [IR tree](../ir.tree/ReadMe.md) from the frontend and progressively simplify it into a form that is easy for the platform-specific backends to consume.

This simplification process is done through a series of transformations known as **lowerings**.

## Lowerings

A **lowering** is a compiler pass that traverses the IR tree and transforms a high-level, Kotlin-specific language construct into a simpler, more fundamental one. The goal is to eliminate complex constructs so that the final code generation phase can be as straightforward as possible.

This module provides a library of common lowering phases. However, **not all lowerings are used on all backends, and they are not necessarily run in the same order.** Each backend is responsible for choosing the specific set of common lowerings it needs and combining them with its own custom, platform-specific phases.

Examples of common lowerings include:

-   **`LateinitLowering`:** Replaces `lateinit` property accesses with explicit null checks and exceptions.
-   **`ForLoopsLowering`:** Converts `for` loops over iterables into simpler `while` loops with explicit `iterator` and `hasNext()` calls.
-   **`DataClassLowering`:** Synthesizes the `equals()`, `hashCode()`, `toString()`, and `componentN()` methods for data classes.
-   **`CoercionToUnitLowering`:** Inserts explicit coercions to `Unit` where required by the language semantics.
-   **`SuspendFunctionsLowering`:** Transforms `suspend` functions into state machines, which is the foundation of how coroutines are implemented.

## Structure

The `src` directory contains the implementations of the various lowering passes, as well as the common infrastructure for managing the compilation context, which is shared across all backends. This ensures that all backends can operate on a consistent and predictable IR structure before they begin their platform-specific code generation.
