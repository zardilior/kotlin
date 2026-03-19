# Compiler: Program Structure Interface (PSI)

This module is the foundation of the Kotlin compiler's understanding of source code. It is responsible for parsing raw Kotlin text into a structured tree representation known as the **Program Structure Interface (PSI)** tree. The PSI tree is a concrete syntax tree that faithfully represents the source code, including all of its syntactic elements like keywords, operators, and comments.

The PSI tree is the entry point for all further semantic analysis in the compiler. It is the structure upon which the new [FIR frontend](../fir/ReadMe.md) is built.

## Key Concepts

- **PSI Element (`PsiElement`):** The fundamental building block of the PSI tree. Every syntactic construct in the language, from a single keyword to an entire file, is represented by a `PsiElement`.
- **Concrete Syntax Tree (CST):** Unlike an Abstract Syntax Tree (AST), the PSI tree retains all syntactic information from the original source code, making it perfectly suited for tasks that require detailed source code analysis and manipulation, such as IDE features (refactoring, code completion) and compiler diagnostics.
- **Visitor Pattern:** The PSI tree is traversed using the visitor pattern. Different compiler phases can implement visitors to walk the tree and perform specific analyses.

## Sub-modules

The `psi` module is broken down into the following key components:

- **`/psi-api`:** Defines the core interfaces for the PSI tree. This module contains the `KtElement` interface and other fundamental interfaces that describe the shape of Kotlin's syntax tree.
- **`/psi-impl`:** Provides the concrete implementations of the interfaces defined in `/psi-api`.
- **`/parser`:** This is where the actual parsing happens. It contains the grammar definition and the parser logic that transforms a stream of tokens from the lexer into a valid PSI tree. It is the heart of the initial source-to-tree conversion.
- **`/psi-utils`:** Contains a collection of utility functions and extension methods for working with PSI trees. These helpers make it easier to traverse the tree, find specific elements, and perform common queries.
- **`/psi-frontend-utils`:** A set of higher-level utilities that are specifically used by the compiler's frontend phases.
