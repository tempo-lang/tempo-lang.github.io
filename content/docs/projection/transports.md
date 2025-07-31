---
title: Transports
type: docs
prev: docs/projection
next: docs/projection/interact-with-host-language
weight: 1
---

Processes use a transport implementation in order to communicate with the other processes.
You can use any implementation of the `Transport` interface.

```go {filename=Go}
type Transport interface {
  Send(value any, roles ...string)
  Recv(role string, value any) *Async[any]
}
```

## Local transport

The Go implementation comes with a built-in local transportation implementation.

Consider the following choreography.

```tempo {filename=Tempo}
func@(A,B) PingPong() {
  let ping = await A->B "ping";
  let pong = await B->A "pong";
}
```

```go {filename=Go}
package main

import (
  "github.com/tempo-lang/tempo/runtime"
  "github.com/tempo-lang/tempo/transports"
)

func main() {
  local := transports.NewLocal()

  go func() {
    PingPong_A(runtime.New(local.Role("A")))
  }()

  go func() {
    PingPong_B(runtime.New(local.Role("B")))
  }()
}
```

### Simulator

You can use the `simulator` package to simplify testing choreographies locally.
Using that, we can rewrite the example above like so.

```go {filename=Go}
package main

import (
  "github.com/tempo-lang/tempo/runtime"
  "github.com/tempo-lang/tempo/simulator"
)

func main() {
  result := simulator.Run(
    simulator.Proc("A", func(env *runtime.Env) any {
      PingPong_A(env)
      return nil
    }),
    simulator.Proc("B", func(env *runtime.Env) any {
      PingPong_B(env)
      return nil
    }),
  )

  assert(result[0].Sends[0].Value == "ping")
}
```

The simulator returns a result object which contains every message exchanged between roles.
This makes it easy to write unit tests, see the tests in the [`examples` module](https://github.com/tempo-lang/tempo/tree/main/examples).
