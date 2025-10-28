# Compiler: Frontend Intermediate Representation (FIR)

FIR (Frontend Intermediate Representation) is the K2 implementation of the compiler frontend, responsible for desugaring, resolution, type inference, and diagnostics.

**[Design Docs](../../docs/fir/fir-basics.md)**

FIR is a modern, high-performance frontend for the Kotlin compiler, designed to be robust, efficient, and highly extensible. Its primary role is to take the parsed source code ([PSI](../../psi/ReadMe.md)) and perform all the necessary semantic analysis to produce a resolved and checked intermediate representation that can then be passed to the [compiler backends](../../ir/ReadMe.md).

The FIR pipeline is a multi-stage process where the FIR tree is progressively enriched with information.

## The FIR Pipeline

The FIR frontend operates in a clear, sequential pipeline, with each major stage residing in its own subdirectory.

1.  **[Raw FIR Creation](./raw-fir/ReadMe.md):**
    The process begins here. The raw source code, represented as a PSI tree, is converted into an initial, unresolved FIR tree. This tree is a direct syntactic representation of the code, without any semantic meaning attached.

2.  **[Resolution](./resolve/ReadMe.md):**
    This is the heart of the frontend. The raw FIR tree is fed into the resolution stage, which performs all the critical semantic analysis. This includes:
    -   Type inference
    -   Overload resolution
    -   Symbol binding (connecting identifiers to their declarations)
    -   Data flow analysis (for smart casting and nullability)

3.  **[Checkers](./checkers/ReadMe.md):**
    Once the FIR tree is fully resolved, it is passed to the checkers. This final stage traverses the resolved tree to find and report any diagnostics, such as compile-time errors and warnings. The checkers are designed to be platform-specific, allowing for different rules on JVM, JS, and Native targets.

After the checker phase, the fully resolved and validated FIR tree is ready to be handed off to the unified [IR backend](../../ir/ReadMe.md) for code generation.

## Other Key Sub-modules

In addition to the main pipeline stages, the `fir` module contains several other important components:

-   **`/tree`**: Defines the data structures for the FIR tree itself. This is where the various `FirElement` classes are declared.
-   **`/cones`**: Contains the representation of unresolved types, which are used during the resolution phase.
-   **`/providers`**: Logic for retrieving symbols and FIR elements.
-   **`/fir2ir`**: The component responsible for converting the resolved FIR tree into the unified backend IR.
-   **`/fir-serialization`**: Logic for serializing FIR trees, which is essential for incremental compilation and multi-module projects.
-   **`/plugin-utils`**: Provides utilities and extension points for compiler plugin authors to interact with the FIR tree.
