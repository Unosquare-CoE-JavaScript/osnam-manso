# Motivation

A high-level blueprint for an algorithm to bew completed by ingeritors.

- Algorithms can be decomposed into common parts + specifics.
- Strategy pattern does this through composition.
  - High-level algorithm uses an interface.
  - Concrete inplementations implement the interface.
- Template Method does the same thing through ingeritance.
  - Overall algorithm makes use of empty('abstract') members.
  - Ingeritors override those members.
  - Parent template method invoked.

Allows us to define the 'skeleton' of the algorithm, with concrete implementations defined in subclasses.

# Template Method

```jsx
class Game {
  constructor(numberOfPlayers) {
    this.numberOfPlayers = numberOfPlayers
    this.currentPlayer = 0
  }

  run() {
    this.start()
    while (!this.haveWinner) {
      this.takeTurn()
    }
    console.log(`Player ${this.winningPlayer} wins.`)
  }

  start() {}
  get haveWinner() {}
  takeTurn() {}
  get winningPlayer() {}
}

class Chess extends Game {
  constructor() {
    super(2)
    this.maxTurns = 10
    this.turn = 1
  }

  start() {
    console.log(`Starting a game of chess with ${this.numberOfPlayers} players.`)
  }

  get haveWinner() {
    return this.turn === this.maxTurns
  }

  takeTurn() {
    console.log(`Turn ${this.turn++} taken by player ${this.currentPlayer}.`)
    this.currentPlayer = (this.currentPlayer + 1) % this.numberOfPlayers
  }

  get winningPlayer() {
    return this.currentPlayer
  }
}

let chess = new Chess()
chess.run()
```

# Summary

- Define an algorithm at a high level.
- Define constituent parts as empty methods/properties.
- Ingerit the algorithm class, providing necessary overrides.
