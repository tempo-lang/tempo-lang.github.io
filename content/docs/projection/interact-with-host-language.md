---
title: Interact with host language
type: docs
prev: docs/projection
next: docs/projection/transports
weight: 1
---

Functions from the host language can be called from Tempo through interfaces.

```tempo
interface@A Printer {
  func@A print(value: String@A);
}

func@(A,B) hello(printA: Printer@A, printB: Printer@B) {
  printA.print("Hello from A");
  printB.print("Hello from B");
}
```
