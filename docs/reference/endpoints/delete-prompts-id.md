---
layout: page
---

# Delete a prompt by ID

Deletes a [`prompt`](../resources/prompt.md) by its unique ID. Users can only delete prompts they own.

## URL

```text
https://promptcrafter-production.up.railway.app/prompts/:id
```

## Method

`DELETE`

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
curl -X DELETE https://promptcrafter-production.up.railway.app/prompts/prompt2 \
  -H "Authorization: Bearer {your_token}"
```

## Response body

None

## Return status

| Status code | Status                 | Description                                           |
|-------------|------------------------|-------------------------------------------------------|
| 204         | No Content             | Prompt deleted successfully.                          |
| 400         | Bad Request            | The request is malformed or the prompt ID format is invalid. |
| 401         | Unauthorized           | Authentication token is missing, expired, or invalid. |
| 403         | Forbidden              | Prompt does not belong to the authenticated user.     |
| 404         | Not Found              | No prompt found with the specified ID.                |
| 500         | Internal Server Error  | An unexpected server error occurred.                  |

## Related

[Retrieve all prompts](get-prompts.md): `GET /prompts`  
[Save a prompt](post-prompts.md): `POST /prompts`  
[Retrieve a prompt by ID](get-prompts-id.md): `GET /prompts/:id`  
[Update a prompt](patch-prompts-id.md): `PATCH /prompts/:id`
