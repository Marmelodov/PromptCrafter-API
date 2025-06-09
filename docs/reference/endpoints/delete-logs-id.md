---
layout: page
---

# Delete a log by ID

Deletes a [`log`](../resources/log.md) by its unique ID. Users can only delete logs they own.

## URL

```text
{server_url}/logs/:id
```

## Method

`DELETE`

## Parameters

| Parameter name | Type   | Required | Description                   |
|----------------|--------|----------|-------------------------------|
| `id`           | string | Yes      | Unique identifier of the log. |

## Request headers

| Header name     | Required | Description                                |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | Bearer token used to authenticate the user |

## Request body

None

## Request example

```shell
curl -X DELETE {server_url}/logs/log3 \
  -H "Authorization: Bearer {your_token}"
```

## Response body

None

## Return status

| Status code  | Status       | Description                                        |
|--------------|--------------|----------------------------------------------------|
| 204          | No content   | Log deleted successfully                           |
| 401          | Unauthorized | Authentication token missing or invalid            |
| 403          | Forbidden    | Log does not belong to the authenticated user      |
| 404          | Not found    | Log with the specified ID was not found            |
| ECONNREFUSED | N/A          | Server is offline. Start the service and try again |

## Related

[Get all logs](get-logs.md): `GET /logs`  
[Create a log](post-logs.md): `POST /logs`  
[Retrieve logs for a specific prompt](get-logs-by-prompt.md): `GET /logs?promptId=...`
