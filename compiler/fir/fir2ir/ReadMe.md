# FIR-to-IR Conversion

This module is the critical bridge between the modern [FIR frontend](../../fir/ReadMe.md) and the unified [IR backend](../../ir/ReadMe.md). Its sole responsibility is to convert a fully resolved and checked FIR tree into its equivalent representation in the backend [IR tree](../../ir/ir.tree/ReadMe.md).

This conversion is the final step of the compiler's frontend. After this point, all semantic and syntactic information from the original source code has been encoded into the IR, and the FIR tree is discarded.

## Key Concepts

-   **Semantic Equivalence:** The `fir2ir` converter's primary goal is to produce an IR tree that is semantically equivalent to the input FIR tree. This is a complex, stateful process that involves mapping FIR's concepts and structures to their IR counterparts.
-   **Symbol Table Management:** A key part of the conversion is the creation and management of IR symbols (`IrSymbol`). The converter is responsible for generating a unique `IrSymbol` for each declaration in the FIR tree and ensuring that all usages of that declaration in the IR tree point to the correct symbol.
-   **Stateful Conversion:** The conversion is not a simple one-to-one mapping. The converter maintains state, including a symbol table and a mapping of FIR elements to their corresponding IR elements, to handle complex cases like recursive functions and forward declarations correctly.

## Structure

-   **`/src`**: Contains the core, platform-agnostic logic for the FIR-to-IR conversion. This includes the main visitor that traverses the FIR tree and the logic for creating the corresponding IR nodes and symbols.
-   **`/jvm-backend`**: Contains code that is specific to the FIR-to-IR conversion process when the final target is the JVM. This component handles JVM-specific concepts and annotations that need to be correctly translated into the IR.
