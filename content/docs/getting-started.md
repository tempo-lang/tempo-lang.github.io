---
title: Getting Started
type: docs
prev: docs
next: docs/introduction
weight: 1
---

Read how to get started with Tempo.

## Installation

To install Tempo, you first need to have [Go](https://go.dev/) installed.
To build and install Tempo on your machine, run the following command.

```sh {filename=Terminal}
$ go install github.com/tempo-lang/tempo@latest
```

To check that Tempo is successfully installed, run the `tempo` command in the terminal.

> [!TIP]
> We recommended installing the [vscode extension](https://marketplace.visualstudio.com/items?itemName=tempo-lang.vscode-tempo) for language support in the editor.

## Your first choreography

Now where you have successfully installed Tempo, we can test it by writing the _hello world_ example for choreographies.
Create a new file with the following code and name it `greeting.tempo`.

```tempo {filename=greeting.tempo}
func@(Alice, Bob) greet() {
  Alice->Bob "Hello Bob, this is Alice";
  Bob->Alice "Hi Alice, nice to meet you";
}
```

In a terminal, navigate to the directory of the file and run this command to build the choreography.

```sh {filename=Terminal}
$ tempo build greeting.tempo
```

You should obtain the following projected Go code.

```go {filename=Go}
// Projection of choreography greet
func greet_Alice(env *runtime.Env) {
    runtime.Send(env, "Hello Bob, this is Alice", "Bob")
    _ = runtime.Recv[string](env, "Bob")
}
func greet_Bob(env *runtime.Env) {
    _ = runtime.Recv[string](env, "Alice")
    runtime.Send(env, "Hi Alice, nice to meet you", "Alice")
}
```

The function has been projected to two functions, the one from the perspective of `Alice` and `Bob` respectively.

Notice that in `greet_Alice`, the function first sends a message and then receives a reply,
and for `greet_Bob` the same thing happens but in reverse.
