# Command pattern (Action, Transaction)

Encapsulate a request as an object, thereby letting you parametrize clients with different requests, queue or log requests, and support undoable operations.

## Problem

Necessity to issue requests to objects without knowing anything about the operation being requested or the receiver of the request. (example some menu button, which carries request in response to user input(click). The request isn't implemented explicitly in the implementation of the UI menu button in a specific library/toolkit, but it's implemented later on by the developer who whishes to use the button in some form/etc.. So the button iteself doesn't know what should be done, who is the receiver of the event, etc.)

## Solution

* turn the request itself into and object
* the object can be passed around and stored as other objects
* key is an abstract Command, which declares an interface for executing operations

<pre>
Command
-------
+ Execute()
</pre>

* concrete implementations of Command specify a receiver-action pair by storing the receiver as an instance variable and by implementing *Execute* to invoke the request. The receiver has the knowledge required to carry out the request. (e.g. save button -> SaveCommand->Execute() = some specific action which calls insert into the database, where the database is the receiver)

## Applicability

* when you want to parametrize object by an action to perform. In a procedural way it would  be a **callback** method. Commands are an object-oriented replacement for callbacks.

* when you need to specify, queue, and execute requests at different time.Command object can have independent lifetime of the original request. Can be a different process, etc..

* support undo. Could be supported via an *Unexecute* method, which restores original state. Also a history of commands could be used to make unlimited undo.

* support logging changes so that they can be reapplied in case of a system crash. By augmenting the Command with load and store operations, you can keep a persistent log of changes. Recovering from a crash involves reloading logged commands from disk and reexecuting them.

* structure a system around high level operations build on primitive operations.Common in transactions.

## Participants

* Command - declares an interface for executing commands
* Concrete Command :
	* defines a binding between a Receiver object and an action
	* implements Execute by invoking the corresponding operation(s) on Receiver
* Client(Applicaiton) - creates a Concrete Command and sets its Receiver
* Invoker - asks the Command to carry out the request
* Receiver - knows how to perform the operations associated with carrying out a request.

## Collaborations

* client creates a Concrete Command and specifies it receiver
* invoker storec the Concrete Command
* invoker issues requests by calling Execute on the Command.If it is undoable, the Command stores state prior of Execute.
* Concrete Command invokes operations on its receiver to carry out the request.

## Consequences

* **Command decouples the object that invokes the operation from the one that knows how to perform it**.
* You can assemble commands into a composite command(see [Composite pattern](https://en.wikipedia.org/wiki/Composite_pattern)).
* it's easy to add new commands 

## Implementation

* How intelligent should a command be?
* Supporting undo and redo?
* Avoiding error accumulation in undo/redo process.

## Sources

[Design Patterns: Elements of Reusable Object-Oriented Software](https://www.goodreads.com/book/show/85009.Design_Patterns)
