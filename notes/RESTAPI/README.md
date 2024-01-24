# Rest API, my way

Some notes about how I like to structure my REST API projects.

## Verbs

| Verb | Description |
| --- | --- |
| GET | Retrieve a resource |
| POST | Create a resource |
| PUT | Update a resource |
| DELETE | Delete a resource |
| PATCH | Partially update a resource |

## Name Convention

| Verb | Route Name | Description |
| --- | --- | --- |
| GET | /api/v1/users | Get all users |
| GET | /api/v1/users/1 | Get user with id 1 |
| POST | /api/v1/users | Create a new user |
| PUT | /api/v1/users/1 | Update user with id 1 |
| DELETE | /api/v1/users/1 | Delete user with id 1 |
| PATCH | /api/v1/users/1 | Partially update user with id 1 |

### Resources

Resources describe the data that will be transmitted through the API. 
**They are usually nouns and should be pluralized.**

Examples:

- /api/v1/users instead of /api/v1/user

Hypens can be used to improve readability:

- /api/v1/user-roles instead of /api/v1/userroles or /api/v1/user_roles

Always use lowercase:

- /api/v1/users instead of /api/v1/Users

### Versioning

Versioning is important to keep track of changes in the API.

Examples:

- /api/v1/users instead of /api/users

### Pagination

Pagination is important to avoid sending too much data through the API.

Examples:

- /api/v1/users?page=1&limit=10

### Filtering

Filtering is important to avoid sending too much data through the API.

Examples:

- /api/v1/users?name=John

## How I structure my REST API projects

Usually I like to have a folder called `docs` inside the `api` folder. Within this folder I create two files:

- api.md: Here I gather some examples of how to use the API. With some expected requests and responses.
- api.yaml: Here I gather the API specification using the [OpenAPI Specification](https://www.openapis.org/).

The api folder, is the place where I put all the routes and handlers of the API. 

If my API have different resources, I like to create a different file for each resource. For example:

```
- service
    - api
        - docs
            - api.md
            - api.yaml
        - users.go
        - users_test.go
        - alerts.go
        - alerts_test.go
```

And inside of each one, the implementation and business logic of the resource.
It's also important to keep in mind the structure definition of each resource, for example:

```go
//service/api/users.go

type User struct {
    ID        int64     `json:"id"`
    Name      string    `json:"name"`
    Email     string    `json:"email"`
}
```

The struct that was defined above, inside the `service/api/users.go` file, will be used to define the **response** of the API.

If we are dealing with some database connection, I create different Structs to represent the data that it's being dealt in the diffrent layers of the application. For example:

```
- service
    - api
    - postgres
        - db.go
        - users_storage.go <- could also be called users_repository.go or just users.go
        - users_storage_test.go
```
Obs: I like to create a folder for each database that I'm using in the application, for example: postgres, mysql, mongo, etc.

And inside of this folder, the functions that will be used to interact with the resource that was defined in the API folder.

Also if the structures across the different layers are close, I like to define them in different packages, for example:

```go
//service/postgres/users_storage.go
type User struct {
    ID        int64     `json:"id"`
    Name      string    `json:"name"`
    Email     string    `json:"email"`
    CreatedAt time.Time `json:"created_at"`
}
```

# Dealing with data consumption throught event driven architecture

## What is event driven architecture?

Send and receive events between a producer and a consumer.

## How I kept track of the events that were sent between the producer and the consumer?

In the root of the service, I created a folder called `docs` and inside of it, I create a file called `events.md`. Where I have some example of the input and output, and what service are producing and consuming them. At this way, someone that is not familiar with the service, can understand what is happening, and for someone like me tha don't have a good memory, can remember what is happening.

I learn how to document, using the following pattern, don't know if it's the best, but it's better to start somewhere:

- [Event Catalog](https://www.eventcatalog.dev/)

**Think about this approach as the same way that we document APIs using the OpenAPI Specification, but for events.**

# Metrics 

I like the following philosophy for metrics:

```
You can't improve what you aren't measuring
```

For this reason, I learn to AWAYS have some metrics in my services, and I like to use the following measures:

## For APIS: 

### Latency (Histogram)

- metric name: http_request_duration_seconds
- labels: status_code, method, path or handler

### Error Rate (Counter)

- matric name: http_total_requests
- labels: status_code, method, path or handler
