# Factory Method

### Intent <a href="#intent" id="intent"></a>

**Factory Method** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

<figure><img src="https://refactoring.guru/images/patterns/content/factory-method/factory-method-en.png" alt="Factory Method pattern" width="640"><figcaption></figcaption></figure>

### &#x20;Problem <a href="#problem" id="problem"></a>

Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the `Truck` class.

After a while, your app becomes pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/factory-method/problem1-en.png" alt="Adding a new transportation class to the program causes an issue" width="600"><figcaption><p>Adding a new class to the program isn’t that simple if the rest of the code is already coupled to existing classes.</p></figcaption></figure>

Great news, right? But how about the code? At present, most of your code is coupled to the `Truck` class. Adding `Ships` into the app would require making changes to the entire codebase. Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.

As a result, you will end up with pretty nasty code, riddled with conditionals that switch the app’s behavior depending on the class of transportation objects.

### &#x20;Solution <a href="#solution" id="solution"></a>

The Factory Method pattern suggests that you replace direct object construction calls (using the `new` operator) with calls to a special _factory_ method. Don’t worry: the objects are still created via the `new` operator, but it’s being called from within the factory method. Objects returned by a factory method are often referred to as _products._

<figure><img src="https://refactoring.guru/images/patterns/diagrams/factory-method/solution1.png" alt="The structure of creator classes" width="620"><figcaption><p>Subclasses can alter the class of objects being returned by the factory method.</p></figcaption></figure>

At first glance, this change may look pointless: we just moved the constructor call from one part of the program to another. However, consider this: now you can override the factory method in a subclass and change the class of products being created by the method.

There’s a slight limitation though: subclasses may return different types of products only if these products have a common base class or interface. Also, the factory method in the base class should have its return type declared as this interface.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/factory-method/solution2-en.png" alt="The structure of the products hierarchy" width="490"><figcaption><p>All products must follow the same interface.</p></figcaption></figure>

For example, both `Truck` and `Ship` classes should implement the `Transport` interface, which declares a method called `deliver`. Each class implements this method differently: trucks deliver cargo by land, ships deliver cargo by sea. The factory method in the `RoadLogistics` class returns truck objects, whereas the factory method in the `SeaLogistics` class returns ships.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/factory-method/solution3-en.png" alt="The structure of the code after applying the factory method pattern" width="640"><figcaption><p>As long as all product classes implement a common interface, you can pass their objects to the client code without breaking it.</p></figcaption></figure>

The code that uses the factory method (often called the _client_ code) doesn’t see a difference between the actual products returned by various subclasses. The client treats all the products as abstract `Transport`. The client knows that all transport objects are supposed to have the `deliver` method, but exactly how it works isn’t important to the client.

### &#x20;Structure <a href="#structure" id="structure"></a>

<figure><img src="https://refactoring.guru/images/patterns/diagrams/factory-method/structure-indexed.png" alt="The structure of the Factory Method pattern" width="660"><figcaption></figcaption></figure>

1. The **Product** declares the interface, which is common to all objects that can be produced by the creator and its subclasses.
2. **Concrete Products** are different implementations of the product interface.
3.  The **Creator** class declares the factory method that returns new product objects. It’s important that the return type of this method matches the product interface.

    You can declare the factory method as `abstract` to force all subclasses to implement their own versions of the method. As an alternative, the base factory method can return some default product type.

    Note, despite its name, product creation is **not** the primary responsibility of the creator. Usually, the creator class already has some core business logic related to products. The factory method helps to decouple this logic from the concrete product classes. Here is an analogy: a large software development company can have a training department for programmers. However, the primary function of the company as a whole is still writing code, not producing programmers.
4.  **Concrete Creators** override the base factory method so it returns a different type of product.

    Note that the factory method doesn’t have to **create** new instances all the time. It can also return existing objects from a cache, an object pool, or another source.

### Applicability <a href="#applicability" id="applicability"></a>

&#x20;Use the Factory Method when you don’t know beforehand the exact types and dependencies of the objects your code should work with.

&#x20;The Factory Method separates product construction code from the code that actually uses the product. Therefore it’s easier to extend the product construction code independently from the rest of the code.

For example, to add a new product type to the app, you’ll only need to create a new creator subclass and override the factory method in it.

&#x20;Use the Factory Method when you want to provide users of your library or framework with a way to extend its internal components.

&#x20;Inheritance is probably the easiest way to extend the default behavior of a library or framework. But how would the framework recognize that your subclass should be used instead of a standard component?

The solution is to reduce the code that constructs components across the framework into a single factory method and let anyone override this method in addition to extending the component itself.

Let’s see how that would work. Imagine that you write an app using an open source UI framework. Your app should have round buttons, but the framework only provides square ones. You extend the standard `Button` class with a glorious `RoundButton` subclass. But now you need to tell the main `UIFramework` class to use the new button subclass instead of a default one.

To achieve this, you create a subclass `UIWithRoundButtons` from a base framework class and override its `createButton` method. While this method returns `Button` objects in the base class, you make your subclass return `RoundButton` objects. Now use the `UIWithRoundButtons` class instead of `UIFramework`. And that’s about it!

&#x20;Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.

&#x20;You often experience this need when dealing with large, resource-intensive objects such as database connections, file systems, and network resources.

Let’s think about what has to be done to reuse an existing object:

1. First, you need to create some storage to keep track of all of the created objects.
2. When someone requests an object, the program should look for a free object inside that pool.
3. … and then return it to the client code.
4. If there are no free objects, the program should create a new one (and add it to the pool).

That’s a lot of code! And it must all be put into a single place so that you don’t pollute the program with duplicate code.

Probably the most obvious and convenient place where this code could be placed is the constructor of the class whose objects we’re trying to reuse. However, a constructor must always return **new objects** by definition. It can’t return existing instances.

Therefore, you need to have a regular method capable of creating new objects as well as reusing existing ones. That sounds very much like a factory method.

### &#x20;How to Implement <a href="#checklist" id="checklist"></a>

1. Make all products follow the same interface. This interface should declare methods that make sense in every product.
2. Add an empty factory method inside the creator class. The return type of the method should match the common product interface.
3.  In the creator’s code find all references to product constructors. One by one, replace them with calls to the factory method, while extracting the product creation code into the factory method.

    You might need to add a temporary parameter to the factory method to control the type of returned product.

    At this point, the code of the factory method may look pretty ugly. It may have a large `switch` statement that picks which product class to instantiate. But don’t worry, we’ll fix it soon enough.
4. Now, create a set of creator subclasses for each type of product listed in the factory method. Override the factory method in the subclasses and extract the appropriate bits of construction code from the base method.
5.  If there are too many product types and it doesn’t make sense to create subclasses for all of them, you can reuse the control parameter from the base class in subclasses.

    For instance, imagine that you have the following hierarchy of classes: the base `Mail` class with a couple of subclasses: `AirMail` and `GroundMail`; the `Transport` classes are `Plane`, `Truck` and `Train`. While the `AirMail` class only uses `Plane` objects, `GroundMail` may work with both `Truck` and `Train` objects. You can create a new subclass (say `TrainMail`) to handle both cases, but there’s another option. The client code can pass an argument to the factory method of the `GroundMail` class to control which product it wants to receive.
6. If, after all of the extractions, the base factory method has become empty, you can make it abstract. If there’s something left, you can make it a default behavior of the method.



| PROS                                                                                                                                                                                                                                                                                                                                                                                                  | CONS                                                                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p></p><ul><li>You avoid tight coupling between the creator and the concrete products.</li><li> <em>Single Responsibility Principle</em>. You can move the product creation code into one place in the program, making the code easier to support.</li><li> <em>Open/Closed Principle</em>. You can introduce new types of products into the program without breaking existing client code.</li></ul> | <ul><li>The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you’re introducing the pattern into an existing hierarchy of creator classes.</li></ul> |

### Relations with Other Patterns <a href="#relations" id="relations"></a>

* Many designs start by using [Factory Method](https://refactoring.guru/design-patterns/factory-method) (less complicated and more customizable via subclasses) and evolve toward [Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory), [Prototype](https://refactoring.guru/design-patterns/prototype), or [Builder](https://refactoring.guru/design-patterns/builder) (more flexible, but more complicated).
* [Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory) classes are often based on a set of [Factory Methods](https://refactoring.guru/design-patterns/factory-method), but you can also use [Prototype](https://refactoring.guru/design-patterns/prototype) to compose the methods on these classes.
* You can use [Factory Method](https://refactoring.guru/design-patterns/factory-method) along with [Iterator](https://refactoring.guru/design-patterns/iterator) to let collection subclasses return different types of iterators that are compatible with the collections.
* [Prototype](https://refactoring.guru/design-patterns/prototype) isn’t based on inheritance, so it doesn’t have its drawbacks. On the other hand, _Prototype_ requires a complicated initialization of the cloned object. [Factory Method](https://refactoring.guru/design-patterns/factory-method) is based on inheritance but doesn’t require an initialization step.
* [Factory Method](https://refactoring.guru/design-patterns/factory-method) is a specialization of [Template Method](https://refactoring.guru/design-patterns/template-method). At the same time, a _Factory Method_ may serve as a step in a large _Template Method_.

### Applicability

Here’s a simple Java example of the Factory Pattern, with an explanation:

#### Example Scenario

Suppose we have a notification system where different types of notifications can be sent (e.g., Email and SMS). We’ll use the Factory Pattern to create these notifications.

#### Step 1: Define the `Notification` Interface

```java
public interface Notification {
    void send(String message);
}
```

#### Step 2: Implement Concrete Notification Classes

```java
public class EmailNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending Email: " + message);
    }
}

public class SMSNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}
```

#### Step 3: Create the `NotificationFactory` Class

```java
public class NotificationFactory {
    public static Notification getNotification(String type) {
        if (type == null) {
            return null;
        }
        if (type.equalsIgnoreCase("EMAIL")) {
            return new EmailNotification();
        } else if (type.equalsIgnoreCase("SMS")) {
            return new SMSNotification();
        }
        return null;
    }
}
```

#### Step 4: Use the Factory in Client Code

```java
public class FactoryPatternDemo {
    public static void main(String[] args) {
        Notification emailNotification = NotificationFactory.getNotification("EMAIL");
        emailNotification.send("Hello via Email!");

        Notification smsNotification = NotificationFactory.getNotification("SMS");
        smsNotification.send("Hello via SMS!");
    }
}
```

#### Explanation

1. **`Notification` Interface:**
   * Defines a common interface for different types of notifications. It has a method `send` that concrete implementations will define.
2. **Concrete Notification Classes (`EmailNotification`, `SMSNotification`):**
   * Implement the `Notification` interface. Each class provides its own implementation of the `send` method, specific to the type of notification.
3. **`NotificationFactory` Class:**
   * Contains a static method `getNotification` that returns an instance of `Notification`. The method takes a `type` parameter to determine which concrete class to instantiate.
   * This class encapsulates the object creation logic, allowing the client code to remain unaware of the specific classes being instantiated.
4. **Client Code (`FactoryPatternDemo`):**
   * Uses the factory to get instances of `Notification`. It calls the `send` method on the objects created by the factory.

#### Advantages of the Factory Pattern

* **Encapsulation:** Object creation logic is centralized in the factory, which hides the instantiation details from the client code.
* **Flexibility:** New notification types can be added without changing the client code. The factory method can be updated to handle new types.
* **Decoupling:** Reduces coupling between the client code and the concrete classes. The client code interacts with the interface, not the specific implementations.

#### Summary

The Factory Pattern is useful for managing and encapsulating object creation, especially when the creation logic is complex or when different types of objects might be needed. It provides a flexible and scalable way to manage object creation in an application.
