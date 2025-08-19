---
title: Interfaces
type: docs
prev: docs/language-guide/control-flow
next: docs/projection
weight: 7
---

An interface defines a blueprint for a set of methods.
[Structures](/docs/language-guide/composite-types/#structures) can implement these blueprints in order to conform to the interface.
Multiple structures can implement the same interface and a structure can implement multiple interfaces.

## RPC Example

In this example we define the `RPC` interface for a remote procedure call.
The method `call` will send the value from `A` to `B`, then `B` will make a computation on the value and send the result back to `A`.

```tempo {filename=Tempo}
interface@(A, B) RPC {
  func@(A, B) call(input: Int@A) async Int@A;
}
```

Next, we make the `RemoteCall` structure which will convert a local function `fn` at `B` to a remote procedure call at `A`.
By having `RemoteCall` implement the `RPC` interface allows it to be used more generally.
This way, the implementation of the `RPC` does not matter, only that it has the right `call` method.

```tempo {filename=Tempo}
struct@(A,B) RemoteCall implements RPC@(A,B) {
  fn: func@(B)(Int@B)Int@B;

  func@(A, B) call(input: Int@A) async Int@A {
    let output = fn(await A->B input);
    return B->A output;
  }
}
```

Lastly, we define the `main` function to demonstrate how it works.
Here, we define the `double` closure at `B` to compute the double of the input.

Role `A` can not directly call `double` as it's located at `B`.
However, by wrapping it in the `RemoteCall` structure, we can expose it `A` while still having `B` do the computation.

```tempo {filename=Tempo}
func@(A,B) main() {
  let double = func@B (input: Int@B) Int@B {
    return input * 2;
  };

  let rpc: RPC@(A,B) = RemoteCall@(A,B) { fn: double };

  let result: Int@A = await rpc.call(10@A);
}
```
