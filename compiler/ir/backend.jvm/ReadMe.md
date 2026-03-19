# IR Backend: JVM

This module is the backend responsible for the compilation of the Kotlin [IR tree](../ir.tree/ReadMe.md) into Java Virtual Machine (JVM) `.class` files. It takes the lowered IR from the [`backend.common`](../backend.common/ReadMe.md) module and performs the final translation.

The JVM backend is a multi-stage process that includes some final, JVM-specific lowerings before the actual bytecode generation.

## The JVM Backend Pipeline

1.  **JVM-Specific Lowerings:**
    While most high-level Kotlin constructs are handled by the common lowerings, some features require special handling on the JVM. This module contains additional lowering passes that are run after the common ones. Examples include the implementation of default interface methods and lambda lifting strategies specific to the JVM. This logic is primarily located in the `/lower` subdirectory.

2.  **Bytecode Generation:**
    After all lowerings are complete, the IR tree is in a very simplified, "machine-readable" state. The final and most critical stage is the traversal of this tree to emit the corresponding JVM bytecode. This is done using the **ASM** library, a powerful tool for bytecode manipulation. The core of this logic, which maps IR instructions to bytecode instructions, resides in the `/codegen` subdirectory.

3.  **Entry Point Generation:**
    This stage is responsible for creating the necessary entry points for the program to be executed, such as generating the `main` method in the correct class. This is handled by the `/entrypoint` subdirectory.

## Structure

-   **`/lower`**: Contains the implementation of JVM-specific lowering passes that run after the common lowerings.
-   **`/codegen`**: The heart of the backend. This directory contains the code that traverses the final, fully-lowered IR tree and uses the ASM library to generate the corresponding JVM bytecode.
-   **`/entrypoint`**: Contains the logic for generating the main entry point of the application.
-   **`/src`**: Contains the main `JvmBackendContext` and other shared infrastructure that wires together the different stages of the backend.
