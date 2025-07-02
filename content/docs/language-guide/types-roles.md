---
title: Types and Roles
type: docs
prev: docs/language-guide/basics
next: docs/language-guide/communication
weight: 2
---

## Values and Variables

Values are assigned to variables in the following way.

```tempo {filename=Tempo}
let name: Type = value;
```

The value can be any expression or literal, such as the number `5`.

### Primitive types

There exists the following primitive types.

- **Int** integer numbers like the number `10`.
- **Float** floating-point numbers, written like `1.2`, `1.`, or `.5`.
- **String** text string literals, written as `"hello world"`.
- **Bool** the boolean values `true` and `false`.

## Roles

Unlike traditional programming languages, types are annotated with _roles_.

When you project a choreography, a local program is generated for each role.
For each projection only code for the corresponding role will be included.

A type consists of a base and a role, with an `@` sign between.
In the example below `String@Bob` is a type denoting a `String` that exists at role `Bob`.

```tempo {filename=Tempo}
let x: Int@Alice = 10;
let y: String@Bob = "Hello";
```

Thus, in the projection for Alice, the variable `x` will exist, but not `y`, and vice versa for Bob.

There exists three types of roles, _local_, _shared_ and _distributed_ ones.

### Local Roles

A type with a local role, denoted `Type@A`, exists only at one role.

### Shared Roles

A type with a shared role, denoted `Type@[A,B,C]`, describes a value that at runtime is guaranteed to be the same across all mentioned roles.

A shared variable can be coerced to one of a subset of the roles, or even a single role.
Constant literals are automatically shared between all roles and coerced to the subset needed.

```tempo {filename=Tempo}
let x: Bool@[A,B,C] = true
let y: Bool[A,B] = x
```

Shared variables is an alternative to traditional labels for determining [knowledge of choice](/docs/introduction/choreographic-programming/#knowledge-of-choice).
Instead, when a choice is made, all participants of the choice will calculate it independently using shared variables.

```tempo {filename=Tempo}
let x: Int@[A,B] = 3;
if x > 0 {
  // both A and B knows choice
}
```

Shared variables can only be mutated by expressions that are of the same shared roles.
This ensures that shared variables always agree on the same value across the roles.

Shared variables can be expanded by sending it to further participants transitively.

```tempo {filename=Tempo}
let x: Int@A = 42
let y: Int@[A,B] = await A->B x
let z: Int@[A,B,C] = await B->C y
```

### Distributed Roles

A type with a distributed role, denoted `Type@(A,B,C)`, has different parts of the data at the specified roles.
[Functions](/docs/language-guide/functions), [interfaces](/docs/projection/interact-with-host-language/) and [structures](/docs/language-guide/composite-types/#structures) can be distributed.

## Asynchronous Types

An asynchronous type, indicates that the underlying value is not necessarily present yet.
You use the `await` expression to get the underlying value, which will wait until it arrives before continuing with the underlying value.

Normal types can be coerced into asynchronous types that immediately return the result when await is used.

```tempo {filename=Tempo}
let x: async Bool@A = true;
let y: Bool@A = await x; // value is already present
let z: async Bool@A = 3 + x; // expression will be coerced to async
```

## Value Semantics

Variables have pass-by-value semantics, which means that when a value is assigned to a new variable, the value is effectively copied to the new variable.
This is the case for all values, including lists and structs which are often pass-by-reference in many common programming languages.

The only exception is that a value which is captured by a closure references the same underlying value as the variable outside the scope of the closure.
