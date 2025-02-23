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

*(Coming soon...)*

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

