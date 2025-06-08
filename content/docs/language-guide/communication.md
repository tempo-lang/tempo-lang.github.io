---
title: Communication
type: docs
prev: docs/language-guide/types-roles
next: docs/language-guide/functions
weight: 3
---

Channels are built-in primitives. All roles can communicate with each other by writing `A->B` where `A` and `B` are roles.

```tempo
// send value from `A` to `B`.
let y@B = await A -> B "hello"@A;

// send value from `A` to `B` and `C` to obtain a shared value.
let y@[A,B,C] = await A->[B,C] "hi everyone"@A
```
