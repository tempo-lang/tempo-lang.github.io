---
title: Functions
type: docs
prev: docs/language-guide/communication
next: docs/language-guide/composite-types
weight: 4
---

All choreographies start with a function defined over a set of roles.

```tempo {filename=Tempo}
func@(A,B,C) hello() {
  let hello: String@B = await A->B "Hello";
  let greeting: String@C = await B->C (hello + ", World!");
}
```

Before you can call a function, its roles must first be instantiated (similarly to generic type parameters).
You instantiate a function by substituting its roles written `funcName@(A,B,C)`, where `funcName` is the name of the function and `(A,B,C)` are the roles to substitute with.
All roles in a function substitution must be unique.
When a function has been instantiated, you can call it like a regular function `funcName@(A,B,C)(arg1, arg2)`.

```tempo {filename=Tempo}
func@(Sender, Receiver) say(message: String@Sender) String@Receiver {
  return await Sender->Receiver message;
}

func@(A,B,C) hello() {
  let hello: String@B = say@(A,B)("Hello");
  let greeting: String@C = say@(B,C)(hello + " World!");
}
```

## Return

A function can return, only if all roles of the function has knowledge about the return statement.

## Closures

You can pass around functions as closures after they have been instantiated.

```tempo {filename=Tempo}
func@(Sender, Receiver) say(message: String@Sender) String@Receiver {
  return await Sender->Receiver message;
}

func@(A,B) main() {
  let f: func@(A,B)(String@A)String@B = say@(A,B);
  let hello: String@B = f("hello");
}
```

You can also capture variables with a closure like so.

```tempo {filename=Tempo}
func@(A,B) main() {
  let count: Int@A = 0;
  let counter: func@(A,B)()Int@B = func@(A,B) () Int@B {
    count = count + 1;
    return await A->B count;
  };

  counter(); // 1@B
  counter(); // 2@B
  counter(); // 3@B
}
```
