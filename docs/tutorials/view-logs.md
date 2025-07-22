# Tutorial: View logs

Retrieve prompt test logs with a `GET /logs` request. Each log stores the model’s output, the prompt ID, the model name, optional notes, and a score. Reviewing logs lets you analyze and compare results across tests. This tutorial takes about 5 minutes to complete.

## Before you start

Make sure you have:

- A PromptCrafter user account and the bearer token you received when you logged in. For help signing up or logging in, see the [Quickstart](../quickstart.md).
- cURL or Postman.  

## Make the request

Send a GET request to the following endpoint:

```text
https://promptcrafter-production.up.railway.app/logs
```

Include the following request header:

- `Authorization: Bearer {your_token}`

### Using Postman

1. Navigate to **Send a new API request** and click **New Request**.
2. Set method to `GET`
3. Set URL to:  
   `https://promptcrafter-production.up.railway.app/logs`
4. Click the **Authorization** tab:
   - In the **Type** dropdown, select `Bearer Token`.
   - In the **Token** field, enter the bearer token you received after logging in.
5. Click **Send** to submit the request.

### Using cURL

```bash
curl -H "Authorization: Bearer {your_token}" \
  https://promptcrafter-production.up.railway.app/logs
```

Replace `{your_token}` with your actual bearer token.

## Check the response

If your request is successful, the server returns a status code `200 OK` and a response body like this:

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
  },
  {
    "_id": "log87",
    "promptId": "prompt34",
    "output": "I am the ancient apple queen.",
    "notes": "This is a William Morris line.",
    "modelUsed": "GPT-4",
    "score": 4,
    "createdAt": "2025-03-03T13:14:00Z"
  }
]
```

Each object in the array is a log entry. The `promptId` field shows which prompt the log links to.

## What to do if the request doesn't work

Here are error codes you might encounter, what each means, and what you can do to fix them.

| Status | Example response body | Meaning | How to fix |
|--------|----------------------|---------|------------|
| **400 Bad Request** | `{ "message": "Invalid query parameter" }` | The query string is malformed or contains unsupported parameters. | Use `/logs` to fetch all logs, or `/logs?promptId=<id>` with a valid prompt ID. |
| **401 Unauthorized** | `{ "message": "Invalid or missing bearer token" }` | Authentication failed. | Add `Authorization: Bearer {your_token}` and confirm the token hasn’t expired. |
| **403 Forbidden** | `{ "message": "You do not own this prompt" }` | You authenticated, but the prompt specified by `promptId` belongs to another account. | Request logs only for prompts you own, or switch to an account that owns the prompt. |
| **404 Not Found** | `{ "message": "Prompt not found" }` | The server can’t find any prompt with the supplied `promptId`. | Verify the `promptId` value is correct and still exists. |
| **500 Internal Server Error** | `{ "message": "Unexpected server error" }` | The server encountered an error while processing the request. | Retry later; if the error persists, contact support. |

## Next steps

Learn how to [log a generated output](tutorials/test-prompt.md) or [save a prompt](tutorials/create-prompt.md).

## Related

[Log](reference/resources/log.md)  
[Retrieve all logs](reference/endpoints/get-logs.md): `GET /logs`  
[Retrieve logs for a specific prompt](reference/endpoints/get-logs-by-prompt.md) `GET /logs?promptId=`  
[Log a generated output](reference/endpoints/post-logs.md): `POST /logs`  
[Delete a log](reference/endpoints/delete-logs-id.md): `DELETE /logs`  
