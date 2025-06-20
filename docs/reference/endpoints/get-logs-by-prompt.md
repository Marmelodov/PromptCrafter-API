---
layout: page
---

# Retrieve logs for a specific prompt

Returns an array of [logs](../resources/log.md) recording the outputs of a specified prompt.

## URL

```text
https://promptcrafter-production.up.railway.app/logs?promptId={prompt_id}
```

## Method

`GET`

## Parameters

| Parameter name | Type   | Required | Description                      |
|----------------|--------|----------|----------------------------------|
| `promptId`     | string | Yes      | The unique ID of the prompt whose logs are to be retrieved |

## Request headers

| Header name     | Required | Description                                |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | Bearer token used to authenticate the user |

## Request body

None

## Request example

```shell
curl -H "Authorization: Bearer {your_token}" \
  "https://promptcrafter-production.up.railway.app/logs?promptId=prompt101"
```

## Response body

A JSON array of `log` objects associated with the specified prompt:

```json
[
  {
    "_id": "log106",
    "promptId": "prompt101",
    "output": "The business report finds that revenue increased 12% last quarter, driven by strong online sales. Recommendations include expanding digital marketing and improving supply chain efficiency. No significant risks were found, but leadership should monitor supplier stability. Immediate action is suggested to secure long-term supplier contracts.",
    "notes": "Summary is clear and actionable. Matches requirements.",
    "modelUsed": "GPT-4o",
    "score": 9,
    "createdAt": "2024-06-20T13:10:00Z"
  },
]
```

## Return status

| Status code  | Status       | Description                                            |
|--------------|--------------|--------------------------------------------------------|
| 200          | Success      | Logs returned successfully                             |
| 400          | Bad request  | `promptId` missing or malformed                        |
| 401          | Unauthorized | Authentication token missing or invalid               |
| 403          | Forbidden    | User is not authorized to view logs for this prompt    |
| 404          | Not found    | No logs found for the specified prompt ID              |
| 500          | Server error | An unexpected error occurred on the server             |

## Related

[Retrieve all logs](get-logs.md): `GET /logs`  
[Log a generated output](post-logs.md): `POST /logs`  
[Delete a log](delete-logs-id.md): `DELETE /logs/:id`  
