---
layout: page
---

# Retrieve all prompts

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
    "_id": "prompt101",
    "ownerId": "user4",
    "title": "Executive Summary Generator",
    "content": "Read the following business report and generate a clear, well-organized executive summary highlighting the main findings, recommendations, and any critical risks or action items for decision-makers. Limit your summary to three concise paragraphs.",
    "model": "GPT-4o",
    "tags": ["business", "summary", "report"],
    "createdAt": "2024-06-20T13:00:00Z",
    "updatedAt": "2024-06-20T13:00:00Z"
  },
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
]
```

## Return status

| Status code  | Status       | Description                                        |
| ------------ | ------------ | -------------------------------------------------- |
| 200          | Success      | Prompts returned successfully      |
| 401          | Unauthorized | Authentication token missing or invalid            |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Save a prompt](post-prompts.md): `POST /prompts`  
[Retrieve a prompt by ID](get-prompts-id.md): `GET /prompts/:id`  
[Update a prompt](patch-prompts-id.md): `PATCH /prompts/:id`  
[Delete a prompt](delete-prompts-id.md): `DELETE /prompts/:id`
