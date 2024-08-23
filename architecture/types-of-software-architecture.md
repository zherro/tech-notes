# Types of Software Architecture

An architecture refers to the segmentation of a system into parts, the organization and attributes of these parts, and how these parts interact with each other.

An effective software architecture ensures that the software can be modified throughout its lifespan with minimal, consistent effort, resulting in predictable costs for the client.

Changes might include:

* Fulfillment of client requests
* Modifications to comply with changes in legal regulations
* Adoption of newer technologies
* Upgrading infrastructure elements
* Switching third-party systems
* Even substituting the application server

### **Hexagonal Architecture (Ports and Adapters)**

* **Description:** Also known as the "Ports and Adapters" architecture, it emphasizes creating a flexible, loosely-coupled system by isolating the core logic from external concerns like databases, user interfaces, or other services. The core application is at the center, with different "ports" (interfaces) and "adapters" (implementations) connecting the core to the outside world.
* **Key Principles:**
  * The core logic is independent of external systems.
  * Ports define the interfaces for communication with external components.
  * Adapters implement these interfaces to connect the core to specific technologies.
* **Use Case:** Systems that need to be highly testable and maintainable, with a clear separation between business logic and external dependencies.
* **Example:** Applications where business logic is expected to change frequently, or where you need to swap out external components like databases or APIs without affecting the core logic.

### **Clean Architecture**

* **Description:** A software design philosophy introduced by Robert C. Martin (Uncle Bob), Clean Architecture emphasizes the separation of concerns and independence of the business logic from external components. It organizes the code into layers, with the innermost layer representing the core business logic and the outer layers representing interfaces to the outside world.
* **Key Principles:**
  * **Entities:** Represent the core business logic and rules, independent of any external framework.
  * **Use Cases (or Interactors):** Contain application-specific business rules, orchestrating the flow of data between entities and external systems.
  * **Interface Adapters:** Translate data between the core logic and external interfaces like databases, user interfaces, or APIs.
  * **Frameworks and Drivers:** The outermost layer that includes the tools, frameworks, and external components (e.g., web frameworks, databases).
* **Use Case:** Ideal for complex systems requiring high maintainability, where business rules must remain unaffected by changes in technology.
* **Example:** Enterprise applications with complex business rules, where the technology stack may change over time but the core business logic needs to remain consistent.

Both Hexagonal and Clean Architecture share a focus on decoupling the core business logic from external dependencies. They aim to make the system more testable, maintainable, and flexible, allowing changes to be made in one part of the system without affecting others.



### Hexagonal Architecture vs Clean Architecture <a href="#id-9853" id="id-9853"></a>

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Hexagonal Architecture vs Clean Architecture</p></figcaption></figure>

The “Clean Architecture” also places business logic at the center, similar to the Hexagonal Architecture. Interface adapters surround this core, connecting it with the user interface, database, and other external components. The core is only aware of the interfaces of these adapters and not their concrete implementations.

In Clean Architecture, all source code dependencies point towards the core. The Dependency Inversion Principle is applied when calls point from inside to outside, opposite to the source code dependency direction.

The Hexagonal Architecture can be almost directly mapped to the Clean Architecture:

* **Eexternal agencies** around the outer hexagon correspond to the outer ring of Clean Architecture, “frameworks & drivers”
* **Outer hexagon adapters** match the “interface adapters” ring in Clean Architecture
* **Application hexagon** aligns with the “business rules” in Clean Architecture. These are further divided into “enterprise-related business rules” (entities) and “application business rules” (use cases that orchestrate entities and control data flow). However, Hexagonal Architecture leaves the architecture within the application hexagon open
* **Ports** are not explicitly mentioned in Clean Architecture but are present in associated UML diagrams and source code examples as interfaces



| Aspect                                       | Traditional Layered Architecture                                                                                                                                                     | Clean Architecture                                                                                                                                                                                       | Hexagonal Architecture                                                                                                                                                                                                                                                                                      |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Definition                                   | Divides the application into layers based on functionality, with each layer having dependencies on the layer below it                                                                | Emphasizes separation of concerns and the use of SOLID principles for building scalable and maintainable software                                                                                        | Also known as Ports and Adapters Architecture, it focuses on separating the application core from the surrounding infrastructure layers                                                                                                                                                                     |
| Dependency Flow                              | Dependencies flow from higher-level layers to lower-level layers, with each layer depending on the layer directly below it                                                           | The dependency flow is unidirectional, with higher-level layers depending on lower-level layers. Dependencies are not allowed to flow in the opposite direction                                          | The flow of dependencies is inward, with the application core defining interfaces (ports) that are implemented by the adapters. The adapters depend on the interfaces defined by the core                                                                                                                   |
| Dependency Inversion Principle (DIP) Support | May not explicitly enforce the DIP, leading to potentially tightly coupled dependencies between layers                                                                               | Emphasizes the DIP by using interfaces and dependency injection to invert dependencies, enabling higher-level layers to depend on abstractions defined by lower-level layers                             | Strongly aligns with the DIP by relying on interfaces (ports) that are implemented by adapters, allowing the core to depend on abstractions                                                                                                                                                                 |
| Deployment and Infrastructure                | May have tight coupling with the infrastructure, making it more difficult to adapt or switch deployment environments without impacting the entire system                             | Is agnostic to the deployment and infrastructure details, allowing for easy adaptation to different environments                                                                                         | Abstracts away the infrastructure, making it easier to switch or adapt to different deployment environments without affecting the core                                                                                                                                                                      |
| Flexibility and Adaptability                 | Can be less flexible and adaptable due to tight coupling, making it harder to change or replace components without impacting other parts of the system                               | Promotes flexibility and adaptability by enforcing separation of concerns, allowing for independent modification of layers without affecting others                                                      | Offers high flexibility and adaptability by isolating the core from external dependencies, making it easier to switch or upgrade components                                                                                                                                                                 |
| Layering Philosophy                          | Layers are organized vertically based on functionality, with each layer providing services to the layer above it                                                                     | Layers are organized based on business logic and responsibilities. Each layer has a clear purpose and can depend only on the layer below it                                                              | Ports (interfaces) and Adapters (implementations) are the core elements. The application core is isolated from the external dependencies                                                                                                                                                                    |
| Scalability and Maintainability              | Can be scalable and maintainable, but tight coupling between layers can make it more challenging to modify or add new features without affecting other parts of the system           | Promotes scalability and maintainability by enforcing separation of concerns and dependency inversion, making it easier to modify and add new features without impacting other parts of the system       | Enables scalability and maintainability by decoupling the application core from the infrastructure, allowing for easier modification and replacement of external dependencies                                                                                                                               |
| Testing and Isolation                        | Can make testing more challenging due to tight coupling between layers, requiring more complex integration testing                                                                   | Advocates for testability by employing dependency inversion and interfaces, enabling mocking and unit testing of individual layers                                                                       | Promotes easier testing and isolation of the application core by using ports and adapters, allowing the core to be tested independently of the infrastructure                                                                                                                                               |
| Use cases                                    | <p>- project requires a simple and straightforward architecture<br>- project involves a small number of external integrations<br>- project requires a high degree of scalability</p> | <p>- project requires a high degree of maintainability and understandability<br>- project involves a large number of business rules and use cases<br>- project requires a high degree of testability</p> | <p>- project requires a high degree of testability and maintainability<br>- project involves a large number of external integrations, such as databases, file systems, or web services<br>- project requires a high degree of flexibility, such as the ability to switch from one technology to another</p> |

### Conclusion <a href="#id-523f" id="id-523f"></a>

Hexagonal, Clean, and Onion Architectures are alternative architectural styles that can help you design more modular, testable, and scalable applications. However, they also come with some trade-offs that you need to consider before adopting them. There is no one-size-fits-all solution for software architecture, so you need to choose the style that best suits your project’s requirements and constraints.
