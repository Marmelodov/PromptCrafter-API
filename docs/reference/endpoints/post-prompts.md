# API Reference: Save a prompt

Creates a `prompt` object owned by the user.

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

A JSON object representing the new `prompt`. All fields except `tags` are required.

```json
{
  "title": "Positive Product Review Writer",
  "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  "model": "Grok-3 Beta",
  "tags": ["review", "product", "writing"]
}
```

## Example request

```shell
curl -X POST https://promptcrafter-production.up.railway.app/prompts \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
    "model": "Grok-3 Beta",
    "tags": ["review", "product", "writing"]
  }'
```

## Response body

The newly created `prompt` object.

```json
{
  "_id": "prompt102",
  "ownerId": "user5",
  "title": "Positive Product Review Writer",
  "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  "model": "Grok-3 Beta",
  "tags": ["review", "product", "writing"],
  "createdAt": "2024-06-20T13:01:00Z",
  "updatedAt": "2024-06-20T13:01:00Z"
}
```

## Return status

| Status code | Status                 | Description                                           |
|-------------|------------------------|-------------------------------------------------------|
| 201         | Created                | Prompt created successfully.                          |
| 400         | Bad Request            | Required fields are missing or invalid.               |
| 401         | Unauthorized           | Authentication token is missing, expired, or invalid. |
| 500         | Internal Server Error  | An unexpected server error occurred.                  |

## Related

[Retrieve all prompts](reference/endpoints/get-prompts.md): `GET /prompts`  
[Retrieve a prompt by ID](reference/endpoints/get-prompts-id.md): `GET /prompts/:id`  
[Update a prompt](reference/endpoints/patch-prompts-id.md): `PATCH /prompts/:id`  
[Delete a prompt](reference/endpoints/delete-prompts-id.md): `DELETE /prompts/:id`
