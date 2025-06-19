---
layout: page
---

# Create a log

Creates a new [`log`](../resources/log.md) to record the output of a tested prompt. The log is owned by the user and linked to a [`prompt`](../resources/prompt.md).

## URL

```text
https://promptcrafter-production.up.railway.app/logs
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

A JSON object representing the new log. All fields are required except `notes` and `score`.

```json
{
  "promptId": "prompt3",
  "output": "Ready to Upgrade? Discover the Future of Tech.",
  "notes": "Engaging and clear subject line.",
  "modelUsed": "Claude",
  "score": 7
}
```

## Request example

```shell
curl -X POST https://promptcrafter-production.up.railway.app/logs \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "promptId": "prompt3",
    "output": "Ready to Upgrade? Discover the Future of Tech.",
    "notes": "Engaging and clear subject line.",
    "modelUsed": "Claude",
    "score": 7
  }'
```

## Response body

The newly created `log` object.

```json
{
  "_id": "log3",
  "promptId": "prompt3",
  "output": "Ready to Upgrade? Discover the Future of Tech.",
  "notes": "Engaging and clear subject line.",
  "modelUsed": "Claude",
  "score": 7,
  "createdAt": "2024-03-03T13:00:00Z"
}
```

## Return status

| Status code  | Status       | Description                                        |
|--------------|--------------|----------------------------------------------------|
| 201          | Created      | Log created successfully                           |
| 400          | Bad request  | Required fields are missing or invalid             |
| 401          | Unauthorized | Authentication token missing or invalid            |
| 404          | Not found    | Prompt ID does not exist or is not accessible      |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Get all logs](get-logs.md): `GET /logs`  
[Retrieve logs for a specific prompt](get-logs-by-prompt.md): `GET /logs?promptId=...`  
[Delete a log](delete-logs-id.md): `DELETE /logs/:id`
