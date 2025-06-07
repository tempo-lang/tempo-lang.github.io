---
title: Getting Started
type: docs
prev: docs
next: docs/introduction
---

Read how to get started with Tempo.

## Installation

- Install Tempo
- Install vscode extension

## Your first choreography

```tempo {filename=greeting.tempo}
func@(Alice, Bob) greet() {
  Alice->Bob "Hello Bob, this is Alice"
  Bob->Alice "Hi Alice, nice to meet you"
}
```
