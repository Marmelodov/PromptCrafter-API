---
layout: page
---

# GET all logs

Returns an array of [`log`](../resources/log.md) objects belonging to the user.  

## URL

```text
https://promptcrafter-production.up.railway.app/logs
```

## Method

`GET`

## Parameters

None

## Request headers

| Header name     | Required | Description                                |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | Bearer token used to authenticate the user |

## Request body

None

## Request example

```shell
curl -H "Authorization: Bearer {your_token}" https://promptcrafter-production.up.railway.app/logs
```

## Response body

```json
[
  {
    "_id": "log3",
    "promptId": "prompt3",
    "output": "Ready to Upgrade? Discover the Future of Tech.",
    "notes": "Engaging and clear subject line.",
    "modelUsed": "Claude",
    "score": 7,
    "createdAt": "2024-03-03T13:00:00Z"
  }
]
```

## Return status

| Status code  | Status       | Description                                        |
|--------------|--------------|----------------------------------------------------|
| 200          | Success      | Logs returned successfully                         |
| 401          | Unauthorized | Authentication token missing or invalid            |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Create a log](post-logs.md): `POST /logs`  
[Retrieve logs for a specific prompt](get-logs-by-prompt.md): `GET /logs?promptId=...`  
[Delete a log](delete-logs-id.md): `DELETE /logs/:id`
