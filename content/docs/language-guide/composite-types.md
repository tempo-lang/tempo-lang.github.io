---
title: Composite Types
type: docs
prev: docs/language-guide/functions
next: docs/language-guide/control-flow
weight: 6
---

## Structures

Structures work similar to in regular programming languages, they compose multiple value into a single value.
The main difference now is that different values can exist at different roles.

```tempo {filename=Tempo}
struct@(A,B) Pair {
  left: Int@A
  right: Int@B
}
```

## Lists

Lists are written like this.

```tempo {filename=Tempo}
let x: [Int@[A,B]] = [1, 2, 3];
```

You can combine lists using the `+` operator if they share the same underlying type and their roles intersect.

```tempo {filename=Tempo}
let x: [Int@[A,B]] = [1,2,3];
let y: [Int@[B,C]] = [4,5,6];

// the combined list exists only at `B`
let combined: [Int@B] = x + y;
```

Similarly, you can index lists if the roles of the index intersect with the underlying type of the list.

```tempo {filename=Tempo}
let x: [String@[A,B]] = ["hello", "hi", "hey"];
let i: Int@A = 1;

let y: String@A = x[i]; // "hi"
```
