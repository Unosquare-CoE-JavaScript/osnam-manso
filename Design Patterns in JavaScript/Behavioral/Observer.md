# Motivation

I am watching you!

- We need to be informed when certain things happen.
  - Object's property changes.
  - Object does something.
  - Some external event occurs.
- We want to listen to events and be notified when they occur.
  - Notifications should include useful data.
- Want to unsubscribe from events if we're no longer interested.

An observer is an object that wishes to be informed about events happening in the system. The entity generating the events is an observable.

# Events

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

  // 1) who fired the event?
  // 2) additional data (event args)
  fire(sender, args) {
    this.handlers.forEach((v, k) => v(sender, args))
  }
}

class FallsIllArgs {
  constructor(address) {
    this.address = address
  }
}

class Person {
  constructor(address) {
    this.address = address
    this.fallsIll = new Event()
  }

  catchCold() {
    this.fallsIll.fire(this, new FallsIllArgs(this.address))
  }
}

let person = new Person('123 London Road')
let sub = person.fallsIll.subscribe((s, a) => {
  console.log(`A doctor has been called ` + `to ${a.address}`)
})
person.catchCold()
person.catchCold()

person.fallsIll.unsubscribe(sub)
person.catchCold()
```

# Property Observers

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

  // 1) who fired the event?
  // 2) additional data (event args)
  fire(sender, args) {
    this.handlers.forEach((v, k) => v(sender, args))
  }
}

class PropertyChangedArgs {
  constructor(name, newValue) {
    this.name = name
    this.newValue = newValue
  }
}

class Person {
  constructor(age) {
    this._age = age
    this.propertyChanged = new Event()
  }

  get age() {
    return this._age
  }

  set age(value) {
    if (!value || this._age === value) return
    this._age = value
    this.propertyChanged.fire(this, new PropertyChangedArgs('age', value))
  }
}

class RegistrationChecker {
  constructor(person) {
    this.person = person
    this.token = person.propertyChanged.subscribe(this.age_changed.bind(this))
  }

  age_changed(sender, args) {
    if (sender === this.person && args.name === 'age') {
      if (args.newValue < 13) {
        console.log(`Sorry, you are still to young`)
      } else {
        console.log(`Okay, you can register`)
        sender.propertyChanged.unsubscribe(this.token)
      }
    }
  }
}

let person = new Person('John')
let checker = new RegistrationChecker(person)
for (let i = 10; i < 20; ++i) {
  console.log(`Changing age to ${i}`)
  person.age = i //
}
```

# Property Dependencies

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

  // 1) who fired the event?
  // 2) additional data (event args)
  fire(sender, args) {
    this.handlers.forEach((v, k) => v(sender, args))
  }
}

class Person {
  constructor(age) {
    this._age = age
    this.property_changed = new Event()
  }

  get age() {
    return _age
  }

  set age(value) {
    if (!value || this._age === value) return // nothing to do here

    let oldCanVote = this.canVote

    this._age = value
    this.property_changed.fire(this, new PropertyChangedArgs('age', value))

    if (oldCanVote !== this.canVote) {
      this.property_changed.fire(this, new PropertyChangedArgs('canVote', this.canVote))
    }
  }

  get canVote() {
    return this._age >= 16
  }
}

class PropertyChangedArgs {
  constructor(name, newValue) {
    this.name = name
    this.newValue = newValue
  }
}

class VotingChecker {
  constructor(person) {
    this.person = person
    this.person.property_changed.subscribe(this.voting_changed.bind(this))
  }

  voting_changed(sender, args) {
    if (sender === this.person && args.name === 'canVote') {
      console.log('Voting status changed to ' + args.newValue)
    }
  }
}

let person = new Person('John')
let checker = new VotingChecker(person)
for (let i = 10; i < 20; ++i) {
  console.log(`Changing age to ${i}`)
  person.age = i
}
```

# Summary

- Obsrver is an intrusive approach: an obsrvable must provide an event to subscribe to.
- Subscription and unsubscription handled with addition/removal of items in a list.
- Property notifications are easy; dependent property notifications are tricky.
