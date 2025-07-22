# API Reference: Sign up

Creates a new user account. All fields are required.

## URL

```text
https://promptcrafter-production.up.railway.app/auth/signup
```

## Method

`POST`

## Parameters

None

## Request headers

| Header name     | Required | Description                       |
|-----------------|----------|-----------------------------------|
| `Content-Type`  | Yes      | Must be set to `application/json` |

## Request body

A JSON object containing user details. All fields are required.

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

## Example request

```shell
curl -X POST https://promptcrafter-production.up.railway.app/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123"
  }'
```

## Response body

A JSON object containing an authentication token.

```json
{
  "message": "User account created successfully."
}
```

## Return status

| Status code | Status                 | Description                                           |
|-------------|------------------------|-------------------------------------------------------|
| 201         | Created                | Account created successfully.                         |
| 400         | Bad Request            | Required fields are missing or invalid.               |
| 409         | Conflict               | Email already in use.                                 |
| 500         | Internal Server Error  | An unexpected server error occurred.                  |

## Related

[Log in](reference/endpoints/post-auth-login.md): `POST /auth/login`
