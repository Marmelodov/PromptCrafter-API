# Retrieve all logs

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
    "_id": "log106",
    "promptId": "prompt101",
    "output": "The business report finds that revenue increased 12% last quarter, driven by strong online sales. Recommendations include expanding digital marketing and improving supply chain efficiency. No significant risks were found, but leadership should monitor supplier stability. Immediate action is suggested to secure long-term supplier contracts.",
    "notes": "Summary is clear and actionable. Matches requirements.",
    "modelUsed": "GPT-4o",
    "score": 9,
    "createdAt": "2024-06-20T13:10:00Z"
  },
  {
    "_id": "log109",
    "promptId": "prompt103",
    "output": "Dear Ms. Lee, I sincerely apologize for the delay in your order. We take full responsibility for the inconvenience this caused. To make things right, we have expedited your shipment and included a discount on your next purchase. Thank you for your patience.",
    "notes": "Professional and empathetic. Offers compensation as requested.",
    "modelUsed": "GPT-4 Turbo",
    "score": 9,
    "createdAt": "2024-06-20T13:12:00Z"
  }
]
```

## Return status

| Status code | Status                 | Description                                           |
|-------------|------------------------|-------------------------------------------------------|
| 200         | Success                | Logs returned successfully.                           |
| 400         | Bad Request            | The request is malformed or contains invalid parameters. |
| 401         | Unauthorized           | Authentication token is missing, expired, or invalid. |
| 500         | Internal Server Error  | An unexpected server error occurred.                  |

## Related

[Log a generated output](post-logs.md): `POST /logs`  
[Retrieve logs for a specific prompt](get-logs-by-prompt.md): `GET /logs?promptId=...`  
[Delete a log](delete-logs-id.md): `DELETE /logs/:id`
