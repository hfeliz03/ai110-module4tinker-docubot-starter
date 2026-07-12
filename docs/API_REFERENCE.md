# API Reference

This document is part of the fictional developer corpus that DocuBot uses for retrieval experiments. It describes sample endpoints, request requirements, and response structures.

## Base URL

```plaintext
/api
```

## Authentication Endpoints

### POST /api/login

Authenticates a user and returns a short-lived access token.

Request body:

```json
{
  "username": "example",
  "password": "secret"
}
```

Successful response:

```json
{
  "access_token": "<token>"
}
```

Notes:

- Returns `401` if credentials are invalid.
- The token must be included in the `Authorization` header for protected endpoints.

### POST /api/refresh

Exchanges an existing valid token for a new one with a refreshed expiration time.

Headers:

```plaintext
Authorization: Bearer <token>
```

Response:

```json
{
  "access_token": "<new_token>"
}
```

## User Data Endpoints

### GET /api/users/<user_id>

Fetches profile data for a specific user.

Headers:

```plaintext
Authorization: Bearer <token>
```

Response example:

```json
{
  "user_id": 42,
  "email": "user@example.com",
  "joined_at": "2024-01-15T10:22:00Z"
}
```

Failure cases:

- `401` if the token is missing or expired
- `403` if the user lacks permission to view this profile
- `404` if no user exists with the given ID

### GET /api/users

Returns a list of all users. Only accessible to admins.

Headers:

```plaintext
Authorization: Bearer <token>
```

Successful response:

```json
[
  {
    "user_id": 1,
    "email": "admin@example.com"
  },
  {
    "user_id": 2,
    "email": "guest@example.com"
  }
]
```

Notes:

- Returns `403` for non-admin accounts.

## Project Data Endpoints

### GET /api/projects

Returns all projects visible to the calling user.

Headers:

```plaintext
Authorization: Bearer <token>
```

Response example:

```json
[
  {
    "project_id": "alpha",
    "name": "Alpha Project",
    "status": "active"
  }
]
```

### GET /api/projects/<project_id>

Fetches detailed information for a single project.

Headers:

```plaintext
Authorization: Bearer <token>
```

Response example:

```json
{
  "project_id": "alpha",
  "name": "Alpha Project",
  "description": "Internal research initiative.",
  "status": "active",
  "owner": 1
}
```

Failure cases:

- `401` for missing token
- `403` if the caller cannot access the project
- `404` if the project does not exist

## Error Format

All error responses follow this structure:

```json
{
  "error": "Description of the problem"
}
```

Common messages include `Unauthorized`, `Forbidden`, `Not Found`, and `Invalid Request`.
