# Decorator

### &#x20;Intent <a href="#intent" id="intent"></a>

**Decorator** is a structural design pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.

<figure><img src="https://refactoring.guru/images/patterns/content/decorator/decorator.png" alt="Decorator design pattern" width="640"><figcaption></figcaption></figure>

### &#x20;Problem <a href="#problem" id="problem"></a>

Imagine that you’re working on a notification library which lets other programs notify their users about important events.

The initial version of the library was based on the `Notifier` class that had only a few fields, a constructor and a single `send` method. The method could accept a message argument from a client and send the message to a list of emails that were passed to the notifier via its constructor. A third-party app which acted as a client was supposed to create and configure the notifier object once, and then use it each time something important happened.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/decorator/problem1-en.png" alt="Structure of the library before applying the Decorator pattern" width="540"><figcaption><p>A program could use the notifier class to send notifications about important events to a predefined set of emails.</p></figcaption></figure>

At some point, you realize that users of the library expect more than just email notifications. Many of them would like to receive an SMS about critical issues. Others would like to be notified on Facebook and, of course, the corporate users would love to get Slack notifications.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/decorator/problem2.png" alt="Structure of the library after implementing other notification types" width="440"><figcaption><p>Each notification type is implemented as a notifier’s subclass.</p></figcaption></figure>

How hard can that be? You extended the `Notifier` class and put the additional notification methods into new subclasses. Now the client was supposed to instantiate the desired notification class and use it for all further notifications.

But then someone reasonably asked you, “Why can’t you use several notification types at once? If your house is on fire, you’d probably want to be informed through every channel.”

You tried to address that problem by creating special subclasses which combined several notification methods within one class. However, it quickly became apparent that this approach would bloat the code immensely, not only the library code but the client code as well.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/decorator/problem3.png" alt="Structure of the library after creating class combinations" width="630"><figcaption><p>Combinatorial explosion of subclasses.</p></figcaption></figure>

You have to find some other way to structure notifications classes so that their number won’t accidentally break some Guinness record.

### &#x20;Solution <a href="#solution" id="solution"></a>

Extending a class is the first thing that comes to mind when you need to alter an object’s behavior. However, inheritance has several serious caveats that you need to be aware of.

* Inheritance is static. You can’t alter the behavior of an existing object at runtime. You can only replace the whole object with another one that’s created from a different subclass.
* Subclasses can have just one parent class. In most languages, inheritance doesn’t let a class inherit behaviors of multiple classes at the same time.

One of the ways to overcome these caveats is by using _Aggregation_ or _Composition_  instead of _Inheritance_. Both of the alternatives work almost the same way: one object _has a_ reference to another and delegates it some work, whereas with inheritance, the object itself _is_ able to do that work, inheriting the behavior from its superclass.

With this new approach you can easily substitute the linked “helper” object with another, changing the behavior of the container at runtime. An object can use the behavior of various classes, having references to multiple objects and delegating them all kinds of work. Aggregation/composition is the key principle behind many design patterns, including Decorator. On that note, let’s return to the pattern discussion.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/decorator/solution1-en.png" alt="Inheritance vs. Aggregation" width="550"><figcaption><p>Inheritance vs. Aggregation</p></figcaption></figure>

“Wrapper” is the alternative nickname for the Decorator pattern that clearly expresses the main idea of the pattern. A _wrapper_ is an object that can be linked with some _target_ object. The wrapper contains the same set of methods as the target and delegates to it all requests it receives. However, the wrapper may alter the result by doing something either before or after it passes the request to the target.

When does a simple wrapper become the real decorator? As I mentioned, the wrapper implements the same interface as the wrapped object. That’s why from the client’s perspective these objects are identical. Make the wrapper’s reference field accept any object that follows that interface. This will let you cover an object in multiple wrappers, adding the combined behavior of all the wrappers to it.

In our notifications example, let’s leave the simple email notification behavior inside the base `Notifier` class, but turn all other notification methods into decorators.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/decorator/solution2.png" alt="The solution with the Decorator pattern" width="640"><figcaption><p>Various notification methods become decorators.</p></figcaption></figure>

The client code would need to wrap a basic notifier object into a set of decorators that match the client’s preferences. The resulting objects will be structured as a stack.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/decorator/solution3-en.png" alt="Apps might configure complex stacks of notification decorators" width="300"><figcaption><p>Apps might configure complex stacks of notification decorators.</p></figcaption></figure>

The last decorator in the stack would be the object that the client actually works with. Since all decorators implement the same interface as the base notifier, the rest of the client code won’t care whether it works with the “pure” notifier object or the decorated one.

We could apply the same approach to other behaviors such as formatting messages or composing the recipient list. The client can decorate the object with any custom decorators, as long as they follow the same interface as the others.

### &#x20;Real-World Analogy <a href="#analogy" id="analogy"></a>

<figure><img src="https://refactoring.guru/images/patterns/content/decorator/decorator-comic-1.png" alt="Example of the Decorator pattern" width="600"><figcaption><p>You get a combined effect from wearing multiple pieces of clothing.</p></figcaption></figure>

Wearing clothes is an example of using decorators. When you’re cold, you wrap yourself in a sweater. If you’re still cold with a sweater, you can wear a jacket on top. If it’s raining, you can put on a raincoat. All of these garments “extend” your basic behavior but aren’t part of you, and you can easily take off any piece of clothing whenever you don’t need it.

### &#x20;Applicability <a href="#applicability" id="applicability"></a>

&#x20;Use the Decorator pattern when you need to be able to assign extra behaviors to objects at runtime without breaking the code that uses these objects.

&#x20;The Decorator lets you structure your business logic into layers, create a decorator for each layer and compose objects with various combinations of this logic at runtime. The client code can treat all these objects in the same way, since they all follow a common interface.

&#x20;Use the pattern when it’s awkward or not possible to extend an object’s behavior using inheritance.

&#x20;Many programming languages have the `final` keyword that can be used to prevent further extension of a class. For a final class, the only way to reuse the existing behavior would be to wrap the class with your own wrapper, using the Decorator pattern.

### &#x20;How to Implement <a href="#checklist" id="checklist"></a>

1. Make sure your business domain can be represented as a primary component with multiple optional layers over it.
2. Figure out what methods are common to both the primary component and the optional layers. Create a component interface and declare those methods there.
3. Create a concrete component class and define the base behavior in it.
4. Create a base decorator class. It should have a field for storing a reference to a wrapped object. The field should be declared with the component interface type to allow linking to concrete components as well as decorators. The base decorator must delegate all work to the wrapped object.
5. Make sure all classes implement the component interface.
6. Create concrete decorators by extending them from the base decorator. A concrete decorator must execute its behavior before or after the call to the parent method (which always delegates to the wrapped object).
7. The client code must be responsible for creating decorators and composing them in the way the client needs.

| PROS                                                                                                                                                                                                                                                                                                                                                                                                                              | CONS                                                                                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <ul><li> You can extend an object’s behavior without making a new subclass.</li><li> You can add or remove responsibilities from an object at runtime.</li><li> You can combine several behaviors by wrapping an object into multiple decorators.</li><li> <em>Single Responsibility Principle</em>. You can divide a monolithic class that implements many possible variants of behavior into several smaller classes.</li></ul> | <p></p><ul><li> It’s hard to remove a specific wrapper from the wrappers stack.</li></ul><ul><li> It’s hard to implement a decorator in such a way that its behavior doesn’t depend on the order in the decorators stack.</li></ul><ul><li> The initial configuration code of layers might look pretty ugly.</li></ul> |

### Relations with Other Patterns <a href="#relations" id="relations"></a>

* [Adapter](https://refactoring.guru/design-patterns/adapter) provides a completely different interface for accessing an existing object. On the other hand, with the [Decorator](https://refactoring.guru/design-patterns/decorator) pattern the interface either stays the same or gets extended. In addition, _Decorator_ supports recursive composition, which isn’t possible when you use _Adapter_.
* With [Adapter](https://refactoring.guru/design-patterns/adapter) you access an existing object via different interface. With [Proxy](https://refactoring.guru/design-patterns/proxy), the interface stays the same. With [Decorator](https://refactoring.guru/design-patterns/decorator) you access the object via an enhanced interface.
*   [Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility) and [Decorator](https://refactoring.guru/design-patterns/decorator) have very similar class structures. Both patterns rely on recursive composition to pass the execution through a series of objects. However, there are several crucial differences.

    The _CoR_ handlers can execute arbitrary operations independently of each other. They can also stop passing the request further at any point. On the other hand, various _Decorators_ can extend the object’s behavior while keeping it consistent with the base interface. In addition, decorators aren’t allowed to break the flow of the request.
*   [Composite](https://refactoring.guru/design-patterns/composite) and [Decorator](https://refactoring.guru/design-patterns/decorator) have similar structure diagrams since both rely on recursive composition to organize an open-ended number of objects.

    A _Decorator_ is like a _Composite_ but only has one child component. There’s another significant difference: _Decorator_ adds additional responsibilities to the wrapped object, while _Composite_ just “sums up” its children’s results.

    However, the patterns can also cooperate: you can use _Decorator_ to extend the behavior of a specific object in the _Composite_ tree.
* Designs that make heavy use of [Composite](https://refactoring.guru/design-patterns/composite) and [Decorator](https://refactoring.guru/design-patterns/decorator) can often benefit from using [Prototype](https://refactoring.guru/design-patterns/prototype). Applying the pattern lets you clone complex structures instead of re-constructing them from scratch.
* [Decorator](https://refactoring.guru/design-patterns/decorator) lets you change the skin of an object, while [Strategy](https://refactoring.guru/design-patterns/strategy) lets you change the guts.
* [Decorator](https://refactoring.guru/design-patterns/decorator) and [Proxy](https://refactoring.guru/design-patterns/proxy) have similar structures, but very different intents. Both patterns are built on the composition principle, where one object is supposed to delegate some of the work to another. The difference is that a _Proxy_ usually manages the life cycle of its service object on its own, whereas the composition of _Decorators_ is always controlled by the client.
