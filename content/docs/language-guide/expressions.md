---
title: Expressions
type: docs
prev: docs/language-guide/functions
next: docs/language-guide/composite-types
weight: 5
---

A base expression consists of a [primitive type](/docs/language-guide/types-roles/#primitive-types) along with an optional role specifier.
For instance the expression `"Hello"@Alice` evaluates to a value of type `String@Alice`.
If no role is specified, then the value of the resulting value will exist at all roles in the current scope.
Thus, in the example below, `x` will have type `String@[A,B,C]`.

```tempo {filename=Tempo}
func@(A,B,C) main() {
  let x = "hello";
}
```

## Binary operations

The following binary operations are supported:

- `+` addition, `-` subtraction, `/` division, `%` modulo.
- `==` equality, `!=` negated equality.
- `<` less than, `<=` less than or equal, `>` greater than, `>=` greater than or equal.
- `&&` logical and, `||` logical or

Binary operations are only allowed if the types of the two values have intersecting roles.
This means that `Int@A + Int@A` is not allowed, but `Int@A + Int@B` is not.

When carrying out operations on shared types, the result will exist at the intersection of the roles.

```tempo {filename=Tempo}
let x: Int@[A,B,C] = 5;
let y: Int@[B,C,D] = 7;

let sum: Int@[C,D] = x + y;
```
