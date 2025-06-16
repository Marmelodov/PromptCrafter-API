---
layout: page
---

# Search prompts

Returns an array of [`prompt`](../resources/prompt.md) objects matching a full-text search query. The query matches against the `title`, `content`, and `tags` fields of each prompt.

## URL

```text
{server_url}/search?q=your+query+terms
```

## Method

`GET`

## Parameters

| Parameter name | Type   | Required | Description                                 |
|----------------|--------|----------|---------------------------------------------|
| `q`            | string | Yes      | Full-text search query. Matches title, content, and tags. |

## Request headers

| Header name     | Required | Description                                |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | Bearer token used to authenticate the user |

## Request body

None

## Request example

```shell
curl -H "Authorization: Bearer {your_token}" {server_url}/search?q=marketing+email
```

## Response body

```json
[
  {
    "_id": "prompt3",
    "ownerId": "user3",
    "title": "Newsletter subject lines",
    "content": "Generate subject lines for a tech product launch email.",
    "model": "Claude",
    "tags": ["email", "marketing", "tech"],
    "createdAt": "2024-02-03T10:00:00Z",
    "updatedAt": "2024-02-03T10:00:00Z"
  }
]
```

## Return status

| Status code  | Status       | Description                                        |
|--------------|--------------|----------------------------------------------------|
| 200          | Success      | Prompts matching the search query returned         |
| 400          | Bad request  | Missing or malformed query string                 |
| 401          | Unauthorized | Authentication token missing or invalid           |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Get all prompts](get-prompts.md): `GET /prompts`  
[Retrieve a prompt by ID](get-prompts-id.md): `GET /prompts/:id`  
[Create a prompt](post-prompts.md): `POST /prompts`  
[Update a prompt](patch-prompts-id.md): `PATCH /prompts/:id`  
[Delete a prompt](delete-prompts-id.md): `DELETE /prompts/:id`

