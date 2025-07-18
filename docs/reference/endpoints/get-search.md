---
layout: page
---

# Search prompts

Returns an array of [`prompt`](../resources/prompt.md) objects matching a full-text search query. The query matches against the `title`, `content`, and `tags` fields of each prompt.

## URL

```text
https://promptcrafter-production.up.railway.app/search?q=your+query+terms
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
curl -H "Authorization: Bearer {your_token}" https://promptcrafter-production.up.railway.app/search?q=marketing+email
```

## Response body

```json
[
  {
    "_id": "prompt3",
    "ownerId": "user3",
    "title": "Newsletter subject lines",
    "content": "Generate subject lines for a tech product launch email.",
    "model": "Claude 4.0 Sonnet",
    "tags": ["email", "marketing", "tech"],
    "createdAt": "2025-02-03T10:00:00Z",
    "updatedAt": "2025-02-03T10:00:00Z"
  }
]
```

## Return status

| Status code | Status                 | Description                                              |
|-------------|------------------------|----------------------------------------------------------|
| 200         | Success                | Prompts matching the search query returned.              |
| 400         | Bad Request            | Missing or malformed query string.                       |
| 401         | Unauthorized           | Authentication token is missing, expired, or invalid.    |
| 500         | Internal Server Error  | An unexpected server error occurred.                     |

## Related

[Save a prompt](post-prompts.md): `POST /prompts` 
[Retrieve all prompts](get-prompts.md): `GET /prompts`  
[Retrieve a prompt by ID](get-prompts-id.md): `GET /prompts/:id`  
[Update a prompt](patch-prompts-id.md): `PATCH /prompts/:id`  
[Delete a prompt](delete-prompts-id.md): `DELETE /prompts/:id`
