---
title: Composite Types
type: docs
prev: docs/language-guide/functions
next: docs/language-guide/control-flow
weight: 5
---

## Structures

Structures work similar to records in regular programming languages, they compose multiple value into a single value.
The main difference now is that different values can exist at different roles.

```tempo
struct@(A,B) Pair {
  left: Int@A
  right: Int@B
}
```
