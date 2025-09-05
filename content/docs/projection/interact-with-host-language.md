---
title: Interact with host language
type: docs
prev: docs/projection/transports
weight: 2
---

Functions from the host language can be called from Tempo through [interfaces](/docs/language-guide/interfaces/).

```tempo {filename=hello.tempo}
interface Printer {
  func Print(value: String);
}

func@(A,B) Hello(printA: Printer@A, printB: Printer@B) {
  printA.Print("Hello from A");
  printB.Print("Hello from B");
}
```

Now project the choreography by executing the following command.
A new file called `hello.go` will be generated with the Go projections of the choreography.

```sh {filename=Terminal}
$ tempo build --package main hello.tempo > hello.go
```

Create a `main.go` file with the following contents.
When you instantiate the projection, you provide an implementation of the interface.

```go {filename=main.go}
package main

import (
  "fmt"

  "github.com/tempo-lang/tempo/runtime"
  "github.com/tempo-lang/tempo/simulator"
)

// define printer struct that implements the Printer interface
type PrinterImpl struct{}
func (p *PrinterImpl) Print(env *runtime.Env, value string) {
  fmt.Printf("%s\n", value)
}

func main() {
  simulator.Run(
    simulator.Proc("A", func(env *runtime.Env) any {
      // provide printer implementation
      Hello_A(env, &PrinterImpl{})
      return nil
    }),
    simulator.Proc("B", func(env *runtime.Env) any {
      // provide printer implementation
      Hello_B(env, &PrinterImpl{})
      return nil
    }),
  )
}
```

Now run the following commands to download dependencies, build, and run the example

```sh {filename=Terminal}
$ go mod init example    # create go module
$ go mod tidy            # download dependencies
$ go run .               # build and run it
```

You should see that it prints out the following two lines,
but since the two processes run concurrently the lines may come in any order.

```
Hello from A
Hello from B
```
