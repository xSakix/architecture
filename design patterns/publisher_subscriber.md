# Publisher Sunscriber pattern (observer pattern, dependents pattern)

Helps to keep the state of cooperating components synchronized.

## The problem

• one or more component needs to be notified about state changes in another comp
• the number of notified comp is not known apriori and can change over time
• explicit polling for new information for dependent components is not feasible
• the info publisher and its dependents should not be tightly coupled when introducing change propagation mechanism

## Solution

* Dedicated component is called **publisher** (pub). 
* Dependent components are called **subscribers**(subs, observers).
* Publisher contains a registry of currently subscribed components. 
* Whenever a subscriber wants to subscribe/unsubscribe it uses the Subscriber interface (Note: this seems like an implementation detail. In my opinion, the mechanism at which one subscribes can wary) of the publisher. 
* Whenever the publisher changes state it notifies/sends notification to all subscribers. Subscribers may then retrieve the data at their discretion.
* the publisher decides on which changes of internal state notifies the subs. Can be individual change or it may queue several changes
* one can subscribe to multiple publishers
* one can be a subscriber and a publisher at the sametime
* Notifications can be differentiated by event type. This way the subscriber gets notified only on events it cares about.
* The pub can send the changed data with the notification. Or just notify about data change and.leave the data gathering task to the sub(if it needs it)

There are two models:

* **push** - publisher send the data with the notification. Subscribers cant decide when and how.they will.retrieve the data.
* **pull** - pub snd notification. Subs retrieve the changed data.

Push is not suitable for complex data changes, when the pub sends all the data to the subs and especially when the sub isn't interested in the data. Even when pub sends only info about which data changed and not the whole data, it can be a significant overhead.

The process of finding sucessive data changes can be organized into a [decision tree](https://en.wikipedia.org/wiki/Decision_tree). (I would realy like to know how?)

* Use push when subs need data all the time.
* Use pull if not all subs need data or not all of the data all the time.

## Variants

### Gatekeeper

* applied to **distributed systems**
* publisher instance in one process notifies remote subscribers
* publisher may be spread into two processes 
  * One process to send messages
  * Receiving process demultiplexes(???) them by surveying the entry points to the process (???)
* Event driven. Subs are notified on event/events.
* Uses [The Reactor pattern](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.37.9570)

Note:

I guess this is not a MQ solution, but the event channel could be. This is more like an Akka solution?

### Event chanel

* targets **distributed systems**
* **Strongly** decouples publishers and subscribers
* There can be more publishers
* Subscribers will be notified only on the changes, not on the identity of the publisher
* Publishers are not interested which components are subscribing
* Event channel is created and put between pub and sub
* To pub the event channel looks like a sub
* To sub the event channel looks like a pub
* multiple event channels
* additional channels can provide additional capabilities like filtering, storing event for a period of time (QoS) 
* a chain can be used to sum all the needed capabilities for a system (e.g. Unix pipes)



Note no.1:
POSA provides a possible implementation with proxes

Pub <-> pub proxy <-> sub proxy <-> event channel <-> pub proxy <-> sub proxy <-> sub


Note no.2:  
MQ distributed solutions? Like RabbitMQ, Redis, Kafka? How are they implemented with regards to this archi pattern?

### Producer-Consumer

* uses (Producer-Consumer)[https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem] style of cooperation
* producer supplies information for the consumer to process
* strongly decoupled, by placing e.g. a buffer between them
* the producer is suspended when the buffer is full
* consumer waits for the buffer to read data when it's empty
* prod-con are usually 1:1
* only event channel can simulate a prod-con pattern with multiple prod/con
* in case of multiple producers, they write into buffer in series, directly or indirectly
* in case of multiple consumers, when the event is read, it is not deleted. Instead it is marked as such(READ) for the consumer. It gives the ilusion, for the given consumer, that it is consumed. While other consumers can still read/consume it. A good way of implementing it is via (Iterator pattern)[https://en.wikipedia.org/wiki/Iterator_pattern]. Each consumer will have its own iterator on the buffer.The position of the iterator on the buffer reflects how far the consumer has read the buffer. Data can be purged once all the iterators have read it.

Note:

The iterator idea is nice. Maybe a Task which can be divided into individual subtasks is a nice candidate for this 'pattern'. 

Also could be MQ solutions, Kafka, etc.. 

















