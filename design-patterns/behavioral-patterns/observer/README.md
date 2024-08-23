---
description: 'Also known as: Event-Subscriber, Listener'
---

# Observer

### Intent <a href="#intent" id="intent"></a>

**Observer** is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

<figure><img src="https://refactoring.guru/images/patterns/content/observer/observer.png" alt="Observer Design Pattern" width="640"><figcaption></figcaption></figure>

### &#x20;Problem <a href="#problem" id="problem"></a>

Imagine that you have two types of objects: a `Customer` and a `Store`. The customer is very interested in a particular brand of product (say, it’s a new model of the iPhone) which should become available in the store very soon.

The customer could visit the store every day and check product availability. But while the product is still en route, most of these trips would be pointless.

<figure><img src="https://refactoring.guru/images/patterns/content/observer/observer-comic-1-en.png" alt="Visiting store vs. sending spam" width="600"><figcaption><p>Visiting the store vs. sending spam</p></figcaption></figure>

On the other hand, the store could send tons of emails (which might be considered spam) to all customers each time a new product becomes available. This would save some customers from endless trips to the store. At the same time, it’d upset other customers who aren’t interested in new products.

It looks like we’ve got a conflict. Either the customer wastes time checking product availability or the store wastes resources notifying the wrong customers.

### &#x20;Solution <a href="#solution" id="solution"></a>

The object that has some interesting state is often called _subject_, but since it’s also going to notify other objects about the changes to its state, we’ll call it _publisher_. All other objects that want to track changes to the publisher’s state are called _subscribers_.

The Observer pattern suggests that you add a subscription mechanism to the publisher class so individual objects can subscribe to or unsubscribe from a stream of events coming from that publisher. Fear not! Everything isn’t as complicated as it sounds. In reality, this mechanism consists of 1) an array field for storing a list of references to subscriber objects and 2) several public methods which allow adding subscribers to and removing them from that list.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/observer/solution1-en.png" alt="Subscription mechanism" width="470"><figcaption><p>A subscription mechanism lets individual objects subscribe to event notifications.</p></figcaption></figure>

Now, whenever an important event happens to the publisher, it goes over its subscribers and calls the specific notification method on their objects.

Real apps might have dozens of different subscriber classes that are interested in tracking events of the same publisher class. You wouldn’t want to couple the publisher to all of those classes. Besides, you might not even know about some of them beforehand if your publisher class is supposed to be used by other people.

That’s why it’s crucial that all subscribers implement the same interface and that the publisher communicates with them only via that interface. This interface should declare the notification method along with a set of parameters that the publisher can use to pass some contextual data along with the notification.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/observer/solution2-en.png" alt="Notification methods" width="460"><figcaption><p>Publisher notifies subscribers by calling the specific notification method on their objects.</p></figcaption></figure>

If your app has several different types of publishers and you want to make your subscribers compatible with all of them, you can go even further and make all publishers follow the same interface. This interface would only need to describe a few subscription methods. The interface would allow subscribers to observe publishers’ states without coupling to their concrete classes.

### &#x20;Real-World Analogy <a href="#analogy" id="analogy"></a>

<figure><img src="https://refactoring.guru/images/patterns/content/observer/observer-comic-2-en.png" alt="Magazine and newspaper subscriptions" width="600"><figcaption><p>Magazine and newspaper subscriptions.</p></figcaption></figure>

If you subscribe to a newspaper or magazine, you no longer need to go to the store to check if the next issue is available. Instead, the publisher sends new issues directly to your mailbox right after publication or even in advance.

The publisher maintains a list of subscribers and knows which magazines they’re interested in. Subscribers can leave the list at any time when they wish to stop the publisher sending new magazine issues to them.

### Applicability <a href="#applicability" id="applicability"></a>

&#x20;Use the Observer pattern when changes to the state of one object may require changing other objects, and the actual set of objects is unknown beforehand or changes dynamically.

&#x20;You can often experience this problem when working with classes of the graphical user interface. For example, you created custom button classes, and you want to let the clients hook some custom code to your buttons so that it fires whenever a user presses a button.

The Observer pattern lets any object that implements the subscriber interface subscribe for event notifications in publisher objects. You can add the subscription mechanism to your buttons, letting the clients hook up their custom code via custom subscriber classes.

&#x20;Use the pattern when some objects in your app must observe others, but only for a limited time or in specific cases.

&#x20;The subscription list is dynamic, so subscribers can join or leave the list whenever they need to.

### &#x20;How to Implement <a href="#checklist" id="checklist"></a>

1. Look over your business logic and try to break it down into two parts: the core functionality, independent from other code, will act as the publisher; the rest will turn into a set of subscriber classes.
2. Declare the subscriber interface. At a bare minimum, it should declare a single `update` method.
3. Declare the publisher interface and describe a pair of methods for adding a subscriber object to and removing it from the list. Remember that publishers must work with subscribers only via the subscriber interface.
4.  Decide where to put the actual subscription list and the implementation of subscription methods. Usually, this code looks the same for all types of publishers, so the obvious place to put it is in an abstract class derived directly from the publisher interface. Concrete publishers extend that class, inheriting the subscription behavior.

    However, if you’re applying the pattern to an existing class hierarchy, consider an approach based on composition: put the subscription logic into a separate object, and make all real publishers use it.
5. Create concrete publisher classes. Each time something important happens inside a publisher, it must notify all its subscribers.
6.  Implement the update notification methods in concrete subscriber classes. Most subscribers would need some context data about the event. It can be passed as an argument of the notification method.

    But there’s another option. Upon receiving a notification, the subscriber can fetch any data directly from the notification. In this case, the publisher must pass itself via the update method. The less flexible option is to link a publisher to the subscriber permanently via the constructor.
7. The client must create all necessary subscribers and register them with proper publishers.

| PROS                                                                                                                                                                                                                                                         | CONS                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| <ul><li><em>Open/Closed Principle</em>. You can introduce new subscriber classes without having to change the publisher’s code (and vice versa if there’s a publisher interface).</li><li> You can establish relations between objects at runtime.</li></ul> | <p></p><ul><li> Subscribers are notified in random order.</li></ul><p></p> |

### Relations with Other Patterns

* [Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility), [Command](https://refactoring.guru/design-patterns/command), [Mediator](https://refactoring.guru/design-patterns/mediator) and [Observer](https://refactoring.guru/design-patterns/observer) address various ways of connecting senders and receivers of requests:
  * _Chain of Responsibility_ passes a request sequentially along a dynamic chain of potential receivers until one of them handles it.
  * _Command_ establishes unidirectional connections between senders and receivers.
  * _Mediator_ eliminates direct connections between senders and receivers, forcing them to communicate indirectly via a mediator object.
  * _Observer_ lets receivers dynamically subscribe to and unsubscribe from receiving requests.
*   The difference between [Mediator](https://refactoring.guru/design-patterns/mediator) and [Observer](https://refactoring.guru/design-patterns/observer) is often elusive. In most cases, you can implement either of these patterns; but sometimes you can apply both simultaneously. Let’s see how we can do that.

    The primary goal of _Mediator_ is to eliminate mutual dependencies among a set of system components. Instead, these components become dependent on a single mediator object. The goal of _Observer_ is to establish dynamic one-way connections between objects, where some objects act as subordinates of others.

    There’s a popular implementation of the _Mediator_ pattern that relies on _Observer_. The mediator object plays the role of publisher, and the components act as subscribers which subscribe to and unsubscribe from the mediator’s events. When _Mediator_ is implemented this way, it may look very similar to _Observer_.

    When you’re confused, remember that you can implement the Mediator pattern in other ways. For example, you can permanently link all the components to the same mediator object. This implementation won’t resemble _Observer_ but will still be an instance of the Mediator pattern.

    Now imagine a program where all components have become publishers, allowing dynamic connections between each other. There won’t be a centralized mediator object, only a distributed set of observers.\


\
