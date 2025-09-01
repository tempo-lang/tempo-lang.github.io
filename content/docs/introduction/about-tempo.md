---
title: About Tempo
type: docs
prev: docs/introduction/choreographic-programming
next: docs/language-guide
weight: 2
---

Tempo is a choreographic programming language that aims to be simple and practical to use.
It's inspired by prior choreographic programming language [Klor](https://github.com/lovrosdu/klor) and [Choral](https://www.choral-lang.org/).

### Multi target projection

Unlike Klor (a DSL for Clojure) and Choral (which is an extension of Java),
Tempo is not tied to any specific target language.
The compiler can project the same Tempo program into source code in different languages.
This makes it possible to run different roles of a choreographic program in different environments.
One role might run in a cloud environment using Go, while another role runs in a browser using JavaScript.

Currently, the compiler can emit Go, TypeScript/JavaScript and Java code, but more targets such as Swift and Rust are planned to be added in the future.

### Simple and practical

Tempo strives to be easy to learn and use in real-world scenarios.
This is achieved by making the language syntax and type system simple and intuitive.

In addition to that, the compiler will also give useful error messages that are self-explanatory.
Also, the compiler includes a native [Language Server Protocol](https://microsoft.github.io/language-server-protocol/) implementation, which provides robust IDE integration.
The [vscode extension](https://marketplace.visualstudio.com/items?itemName=tempo-lang.vscode-tempo), provides syntax highlighting and LSP integration for Visual Studio Code.
