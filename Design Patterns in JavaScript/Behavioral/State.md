# Motivation

Fun with Finite State Machines.

- Consider an ordinary telephone.
- What you do with it depends on the state of the phone/line.
  - If it's ringing or your want to make a call , you can pick it up.
  - Phone muste be off the hook to talk/make a call.
  - If you try calling someone, and it's bysy, you put the handset down.
- Changes in state can be explicit or in response to event (Observer pattern)

# Clasic State

```jsx
class Switch {
  constructor() {
    this.state = new OffState()
  }

  on() {
    this.state.on(this)
  }

  off() {
    this.state.off(this)
  }
}

class State {
  constructor() {
    if (this.constructor === State) throw new Error('abstract!')
  }

  on(sw) {
    console.log('Light is already on.')
  }

  off(sw) {
    console.log('Light is already off.')
  }
}

class OnState extends State {
  constructor() {
    super()
    console.log('Light turned on.')
  }

  off(sw) {
    console.log('Turning light off...')
    sw.state = new OffState()
  }
}

class OffState extends State {
  constructor() {
    super()
    console.log('Light turned off.')
  }

  on(sw) {
    console.log('Turning light on...')
    sw.state = new OnState()
  }
}

let sw = new Switch()
sw.on()
sw.off()
sw.off()
```

# Handmade State Machine

```jsx
const readline = require('readline')

let rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
})

let State = Object.freeze({
  offHook: 'off hook',
  connecting: 'connecting',
  connected: 'connected',
  onHold: 'on hold',
  onHook: 'on hook',
})

let Trigger = Object.freeze({
  callDialed: 'dial a number',
  hungUp: 'hang up',
  callConnected: 'call is connected',
  placedOnHold: 'placed on hold',
  takenOffHold: 'taken off hold',
  leftMessage: 'leave a message',
})

let rules = {}
rules[State.offHook] = [
  {
    trigger: Trigger.callDialed,
    state: State.connecting,
  },
]
rules[State.connecting] = [
  {
    trigger: Trigger.hungUp,
    state: State.onHook,
  },
  {
    trigger: Trigger.callConnected,
    state: State.connected,
  },
]
rules[State.connected] = [
  {
    trigger: Trigger.leftMessage,
    state: State.onHook,
  },
  {
    trigger: Trigger.hungUp,
    state: State.onHook,
  },
  {
    trigger: Trigger.placedOnHold,
    state: State.onHold,
  },
]
rules[State.onHold] = [
  {
    trigger: Trigger.takenOffHold,
    state: State.connected,
  },
  {
    trigger: Trigger.hungUp,
    state: State.onHook,
  },
]

let state = State.offHook
let exitState = State.onHook

let getInput = function () {
  let prompt = [`The phone is currently ${state}`, "What's next:"]

  for (let i = 0; i < rules[state].length; ++i) {
    let t = rules[state][i].trigger
    prompt.push(`${i}. ${t}`)
  }

  // force an extra line break
  prompt.push('')

  rl.question(prompt.join('\n'), function (answer) {
    let input = parseInt(answer)
    state = rules[state][input].state

    if (state !== exitState) getInput()
    else {
      console.log('We are done using the phone.')
      rl.close()
    }
  })
}

getInput()
```

# Summary

- Given sufficient complexity, it pays to formally define possible states and events/triggers.
  Can define
  - State entry/exit behaviors.
  - Action when a particular event causes a transition.
  - Guard conditions enabling/disabling a transition.
  - Default action when no transitions are found for an event.
