# ![GraphQL Logo](/public/assets/graphql-logo.png)


## Table of Contents
- [What is GraphQL](#what-is-graphql)
- [GraphQL vs REST](#graphql-vs-rest)
- [GraphQL vs gRPC](#graphql-vs-grpc)
- [Core Concepts](#core-concepts)
- [Introspection](#introspection)
- [Queries](#queries)
- [Mutations](#mutations)
- [Subscriptions](#subscriptions)
- [GraphQL Servers](#graphql-servers)
- [GraphQL Clients](#graphql-clients)
- [Next Steps](#next-steps)

## What is GraphQL

GraphQL is a **specification** for communicating with an API. It is typically used over HTTP and allows clients (mobile/web apps) to fetch exactly the data they need from backend APIs.

### GraphQL Over HTTP

GraphQL follows a different approach compared to traditional REST APIs. Instead of multiple endpoints for different resources, GraphQL uses a **single endpoint** where clients send a **POST request** containing a query specifying the data they need.

![GraphQL Over HTTP](/public/images/graphql-Graphql_Http.jpg)

### GraphQL Client-Server Flow

1. A GraphQL query is **not** JSON but resembles it. However, when making a `POST` request, the query is sent as a **string**.
2. The server extracts the query string from the JSON Object, processes and validates it against the GraphQL schema.
3. The server fetches the required data from a database or other services.
4. The response is returned in a **JSON object**.

## Example GraphQL Client Setup

Using a GraphQL client simplifies making API calls, similar to using Axios for REST APIs.

```javascript
// Setup a GraphQL client
const client = new Client("https://myapi.com/graphql");

// Send a query
client.query(`
  query {
    user {
      id
      name
    }
  }
`);
```

> **Note**: You can use the native JavaScript `fetch` API instead of a GraphQL client for simple use cases.

## GraphQL vs REST

GraphQL is often compared to REST APIs. Let's look at the differences and how they can complement each other.

### Example: Fetching User Details and Address

In REST:
- API endpoints are **resource-based**.
- You make multiple calls: one to get user details and another to get the address.

![REST API Call](/public/images/graphql-Rest-example.jpg)

In GraphQL:
- A single request fetches both user details and their address.

![GraphQL Query](/public/images/graphql-GraphQL%20example%20.jpg)

Chart to show GRAPHQL analogs of typical REST-ish terms:
![GraphQL Action Terms](/public/images/graphql-rest%20graphql%20comparison%20table.jpg)

### Key Differences

| Feature           | REST API         | GraphQL         |
|------------------|----------------|----------------|
| Data Fetching    | Multiple endpoints | Single endpoint (`/graphql`) |
| Overfetching     | Yes, fetches extra data | No, fetches only requested fields |
| Underfetching    | Requires multiple requests | Single request can include all data |
| Versioning       | API versions (`v1`, `v2`, etc.) | Schema evolves with fields deprecation |
| Error Handling   | HTTP status codes | Always returns `200 OK`, errors in response body |

### Benefits of GraphQL

- **Avoid Overfetching**: Request only the necessary fields.
- **Prevent Multiple API Calls**: Fetch related data in a single query.
- **Less Backend Dependency**: No need to request new endpoints from API developers.
- **Self-Documenting**: Schema defines available queries, types, and relationships.

### Schema and Type System

GraphQL APIs follow a **strong type system** with schemas defining data structure.

In REST APIs, there is no strict schema enforcement. The closest alternative is using **OpenAPI Spec** for documentation.

### HTTP Status Codes

In REST:
- HTTP status codes (`200`, `400`, `500`, etc.) indicate different outcomes.

In GraphQL:
- Every request returns `200 OK`.
- Errors are included in the response body under an `errors` object.

![GraphQL HTTP Status Codes](/public/images/graphql-status%20code%20comparisons.jpg)

### Monitoring

REST APIs:
- Monitoring is straightforward using HTTP status codes.

GraphQL APIs:
- Monitoring tools need to parse response bodies to check for errors.

### Caching

REST APIs:
- GET requests can be cached at the server, CDN, or browser level.

GraphQL:
- Standard HTTP caching is **not** applicable since queries go through a single endpoint.
- However, GraphQL clients (e.g., **Apollo Client, URQL**) implement caching using **Introspection**.

## GraphQL vs gRPC

GraphQL and REST are not the only API technologies. There is also **gRPC**, an open-source Remote Procedure Call (RPC) framework that enables you to build APIs.

In this document, we'll explore how gRPC differs from and compares with GraphQL.

---

### How Does gRPC Work?

**"RPC" stands for Remote Procedure Call**, which describes how communication happens.

In gRPC, the client communicates with the server by calling its methods as if they were local. This is made possible by creating a **client stub**, which has the same methods as the gRPC server. The client then uses this stub to call the server's methods.

They exchange data using **Protocol Buffers (Protobuf)**, whereas GraphQL uses text-based JSON for data exchange. **Protobuf** is a mechanism that serializes structured data into a binary format, making it more efficient. Learn more about Protobuf [here](https://protobuf.dev/overview/).

Similar to GraphQL, gRPC has a schema-like file (`.proto`) that describes the API service. It specifies the available methods, their parameters, and their return types.

The default and most common **Interface Definition Language** for writing `.proto` files is **Protobuf**.

#### Example:

```proto
syntax = "proto3";

service User {
  rpc GetUser (UserRequest) returns (UserResponse) {}
}

message UserRequest {
  int32 id = 1;
}

message UserResponse {
  string name = 1;
  int32 age = 2;
  string address = 3;
}
```

The above code defines a service that allows you to retrieve details about a specific user.
- The `GetUser` method represents the API endpoint the client can call to retrieve a user.
- Due to the `.proto` files and Protocol Buffers, gRPC can **auto-generate** the client and server boilerplate code in various programming languages.

---

### Benefits of gRPC

- **Performance of data exchange:** gRPC uses Protobuf to serialize data into a binary format, improving payload size and making data exchange faster and more efficient.
- **Uses HTTP/2 transport protocol:** HTTP/2 enables request/response multiplexing, splitting requests and responses into multiple frames sent individually and reassembled at the destination. This allows multiple requests and responses to be transferred over a single connection.

- **Supports multiple streaming types:**
  1. **Server streaming** – The client makes a request, and the server responds with a stream of messages.
  2. **Client streaming** – The client sends a stream of messages, and the server responds after the client has finished streaming.
  3. **Bidirectional streaming** – Both the client and server send independent streams of messages to each other.
  4. **Unary interaction** – The client sends one request, and the server sends one response back.
- **Code generation:** gRPC's `protoc` compiler can use the `.proto` file to generate server and client code in **11 programming languages**.

---

### Drawbacks of gRPC

1. **Limited language support:** Protobuf-based code generation is limited to **C#/.NET, C++, Dart, Go, Java, Kotlin, Node.js, Objective-C, PHP, Python, and Ruby**.
2. **Not human-readable:** Protobuf messages are in binary format, making them difficult to read and inspect without extra steps or tools.
3. **Browser support limitations:** gRPC does not have full browser support because not all browsers support HTTP/2.

---

### GraphQL vs gRPC

| Feature           | GraphQL | gRPC |
|------------------|---------|------|
| **Data Fetching** | Precise, fetch exactly what you need | May return extra data depending on API design |
| **Performance**   | Uses JSON (text-based) | Uses Protobuf (binary), making it faster and more efficient |
| **Code Generation** | Requires third-party tools | Native support for code generation |
| **Browser Support** | Fully supported (uses HTTP/1.1) | Limited support (depends on HTTP/2 in browsers) |
| **Message Format** | Human-readable JSON/XML | Binary Protobuf, not human-readable |
| **Ease of Learning** | Easier to learn, more documentation | More complex due to Protobuf and HTTP/2 |

---

### Conclusion

Both GraphQL and gRPC are great for specific use cases. No technology is a **one-size-fits-all** solution. In some scenarios, combining both technologies can create an even better solution.


## Core Concepts

*(Coming soon...)*

## Introspection

*(Coming soon...)*

## Queries

*(Coming soon...)*

## Mutations

*(Coming soon...)*

## Subscriptions

*(Coming soon...)*

## GraphQL Servers

*(Coming soon...)*

## GraphQL Clients

*(Coming soon...)*

## Next Steps

GraphQL is a powerful alternative to REST, offering flexibility and efficiency in API communication. To dive deeper:

- Check out the official [GraphQL documentation](https://graphql.org/)
- Experiment with [GraphiQL](https://github.com/graphql/graphiql) for querying APIs interactively
- Try implementing a GraphQL server with [Apollo Server](https://www.apollographql.com/docs/apollo-server/)

Happy coding! 🚀

