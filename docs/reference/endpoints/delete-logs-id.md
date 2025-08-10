# API Reference: Delete a log

Deletes a `log` object by its unique ID. Users can only delete `log` objects they own.

## URL

```text
https://promptcrafter-production.up.railway.app/logs/:id
```

## Method

`DELETE`

## Parameters

| Parameter name | Type   | Required | Description                   |
|----------------|--------|----------|-------------------------------|
| `id`           | string | Yes      | Unique identifier of the `log`. |

## Request headers

| Header name     | Required | Value                                |
|-----------------|----------|--------------------------------------------|
| `Authorization` | Yes      | `Authorization: Bearer {your_token}` |

## Request body

None

## Example request

```shell
curl -X DELETE https://promptcrafter-production.up.railway.app/logs/log3 \
  -H "Authorization: Bearer {your_token}"
```

## Response body

None

## Return status

| Status code | Status                 | Description                                           |
|-------------|------------------------|-------------------------------------------------------|
| 204         | No Content             | Log deleted successfully.                             |
| 400         | Bad Request            | The request is malformed or the log ID format is invalid. |
| 401         | Unauthorized           | Authentication token is missing, expired, or invalid. |
| 403         | Forbidden              | Log does not belong to the authenticated user.        |
| 404         | Not Found              | No log found with the specified ID.                   |
| 500         | Internal Server Error  | An unexpected server error occurred.                  |

## Related

[Retrieve all logs](reference/endpoints/get-logs.md): `GET /logs`  
[Log a generated output](reference/endpoints/post-logs.md): `POST /logs`  
[Retrieve logs for a specific prompt](reference/endpoints/get-logs-by-prompt.md): `GET /logs?promptId=...`
