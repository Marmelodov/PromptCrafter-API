---
layout: page
---

# Save a prompt

Creates a new [`prompt`](../resources/prompt.md) owned by the user.

## URL

```text
https://promptcrafter-production.up.railway.app/prompts
```

## Method

`POST`

## Parameters

None

## Request headers

| Header name     | Required | Description                                |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | Bearer token used to authenticate the user |
| `Content-Type`  | Yes      | Must be set to `application/json`          |

## Request body

A JSON object representing the new prompt. All fields except `tags` are required.

```json
{
  "title": "AI image prompt",
  "content": "Create a prompt for a surreal mountain landscape painting.",
  "model": "DALL·E",
  "tags": ["art", "surreal", "landscape"]
}
```

## Request example

```shell
curl -X POST https://promptcrafter-production.up.railway.app/prompts \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "AI image prompt",
    "content": "Create a prompt for a surreal mountain landscape painting.",
    "model": "DALL·E",
    "tags": ["art", "surreal", "landscape"]
  }'
```

## Response body

The newly created `prompt` object.

```json
{
  "_id": "prompt2",
  "ownerId": "user2",
  "title": "AI image prompt",
  "content": "Create a prompt for a surreal mountain landscape painting.",
  "model": "DALL·E",
  "tags": ["art", "surreal", "landscape"],
  "createdAt": "2024-02-02T09:00:00Z",
  "updatedAt": "2024-02-02T09:00:00Z"
}
```

## Return status

| Status code  | Status       | Description                                        |
|--------------|--------------|----------------------------------------------------|
| 201          | Created      | Prompt created successfully                        |
| 400          | Bad request  | Required fields are missing or invalid             |
| 401          | Unauthorized | Authentication token missing or invalid            |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Retrieve all prompts](get-prompts.md): `GET /prompts`  
[Retrieve a prompt by ID](get-prompts-id.md): `GET /prompts/:id`  
[Update a prompt](patch-prompts-id.md): `PATCH /prompts/:id`  
[Delete a prompt](delete-prompts-id.md): `DELETE /prompts/:id`
