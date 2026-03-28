# KyrylScript (Old) – Public Archive

**KyrylScript (Old)** is a public archive of an earlier version of KyrylScript featuring the implementation of its virtual machine (VM).
This version is experimental and primarily for reference and educational purposes. The VM is functional but far from perfect, and some design choices are rough around the edges.

> An exploration of language runtime and bytecode execution built from scratch in Rust.

---

## How It Works

KyrylScript programs are executed through several stages:

1. **Lexer**
   Converts source code into tokens such as identifiers, literals, operators, and keywords.

2. **Parser**
   Builds an Abstract Syntax Tree (AST) from the token stream, representing program structure.

3. **Semantic Analyzer**
   Performs scope and type checks, ensuring the program is meaningful.

4. **Compiler**
   Transforms the AST into bytecode that the VM can execute.

5. **Virtual Machine (VM)**
   Executes the bytecode instructions, manages memory through reference IDs, and handles control flow and operations.

> **Note:** This is the **first VM implementation** for KyrylScript. It’s experimental, incomplete, and mainly serves as a historical and learning artifact.

---

## Features

- Hand-written lexer and parser in Rust.
- Lexical scoping with reference-based memory management (no garbage collector).
- Basic type system: numbers, strings, booleans, lists, tuples, functions, and native values.
- Control structures: if/else, while loops, recursion, pattern matching.
- Native function registry for extending runtime behavior.

---

## Getting Started

1. **Clone the repository**

```bash
git clone https://github.com/yourname/kyryl-script-old.git
cd kyryl-script-old
```

2. **Build & Run**

```bash
cargo run -- examples/test.ks
```

3. Explore the code to understand the VM implementation and the compilation flow.

---

## Project Structure

- `workspace/ks-core/src/lexer/` – Tokenization logic
- `workspace/ks-core/src/parser/` – AST generation
- `workspace/ks-core/src/compiler/` – Bytecode generation
- `workspace/ks-vm/src/vm/` – The virtual machine executing bytecode

---

## Anchor System

KyrylScript uses a custom **Anchor System** for memory management — a lightweight, safe alternative to traditional garbage collection.

### Key Concepts

- **Automatic memory safety**: Anchors automatically track objects’ lifetimes, preventing dangling references and memory leaks.
- **No heavy GC overhead**: Unlike garbage collectors, Anchors do not require runtime pauses for sweeping or collection cycles.
- **Simple semantics**: Anchors are easier to reason about than complex ownership and borrowing models (like Rust), but still maintain safety guarantees.
- **Stack-like allocation**: Anchors leverage scoped lifetimes where possible, ensuring memory is released as soon as it’s no longer needed.

### How it Works (Conceptually)

1. **Anchor creation**: Every allocated object can be assigned an Anchor.
2. **Scope tracking**: Anchors are tied to scopes (functions, blocks, or modules). Once a scope ends, all Anchors within it are automatically released.
3. **Reference safety**: Objects can be referenced safely as long as their Anchors are alive. Attempting to use a released Anchor results in a runtime error.
4. **Performance-oriented**: By combining scope-based cleanup with minimal bookkeeping, the Anchor System provides speed close to manual memory management, without risking unsafe memory access.

### Benefits

- Fast and deterministic memory management.
- Prevents memory leaks without complex ownership rules.
- Simplifies developer mental load while keeping the VM safe and efficient.

---

## License

MIT © 2026 Kyryl Lebedenko

> Created with love by Kyryl Lebedenko
