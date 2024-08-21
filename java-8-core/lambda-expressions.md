# Lambda Expressions

Java 8 introduced lambda expressions, a powerful feature that allows you to write more concise and readable code by treating functionality as a method argument or treating code as data. Below is a detailed explanation of lambda expressions, their concepts, and practical examples to help you study and learn.

***

#### 1. **Introduction to Lambda Expressions**

Lambda expressions are essentially anonymous functions (a function without a name) that can be passed around as if they were objects. They provide a way to write inline code that implements a functional interface, making your code more concise and expressive.

**Basic Syntax**:

```java
(parameters) -> expression
// OR
(parameters) -> { statements; }
```

**Example**:

```java
// Before Java 8: Using an anonymous class
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello from a thread!");
    }
};

// Java 8: Using a lambda expression
Runnable lambdaRunnable = () -> System.out.println("Hello from a thread!");
```

#### 2. **Functional Interfaces**

A functional interface is an interface with a single abstract method (SAM). Lambda expressions are primarily used to implement these interfaces. The `@FunctionalInterface` annotation can be used to ensure that the interface is functional, though it's optional.

**Examples**:

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void myMethod();
}

// Lambda expression implementing the functional interface
MyFunctionalInterface instance = () -> System.out.println("My method executed");
instance.myMethod();  // Output: My method executed
```

**Common Functional Interfaces in Java**:

* `Runnable` - `void run();`
* `Callable<V>` - `V call();`
* `Comparator<T>` - `int compare(T o1, T o2);`
* `Consumer<T>` - `void accept(T t);`
* `Function<T, R>` - `R apply(T t);`

#### 3. **Lambda Expression Syntax and Examples**

Lambda expressions can take various forms depending on the number of parameters and whether the body consists of a single expression or a block of statements.

**No Parameters**:

```java
// Example: Runnable
Runnable r = () -> System.out.println("Hello, World!");
```

**Single Parameter**:

```java
// Example: Consumer interface
Consumer<String> greeter = name -> System.out.println("Hello, " + name);
greeter.accept("John");  // Output: Hello, John
```

**Multiple Parameters**:

```java
// Example: Comparator interface
Comparator<Integer> comparator = (a, b) -> a.compareTo(b);
System.out.println(comparator.compare(10, 20));  // Output: -1
```

**Block Body with Multiple Statements**:

```java
BinaryOperator<Integer> add = (a, b) -> {
    int result = a + b;
    System.out.println("Result: " + result);
    return result;
};
add.apply(5, 3);  // Output: Result: 8
```

**Returning a Value**:

```java
// Example: Function interface
Function<String, Integer> stringLength = str -> str.length();
System.out.println(stringLength.apply("Lambda"));  // Output: 6
```

#### 4. **Method References**

Method references are a shorthand for lambda expressions that call an existing method. They are more readable and can be used wherever a lambda expression only calls a method.

**Types of Method References**:

1. **Static Method Reference**: `ClassName::staticMethodName`
2. **Instance Method of a Particular Object**: `instance::methodName`
3. **Instance Method of an Arbitrary Object of a Particular Type**: `ClassName::methodName`
4. **Constructor Reference**: `ClassName::new`

**Examples**:

```java
// Static Method Reference
Function<String, Integer> stringToInt = Integer::parseInt;
System.out.println(stringToInt.apply("123"));  // Output: 123

// Instance Method Reference of a Particular Object
Consumer<String> printer = System.out::println;
printer.accept("Hello, Method Reference!");  // Output: Hello, Method Reference!

// Instance Method of an Arbitrary Object of a Particular Type
Function<String, String> toUpperCase = String::toUpperCase;
System.out.println(toUpperCase.apply("lambda"));  // Output: LAMBDA

// Constructor Reference
Supplier<List<String>> listSupplier = ArrayList::new;
List<String> list = listSupplier.get();
```

#### 5. **Type Inference and Type Checking**

Java can often infer the types of parameters in a lambda expression, allowing you to omit them for brevity. However, the compiler must still be able to determine the type based on the context, such as the functional interface being implemented.

**Example**:

```java
// With explicit types
BiFunction<Integer, Integer, Integer> add = (Integer a, Integer b) -> a + b;

// With inferred types
BiFunction<Integer, Integer, Integer> addInferred = (a, b) -> a + b;
```

#### 6. **Scope and Variable Capture**

Lambda expressions can access variables from their enclosing scope, similar to anonymous inner classes. However, these variables must be **effectively final**, meaning their value does not change after it is assigned.

**Example**:

```java
int number = 10;  // Effectively final
Runnable r = () -> System.out.println(number);
// The following line would cause a compilation error because it modifies `number`
// number = 20;
```

#### 7. **Using Lambdas with the Collections Framework**

Lambda expressions make working with Java's Collections Framework much more concise and readable. Common operations include sorting, filtering, and mapping elements.

**Sorting with Lambdas**:

```java
List<String> names = Arrays.asList("John", "Alice", "Bob");
names.sort((a, b) -> a.compareTo(b));
System.out.println(names);  // Output: [Alice, Bob, John]
```

**Filtering and Mapping with Stream API**:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
List<Integer> evens = numbers.stream()
                             .filter(n -> n % 2 == 0)
                             .collect(Collectors.toList());
System.out.println(evens);  // Output: [2, 4, 6]

List<Integer> squares = numbers.stream()
                               .map(n -> n * n)
                               .collect(Collectors.toList());
System.out.println(squares);  // Output: [1, 4, 9, 16, 25, 36]
```

#### 8. **Parallel Processing with Lambdas**

The Stream API, in conjunction with lambda expressions, makes it easy to perform parallel processing on collections, leveraging multiple cores to improve performance.

**Example**:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
int sum = numbers.parallelStream()
                 .filter(n -> n % 2 == 0)
                 .mapToInt(Integer::intValue)
                 .sum();
System.out.println(sum);  // Output: 12 (sum of 2, 4, 6)
```

#### 9. **Lambda Expression Best Practices**

* **Keep It Simple**: Lambdas should be concise and readable. If the logic is complex, consider using a regular method instead.
* **Use Method References When Possible**: Method references are often more readable and concise than lambda expressions.
* **Beware of Performance Impacts**: While lambdas can make code cleaner, they can also introduce performance overhead in some cases, especially in hot code paths.
* **Avoid Statefulness**: Lambdas should ideally be stateless. If you need to maintain state, consider using a class or another approach.

#### 10. **Comparison with Anonymous Classes**

Before lambdas, anonymous inner classes were often used to achieve similar functionality. However, lambdas provide several advantages:

* **Conciseness**: Lambdas are much shorter and more readable than anonymous classes.
* **No Boilerplate**: Lambdas eliminate the need for boilerplate code, such as implementing methods or declaring classes.
* **Improved Type Inference**: The compiler can infer types in lambda expressions, reducing redundancy.

**Example**:

```java
// Anonymous class
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello");
    }
};

// Lambda expression
Runnable r2 = () -> System.out.println("Hello");
```

#### Conclusion

Java 8 lambda expressions are a powerful tool that can make your code more concise, readable, and expressive. By understanding how to use them effectively, you can write cleaner and more maintainable Java code. The key to mastering lambdas is to practice using them in real-world scenarios, especially with the Stream API and the Collections Framework.
