# 10 - API Design

## Introduction to API Design

API (Application Programming Interface) design is the process of developing APIs that effectively expose data and application functionality for consumption by developers and applications. Good API design is crucial for developer experience, system scalability, and long-term maintenance.

## Core API Design Paradigms

### REST (Representational State Transfer)

REST is an architectural style for distributed systems, particularly web services. RESTful APIs use HTTP methods explicitly and have the following characteristics:

- **Stateless**: Server doesn't store client state between requests
- **Resource-based**: Everything is a resource identified by URIs
- **Representation-oriented**: Resources can have multiple representations (JSON, XML, etc.)
- **Uniform interface**: Consistent approach to resource manipulation
- **Client-server architecture**: Separation of concerns between client and server
- **Layered system**: Components cannot "see" beyond their immediate layer

#### REST Maturity Levels (Richardson Maturity Model)

1. **Level 0**: Single URI, single HTTP method (typically POST)
2. **Level 1**: Multiple URIs for different resources, but still a single HTTP method
3. **Level 2**: HTTP verbs used as intended (GET, POST, PUT, DELETE)
4. **Level 3**: HATEOAS (Hypermedia as the Engine of Application State) - APIs provide hyperlinks to guide clients

### RPC (Remote Procedure Call)

RPC focuses on actions and functions rather than resources:

- Typically uses POST for most operations
- Emphasizes operations over resources
- Examples include gRPC, XML-RPC, and JSON-RPC
- Often preferred for internal service-to-service communication

#### gRPC

- Uses Protocol Buffers for serialization
- Supports bidirectional streaming
- Provides auto-generated client libraries

### GraphQL

A query language and runtime for APIs:

- Client specifies exactly what data it needs
- Single endpoint for all operations
- Strongly typed schema
- Reduces over-fetching and under-fetching of data
- Supports queries (read), mutations (write), and subscriptions (real-time updates)

## CRUD Operations

CRUD represents the four basic operations for persistent storage:

| Operation | Description               | HTTP Method (REST) | SQL    |
| --------- | ------------------------- | ------------------ | ------ |
| Create    | Add new resources         | POST               | INSERT |
| Read      | Retrieve resources        | GET                | SELECT |
| Update    | Modify existing resources | PUT/PATCH          | UPDATE |
| Delete    | Remove resources          | DELETE             | DELETE |

### REST CRUD Mapping

```
POST /users             # Create a user
GET /users              # Read all users
GET /users/{id}         # Read specific user
PUT /users/{id}         # Update user (full replacement)
PATCH /users/{id}       # Update user (partial modification)
DELETE /users/{id}      # Delete user
```

## URL Design Best Practices

### Resource Naming

- Use nouns, not verbs (e.g., `/products` not `/getProducts`)
- Use plural nouns for collections (`/users` not `/user`)
- Use concrete names over abstract concepts
- Use lowercase letters and hyphens for multi-word resources

### Hierarchy and Relationships

- Express parent-child relationships in the URL path:
  ```
  /departments/{id}/employees
  /users/{id}/posts
  ```
- Consider the depth of nesting (avoid too many levels)

### Query Parameters vs Path Parameters

- **Path parameters**: Identify specific resources
  ```
  /users/123
  ```
- **Query parameters**: Filter, sort, paginate, or control response format
  ```
  /users?role=admin&status=active
  /products?sort=price&direction=asc
  ```

## Pagination Strategies

### Limit-Offset Pagination

- **Limit**: Maximum number of items to return
- **Offset**: Number of items to skip

```
GET /products?limit=10&offset=20
```

Pros:

- Simple to implement
- Stateless
- Works well with SQL databases (`LIMIT` and `OFFSET` clauses)

Cons:

- Performance issues with large offsets
- Potential inconsistencies with concurrent updates

### Cursor-Based Pagination

Uses a pointer (cursor) to a specific item:

```
GET /products?limit=10&after=eyJpZCI6MTAwfQ==
```

Pros:

- Performance remains consistent regardless of page depth
- Handles insertions/deletions gracefully
- Better for real-time data

Cons:

- More complex to implement
- Usually requires an indexed field for sorting

### Page-Based Pagination

Uses page numbers:

```
GET /products?page=2&per_page=10
```

Pros:

- Intuitive for users
- Simple to implement

Cons:

- Same issues as offset pagination
- Doesn't handle data changes well

## Response Formats

### JSON Structure Best Practices

```json
{
  "data": {
    "id": 123,
    "name": "Example Product",
    "price": 99.99
  },
  "meta": {
    "timestamp": "2023-04-10T15:00:00Z",
    "version": "1.0"
  }
}
```

### Collection Responses with Pagination

```json
{
  "data": [
    { "id": 1, "name": "Product A" },
    { "id": 2, "name": "Product B" }
  ],
  "pagination": {
    "total_items": 50,
    "total_pages": 5,
    "current_page": 1,
    "per_page": 10,
    "next": "/products?page=2&per_page=10",
    "prev": null
  }
}
```

## Error Handling

### HTTP Status Codes

- **2xx**: Success (200 OK, 201 Created, 204 No Content)
- **4xx**: Client errors (400 Bad Request, 401 Unauthorized, 404 Not Found)
- **5xx**: Server errors (500 Internal Server Error, 503 Service Unavailable)

### Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input parameters",
    "details": [
      { "field": "email", "message": "Must be a valid email address" }
    ]
  }
}
```

## Versioning Strategies

### URI Path Versioning

```
/api/v1/products
/api/v2/products
```

### Query Parameter Versioning

```
/api/products?version=1
```

### Header Versioning

```
Accept: application/vnd.myapi.v2+json
```

### Content Negotiation

```
Accept: application/vnd.myapi+json; version=2.0
```

## Authentication & Authorization

### Authentication Methods

- **API Keys**: Simple key passed in header or query parameter
- **OAuth 2.0**: Token-based authorization framework
- **JWT (JSON Web Tokens)**: Self-contained tokens with encoded claims
- **Basic Auth**: Base64 encoded username:password

### Authorization Patterns

- **Role-Based Access Control (RBAC)**: Permissions assigned to roles
- **Attribute-Based Access Control (ABAC)**: Dynamic permissions based on attributes
- **Scopes**: Permissions attached to tokens

## API Documentation

### Documentation Formats

- **OpenAPI/Swagger**: Industry standard for REST APIs
- **API Blueprint**: Markdown-based documentation
- **RAML**: YAML-based API modeling language

### Documentation Best Practices

- Keep documentation in sync with implementation
- Include examples for each endpoint
- Document error responses and edge cases
- Provide SDKs or code snippets when possible

## Performance Considerations

- **Caching**: Use HTTP caching headers (ETag, Last-Modified)
- **Compression**: Enable GZIP/Brotli compression
- **Rate Limiting**: Protect against abuse and ensure fair usage
- **Batch Operations**: Allow multiple operations in a single request

## Security Best Practices

- Always use HTTPS
- Implement proper authentication and authorization
- Validate and sanitize all inputs
- Apply rate limiting and throttling
- Use security headers (CORS, CSP, etc.)
- Regular security audits and testing

## Hypermedia and HATEOAS

```json
{
  "id": 123,
  "name": "Example Product",
  "price": 99.99,
  "_links": {
    "self": { "href": "/products/123" },
    "manufacturer": { "href": "/manufacturers/456" },
    "related": { "href": "/products/123/related" }
  }
}
```

## API Design Tools

- **Swagger/OpenAPI Editor**: For designing and documenting APIs
- **Postman**: API testing and documentation
- **Insomnia**: REST and GraphQL client
- **Apiary**: API design, prototyping, and documentation
