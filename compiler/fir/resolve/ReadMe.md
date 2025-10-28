# FIR: Resolution

This module is the heart of the FIR frontend. It is responsible for taking the "raw," purely syntactic FIR tree from the [raw-fir](../raw-fir/ReadMe.md) stage and enriching it with semantic information. The process, known as **resolution**, transforms the tree into a state where all identifiers are bound to their declarations, all types are resolved, and all expressions have a determined type.

The resolution process is primarily implemented using a series of **transformer** visitors that walk the FIR tree, resolving different aspects of the code in a specific order.

## Key Semantic Analysis Phases

The `resolve` module performs several critical semantic analysis tasks:

- **Overload Resolution:** When the code contains a call to a function or an operator, this phase determines exactly which declaration is being referred to, especially when multiple candidates exist. The logic for this is primarily located in the `/calls` subdirectory.
- **Type Inference:** This is the process of automatically determining the type of an expression. It is one of the most complex parts of the compiler and is handled by the logic within the `/inference` subdirectory.
- **Data Flow Analysis (DFA):** This phase analyzes the flow of data through the program to derive important information, such as smart casting (knowing a variable has a more specific type after a check) and nullability analysis. This is managed by the code in the `/dfa` subdirectory.
- **Scope Management:** To resolve identifiers correctly, the compiler needs to know which declarations are visible at any given point in the code. The `scopes` package is responsible for managing this lexical scoping.
- **Symbol Binding:** This is the process of linking a used identifier (like a variable or function name) to its original declaration.

## Core Sub-modules and Components

The logic is organized into several key directories:

- **`/transformers`:** This is the engine of the resolution process. It contains a set of `FirTransformer` implementations, each responsible for a specific resolution task (e.g., resolving types, resolving bodies of functions, etc.). These transformers are applied in a specific sequence to the FIR tree.
- **`/calls`:** This directory contains the logic for overload resolution, including candidate selection, ambiguity detection, and the handling of function calls.
- **`/inference`:** This module houses the complex machinery for type inference.
- **`/dfa`:** Contains the implementation of the data flow analysis, which is crucial for Kotlin's smart casting and type system features.
- **`/scopes`:** Provides the data structures and logic for managing lexical scopes, which are essential for correct name resolution.
- **`/providers`:** Contains logic for retrieving symbols and FIR elements from different sources, such as the session cache or other modules.
