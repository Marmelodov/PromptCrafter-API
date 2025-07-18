# API Reference: `prompt` resource

The `prompt` resource stores a generative AI prompt created and managed by a user. It includes the prompt text, associated AI model, optional tags, and timestamps. All operations on the prompt resource require user authentication with a bearer token. 

**Resource endpoint**:

```text
https://promptcrafter-production.up.railway.app/prompts
```

## Resource properties

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

Sample `prompt` resource:  

```json
 {
  "_id": "prompt104",
  "ownerId": "user7",
  "title": "Historical Explanation for Students",
  "content": "Explain the significance of the Industrial Revolution to high school students using clear, accessible language. Include at least two key inventions and describe how these changes affected daily life in Europe and America.",
  "model": "Claude 3 Sonnet",
  "tags": ["history", "education", "explanation"],
  "createdAt": "2025-06-20T13:03:00Z",
  "updatedAt": "2025-06-20T13:03:00Z"
 }
```

## Supported operations

| Method | Endpoint       | Description                                      |
| ------ | -------------- | ------------------------------------------------ |
| GET    | `/prompts`     | [Retrieve all prompts](../endpoints/get-prompts.md) |
| POST   | `/prompts`     | [Save a prompt](../endpoints/post-prompts.md)                             |
| GET    | `/prompts/:id` | [Retrieve a prompt by ID](../endpoints/get-prompts-id.md)                  |
| PATCH  | `/prompts/:id` | [Update a prompt](../endpoints/patch-prompts-id.md)                           |
| DELETE | `/prompts/:id` | [Delete a prompt](../endpoints/delete-prompts-id.md)                           |
