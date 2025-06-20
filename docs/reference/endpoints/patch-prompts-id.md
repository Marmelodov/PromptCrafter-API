---
layout: page
---

# Update a prompt

Updates a [`prompt`](../resources/prompt.md) by its unique ID. Users can only update prompts they own. Only the fields included in the request body will be modified.

## URL

```text
https://promptcrafter-production.up.railway.app/prompts/:id
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

A JSON object containing updated versions of one or more of the prompt's fields. All fields are optional.

```json
{    
  "model": "GPT-4 Turbo",
  "tags": ["email", "apology", "customer"]
}
```

## Request example

```shell
curl -X PATCH https://promptcrafter-production.up.railway.app/prompts/prompt103 \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{    
    "model": "GPT-4 Turbo",
    "tags": ["email", "apology", "customer"]
  }'
```

## Response body

The updated `prompt` object.

```json
{
  "_id": "prompt103",
  "ownerId": "user6",
  "title": "Email Apology Draft",
  "content": "Draft a professional but empathetic apology email to a client whose order was delayed. Clearly acknowledge the issue, accept responsibility, and offer a practical solution or compensation to rebuild trust and customer satisfaction.",
  "model": "GPT-4 Turbo",
  "tags": ["email", "apology", "customer"],
  "createdAt": "2024-06-20T13:02:00Z",
  "updatedAt": "2025-06-20T11:07:00Z"
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

[Save a prompt](post-prompts.md): `POST /prompts` 
[Retrieve all prompts](get-prompts.md): `GET /prompts`  
[Retrieve a prompt by ID](get-prompts-id.md): `GET /prompts/:id`  
[Delete a prompt](delete-prompts-id.md): `DELETE /prompts/:id`
