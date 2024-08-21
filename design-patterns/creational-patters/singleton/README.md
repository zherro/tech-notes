# Singleton

### &#x20;Intent <a href="#intent" id="intent"></a>

**Singleton** is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

&#x20;Problem

The Singleton pattern solves two problems at the same time, violating the _Single Responsibility Principle_:

1.  **Ensure that a class has just a single instance**. Why would anyone want to control how many instances a class has? The most common reason for this is to control access to some shared resource—for example, a database or a file.

    Here’s how it works: imagine that you created an object, but after a while decided to create a new one. Instead of receiving a fresh object, you’ll get the one you already created.

    Note that this behavior is impossible to implement with a regular constructor since a constructor call **must** always return a new object by design.

<figure><img src="https://refactoring.guru/images/patterns/content/singleton/singleton-comic-1-en.png" alt="The global access to an object" width="600"><figcaption><p>Clients may not even realize that they’re working with the same object all the time.</p></figcaption></figure>

2.  **Provide a global access point to that instance**. Remember those global variables that you (all right, me) used to store some essential objects? While they’re very handy, they’re also very unsafe since any code can potentially overwrite the contents of those variables and crash the app.

    Just like a global variable, the Singleton pattern lets you access some object from anywhere in the program. However, it also protects that instance from being overwritten by other code.

    There’s another side to this problem: you don’t want the code that solves problem #1 to be scattered all over your program. It’s much better to have it within one class, especially if the rest of your code already depends on it.

Nowadays, the Singleton pattern has become so popular that people may call something a _singleton_ even if it solves just one of the listed problems.

### &#x20;Solution <a href="#solution" id="solution"></a>

All implementations of the Singleton have these two steps in common:

* Make the default constructor private, to prevent other objects from using the `new` operator with the Singleton class.
* Create a static creation method that acts as a constructor. Under the hood, this method calls the private constructor to create an object and saves it in a static field. All following calls to this method return the cached object.

If your code has access to the Singleton class, then it’s able to call the Singleton’s static method. So whenever that method is called, the same object is always returned.

### &#x20;Real-World Analogy <a href="#analogy" id="analogy"></a>

The government is an excellent example of the Singleton pattern. A country can have only one official government. Regardless of the personal identities of the individuals who form governments, the title, “The Government of X”, is a global point of access that identifies the group of people in charge.

### &#x20;Structure <a href="#structure" id="structure"></a>

<figure><img src="https://refactoring.guru/images/patterns/diagrams/singleton/structure-en-indexed.png" alt="The structure of the Singleton pattern" width="430"><figcaption></figcaption></figure>

1.  The **Singleton** class declares the static method `getInstance` that returns the same instance of its own class.

    The Singleton’s constructor should be hidden from the client code. Calling the `getInstance` method should be the only way of getting the Singleton object.

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

* A [Facade](https://refactoring.guru/design-patterns/facade) class can often be transformed into a [Singleton](https://refactoring.guru/design-patterns/singleton) since a single facade object is sufficient in most cases.
* [Flyweight](https://refactoring.guru/design-patterns/flyweight) would resemble [Singleton](https://refactoring.guru/design-patterns/singleton) if you somehow managed to reduce all shared states of the objects to just one flyweight object. But there are two fundamental differences between these patterns:
  1. There should be only one Singleton instance, whereas a _Flyweight_ class can have multiple instances with different intrinsic states.
  2. The _Singleton_ object can be mutable. Flyweight objects are immutable.
* [Abstract Factories](https://refactoring.guru/design-patterns/abstract-factory), [Builders](https://refactoring.guru/design-patterns/builder) and [Prototypes](https://refactoring.guru/design-patterns/prototype) can all be implemented as [Singletons](https://refactoring.guru/design-patterns/singleton).

### &#x20;Relations with Other Patterns <a href="#relations" id="relations"></a>

* &#x20;You can be sure that a class has only a single instance.
* &#x20;You gain a global access point to that instance.
* &#x20;The singleton object is initialized only when it’s requested for the first time.
* &#x20;Violates the _Single Responsibility Principle_. The pattern solves two problems at the time.
* &#x20;The Singleton pattern can mask bad design, for instance, when the components of the program know too much about each other.
* &#x20;The pattern requires special treatment in a multithreaded environment so that multiple threads won’t create a singleton object several times.
* &#x20;It may be difficult to unit test the client code of the Singleton because many test frameworks rely on inheritance when producing mock objects. Since the constructor of the singleton class is private and overriding static methods is impossible in most languages, you will need to think of a creative way to mock the singleton. Or just don’t write the tests. Or don’t use the Singleton pattern.

### &#x20;Pros and Cons <a href="#pros-cons" id="pros-cons"></a>

1.
   * &#x20;You can be sure that a class has only a single instance.
   * &#x20;You gain a global access point to that instance.
   * &#x20;The singleton object is initialized only when it’s requested for the first time.
   * &#x20;Violates the _Single Responsibility Principle_. The pattern solves two problems at the time.
   * &#x20;The Singleton pattern can mask bad design, for instance, when the components of the program know too much about each other.
   * &#x20;The pattern requires special treatment in a multithreaded environment so that multiple threads won’t create a singleton object several times.
   * &#x20;It may be difficult to unit test the client code of the Singleton because many test frameworks rely on inheritance when producing mock objects. Since the constructor of the singleton class is private and overriding static methods is impossible in most languages, you will need to think of a creative way to mock the singleton. Or just don’t write the tests. Or don’t use the Singleton pattern.

### &#x20;How to Implement <a href="#checklist" id="checklist"></a>

Note that you can always adjust this limitation and allow creating any number of Singleton instances. The only piece of code that needs changing is the body of the `getInstance` method.

&#x20;Unlike global variables, the Singleton pattern guarantees that there’s just one instance of a class. Nothing, except for the Singleton class itself, can replace the cached instance.

&#x20;Use the Singleton pattern when you need stricter control over global variables.

&#x20;The Singleton pattern disables all other means of creating objects of a class except for the special creation method. This method either creates a new object or returns an existing one if it has already been created.

&#x20;Use the Singleton pattern when a class in your program should have just a single instance available to all clients; for example, a single database object shared by different parts of the program.

### Applicability <a href="#applicability" id="applicability"></a>

Below is a complete example of a Singleton design pattern in Java, including various considerations such as thread safety, lazy initialization, and handling serialization. This example covers different approaches to implementing a Singleton.

#### 1. Basic Singleton Implementation

This is the simplest way to create a Singleton, but it has limitations.

```java
public class BasicSingleton {
    private static final BasicSingleton instance = new BasicSingleton();

    private BasicSingleton() {
        // Private constructor prevents instantiation from other classes
    }

    public static BasicSingleton getInstance() {
        return instance;
    }
}
```

**Considerations:**

* **Eager Initialization:** The instance is created at the time of class loading. This can waste resources if the instance is never used.

#### 2. Lazy Initialization Singleton

This version delays the creation of the instance until it's needed.

```java
public class LazySingleton {
    private static LazySingleton instance;

    private LazySingleton() {
        // Private constructor prevents instantiation from other classes
    }

    public static LazySingleton getInstance() {
        if (instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

**Considerations:**

* **Not Thread-Safe:** If multiple threads access `getInstance()` simultaneously, multiple instances could be created.

#### 3. Thread-Safe Singleton

To ensure thread safety, synchronization is added.

```java
public class ThreadSafeSingleton {
    private static ThreadSafeSingleton instance;

    private ThreadSafeSingleton() {
        // Private constructor prevents instantiation from other classes
    }

    public static synchronized ThreadSafeSingleton getInstance() {
        if (instance == null) {
            instance = new ThreadSafeSingleton();
        }
        return instance;
    }
}
```

**Considerations:**

* **Synchronized Method:** Ensures thread safety but can reduce performance due to locking overhead.

#### 4. Double-Checked Locking Singleton

This approach improves performance by reducing the overhead of synchronization.

```java
public class DoubleCheckedLockingSingleton {
    private static volatile DoubleCheckedLockingSingleton instance;

    private DoubleCheckedLockingSingleton() {
        // Private constructor prevents instantiation from other classes
    }

    public static DoubleCheckedLockingSingleton getInstance() {
        if (instance == null) {
            synchronized (DoubleCheckedLockingSingleton.class) {
                if (instance == null) {
                    instance = new DoubleCheckedLockingSingleton();
                }
            }
        }
        return instance;
    }
}
```

**Considerations:**

* **Volatile Keyword:** Ensures visibility of changes to `instance` across threads.
* **Double-Checked Locking:** Reduces the use of synchronization to only when the instance is first created.

#### 5. Bill Pugh Singleton

This approach leverages the Java memory model's guarantee that class initialization is thread-safe.

```java
public class BillPughSingleton {
    private BillPughSingleton() {
        // Private constructor prevents instantiation from other classes
    }

    private static class SingletonHelper {
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }

    public static BillPughSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

**Considerations:**

* **Inner Static Helper Class:** The Singleton instance is created only when the `getInstance()` method is called, and this approach is thread-safe without synchronization overhead.

#### 6. Enum Singleton

Using an `enum` is the simplest and most effective way to implement a Singleton in Java, ensuring thread safety and serialization.

```java
public enum EnumSingleton {
    INSTANCE;

    public void someMethod() {
        // Implementation here
    }
}
```

**Considerations:**

* **Serialization:** Enums are inherently serializable, so the Singleton property is maintained even in the presence of serialization.
* **Thread-Safety:** Java guarantees that any enum value is instantiated only once in a Java program.

#### 7. Singleton with Serialization Handling

For classes that need to be serialized, ensure that deserialization doesn't create a new instance.

```java
import java.io.Serializable;

public class SerializedSingleton implements Serializable {
    private static final long serialVersionUID = 1L;

    private SerializedSingleton() {
        // Private constructor
    }

    private static class SingletonHelper {
        private static final SerializedSingleton INSTANCE = new SerializedSingleton();
    }

    public static SerializedSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }

    // Ensure that deserialization returns the same instance
    protected Object readResolve() {
        return getInstance();
    }
}
```

**Considerations:**

* **Serialization:** The `readResolve` method prevents a new instance from being created during deserialization.

#### Conclusion

The Singleton pattern is a powerful tool for ensuring that a class has only one instance, but its implementation in Java requires careful consideration of thread safety, lazy initialization, performance, and serialization.

* **Basic Singleton** is good for simple use cases where resource consumption is not a concern.
* **Lazy and Thread-Safe Singletons** are useful when the instance is expensive to create.
* **Double-Checked Locking** balances performance and safety.
* **Bill Pugh Singleton** is a clean and efficient approach.
* **Enum Singleton** is the most straightforward and robust implementation, covering serialization and thread safety concerns effortlessly.

Each implementation has its trade-offs, and the best choice depends on the specific requirements of your application.

\
\
