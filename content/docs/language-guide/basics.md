---
title: The Basics
type: docs
prev: docs/language-guide
weight: 1
---

This page explains the basics of Tempo.

## Values and Variables

Values are assigned to variables in the following way.

```tempo
let name: Type = value;
```

The value can be any expression or literal, such as the number `5`.

### Primitive types

There exists the following primitive types.

- **Int** integer numbers like the number `10`.
- **Float** floating-point numbers, written like `1.2`, `1.`, or `.5`.
- **String** text string literals, written as `"hello world"`.
- **Bool** the boolean values `true` and `false`.

## Types and Roles

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

A type with a shared role, denoted `Type@[A,B,C]`, describes a value that is guaranteed to be the same at all mentioned roles.

### Distributed Roles

## Communication

## Choreographic Functions

## Comments
