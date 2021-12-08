# Motivation

Sequence of handlers procesing an event one after another.

- Unethical behavior by an emplooyee; who takes the blame?
  - Employee
  - Manager
  - Ceo
- You click a graphical element on a form

  - Butoon handles it, stops further processing
  - Underlying group box
  - Underlying window

- CCG computer game
  - Creature has attack and defense values.
  - Those can be boosted by oters cards.

A chain of components who all get a chance to process a command or a query, optionally having default processing implementations and an ability to terminate the processing chain.

# Method Chain

```jsx
class Creature {
  constructor(name, attack, defense) {
    this.name = name
    this.attack = attack
    this.defense = defense
  }

  toString() {
    return `${this.name} (${this.attack}/${this.defense})`
  }
}

class CreatureModifier {
  constructor(creature) {
    this.creature = creature
    this.next = null
  }

  add(modifier) {
    if (this.next) this.next.add(modifier)
    else this.next = modifier
  }

  handle() {
    if (this.next) this.next.handle()
  }
}

class NoBonusesModifier extends CreatureModifier {
  constructor(creature) {
    super(creature)
  }

  handle() {
    console.log('No bonuses for you!')
  }
}

class DoubleAttackModifier extends CreatureModifier {
  constructor(creature) {
    super(creature)
  }

  handle() {
    console.log(`Doubling ${this.creature.name}'s attack`)
    this.creature.attack *= 2
    super.handle()
  }
}

class IncreaseDefenseModifier extends CreatureModifier {
  constructor(creature) {
    super(creature)
  }

  handle() {
    if (this.creature.attack <= 2) {
      console.log(`Increasing ${this.creature.name}'s defense`)
      this.creature.defense++
    }
    super.handle()
  }
}

let goblin = new Creature('Goblin', 1, 1)
console.log(goblin.toString())

let root = new CreatureModifier(goblin)

//root.add(new NoBonusesModifier(goblin));

root.add(new DoubleAttackModifier(goblin))
//root.add(new DoubleAttackModifier(goblin));

root.add(new IncreaseDefenseModifier(goblin))

// eventually...
root.handle()
console.log(goblin.toString())
```

# Broker Chain

```jsx
class Event {
  constructor() {
    this.handlers = new Map()
    this.count = 0
  }

  subscribe(handler) {
    this.handlers.set(++this.count, handler)
    return this.count
  }

  unsubscribe(idx) {
    this.handlers.delete(idx)
  }

  fire(sender, args) {
    this.handlers.forEach(function (v, k) {
      v(sender, args)
    })
  }
}

let WhatToQuery = Object.freeze({
  attack: 1,
  defense: 2,
})

class Query {
  constructor(creatureName, whatToQuery, value) {
    this.creatureName = creatureName
    this.whatToQuery = whatToQuery
    this.value = value
  }
}

class Game {
  constructor() {
    this.queries = new Event()
  }

  performQuery(sender, query) {
    this.queries.fire(sender, query)
  }
}

class Creature {
  constructor(game, name, attack, defense) {
    this.game = game
    this.name = name
    this.initial_attack = attack
    this.initial_defense = defense
  }

  get attack() {
    let q = new Query(this.name, WhatToQuery.attack, this.initial_attack)
    this.game.performQuery(this, q)
    return q.value
  }

  get defense() {
    let q = new Query(this.name, WhatToQuery.defense, this.initial_defense)
    this.game.performQuery(this, q)
    return q.value
  }

  toString() {
    return `${this.name}: (${this.attack}/${this.defense})`
  }
}

class CreatureModifier {
  constructor(game, creature) {
    this.game = game
    this.creature = creature
    this.token = game.queries.subscribe(this.handle.bind(this))
  }

  handle(sender, query) {
    // implement in inheritors
  }

  dispose() {
    game.queries.unsubscribe(this.token)
  }
}

class DoubleAttackModifier extends CreatureModifier {
  constructor(game, creature) {
    super(game, creature)
  }

  handle(sender, query) {
    if (query.creatureName === this.creature.name && query.whatToQuery === WhatToQuery.attack) {
      query.value *= 2
    }
  }
}

class IncreaseDefenseModifier extends CreatureModifier {
  constructor(game, creature) {
    super(game, creature)
  }

  handle(sender, query) {
    if (query.creatureName === this.creature.name && query.whatToQuery === WhatToQuery.defense) {
      query.value += 2
    }
  }
}

let game = new Game()
let goblin = new Creature(game, 'Strong Goblin', 2, 2)
console.log(goblin.toString())

let dam = new DoubleAttackModifier(game, goblin)
console.log(goblin.toString())

let idm = new IncreaseDefenseModifier(game, goblin)
console.log(goblin.toString())
idm.dispose()

dam.dispose()
console.log(goblin.toString())
```

# Summary

- Chain of Responsibility can be implemented as a chain of references or a centralized construct.
- Enlist objetcts in the chain, possibly controlling ther order/priority
- In a linked-list implementation, one member cam pmpede further processing
- Support removal of objects from the chain (lifetime control)
