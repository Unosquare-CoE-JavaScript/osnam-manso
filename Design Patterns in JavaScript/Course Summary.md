# Creational

## Builder

- Separate component for when object construction gets too complicated

- Both define a skeleton algorithm with details filled in by implementor.
- Can create mutually cooperating sub-builders.
- Often has a fluent interface.

## Factories

- Factory methos more expressive than constructor.
- A separate class with factory methods is a Factory.
- Class hierarchies can have corresponding hierarchies of factories (Abstract Factory).

## Prototype

- Creation of object from an existing object.
- Requires either explicit deep copy or copy through serialization.
- Additional work required to preserve type.

## Singlenton

- When you need to ensure just a single instance exists.
- Can return same object from constructor on every call.
- Direct dependence on a Singleton is dangerous.

# Structural

## Adapter

- Converts the interface you get to the interface you need.

## Bridge

- Decouple abstraction from implementation.

## Composite

- Allows clients to treat individual objects and compositions of objects uniformly.

## Decorator

- Attach additional responsibilities to objects without modifying those objects or ingeriting from them.
- Decorators are composable eith each other.

## Facade

- Provide a single unified interface over a set of systems/interfaces.
- Flyweight
  - Memory saving technique.
  - Efficiently support very large numbers of similar objects.

## Flyweight

- Memory saving technique.
- Efficiently support very large numbers of similar objectgs.

## Proxy

- Provide a surrogate object that forwards calls to the real object whhile performing additional functions.
- E.g., access control, communication, logging, etc.

# Behevioral

## Chain of Responsibility

- Allow compontnts to process information/events in a chain
- Each element in the chain refers to nex element; or
- Make a list and go through it.

## Command

- Encapsulate request into separate objects.
- Good for audit, replay, undo/redo.
- Part of CQS/CQRS

## Interpreter

- Transform textual input into object-oriented structures.
- Used by interpreters, compoilers, static analysis tools, etc.
- Compiler Theory is a separete branch of Computer Science.

## Iterator

- Provide an interface for accessing elements of an aggregate object.
- Objects can bi made iterable (for loop)

## Mediator

- Provides mediation services between two objects.
- E.g., message passing, chat rooom.

## Memento

- Yields tokens representing system states.
- Tokens do not allow directmanipulation, but can be used in appropriate APIs.

## State

- We model systems by having one of a possible states and transitions.
- Such a system is called a state machine.
- Special frameworks exists to orchestrate state machines.

## Strategy & Template Method

- Both define a skeleton algorithm with details filled in by implementor.
- Strategy uses ordinary composition, tempplate method uses inheritance.

## Visitor

- Allows non-intrusive addition of functionality to hierarchies.
