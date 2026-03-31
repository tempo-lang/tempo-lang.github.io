---
title: Composite Types
type: docs
prev: docs/language-guide/functions
next: docs/language-guide/control-flow
weight: 6
---

## Structures

Structures work similar to in regular programming languages, they compose multiple value into a single value.
The main difference now is that different values can exist at different roles.

```tempo {filename=Tempo}
struct@(A,B) Pair {
  left: Int@A;
  right: Int@B;
}
```

The pair can now be instantiated like so.

```tempo {filename=Tempo}
let pair = Pair@(A,B) {
  left: 10,
  right: 20
};
```

### Methods

Structures can have associated functions called methods.
Expanding on the `Pair` structure from above, we add a `swap` method, that returns a new pair with the values swapped.

```tempo {filename=Tempo}
struct@(A,B) Pair {
  left: Int@A;
  right: Int@B;

  func@(A,B) swap() Pair@(B,A) {
    return Pair@(B,A) {
      left: right,
      right: left
    };
  }
}
```

With this we can easily swap any pair by calling the method like so.

```tempo {filename=Tempo}
let pair = Pair@(A,B) {
  left: 10,
  right: 20
};

let swapped: Pair@(B,A) = pair.swap();
```

## Lists

Lists are written like this.

```tempo {filename=Tempo}
let x: [Int@[A,B]] = [1, 2, 3];
```

You can combine lists using the `+` operator if they share the same underlying type and their roles intersect.

```tempo {filename=Tempo}
let x: [Int@[A,B]] = [1,2,3];
let y: [Int@[B,C]] = [4,5,6];

// the combined list exists only at `B`
let combined: [Int@B] = x + y;
```

Similarly, you can index lists if the roles of the index intersect with the underlying type of the list.

```tempo {filename=Tempo}
let x: [String@[A,B]] = ["hello", "hi", "hey"];
let i: Int@A = 1;

let y: String@A = x[i]; // "hi"
```

## Assignment to composite types

A field or an index within a composite type can be modified directly using an assignment,
as long as the roles of the inner value are able to compute the full path to it locally.

```tempo {filename=Tempo}
let pairs = [
  Pair@(A,B) { left: 1, right: 2 },
  Pair@(A,B) { left: 3, right: 4 }
];

let i = 0@A; // index only exists at A

pairs[i].left = 10; // ok, since .left exists at A
pairs[i].right = 20; // error, since .right exists at B
```

## Hiding roles in composite types

Composite types like the `Pair` type defined earlier can hide some of its roles.
This is done by denoting the hidden roles with an underscore `_`.
In the example below, we construct a partial pair that only contains the `left` attribute at `A`.

```tempo {filename=Tempo}
let leftPair: Pair@(A,_) { left: 10 };
```

Doing so, means that the `right` attribute will not be accessible.

```tempo {filename=Tempo}
let right = leftPair.right; // error: right field is hidden
```

Composite types will automatically hide roles that are not in scope.
This makes it possible to access parts of a larger structure in scopes where only a subset of the roles are present.

```
let pair = Pair@(A,B) { left: 10, right: 20 };
let condition: Bool@A = localComputation@A();
if condition {
  print@A(pair.left);
}
```

In the example above, we construct a distributed `Pair` at `A` and `B` and enter a conditional only at `A`.
We can access `pair` in the conditional even though the `Pair` exists at both `A` and `B`.
This is allowed since the type of `pair` will automatically be coerced to `Pair@(A,_)` (hiding `B`) when accessed inside the condition.
