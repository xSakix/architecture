# Mediator pattern

Define an object which defines how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.

## Problem

* Lots of interconnections make it less likely that an object can work without the support of others - the system acts as though it were **monolithic**.

* OO design encourages the distribution of behavior among objects, thus resulting in an object structure with many connections between objects.

* Partitioning system into many objects generally enhances reusability.

* Proliferating interconnections tend to reduce reusability

* It can be difficult to change behavior of system in any significant way, since behavior is distributed among many objects.

(Note: to many dependencies or dependants make any system hard to change.)


## Solution

* Encapsulate collective behavior into a separate object **Mediator**
* **Mediator** is responsible for controlling and coordinating the interactions of a group of objects.
* **Mediator** serves as an intermediary that keeps objects in the group from referring to each other explicitly.


## Applicability

Use when

* a group of objects communicate in a well-defined but complex ways.The resulting interdependencies are unstructured and difficult to understand.
* reusing an object is difficult bcause it refers to and communicates with many other objects.
* a behavior that's distributed among many classes should be customizable without a lot of subclassing.



## Participants

* Mediator - defines an interface for communicating with Colleague objects
* ConcreteMediator - implements cooperative behavior by coordinating Colleague objects. Knows and maintains its collegues.
* Colleague classes - each Colleague class knows its mediator object. Each colleague class communicates with the mediator whenever it would have otherwise communicated with another colleague.


## Collaborations

* Colleages send and receive requests from a Mediator object. The mediator implements the cooperativ behavior  by **routing** requests among the appropriate colleagues.

## Consequences

* limits subclassing by localizing behavior which would otherwise be distributed among several objects
* decouples colleagues by promoting loose coupling between them
* simplifies object protocols by replacing many-to-many interactions with one-to-many interactions between the mediator and its colleagues.
* abstracts how objects cooperate by separating objects interactions from their individual behavior.
* centralizes control


## Implementation

* You don't need an Abstract Mediator, when Colleagues work with only one Mediator
* Colleague-Mediator communicate when an event of interests occurs. One approach is to implement/use the [Observer pattern](https://github.com/xSakix/architecture/blob/master/design%20patterns/publisher_subscriber.md). The colleagues classes send notifications of change to the mediator, which propagates the change to others.


## Other/similar patterns

* [Facade](https://github.com/xSakix/architecture/blob/master/design%20patterns/facade.md)
