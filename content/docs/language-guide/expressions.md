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

## Other expressions

- [Await async value](/docs/language-guide/types-roles/#asynchronous-types) `await x`
- [Struct construction](/docs/language-guide/composite-types/#structures)
- [Function calling](/docs/language-guide/functions) `myFunction()`
- [List construction](/docs/language-guide/composite-types/#lists) `[1,2,3]`
- [List indexing](/docs/language-guide/composite-types/#lists) `list[i]`
- [Communication](/docs/language-guide/communication) `A->B value`
- [Type Casting](/docs/language-guide/types-roles/#type-casting) `String(10)`

### Closure construction

See [closures](/docs/language-guide/functions#closures) for more details.

```tempo {filename=Tempo}
let x = func@(A,B) (input: Int@A) Int@B {
  return await A->B input;
};
```

### Field access

Some types have fields that can be accessed as `value.field`.
This includes:

- Fields of [structures](/docs/language-guide/composite-types#structures)
- Methods of [interfaces](/docs/projection/interact-with-host-language)
- The `.length` field of [lists](/docs/language-guide/composite-types/#lists)
