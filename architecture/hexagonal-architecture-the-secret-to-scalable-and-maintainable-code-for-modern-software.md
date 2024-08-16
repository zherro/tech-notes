# Hexagonal Architecture: The Secret to Scalable and Maintainable Code for Modern Software

H**exagonal architecture**, also known as ports and adapters architecture, is an architectural pattern that aims to create loosely coupled application components that can be easily connected to their software environment by means of ports and adapters. This makes components exchangeable at any level and facilitates test automation.

Hexagonal Architecture’s concept is to position inputs and outputs on the periphery of the design. The business logic should remain unaffected regardless of whether we expose a REST or a GraphQL API, and irrespective of our data sources — be it a database, a microservice API revealed through gRPC or REST.

### Port & Adapters <a href="#id-064c" id="id-064c"></a>

#### Port <a href="#id-0208" id="id-0208"></a>

A port is an interface that facilitates communication between an application and an external system using a specific protocol. It can be either incoming (receiving requests or events from the external system) or outgoing (sending commands or queries to the external system).

A port can support multiple adapters, each implementing the communication protocol differently. For instance, an incoming port could have adapters for a GUI, CLI, or a web service, while an outgoing port could have adapters for various DBMS or repositories.

#### Adapters <a href="#id-18a4" id="id-18a4"></a>

An adapter is a component that links a port to an external system, translating data and messages between the application and the external system using the port’s protocol. It also manages technical aspects like error handling, logging, security, and caching.

Depending on the role of the external system, an adapter can be primary (connecting to an actor initiating communication) or secondary (connecting to an actor responding to communication). Primary actors include users or other applications, while secondary actors include databases or services.

#### Benefits of Hexagonal Architecture <a href="#id-6607" id="id-6607"></a>

* Loose coupling by decopling core from external systems and their technical details
* High cohesion by handling technical concerns with adapters and keeping focus on the business and application logic within core
* Improved Testability
* Easier Maintenance
* Better Domain Modeling
* Scalability
* Exchangeability by changing adapters without changing core
* Reusability by using different adapters
* Improved Readability

### Traditional Layered Architecture <a href="#a512" id="a512"></a>

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Hexagonal Architecture vs Layered Architecture</p></figcaption></figure>

Traditional layered architecture is a software architecture pattern that organizes the components of an application into a series of layers, each with a specific responsibility.

The most common layers in a traditional layered architecture are:

* **Presentation layer**: responsible for handling user input and output, and for displaying the user interface
* **Application layer**: contains the business logic of the application, including the rules and processes that govern the behavior of the system
* **Domain layer**: represents the data and the business concepts of the application, and is responsible for managing the data and the business rules
* **Data access layer**: responsible for accessing and manipulating data, typically through a database
* **Infrastructure layer**: includes the underlying systems and technologies that support the application, such as the operating system, database management system, and network protocols

### Drawbacks of Layered Architecture <a href="#id-75a3" id="id-75a3"></a>

* **Tight coupling**: The layers depend on each other in a specific order, which makes them hard to change or replace. For example, if you want to use a different database provider, you have to change the data access layer and possibly the business layer as well
* **Contamination**: The layers can leak technical details or implementation choices to other layers, which violates the separation of concerns principle. For example, if you use an ORM framework in the data access layer, you may have to use its specific classes or annotations in the business layer or even in the presentation layer
* **Asymmetry**: The layers do not reflect the actual communication patterns between external actors and the system. For example, a user request may go through several layers before reaching the business logic, while a notification may go directly from the business logic to an external actor

### Hexagonal Architecture vs Traditional Layered Architecture <a href="#id-9997" id="id-9997"></a>

#### Tips for Making a Decision <a href="#eeee" id="eeee"></a>

Use Traditional Layered Architecture when the application:

* has a simple structure
* has a small number of components
* doesn’t require extensive integration with external systems
* doesn’t require high scalability

Use Hexagonal Architecture when you need:

* integrate with multiple external systems
* handle a large volume of data
* scale the application horizontally
* test the application extensively

### Comparison Table <a href="#id-883c" id="id-883c"></a>

| Criteria         | Traditional Layered Architecture                                                     | Hexagonal Architecture                                                                                                            |
| ---------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| Complexity       | Simpler, with fewer moving parts and easier to understand and implement              | More complex, with more moving parts and more nuanced design                                                                      |
| Coupling         | Tight coupling between layers                                                        | Loose coupling between layers, with the application layer being the main entry point for external inputs                          |
| Flexibility      | Inflexible, as changes to one layer can affect other layers                          | More flexible, as changes to one layer do not affect other layers                                                                 |
| Focus            | Separation of concerns within the application                                        | Separation of concerns between the application and the external world                                                             |
| Layers           | Fixed set of layers                                                                  | Flexible set of layers that can be added or removed as needed                                                                     |
| Maintenance      | More difficult to modify and extend, as changes to one layer can affect other layers | Easier to modify and extend, as the separation of concerns and use of adapters and ports make it easier to modify the application |
| Responsibilities | Each layer has a specific responsibility                                             | Domain layer responsible for business logic, application layer responsible for handling user input and output                     |
| Scalability      | Difficult to add new adapters or ports without affecting the core domain logic       | Easier to add new adapters and ports, as they can be added without affecting the existing architecture                            |
| Testability      | Difficult to test individual layers in isolation                                     | Easier to test individual layers in isolation, as adapters and ports can be mocked or stubbed                                     |

### Principles <a href="#eb52" id="eb52"></a>

#### Separation of User-/Business-/Server- Side Logic <a href="#id-28a5" id="id-28a5"></a>

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>Principle: Separate User-Side, Business Logic and Server-Side</p></figcaption></figure>

* **Compartmentalization**: This separation allows for focusing on one logic at a time, making each easier to understand without mixing them up. The constraints of each logic have less impact on the others
* **Prioritization of Business Logic**: The business logic is emphasized in the code. It can be isolated into a specific directory or module, making it clear for all developers. It can be developed, refined, and tested without the cognitive load of the rest of the program. This is important as it’s the developers’ understanding of the business that gets deployed
* **Effective Testing**: With this separation, automated tests can efficiently test: The entire Business Logic on its own. The integration between User-Side and Business Logic, independent of the Server-Side. The integration between Business Logic and Server-Side, independent of the User-Side.

### Dependencies Go Inside <a href="#c085" id="c085"></a>

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Principle: Dependencies Go Inside</p></figcaption></figure>

* **Business Logic as Core Dependency**: The software can be manipulated via console and tests, but the Business Logic doesn’t recognize the concept of a console. The User-Side depends on the Business Logic, not vice versa. The User-Side relies on a universal user-initiated “post request” mechanism
* **Inside vs Outside**: The central Business Logic is “inside” and everything else is “outside”. Dependencies point inward. Everything is dependent on the Business Logic, while the Business Logic is independent. This distinction between “inside” and “outside” is emphasized as more crucial than differentiating between User-Side and Server-Side

### Boundaries are Isolated with Interfaces <a href="#a1c0" id="a1c0"></a>

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Principle: Boundaries are Isolated with Interfaces</p></figcaption></figure>

The client-side code interacts with the business logic via an interface, which is established within the business logic itself. Similarly, the business logic communicates with the server-side through another interface, also defined within the business logic. These interfaces serve as clear boundaries separating the internal and external components.

### **Ports & Adapters**

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>Ports &#x26; Adapters</p></figcaption></figure>

Hexagonal Architecture, also known as the Ports & Adapters Pattern, is a design pattern that uses ports and adapters to facilitate interactions between internal and external elements. The business logic creates ports, which can be connected to various adapters interchangeably.

The interfaces created by the business code are referred to as ports, which are considered internal as they are defined by the business. Adapters represent external code that bridges the gap between the port and the rest of the user-side or server-side code.

The architecture is symbolized by a hexagon, providing ample space to depict multiple ports and adapters.

### Dependency Rule & Dependency Inversion <a href="#id-47c9" id="id-47c9"></a>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption><p>Dependency Rule &#x26; Dependency Inversion</p></figcaption></figure>

The main challenge in software architecture is to prevent technical details and libraries from infiltrating the application. The “dependency rule” provides a solution by ensuring that all source code dependencies point inward, towards the core of the application.

The difficulty arises when implementing secondary ports and adapters, where the source code dependency must be opposite to the direction of invocation. For instance, how can the application core access the database if the database is outside the core and the source code dependency is to be directed towards the center?

This is where we apply the “dependency inversion principle.” The port is still defined by an interface, but the relationships between classes are reversed. This allows control the direction of a code dependency — for secondary ports and adapters, it’s opposite to the calling direction.

### Hexagonal Architecture Workflow <a href="#id-23c2" id="id-23c2"></a>

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

The main idea behind hexagonal architecture is to put the inputs and outputs at the edges of the design, and to isolate the core layer from outside concerns. This way, the core layer can be swapped out without changing the adapter layer, and vice versa.

For example, suppose you have a core layer that implements a simple calculator service. It exposes two ports: one for receiving arithmetic expressions as input, and one for sending the results as output. The core layer does not care about how these ports are implemented, as long as they follow the contract defined by the port layer.

Now, you can have different adapters for these ports, depending on your needs. For instance, you can have a web adapter that implements the input port as a REST API endpoint, and the output port as a JSON response. Or you can have a console adapter that implements the input port as a command-line argument, and the output port as a standard output stream. Or you can have a test adapter that implements the input port as a mock object, and the output port as an assertion.

The beauty of this approach is that you can change or add adapters without affecting the core layer or other adapters. You can also test the core layer in isolation, by using test adapters that simulate the external world.

\
High-Level Overview of How Hexagonal Architecture Works

* **Application core:** contains the business logic and algorithms that solve the problem
* **Ports**: define the interfaces that the application uses to communicate with the external systems
* **Adapters:** implement the ports and provide a bridge between the application and the external systems
* **Edge:** handles the communication between the application core and the external systems
* **External systems:** interact with the application through the ports and adapters

### Steps for Applying Hexagonal Architecture <a href="#id-33f1" id="id-33f1"></a>

* **Identify your core domain logic** and encapsulate it in one or more classes or modules. This is your application core
* **Identify your external actors** and their interactions with your system. These are your primary and secondary ports
* **Define abstract interfaces for each port** that specify what methods or operations are available for communication
* **Implement concrete adapters for each port** that handle the communication with the external actors. For example, you can use a web framework to implement a web adapter, or a JDBC driver to implement a database adapter
* **Connect your adapters to your core through dependency injection or inversion of control**. This way, you can easily change or replace your adapters without affecting your core

### Examples <a href="#id-4a96" id="id-4a96"></a>

#### Web Application <a href="#c5e3" id="c5e3"></a>

Web application that allows users to create and manage blog posts.

The **core logic** of this application is responsible for validating and storing blog posts, as well as retrieving them by various criteria.

The **external components** are a web browser that provides a user interface for creating and viewing blog posts, and a relational database that persists blog posts in tables.

The **ports** are an HTTP API that defines how blog posts are created and retrieved using HTTP requests and responses in JSON format, and a repository interface that defines how blog posts are stored and queried using CRUD operations on entities.

The **adapters** are a web controller that implements the HTTP API using Spring MVC framework, and a JDBC repository that implements the repository interface using SQL queries on a MySQL database.

#### Command-Line Application (CLI) <a href="#id-5d10" id="id-5d10"></a>

Command-line application that allows users to generate and analyze reports from data sources.

The **core logic** of this application is responsible for generating and analyzing reports, as well as formatting them in various formats.

The **external components** are a command-line interface that provides a user interface for specifying report parameters and options, and various data sources that provide data for the reports, such as CSV files, web services or databases.

The **ports** are a CLI API that defines how report parameters and options are parsed and validated using command-line arguments and options, and a data source interface that defines how data is retrieved and queried using data source identifiers and queries.

The **adapters** are a CLI parser that implements the CLI API using Apache Commons CLI library, and various data source adapters that implement the data source interface using different technologies, such as Apache Commons CSV, Apache HttpClient or JDBC.

### Conclusion <a href="#id-650b" id="id-650b"></a>

Hexagonal architecture is a useful pattern for designing software applications that are portable, testable, and maintainable. It helps to separate the core logic of the application from the external dependencies, and to use different implementations of them without affecting the core.

Hexagonal architecture also follows some of the best practices of software design, such as dependency inversion principle, interface segregation principle, and single responsibility principle.
