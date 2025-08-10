# API Reference: Retrieve a prompt by ID

Retrieves a `prompt` object by its unique ID. Users can only access `prompt` objects they own.

## URL

```text
https://promptcrafter-production.up.railway.app/prompts/:id
```

## Method

`GET`

## Parameters

| Parameter name | Type   | Required | Description                       |
|----------------|--------|----------|-----------------------------------|
| `id`           | string | Yes      | Unique identifier of the `prompt`. |

## Request headers

| Header name     | Required | Value                               |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | `Authorization: Bearer {your_token}` |

## Request body

None

## Example request

```shell
curl -H "Authorization: Bearer {your_token}" https://promptcrafter-production.up.railway.app/prompts/104
```

## Response body

```json
{
  "_id": "prompt104",
  "ownerId": "user7",
  "title": "Historical Explanation for Students",
  "content": "Explain the significance of the Industrial Revolution to high school students using clear, accessible language. Include at least two key inventions and describe how these changes affected daily life in Europe and America.",
  "model": "Claude 3 Sonnet",
  "tags": ["history", "education", "explanation"],
  "createdAt": "2024-06-20T13:03:00Z",
  "updatedAt": "2024-06-20T13:03:00Z"
}
```

## Return status

| Status code | Status                 | Description                                           |
|-------------|------------------------|-------------------------------------------------------|
| 200         | Success                | Prompt returned successfully.                         |
| 400         | Bad Request            | The request is malformed or the prompt ID format is invalid. |
| 401         | Unauthorized           | Authentication token is missing, expired, or invalid. |
| 403         | Forbidden              | Prompt does not belong to the authenticated user.     |
| 404         | Not Found              | No prompt found with the specified ID.                |
| 500         | Internal Server Error  | An unexpected server error occurred.                  |

## Related

[Retrieve all prompts](reference/endpoints/get-prompts.md): `GET /prompts`  
[Save a prompt](reference/endpoints/post-prompts.md): `POST /prompts`  
[Update a prompt](reference/endpoints/patch-prompts-id.md): `PATCH /prompts/:id`  
[Delete a prompt](reference/endpoints/delete-prompts-id.md): `DELETE /prompts/:id`
