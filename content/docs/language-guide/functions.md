---
title: Functions
type: docs
prev: docs/language-guide/communication
next: docs/language-guide/expressions
weight: 4
---

## Declaring Functions

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

### Return

A function can return, only if all roles of the function has knowledge about the return statement.

### Local Functions

A function is [local](/docs/language-guide/types-roles/#local-roles) if it exists only at a single role.

```tempo {filename=Tempo}
func@A double(value: Int@A) Int@A {
  return value * 2;
}
```

In the example above the function `double` only exists at the single role `A`.
This means that its computations will only happen at a single process when executed,
similar to regular functions in traditional programming languages.

#### Unnamed Roles

Local functions (as well as local [structures](/docs/language-guide/composite-types/#structures) and [interfaces](/docs/language-guide/interfaces/)) can have _unnamed roles_.
To make a function with unnamed roles, you define it without specifying any explicit roles after the `func` keyword.
This means that all types will automatically exist at a single unnamed role.

```tempo {filename=Tempo}
func double(value: Int) Int {
  return value * 2;
}
```

### Shared Functions

Local functions can be instantiated as a _shared function_.
A shared function takes [shared values](/docs/language-guide/types-roles/#shared-roles) as arguments and returns also a shared value.
When a shared function is called, each role will make the same function call with the same values as arguments, which ensures that the return value is identical.

```tempo {filename=Tempo}
func@(A,B) main() {
  let x: Int@[A,B] = 10;
  let y: Int@[A,B] = double@[A,B](x);
}
```

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
