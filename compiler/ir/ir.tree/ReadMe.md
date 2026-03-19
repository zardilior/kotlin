# Compiler: Intermediate Representation (IR) Tree

This module defines the core data structures for the Kotlin compiler's **Intermediate Representation (IR)**, as well as many utilities and common code around it. The IR is a tree-based representation of the program that serves as the common language between the frontend and the various backends.

Unlike the [PSI tree](../../psi/ReadMe.md), which is a concrete syntax tree, the IR is a more abstract representation. It is designed to be semantically rich and largely independent of the specific syntax of the Kotlin language, making it optimized for the code generation and optimization tasks performed by the backends.

## Key Concepts

-   **Unified Backend Format:** The IR is **unified**. Both the old K1 frontend and the new K2 ([FIR](../../fir/ReadMe.md)) frontend are ultimately converted to this IR. This allows all backends (JVM, JS, Native, Wasm) to operate on a single, consistent data structure.

-   **Semantic Richness:** Every node in the IR tree is fully resolved. `IrCall` nodes, for example, have a direct reference to the `IrFunction` they are calling, eliminating the need for name resolution in the backends.

-   **Language-Agnostic Structure:** The IR structure is more generic and abstract than Kotlin source, making it easier for backends to translate it into their target instruction sets.

-   **Mutability:** The IR tree is mutable. The backends perform a series of transformations, known as "lowerings," which modify the tree in place to progressively simplify it for code generation.

-   **Lazy IR for External Code:** There are two implementations of the IR. For the currently compiled module, a regular, mutable IR is created. However, for external dependencies (like libraries), a **Lazy IR** is used. This approach avoids deserializing the entire library, loading only the portions that are actually referenced. Lazy IR is generally immutable and does not contain function bodies (except for inline functions).

## Code Generation

A significant portion of the IR tree's implementation is not written by hand but is **auto-generated**. The `tree-generator` sub-module contains a tool that takes a high-level description of the IR nodes and generates the corresponding Kotlin data classes and visitor implementations. This ensures consistency and reduces boilerplate.

## Structure

-   **`/src`**: Contains the core interfaces (`IrElement`), the hand-written parts of the IR implementation, and the K1 implementation of Lazy IR (`org/jetbrains/kotlin/ir/declarations/lazy`).
-   **`/gen`**: Contains the generated Kotlin source files for the IR tree nodes. **These files should not be edited manually.**
-   **`/tree-generator`**: The tool used to generate the IR node classes.
