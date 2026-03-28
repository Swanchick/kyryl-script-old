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

KyrylScript uses the **Anchor System**, a lightweight memory management model that ensures safety and efficiency without garbage collection or complex ownership rules.

### How It Works

- **Scope-based memory**: Every variable belongs to a scope. When the scope exits, all variables declared in that scope are automatically freed.
- **Escaping values**: If a value from a higher scope is assigned to a variable in a lower scope, it is _not_ freed when the lower scope ends — the value “escapes” and remains alive.
- **Automatic cleanup**: Only variables that do not escape are removed when their scope ends, keeping memory usage deterministic and predictable.

### Example

```ks
let a = 10;

if true {
    let b = 20;
    a = b; // value of b escapes the scope and becomes value of a
}
// At this point:
// b is freed because its scope ends
// a keeps the value 20, which was originally b's
```

### Benefits

- **Fast and predictable**: Memory is freed immediately when scopes exit.
- **Safe**: Escaping values are preserved correctly, avoiding dangling references.
- **Simple**: Easier to reason about than ownership/borrowing models while keeping safety guarantees.

---

## License

MIT © 2026 Kyryl Lebedenko

> Created with love by Kyryl Lebedenko
