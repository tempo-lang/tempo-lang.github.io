---
title: Choreographic Programming
type: docs
prev: /docs
next: /docs/about-tempo/
weight: 1
---

In short, [choreographic programming](https://en.wikipedia.org/wiki/Choreographic_programming) is a programming paradigm
in which one programs a [distributed system](https://en.wikipedia.org/wiki/Distributed_computing) from a global viewpoint.

A **choreographic program** intuitively specifies the control flow of the distributed system as a whole.
That is it specifies the communications performed by the participating processes in the system and the local computations they perform.

A compiler can automatically transform a choreographic program into an executable implementation for each role.
This compilation procedure is known as **projection** and the generated implementations as **projections**.
Running the projections concurrently will yield the global behavior described by the choreography.

Choreographic programs often come with a syntactic primitive for communication between processes.
For example on the form `Alice -> Bob value`, which reads "Alice sends value to Bob".
The connection between send and receive makes it impossible to describe send / receive mismatches or even deadlocks.

## Terms

A list of terms related to choreographic programming.

- **Role**: A role denotes an actor in a choreographic program, at runtime the role becomes a process.
- **Knowledge of Choice** is a problem that arises when one role computes a conditional (eg. an `if` statement),
  and another role needs to act differently based on the choice.

## References

- The book [Introduction to Choreographies](https://doi.org/10.1017/9781108981491) gives a more formal introduction to choreographic programming.
