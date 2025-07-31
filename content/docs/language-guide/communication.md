---
title: Communication
type: docs
prev: docs/language-guide/types-roles
next: docs/language-guide/functions
weight: 3
---

Channels are built-in primitives. All roles can communicate with each other by writing `A->B x`, which reads _"role `A` sends the value `x` to role `B`"_.

```tempo {filename=Tempo}
// send value from `A` to `B`.
let y: String@B = await A -> B "hello"@A;

// send value from `A` to `B` and `C` to obtain a shared value.
let y: String@[A,B,C] = await A->[B,C] "hi everyone"@A
```

Only values with [local](/docs/language-guide/types-roles/#local-roles) or [shared](/docs/language-guide/types-roles/#shared-roles) roles can be communicated.
When a value is communicated its roles are expanded to include the receiving roles.

The result is wrapped in an [`async`](/docs/language-guide/types-roles/#asynchronous-types) type, which allows the recipient(s) to defer waiting for the message to arrive until it is needed.

```tempo {filename=Tempo}
let x: Int@A = 10;

// `A` sends the value to `B` immediately
let y: async Int@B = A->B x;

// `B` can do other things here...

// when `B` needs the value, `await` is used to wait until the value arrives
let z: Int@B = await y;
```
