---
title: Control Flow
type: docs
prev: docs/language-guide/composite-types
next: docs/projection
weight: 6
---

## Conditionals

In this example, A sends the `input` to `B`, now both roles know `x` so they both calculate locally whether `x > 5`.

```tempo {filename=Tempo}
func@(A,B) run(input: Int@A) {
  let x: Int@[A,B] = await A->B input;
  if x > 5 {
    await B->A "x is larger than 5";
  }
}
```

## Loops

In the example below, `A` will locally count down from 10 (notice that `i` has type `Int@A`).
In each iteration of the loop, `A` will send a boolean to `B` to tell whether they should loop again.
After 10 iterations, `A` will send the value `false` to `B`, and they will both exit the loop.

```tempo {filename=Tempo}
let i: Int@A = 10;
while await A->B (i > 0) {
  await B->A "ping";
  i = i - 1;
}
```
