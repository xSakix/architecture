# Facade pattern

Provides a unified interface to a set of interfaces in a subsystem. 

## Motivation

A common design goal is to minimize the communication and dependencies between subsystems. Facade provides a single simplified interface to the more general facilities of a subsystem.

## Applicability

* when you want to provide a simple interface to a complex subsystem.
* when there are many dependencies between clients and the implementation classes of an abstraction. Introduce a facade to decouple the subsystem from clients and other subsystems, promoting subsystems independence and portability.
* in a layered architecture use facade as entry point to each subsystem level.

## Structure

### Facade

* knows which subsystem classes are responsible for a request
* delegates client requests to appropriate subsystem objects

### Subsystem classes

* implement functionality
* handle work assigned by facade
* have no knowledge of the facade


## Collaborations

* clients talk to facade. Facade forwards requests to subsystem objects. facade may tranform request into other form required by the subsystem object.
* clients don't have access to subsystem objects directly.

## Consequences

* shields clients from subsystem components, which make the subsystem easier to use.
* promotes weak coupling between subsystem and clients. The components of the subsystem can change without affecting its clients.
* facade helps to layer the system and dependencies between objects.
* can elimininate complex or circular dependencies
* enables independent change of subsytems
* doesn't prevent the use of subsystem classes, thus you can choose between easy of use and generality.

## Implementation

* Reducing client-subsystem coupling. Consider using an Abstract facade to enable concrete implementations of subsystem types. Alternativly configure facade with different subsystem objects. To customise the facade simply replace one or more of it's subsystem components. (Note: imagine injection and runtime configuration of injected *'objects'*)
* Public versus private subsystem classes - facade provides public interface to the private parts of the subsystem.

## Related patterns

* [Abstract factory](https://en.wikipedia.org/wiki/Abstract_factory_pattern)
* [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern)

## Other/similar patterns

* [Mediator](https://en.wikipedia.org/wiki/Mediator_pattern)

