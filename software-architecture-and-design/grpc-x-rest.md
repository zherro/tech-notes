# gRPC x REST

**What is the difference between gRPC and REST?**

gRPC and REST are two ways to create an API. An API is a mechanism that allows two software components to communicate using a set of definitions and protocols. In gRPC, one component (the client) calls or invokes specific functions in another software component (the server). In REST, instead of calling functions, the client requests or updates data on the server.

**What is gRPC?**

gRPC is an open-source architecture and API system controlled by the Cloud Native Computing Foundation. It is based on the Remote Procedure Call (RPC) model. Although the RPC model is broad, gRPC is a specific implementation.

**What is RPC?**

In RPC, client-server communications work as if the client's API requests were a local operation or as if the request were internal server code.

In RPC, a client sends a request to a process on the server that is always listening for remote calls. The request contains the server function to be called, along with any parameters to be passed. An RPC API uses a protocol like HTTP, TCP, or UDP as the underlying data exchange mechanism.

**How is gRPC different from RPC?**

gRPC is a system that implements traditional RPC with some optimizations. For example, gRPC uses protocol buffers and HTTP/2 for data transmission.

It also abstracts the data exchange mechanism from the developer. For example, another commonly used RPC API implementation, OpenAPI, requires developers to map RPC concepts to the HTTP protocol. But gRPC abstracts the underlying HTTP communication. These optimizations make gRPC faster, easier to implement, and more web-compatible than other RPC implementations.

**What is REST?**

REST is a software architecture approach that defines a set of rules for exchanging data between software components. It is based on HTTP, the standard web communication protocol. RESTful APIs manage communications between a client and a server using HTTP verbs such as POST, GET, PUT, and DELETE for create, read, update, and delete operations. The server-side resource is identified by a URL known as an endpoint.

REST works as follows:

* The client makes a request to create, modify, or delete a resource on the server.
* The request contains the resource endpoint and may also include additional parameters.
* The server responds by returning the entire resource to the client when the operation is completed.
* The response contains data in JSON format and status codes.

APIs created using REST guidelines are called RESTful APIs or REST APIs.

**Why do organizations use gRPC and REST?**

gRPC and REST are two different approaches to API development.

An API works similarly to ordering food in a restaurant through a menu. In any restaurant, a customer (client) can order food from the menu (API), which has a fixed set of dishes. This is communicated to the kitchen (server), which prepares the requested dish and sends it to the customer. The client does not need to know how the kitchen prepared the order, only what to expect in return. Standardizing menu formats means that customers and kitchens know how to use them.

Without APIs, there would be no shared agreement on how different applications or software services communicate. Programmers of two separate applications would need to talk to each other to determine how to develop data exchange every time.

There are different types of API architectures, such as gRPC and REST, because different architectures can be better for different use cases within an organization. An API designer should choose their preferred client-server architecture based on system requirements.

**What are the similarities between gRPC and REST?**

REST and gRPC share some inherent similarities in API architectural approaches.

**Data Exchange Mechanism**\
Both allow two software components, a client and a server, to communicate and exchange data based on a shared set of rules. These rules apply regardless of how each software component operates internally.

**HTTP-based Communication**\
Both transmit data through the HTTP request-response mechanism, the web's preferred efficient communication protocol. However, in gRPC, this is hidden from the developer, while in REST, it is more apparent.

**Implementation Flexibility**\
You can implement REST and gRPC in a wide variety of programming languages. This quality makes them highly portable across programming environments, leading to optimal interoperability with near-universal support.

**Suitability for Distributed and Scalable Systems**\
Both gRPC and REST use the following:

* Asynchronous communication, so the client and server can communicate without interrupting operations.
* Stateless design, so the server does not need to remember the client's state.

This means that developers can use gRPC and REST to create fault-tolerant systems with a large number of simultaneous requests. You can build distributed and scalable systems with multiple clients.

**Architectural Principles: gRPC versus REST**\
While REST and gRPC offer similar functionality, the underlying models differ significantly in their architecture.

**Communication Model**\
Using a REST API, a client sends a single REST API request to a server, and the server then returns a single response. The client must wait for the server to respond before continuing operations. This mechanism is a request-response model and is a unary (one-to-one) data connection.

On the other hand, with gRPC, a client can send one or more API requests to the server, which can result in one or multiple responses from the server. Data connections can be unary (one-to-one), server streaming (one-to-many), client streaming (many-to-one), or bidirectional streaming (many-to-many). This mechanism is a client response communication model and is possible because gRPC is based on HTTP/2.

**Operations That Can Be Called on the Server**\
In a gRPC API, the server operations that can be called are defined by services, also known as functions or procedures. The gRPC client invokes these functions just as you would call a function internally in an application. This is known as service-oriented design. See an example below:

```javascript
createNewOrder(customer_id, item_id, item_quantity) -> order_id
```

In REST, there is a limited set of HTTP request verbs the client can use on server resources defined by a URL. The client calls the resource itself. This is known as entity-oriented design, which aligns well with object-oriented programming methods. See an example below:

```javascript
POST /orders <headers> (customer_id, item_id, item_quantity) -> order_id
```

Although you can design gRPC APIs in an entity-oriented approach, this is not a restriction of the system itself.

**Data Exchange Format**\
With a REST API, the data structures passed between software components are typically expressed in JSON data exchange format. Other data formats like XML and HTML can also be passed. JSON is flexible and easy to read, although it must be serialized and translated into a programming language.

On the other hand, gRPC uses the Protocol Buffers (Protobuf) format by default but also natively supports JSON. The server defines a data structure using the Protocol Buffer interface description language (IDL) in a proto-specification file. gRPC then serializes the structure into binary format and deserializes it into any specified programming language. This mechanism makes it faster than using JSON, which is not compressed during transmission. Protocol buffers are not human-readable, unlike a REST API used with JSON.

**Other Key Differences: gRPC versus REST**\
**Client-Server Coupling**\
REST is loosely coupled, meaning the client and server do not need to know anything about each other's implementation. This loose coupling makes it easier to evolve the API over time because a change in server definitions does not necessarily require a change in client code.

gRPC is tightly coupled, meaning the client and server must have access to the same proto file. Any update to the file requires updates to both the server and the client.

**Code Generation**\
gRPC offers a built-in selection of native client-side and server-side code generation features. They are available in multiple languages due to protoc, the Protocol Buffers compiler. Once you define the structure in the proto file, gRPC generates client-side and server-side code. Code generation makes API development less time-consuming.

On the other hand, REST does not offer any built-in code generation mechanism, so developers must use additional third-party tools if they need this feature.

**Bidirectional Streaming**\
gRPC offers bidirectional streaming communication. This means both the client and server can send and receive multiple requests and responses simultaneously over a single connection.

REST does not offer this feature.

**When to Use gRPC versus REST**\
Currently, REST is the most popular API architecture for web services and microservices architectures. REST's popularity is due to its simple implementation and mapping, readability, and flexibility of the data structure. It's easy for new programmers to start developing RESTful APIs for their applications, whether for web service development or internal microservices.

Here are the use cases for a REST API:

* Web-based architectures
* Public-facing APIs for easier understanding by external users
* Simple data communications

Unlike REST, gRPC was specifically designed to enable developers to create high-performance APIs for microservices architectures in distributed datacenters. It is better suited for internal systems requiring real-time streaming and large data loads. gRPC is also a good option for microservices architectures composed of multiple programming languages when the API is unlikely to change over time.

A gRPC API is better for these use cases:

* Secure and high-performance systems
* High data loads
* Real-time or streaming applications

**A Note on Web Software Development**\
While HTTP is the primary protocol of the web, there are different versions of HTTP with varying degrees of adoption in web browsers and servers.

A gRPC API always uses HTTP/2, and a REST API typically uses HTTP/1.1, which is not the same HTTP protocol. While HTTP/2 is now a common web protocol, it does not have universal support for browsers, unlike HTTP/1.1. This limited browser support can make gRPC a less attractive option for developers who want to support web applications.

\*\*Summary of Differences
