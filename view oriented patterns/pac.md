# Presentation Abstraction Control (PAC) pattern

Structure of system is defined by a hierarchy of cooperating agents. Every agent is responsible for a specific part of the application and consists of three parts:

* Presentation - UI
* Abstraction - model, functionality
* Control - communication with other agents

## Problem

UI decomposed into cooperating parts = agents. Agents can be interactive(UI) or non interactive(communicating with other parts of the system, or other systems.). Each agent is specialized on a specific task and together they provide the system functionality. 

* Agents maintain their own state and data. 
* Individual agents must effectivly cooperate to provide the overall task of the application. Thus they need a mechanism for exchanging data, messages and events.
* Interactive agents provide they own UI.
* System evolve over time. The presentation part is prone to change. Changes to individual agents or extension with new agents shouldn't affect the whole system.

## Solution

Structure the application as  a tree like hierarchy of PAC agents:

* **Top agent** - only one(one rules them all). Provides the functional core of the system. Most of the other agents depend or operate on this core. Contains functionality which cannot be assigned to some specific task, or component.

* **Intermediate agents** - several agents. Represent combinations of or relationships between lower-level agents (e.g. may represent several views of one data, where each view is some bottom level agent). 

* **Bottom agents** - the most of agents will be here. Represent self contained semantic concepts on which users can act (specific detail/form/list/tree/etc..). They present these to the user and support all operations on them. 

**Note**:
*Centralized structure of agents/components*

Each agent consists of:

* **Presentation** - the visible part of behavior
* **Abstraction** - maintains the data model and provides functionality to operate on the data
* **Control** - connect the presentation and abstraction components, and provides means of communication with other agents.

**Note**:
In an example POSA specifies: 
* a repository for data, which is outside of the system
* **only** the top level agent acceses the repository to work with it's data. Other agents communicate with the top agent to access the data indirectly. (By access they mean CRUD + query on data)


### Top level agent

* provides the functional core of the system
* controls the PAC hierarchy
* collaborates with:
  * intermediate-level agents
  * bottom-level agents
  
### Intermediate level agent

* coordinates lower-level PAC agents
* composes lower-level PAC agents to a single unit of higher abstraction
* collaborates with:
  * top-level agent
  * other intermediate-level agents
  * bottom-level agents
  
### Bottom level agent

* provides specific view of the software or a system service, including its user interactions
* collaborates with:
  * top level agent
  * intermediate-level agent
  
**Note start**

POSA in a example structure defines for each agent two methods:
* sendMessage
* receiveMessage

Each agent has its own data which belong to its function. Top level has repository. Intermediate(viewcontrol) has a list of bottom-level agents which present the data from top level agents repository. Bottom level agents are:
* ErrorHandler - contains errorData, displays Error Message to the user, and handles the error messages
* Spreadsheet - contains election data(spreadsheet data), which it stores into top level agents. It contains functions for open and close (I guess of the spreadsheet) and a function to enter data into it.
* BarChart - contains bar data, and functions for open, close, zoom, move and printing of the chart
* PieChart - contains pie data, and functions for open, close, zoom, move, print
* Seats(special view showing elected seats in senate/parliament/etc.) - contains the seat data, and functions for open, close, zoom, move, print

Dynamics is as follows:

* User wants to open a new bar chart
* He asks the **Presentation** component of **ViewCoordinator**(intermediate agent) to open a new bar chart
* The **Control** part of the intermediate-level agent instantiates the BarChart bottom-level agent
* The **Control** part of the intermediate-level agent sends an *open* command to the bottom-level agent
* The **Control** part of bottom-level agent(BarChart) retrieves data from top-level agent. The intermediate-level agent mediates between top-level and bottom-level. 
* The data returned to the bottom-level agent is saved into it's **Abstraction** component.
* Bottom-level's **Control** component calls it's **Presentation** component to display the chart.
* The **Presentation** component of bottom-level agent creates a new window on the screen, retrieves data from the **Abstraction** component and finally displays it within the window.

**Note end**


## Implementation considerations

1. **Define a model of the application** - Analyze the problem domain. Don't consider the distribution of components onto agents for now. Find a proper decomposition and organization of the application domain. Answer following questions:
    * which services should the system provide?
	* which components can fulfill these services?
	* what are the relationships between components?
	* how do the components collaborate?
	* what data do the components operate on?
	* how will the user interact with the software?

2. **Define a general strategy for organizing the PAC hierarchy** - Follow the rule of "lowest common ancestor". When a group of agents depend on the service or data of another agent, we group them into a subtree with the provider agent as root. The deeper the hierarchy the better it often reflects the decomposition of an application into self-contained concepts. But deep hierarchies can be inefficient at runtime (note: does it still hold even today?)

3. **Specify top-level agent** -Identify parts which represent the core functionality of the system. These are mostly components that maintain the global data model of the system and components directing operations on them. 

4. **Specify the bottom-level agents** - idenitfy the smallest self-contained units of the system

5. **Specify bottom-level PAC for system services** - error handling, communication, etc..

6. **Specify intermediate-level agents to compose lower level agents** - several lower-level agents form together a higher semantic concept.

7. **Specify intermediate-level agents to coordinate lower-level agents** - when lower-level agents represent different views on same data, the form the same semantic concept. Can also be a coordination of communication over network, etc.. So not only visual representation in UI of the same data, but in a general way different view on data, be it visual or nonvisual.

8. **Separate core functionality from user interactions** - for every agent introduce **abstraction** and **presentation** components. U can provide unified interface to the abstraction and presentation components using [Facade design pattern](https://github.com/xSakix/architecture/blob/master/design%20patterns/facade.md). The **control** component then exports those parts of the **abstraction** and **presentation** interface that other components can use. 

	Top level agents often do not provide a presentation component. They are suggestions, that these should provide geometric spatial relationships in such agents.
	
	You can also apply the [Command pattern](https://github.com/xSakix/architecture/blob/master/design%20patterns/command.md) to further organize the presentation components. That alows you to shcedule user requests for deferred or prioritized execution and to provide agent specific undo/redo services.

	Some lower-level agents operate on data provided byt other agents. In this case you may either not provide an **abstraction** component or use it as a **cache**  (you will need to provide some kind of replication mechanism on data changes.).
	
	Finally introduce the **control** component to mediate between the **abstraction** and **presentation** component and to avoid **dependences** between them. Use the [Adapter pattern](https://en.wikipedia.org/wiki/Adapter_pattern). It will link the presentation and abstraction components together by performing interface and data adaptation between them. In this step do not consider the parts which deal with the communication between agents. That is a different role and should be separated form the mediation part between abstraction and presentation.

9. **Provide an extrernal interface** - To cooperate with other agents every agent send and receives events and data. Implement this functionality as part of the **control** component. Within and agent events and data are forwarded to their intended recipients. Those recipients can be another agents. In that case the component is a mediator and you may use [Mediator pattern](https://github.com/xSakix/architecture/blob/master/design%20patterns/mediator.md).
	
	One way to implement communication with other agents is via [Composite Message Pattern](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.47.1184&rep=rep1&type=pdf). It allows agents to be independent of the specific interfaces of other agents, and also of particular data formats, marshalling, unmarshalling, fragmentation and re-assembling methods. The **control** component will have to interpret incomming messages. Which can be complex.

	Another option is to provide a public interface that offers every service of an agent as a separate function. These *functions* know how to handle data and events when called. Reduces complexity of *control* component but introduces additional dependencies between agents - they'll depened on specific interfaces of other agents! Aditionaly the interface can explode. Every agent in *chain* must implement it, even to just delegate the execution to higher or lower level agent. As a result the inerfaces may become complex and hard to maintain.
	
	One agent can be connected to another via [Publish Subscribe pattern](https://github.com/xSakix/architecture/blob/master/design%20patterns/publisher_subscriber.md)
	
	If an agent depends on data or information from another agent we should provide a *change propagation mechanism*. Such mechanism should involve **all agents** and **all levels of hierarchy** and work **bidirectional**. When changes happen to agents data, the **abstration** component starts the change propagation. The **control** component forwards change propagation notifications to all dependent agents and often also to the **presentation** component. Incomming change notifications from other agents cause the **abstraction** and **presentation** components to update their internal states. One way to implement this is [Publish Subscribe pattern](https://github.com/xSakix/architecture/blob/master/design%20patterns/publisher_subscriber.md). Another way is to integrate change propagation with the general functionality for sending and receiving events, messages  and data.

10. **Link the hierarchy together** 

## Variants

Multitasking is a common problem. There are two variants which deal with this.

### Agents as Active objects

* multithreading
* every agent as an active object that lives in it's own thread of control*
* see [Active object desing pattern](https://en.wikipedia.org/wiki/Active_object)  or [Half-Sync/Half-Async design pattern](https://www.dre.vanderbilt.edu/~schmidt/PDF/PLoP-95.pdf)

### Agents as Processes 

* to support agents located in different processes or on remote machines, use [Proxies](https://en.wikipedia.org/wiki/Proxy_pattern) to localy represent these agents and to avoid dependencies on their physical location.
* use [Forwarder-Receiver pattern](http://software-pattern.org/Forwarder-Receiver) or
* [Client-Dispatcher-Server pattern](http://software-pattern.org/Client-Dispatcher-Server)
* consider organizing coherent subtrees of agents within different processes. Agents which cooperate closely are then in the same process.

## Known uses

### Network traffic management

* displays traffic in telecomunications network

Functions:

* gathering data from switching units
* threshold checking and generation of overflow exceptions
* logging and routing of network exceptions
* visualization of traffic flow and network exceptions
* displaying various user-configurable views of the whole network
* statistical evaluation of traffic data
* system administration and configuration

Every function is represented by a bottom-level agent. Three intermediate agents coordinate these bottom-level agents, one for each category: vies, jobs and additional services. They also organize use sessions and communicate with functional core. Core is independent of PAC system in a legacy system.

### Mobile robot

Operation of a mobile robot that navigates within a closed and hazardous environment. Each wall

Each element of environment(walls, routes, place,etc) is a bottom-level agent. Together they visualize the environment. The environments are represented by intermediate agents. There is something like a pallete agent (control of environment). Pallete and environment agent for a workspace agent. Multiple workspaces form a Multi workspace agent. Each multi workspace agent belongs to the top agent.

## Benefits

* separation of concern
* support for change and extension
* support for multi-tasking

## Liabilities

* system complexity
* complex control component
* efficiency 
* applicability - the more similar are the lowest units the less applicable is this pattern

## Other similar patterns

* [Model View Component](https://github.com/xSakix/architecture/blob/master/view%20oriented%20patterns/mvc.md)
* Model View Adapter (MVA)
* Model View Presenter (MVP)
* Model View Viewmodel (MVVM)
* State Action Model (SAM)
* Flux



## Sources

[Pattern-Oriented Software Architecture Volume 1: A System of Patterns](https://www.goodreads.com/book/show/85039.Pattern_Oriented_Software_Architecture_Volume_1)


