# Front Controller

A controller which handles all requests for a Web site.

## Problem

In a complex Web site there are similar things you need to do when a request arives:

* security
* internaionalization
* provide views for certain users

If the input controller is scattered across multiple objects much of this behavior can end up duplicated.
(Note: [AOP](https://en.wikipedia.org/wiki/Aspect-oriented_programming) would probably solve this...)

Also it's difficult to change behavior at runtime.

The forces are:

* you want to avoid duplicate control logic
* you want to apply common logic to multiple requests
* you want to separate system processing logic from the view
* you want to centralize controlled access points into your system


## Solution

The Front Controller consolidates all request handling by channeling requests through a single handler object. This object can carry out common behavior which can be modified at runtime with [decorators](https://en.wikipedia.org/wiki/Decorator_pattern). The handler then dispatches to [command object](https://en.wikipedia.org/wiki/Command_pattern) for behavior particular to a request.

In j2ee the Front controller typically uses also an [Application Controller](http://javaguides.net/2018/08/application-controller-design-pattern-in-java.html) which is responsible for action and view management:

* action management refers to locating and routing to the specific actions that will service a request
* view management refers to finding and dispatching the appropriate view. 


(Note: So you either do a command pattern, where specific commands represent actions and respond with particular view. Or you use an Application Controller, which goes a step further, has a Map/Mapper of actions, which are a particular command and a view. )

## Implementation

Two parts:

* Web handler - receives POST and GET requests. Decides based on URL and request which command to invoke. Implemented as a class and not as a server page. Decision which command to run can be:
	* static - based on URL actions commands are defined in code
	* dynamic - additional configuration file/options can be used to asign commands to URL actions
	
* Command hierarchy - classes, not server pages. Don't require knowledge of web environment. They often get the http information on input.Decide which view should be presented to user in response. With each request a new command/commands are created. Thus they should be thread safe.

Can be used with [Interceptor Filter](https://en.wikipedia.org/wiki/Intercepting_filter_pattern). Using filters allows you to dynamically set up filters to use at configuration time.


## Known implementations

* [Spring MVC](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-servlet) and the concrete implementation od [DispatcherServlet](https://github.com/spring-projects/spring-framework/blob/master/spring-webmvc/src/main/java/org/springframework/web/servlet/DispatcherServlet.java)

* [Struts ActionServlet](https://svn.apache.org/repos/asf/struts/archive/trunk/struts-doc-1.1/api/org/apache/struts/action/ActionServlet.html)

## Source

[Patterns of Enterprise Application Architecture](https://www.goodreads.com/book/show/70156.Patterns_of_Enterprise_Application_Architecture)

[Core j2ee patterns](https://www.goodreads.com/book/show/85017.Core_J2EE_Patterns)
