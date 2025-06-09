---
layout: page
---

# Update a prompt by ID

Updates a [`prompt`](../resources/prompt.md) by its unique ID. Users can only update prompts they own. Only the fields included in the request body will be modified.

## URL

```text
{server_url}/prompts/:id
```

## Method

`PATCH`

## Parameters

| Parameter name | Type   | Required | Description                       |
|----------------|--------|----------|-----------------------------------|
| `id`           | string | Yes      | Unique identifier of the prompt. |

## Request headers

| Header name     | Required | Description                                |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | Bearer token used to authenticate the user |
| `Content-Type`  | Yes      | Must be set to `application/json`          |

## Request body

A JSON object with one or more fields to update. All fields are optional.

```json
{
  "title": "Updated title",
  "content": "Updated prompt content.",
  "model": "ChatGPT",
  "tags": ["updated", "tags"]
}
```

## Request example

```shell
curl -X PATCH {server_url}/prompts/prompt2 \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Updated title",
    "content": "Updated prompt content.",
    "model": "ChatGPT",
    "tags": ["updated", "tags"]
  }'
```

## Response body

The updated `prompt` object.

```json
{
  "_id": "prompt2",
  "ownerId": "user2",
  "title": "Updated title",
  "content": "Updated prompt content.",
  "model": "ChatGPT",
  "tags": ["updated", "tags"],
  "createdAt": "2024-02-02T09:00:00Z",
  "updatedAt": "2024-02-10T15:30:00Z"
}
```

## Return status

| Status code  | Status       | Description                                        |
|--------------|--------------|----------------------------------------------------|
| 200          | Success      | Prompt updated successfully                        |
| 400          | Bad request  | Submitted data is invalid                          |
| 401          | Unauthorized | Authentication token missing or invalid            |
| 403          | Forbidden    | Prompt does not belong to the authenticated user   |
| 404          | Not found    | Prompt with the specified ID was not found         |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Get all prompts](get-prompts.md): `GET /prompts`  
[Create a prompt](post-prompts.md): `POST /prompts`  
[Retrieve a prompt by ID](get-prompts-id.md): `GET /prompts/:id`  
[Delete a prompt](delete-prompts-id.md): `DELETE /prompts/:id`
