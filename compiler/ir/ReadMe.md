# Compiler: Intermediate Representation (IR) and Backends

This module and its subdirectories represent the final phase of the Kotlin compilation process: transforming the semantically analyzed code from the frontend into a platform-specific, executable format.

The entire system is built around a central data structure: the **Intermediate Representation (IR)**.

## What is IR?

IR is a lower-level, tree-based representation of Kotlin code used by the compiler backends. While it is a lower-level format, it is still structured similarly to the original source code and is semantically rich.

For example, this Kotlin function:
```kotlin
fun abs(num: Int): Int {
    return if (num >= 0) num
    else -num
}
```
May be represented in IR as (simplified):
```
IrSimpleFunction name:abs visibility:public modality:FINAL returnType:kotlin.Int
  IrValueParameter name:num index:0 type:kotlin.Int
  IrBlockBody
    IrReturn type:kotlin.Int
      IrWhen type:kotlin.Int origin:IF
        IrBranch
          condition: IrCall symbol:'fun greater (arg0: kotlin.Int, arg1: kotlin.Int): kotlin.Boolean'
            arg0: IrGetValue symbol:'num' type:kotlin.Int
            arg1: IrConst type:kotlin.Int value:0
          result: IrGetValue symbol:'num' type:kotlin.Int
        IrBranch
          condition: IrConst type:kotlin.Boolean value:true
          result: IrCall symbol:'fun minus (arg0: kotlin.Int): kotlin.Int'
            arg0: IrGetValue symbol:'num' type:kotlin.Int
```

> **Note:** There is another intermediate representation, [FIR](../fir) (Frontend IR), which is used for resolution and type inference. Therefore, what we refer to as IR here should technically be called **Backend IR** (it isn't for historical reasons).

## The IR-Based Backend Pipeline

The backend pipeline begins after the frontend has produced a fully resolved and checked representation of the code.

1.  **[IR Tree (`./ir.tree/ReadMe.md`):** The process starts with the IR tree, a rich, semantic representation of the Kotlin code that is common to all frontends and backends.

2.  **[Common Backend (`./backend.common/ReadMe.md`):** The IR tree is first processed by the common backend. This crucial stage runs a series of **lowering** and **optimization** passes that transform high-level Kotlin constructs into simpler, more fundamental ones, making it more convenient for the final code generation phase.

3.  **Platform-Specific Backends:** After the common lowerings, the simplified IR tree is passed to a platform-specific backend. Each backend may run a few of its own lowerings before the final code generation.
    -   **[JVM Backend (`./backend.jvm/ReadMe.md`):** Converts the IR into JVM bytecode.
    -   **JS Backend (`./backend.js`):** Converts the IR into JavaScript.
    -   **Native Backend (`../../kotlin-native/backend.native`):** Converts the IR into native machine code via LLVM.
    -   **Wasm Backend (`./backend.wasm`):** Converts the IR into WebAssembly.

This architecture, with a unified IR and a shared common backend, is the key to Kotlin's multiplatform capabilities. It allows for a huge amount of code reuse, ensuring that language features behave consistently across all targets.

## Other Key Sub-modules

-   **`/ir.psi2ir`**: The converter for the old K1 frontend to the unified IR.
-   **`/serialization.common`**: Shared logic for serializing the IR tree, which is essential for creating KLIBs (Kotlin Libraries) and for incremental compilation.
-   **`/ir.interpreter`**: An interpreter that can evaluate parts of the IR tree at compile time, which is used for features like `const val`.
