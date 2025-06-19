---
layout: page
---

# GET all prompts

Returns an array of [`prompt`](../resources/prompt.md) objects belonging to the user.

## URL

```bash
https://promptcrafter-production.up.railway.app/prompts
```

## Method

`GET`

## Parameters

None

## Request headers

| Header name     | Required | Description                                |
| --------------- | -------- | ------------------------------------------ |
| `Authorization` | Yes      | Bearer token used to authenticate the user |

## Request body

None

## Request example

```shell
curl -H "Authorization: Bearer {your_token}" https://promptcrafter-production.up.railway.app/prompts
```

## Response body

```json
[
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
]
```

## Return status

| Status code  | Status       | Description                                        |
| ------------ | ------------ | -------------------------------------------------- |
| 200          | Success      | Prompts returned successfully      |
| 401          | Unauthorized | Authentication token missing or invalid            |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Create a prompt](post-prompts.md): `POST /prompts`  
[Retrieve a prompt by ID](get-prompts-id.md): `GET /prompts/:id`  
[Update a prompt](patch-prompts-id.md): `PATCH /prompts/:id`  
[Delete a prompt](delete-prompts-id.md): `DELETE /prompts/:id`
