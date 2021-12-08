# Motivation

Exposing several components through a single interface.

- Balancing complexity and presentation/usability
- Typical home
  - Many subsystems (electrical, sanitation)
  - Complex internal structure (e.g., floor laters)
  - End user is not exposed to internals.
- Same with software
  - Many sustems working to provide flexibility, but...
  - API consumers wat it to 'just wordk'

Provides a simple, easy to understand/user interface over a large and sophisticated body of code.

# Summary

- Build a Facade to provide a simplified API over a set of classes
- May wish to (optionally) expose internals through the facade.
- May allow users to 'escalate' to use more complex APIs if they need to.
