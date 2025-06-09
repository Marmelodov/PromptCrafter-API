---
layout: page
---

# `prompt` resource

**Resource endpoint**:

```text
{base_url}/prompts
```

The `prompt` resource stores a generative AI prompt created and managed by a user. It includes the prompt text, associated AI model, optional tags, and timestamps. All operations on this resource require user authentication with a bearer token.  

## Resource properties

Sample `prompt` resource:  

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

| Name        | Type   | Required | Description                                                   |
| ----------- | ------ | -------- | ------------------------------------------------------------- |
| `_id`       | string | Yes      | Unique identifier for the prompt.                             |
| `ownerId`   | string | Yes      | ID of the user who created the prompt.                        |
| `title`     | string | Yes      | Short title for the prompt.                                   |
| `content`   | string | Yes      | Text of the prompt.                                  |
| `model`     | string | Yes      | Intended AI model for the prompt (e.g., ChatGPT, DALL·E). |
| `tags`      | array  | No       | Optional tags for categorizing the prompt.          |
| `createdAt` | string | Yes      | Timestamp of when the prompt was created (ISO 8601 format). Set automatically by the server.   |
| `updatedAt` | string | Yes      | Timestamp of the prompt's last update (ISO 8601 format). Set automatically by the server.                  |

## Supported operations

| Method | Endpoint       | Description                                      |
| ------ | -------------- | ------------------------------------------------ |
| GET    | `/prompts`     | [Retrieve all prompts](../endpoints/get-prompts.md) |
| POST   | `/prompts`     | [Create a new prompt](../endpoints/post-prompts.md)                             |
| GET    | `/prompts/:id` | [Retrieve a prompt by ID](../endpoints/get-prompts-id.md)                  |
| PATCH  | `/prompts/:id` | [Update a prompt by ID](../endpoints/patch-prompts-id.md)                           |
| DELETE | `/prompts/:id` | [Delete a prompt by ID](../endpoints/delete-prompts-id.md)                           |
