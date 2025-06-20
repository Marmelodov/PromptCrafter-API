---
layout: page
---

# Log in

Authenticates a user and returns a JWT token. All fields are required.

## URL

```text
https://promptcrafter-production.up.railway.app/auth/login
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

A JSON object with the user's email and password.

```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

## Request example

```shell
curl -X POST https://promptcrafter-production.up.railway.app/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "password123"
  }'
```

## Response body

A JSON object containing a JWT authentication token.

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

## Return status

| Status code | Status                 | Description                                           |
|-------------|------------------------|-------------------------------------------------------|
| 200         | OK                     | Authentication successful.                            |
| 400         | Bad Request            | Required fields are missing or invalid.               |
| 401         | Unauthorized           | Incorrect email or password.                          |
| 500         | Internal Server Error  | An unexpected server error occurred.                  |

## Related

[Sign up](post-auth-signup.md): `POST /auth/signup`
