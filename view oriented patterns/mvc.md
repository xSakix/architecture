# Model View Controller

UI/View(A rest service is also 'just' a view..) is prone to change. Building a system in which UI and logic is tightly coupled is inefficient a hard to change. 

## The problem

* Same information is presented in different views
* It is required to present data manipulation immediatly
* Changes to the view should be easy and at run-time
* Support different views/looks at the data (Diff UI, diff service fronts)

## The solution

MVC divides app into 3 areas: processing, output and input.

**Model** encapsulates core data and functionality.(So not only data but also functionality!!!) 

**View** displays/presents data to the user (can be human, can be another system in case of services). View obtains data from model. There can be multiple views of the model.

Each view has a **Controller**. They receive input as events(UI interactions, system events,etc.). These are translated to **service requests** for the model and or the view. User interacts with the system only through controllers!

## Model

* contains functional core of the app
* encapsulates data and exports *procedures*(functions/API?)
* controller calls procedures on behalf of user/client
* provides entry points/functions to access data to display/present them
* *change propagation mechanism* to inform registered views/controllers of changes to the data (pub/sub)
* changes to the model **trigger** the change propagation mechanism

## View
 
* presents information to the user/client
* defines an *update procedure* that is activated by the *change propagation mechanism*
* when the *update pocedure* is called, the view retrieves the current data from model and displays/presents them to the user
* on init associates with model
* on init creates associated controller (one-to-one relation)
* views provide functionality for controller to manipulate it. These do not alter the model, but only the view.


## Controller

* accepts user/system input as events
* delivery of events to controller is paltform dependent
* event handling
* events get translated into requests for model or view
* if it depends on the state of the model, it registers via an *update procedure* with the model. (for example if a menu entry gets disabled by data change)

## Comment

So the view changes based on the model change. But only to present informations. Not to disable/enable parts of itself. That funcionality is associated with the controller. When data changes in such way, that some part of view needs to enable/disable etc, then the controler is notified from the model. And the controller does the change to the UI/view.

Does this apply to services also? Lets say when logic changes access rights based on some business step and the user of the service doesn't get some parts of the data anymore,.e.g. part of the view is disabled.

## Other similar patterns

* Model View Adapter (MVA)
* Model View Presenter (MVP)
* Model View Viewmodel (MVVM)
* Presentation Abstraction Controll (PAC)
* State Action Model (SAM)

## Sources:

[Pattern-Oriented Software Architecture Volume 1: A System of Patterns](https://www.goodreads.com/book/show/85039.Pattern_Oriented_Software_Architecture_Volume_1)

[Patterns of Enterprise Application Architecture](https://www.goodreads.com/book/show/70156.Patterns_of_Enterprise_Application_Architecture)




