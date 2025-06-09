---
layout: page
---

# GET prompt by ID

Returns a [`prompt`](../resources/prompt.md) by its unique ID. Users can only access prompts they own.

## URL

```text
{server_url}/prompts/:id
```

## Method

`GET`

## Parameters

| Parameter name | Type   | Required | Description                       |
|----------------|--------|----------|-----------------------------------|
| `id`           | string | Yes      | Unique identifier of the prompt. |

## Request headers

| Header name     | Required | Description                                |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | Bearer token used to authenticate the user |

## Request body

None

## Request example

```shell
curl -H "Authorization: Bearer {your_token}" {server_url}/prompts/prompt2
```

## Response body

```json
{
  "_id": "prompt2",
  "ownerId": "user2",
  "title": "AI image prompt",
  "content": "Create a prompt for a surreal mountain landscape painting.",
  "model": "DALL·E",
  "tags": [
    "art",
    "surreal",
    "landscape"
  ],
  "createdAt": "2024-02-02T09:00:00Z",
  "updatedAt": "2024-02-02T09:00:00Z"
}
```

## Return status

| Status code  | Status       | Description                                        |
|--------------|--------------|----------------------------------------------------|
| 200          | Success      | Prompt returned successfully                       |
| 401          | Unauthorized | Authentication token missing or invalid            |
| 403          | Forbidden    | Prompt does not belong to the authenticated user   |
| 404          | Not found    | Prompt with the specified ID was not found         |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Get all prompts](get-prompts.md): `GET /prompts`  
[Create a prompt](post-prompts.md): `POST /prompts`  
[Update a prompt](patch-prompts-id.md): `PATCH /prompts/:id`  
[Delete a prompt](delete-prompts-id.md): `DELETE /prompts/:id`
