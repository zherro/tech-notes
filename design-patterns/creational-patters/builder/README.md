# Builder

### Intent <a href="#intent" id="intent"></a>

**Builder** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

<figure><img src="https://refactoring.guru/images/patterns/content/builder/builder-en.png" alt="Builder design pattern" width="640"><figcaption></figcaption></figure>

### &#x20;Problem <a href="#problem" id="problem"></a>

Imagine a complex object that requires laborious, step-by-step initialization of many fields and nested objects. Such initialization code is usually buried inside a monstrous constructor with lots of parameters. Or even worse: scattered all over the client code.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/builder/problem1.png" alt="Lots of subclasses create another problem" width="600"><figcaption><p>You might make the program too complex by creating a subclass for every possible configuration of an object.</p></figcaption></figure>

For example, let’s think about how to create a `House` object. To build a simple house, you need to construct four walls and a floor, install a door, fit a pair of windows, and build a roof. But what if you want a bigger, brighter house, with a backyard and other goodies (like a heating system, plumbing, and electrical wiring)?

The simplest solution is to extend the base `House` class and create a set of subclasses to cover all combinations of the parameters. But eventually you’ll end up with a considerable number of subclasses. Any new parameter, such as the porch style, will require growing this hierarchy even more.

There’s another approach that doesn’t involve breeding subclasses. You can create a giant constructor right in the base `House` class with all possible parameters that control the house object. While this approach indeed eliminates the need for subclasses, it creates another problem.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/builder/problem2.png" alt="The telescoping constructor" width="600"><figcaption><p>The constructor with lots of parameters has its downside: not all the parameters are needed at all times.</p></figcaption></figure>

In most cases most of the parameters will be unused, making [the constructor calls pretty ugly](https://refactoring.guru/smells/long-parameter-list). For instance, only a fraction of houses have swimming pools, so the parameters related to swimming pools will be useless nine times out of ten.

### &#x20;Solution <a href="#solution" id="solution"></a>

The Builder pattern suggests that you extract the object construction code out of its own class and move it to separate objects called _builders_.

<figure><img src="https://refactoring.guru/images/patterns/diagrams/builder/solution1.png" alt="Applying the Builder pattern" width="410"><figcaption><p>The Builder pattern lets you construct complex objects step by step. The Builder doesn’t allow other objects to access the product while it’s being built.</p></figcaption></figure>

The pattern organizes object construction into a set of steps (`buildWalls`, `buildDoor`, etc.). To create an object, you execute a series of these steps on a builder object. The important part is that you don’t need to call all of the steps. You can call only those steps that are necessary for producing a particular configuration of an object.

Some of the construction steps might require different implementation when you need to build various representations of the product. For example, walls of a cabin may be built of wood, but the castle walls must be built with stone.

In this case, you can create several different builder classes that implement the same set of building steps, but in a different manner. Then you can use these builders in the construction process (i.e., an ordered set of calls to the building steps) to produce different kinds of objects.

<figure><img src="https://refactoring.guru/images/patterns/content/builder/builder-comic-1-en.png" alt="" width="600"><figcaption><p>Different builders execute the same task in various ways.</p></figcaption></figure>

For example, imagine a builder that builds everything from wood and glass, a second one that builds everything with stone and iron and a third one that uses gold and diamonds. By calling the same set of steps, you get a regular house from the first builder, a small castle from the second and a palace from the third. However, this would only work if the client code that calls the building steps is able to interact with builders using a common interface.

**Director**

You can go further and extract a series of calls to the builder steps you use to construct a product into a separate class called _director_. The director class defines the order in which to execute the building steps, while the builder provides the implementation for those steps.

<figure><img src="https://refactoring.guru/images/patterns/content/builder/builder-comic-2-en.png" alt="" width="343"><figcaption><p>The director knows which building steps to execute to get a working product.</p></figcaption></figure>

Having a director class in your program isn’t strictly necessary. You can always call the building steps in a specific order directly from the client code. However, the director class might be a good place to put various construction routines so you can reuse them across your program.

In addition, the director class completely hides the details of product construction from the client code. The client only needs to associate a builder with a director, launch the construction with the director, and get the result from the builder.

### &#x20;Structure <a href="#structure" id="structure"></a>

<figure><img src="https://refactoring.guru/images/patterns/diagrams/builder/structure-indexed.png" alt="Structure of the Builder design pattern" width="470"><figcaption></figcaption></figure>

1. The **Builder** interface declares product construction steps that are common to all types of builders.
2. **Concrete Builders** provide different implementations of the construction steps. Concrete builders may produce products that don’t follow the common interface.
3. **Products** are resulting objects. Products constructed by different builders don’t have to belong to the same class hierarchy or interface.
4. The **Director** class defines the order in which to call construction steps, so you can create and reuse specific configurations of products.
5. The **Client** must associate one of the builder objects with the director. Usually, it’s done just once, via parameters of the director’s constructor. Then the director uses that builder object for all further construction. However, there’s an alternative approach for when the client passes the builder object to the production method of the director. In this case, you can use a different builder each time you produce something with the director.

### Applicability <a href="#applicability" id="applicability"></a>

&#x20;Use the Builder pattern to get rid of a “telescoping constructor”.

&#x20;Say you have a constructor with ten optional parameters. Calling such a beast is very inconvenient; therefore, you overload the constructor and create several shorter versions with fewer parameters. These constructors still refer to the main one, passing some default values into any omitted parameters.

```
class Pizza {
    Pizza(int size) { ... }
    Pizza(int size, boolean cheese) { ... }
    Pizza(int size, boolean cheese, boolean pepperoni) { ... }
    // ...
```

The Builder pattern lets you build objects step by step, using only those steps that you really need. After implementing the pattern, you don’t have to cram dozens of parameters into your constructors anymore.

&#x20;Use the Builder pattern when you want your code to be able to create different representations of some product (for example, stone and wooden houses).

&#x20;The Builder pattern can be applied when construction of various representations of the product involves similar steps that differ only in the details.

The base builder interface defines all possible construction steps, and concrete builders implement these steps to construct particular representations of the product. Meanwhile, the director class guides the order of construction.

&#x20;Use the Builder to construct [Composite](https://refactoring.guru/design-patterns/composite) trees or other complex objects.

&#x20;The Builder pattern lets you construct products step-by-step. You could defer execution of some steps without breaking the final product. You can even call steps recursively, which comes in handy when you need to build an object tree.

A builder doesn’t expose the unfinished product while running construction steps. This prevents the client code from fetching an incomplete result.

### &#x20;How to Implement <a href="#checklist" id="checklist"></a>

1. Make sure that you can clearly define the common construction steps for building all available product representations. Otherwise, you won’t be able to proceed with implementing the pattern.
2. Declare these steps in the base builder interface.
3.  Create a concrete builder class for each of the product representations and implement their construction steps.

    Don’t forget about implementing a method for fetching the result of the construction. The reason why this method can’t be declared inside the builder interface is that various builders may construct products that don’t have a common interface. Therefore, you don’t know what would be the return type for such a method. However, if you’re dealing with products from a single hierarchy, the fetching method can be safely added to the base interface.
4. Think about creating a director class. It may encapsulate various ways to construct a product using the same builder object.
5. The client code creates both the builder and the director objects. Before construction starts, the client must pass a builder object to the director. Usually, the client does this only once, via parameters of the director’s class constructor. The director uses the builder object in all further construction. There’s an alternative approach, where the builder is passed to a specific product construction method of the director.
6. The construction result can be obtained directly from the director only if all products follow the same interface. Otherwise, the client should fetch the result from the builder.



| PROS                                                                                                                                                                                                                                                                                                                                                           | CONS                                                                                                                     |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| <p></p><ul><li> You can construct objects step-by-step, defer construction steps or run steps recursively.</li><li> You can reuse the same construction code when building various representations of products.</li><li> <em>Single Responsibility Principle</em>. You can isolate complex construction code from the business logic of the product.</li></ul> | <ul><li>The overall complexity of the code increases since the pattern requires creating multiple new classes.</li></ul> |

### &#x20;Relations with Other Patterns

* Many designs start by using [Factory Method](https://refactoring.guru/design-patterns/factory-method) (less complicated and more customizable via subclasses) and evolve toward [Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory), [Prototype](https://refactoring.guru/design-patterns/prototype), or [Builder](https://refactoring.guru/design-patterns/builder) (more flexible, but more complicated).
* [Builder](https://refactoring.guru/design-patterns/builder) focuses on constructing complex objects step by step. [Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory) specializes in creating families of related objects. _Abstract Factory_ returns the product immediately, whereas _Builder_ lets you run some additional construction steps before fetching the product.
* You can use [Builder](https://refactoring.guru/design-patterns/builder) when creating complex [Composite](https://refactoring.guru/design-patterns/composite) trees because you can program its construction steps to work recursively.
* You can combine [Builder](https://refactoring.guru/design-patterns/builder) with [Bridge](https://refactoring.guru/design-patterns/bridge): the director class plays the role of the abstraction, while different builders act as implementations.
* [Abstract Factories](https://refactoring.guru/design-patterns/abstract-factory), [Builders](https://refactoring.guru/design-patterns/builder) and [Prototypes](https://refactoring.guru/design-patterns/prototype) can all be implemented as [Singletons](https://refactoring.guru/design-patterns/singleton).

### Applicability <a href="#applicability" id="applicability"></a>

The Builder Pattern is a creational design pattern used to construct complex objects step by step. It allows for more flexible and readable object creation by separating the construction process from the actual representation of the object.

Here’s a detailed Java example of the Builder Pattern with explanations:

#### Example Scenario

Suppose we need to create a `Person` class with various attributes such as name, age, address, and phone number. We’ll use the Builder Pattern to construct instances of this class.

#### Step 1: Define the `Person` Class

```java
public class Person {
    private final String name;
    private final int age;
    private final String address;
    private final String phoneNumber;

    // Private constructor to enforce object creation through Builder
    private Person(PersonBuilder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.address = builder.address;
        this.phoneNumber = builder.phoneNumber;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                ", phoneNumber='" + phoneNumber + '\'' +
                '}';
    }

    // Static nested Builder class
    public static class PersonBuilder {
        private final String name; // Required parameter
        private int age;
        private String address;
        private String phoneNumber;

        // Constructor with required parameters
        public PersonBuilder(String name) {
            this.name = name;
        }

        // Setter for optional parameters
        public PersonBuilder age(int age) {
            this.age = age;
            return this;
        }

        public PersonBuilder address(String address) {
            this.address = address;
            return this;
        }

        public PersonBuilder phoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
            return this;
        }

        // Build method to create the Person object
        public Person build() {
            return new Person(this);
        }
    }
}
```

#### Step 2: Use the Builder to Create `Person` Objects

```java
public class BuilderPatternDemo {
    public static void main(String[] args) {
        Person person = new Person.PersonBuilder("John Doe")
                .age(30)
                .address("123 Main St")
                .phoneNumber("555-1234")
                .build();

        System.out.println(person);

        Person anotherPerson = new Person.PersonBuilder("Jane Smith")
                .age(25)
                .build();

        System.out.println(anotherPerson);
    }
}
```

#### Explanation

1. **`Person` Class:**
   * **Attributes:** `name`, `age`, `address`, and `phoneNumber` are defined as `final` fields. The constructor is private to enforce the use of the `Builder` for object creation.
   * **`PersonBuilder` Class:** A static nested class that provides methods to set the optional parameters (`age`, `address`, and `phoneNumber`). The `name` attribute is required, so it is passed to the `PersonBuilder` constructor.
2. **`PersonBuilder` Class:**
   * **Constructor:** Takes the required parameter `name` and initializes it.
   * **Methods:** Setter methods (`age`, `address`, `phoneNumber`) return the `PersonBuilder` object itself, allowing for method chaining.
   * **`build` Method:** Creates and returns an instance of `Person` using the attributes set in the builder.
3. **Usage in `BuilderPatternDemo`:**
   * Demonstrates how to create `Person` objects using the `PersonBuilder`.
   * Shows how to create a fully specified `Person` and another `Person` with only the required parameter.

#### Advantages of the Builder Pattern

* **Immutability:** The `Person` object is immutable because its fields are `final` and can only be set through the builder.
* **Readability:** The builder pattern makes object construction more readable, especially when dealing with many parameters.
* **Flexibility:** Optional parameters are handled gracefully, and new parameters can be added without modifying the existing constructor.
* **Encapsulation:** The construction logic is encapsulated within the `Builder`, making the `Person` class cleaner.

#### Summary

The Builder Pattern is useful for creating complex objects with many optional parameters. It promotes immutability and encapsulates the object creation logic, leading to more maintainable and readable code.
