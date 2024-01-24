# Rest API Cheatsheet

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
