---
title: Functions
type: docs
prev: docs/language-guide/communication
next: docs/language-guide/composite-types
weight: 4
---

All choreographies start with a function defined over a set of roles.

```tempo
func@(A,B,C) hello() {
  let hello: String@B = await A->B "Hello";
  let greeting: String@C = await B->C (hello + ", World!");
}
```

You call a function by substituting its roles.
All roles in a function substitution must be unique.

```tempo
func@(Sender, Receiver) say(message: String@Sender) String@Receiver {
  return await Sender->Receiver message;
}

func@(A,B,C) hello() {
  let hello: String@B = say@(A,B)("Hello");
  let greeting: String@C = say@(B,C)(hello + ", World!");
}
```

## Return

A function can return, only if all roles of the function has knowledge about the return statement.
