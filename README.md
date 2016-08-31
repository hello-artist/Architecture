# Architecture

## intro 
This is a small model of a complete microservice architecture.
It's going to follow all the good principles and guidelines of microservice architecture.

### Communication  
  ---
  - Microservices should get their dependency microservices as an environment variable (We can change this to a centeralized discovery server)  
    dependencies variable can be a JSON array like this:
```json
[
  {
    "name":"nodejs",
    "address":"localhost:8282"
  },
  {
    "name":"java",
    "address":"localhost:8283"
  }
]
```
  - We only use `HTTP POST` requests with `"content-type": "application/json"` for communication between micro-services and JSON exists in message body.
  - Microservices should have a `/api` endpoint for this purpose 
  - Each request consists of a JSON string in the following format:   
```javascript
  {
    "requestId": "hash of request info"   // generated by gateway or first microservice that gets the request
    "fn":        "apiFunctionName",
    "params":    {}                       // anything from an object
  }
```
  - Refer to [stage1](https://github.com/FutureRose/Architecture/blob/master/Stage1%20-%20Communication.md) for implementation

### Gateway  
  ---
  - An API gateway implemented using one of the famous gateway technologies e.g. [GraphQL](http://graphql.org/docs/getting-started/) or [Falcor](https://netflix.github.io/falcor/).
  - Main goals of gateway are:
    - Hide background architecture from clients
    - Do Athorization on behalf of all microservices (single source of auth)
    - Make query optimization and do caching on API level
    - Generate a unified API for all microservice
