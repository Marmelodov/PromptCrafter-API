# Tutorial: View logs

Retrieve prompt test logs by sending a `GET /logs` request. Each log records a model's `output`, linking it to the `promptId` that produced it. Reviewing logs allows you to analyze and compare results across tests, identify performance patterns, and systematically improve your prompts. You can:

- **Analyze chatbot performance:** Retrieve logs for a specific `promptId` used by a chatbot to see a complete history of its responses to users, then analyze the scores and notes to find areas for improvement.
- **Compare A/B test results:** Fetch logs for two different prompt versions (e.g., `prompt-A` and `prompt-B`) to compare their average scores and determine which is more effective.
- **Monitor for quality degradation:** After modifying a critical prompt, retrieve its most recent logs to ensure that its performance hasn't declined unexpectedly.

This tutorial takes about five minutes to complete.

## Prerequisites

To follow this tutorial, you need:

- **Your JWT token:** All requests require a Bearer token for authentication. If you don't have a token, complete the authentication steps in the [Quickstart guide](../quickstart.md) first.
- **At least one saved log:** You need to have logged at least one output. If you haven't, complete the [Log a generated output tutorial](test-prompt.md) first.
- **An HTTP client:** This tutorial provides examples for both cURL and Postman.
    - If you're using Postman, import the [PromptCrafter Postman Collection](postman.md) to follow along.

## Build the request

To view logs, send a `GET` request to the `/logs` endpoint. You can either retrieve all logs associated with your account or filter them to see only the logs for a specific prompt.

### Headers

Add the following header to your request:

- `Authorization: Bearer {your_token}` authenticates you as the user. Replace `{your_token}` with the bearer token you received after logging in.

### Query parameters

To retrieve logs for a single prompt, include the following optional query parameter:

| Field      | Type   | Required | Description                                               |
|------------|--------|----------|-----------------------------------------------------------|
| `promptId` | string | No       | The unique `_id` of a prompt to filter the logs by. |

If you omit the `promptId` parameter, the API returns all logs for your account.

## Send the request

<details>
<summary>cURL</summary>

To make the cURL commands cleaner, set shell variables for the base URL and your token. This avoids repeating them in every request.

```bash
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token
```

**To retrieve all logs:**

```bash
curl -X GET $BASE_URL/logs \
  -H "Authorization: Bearer $TOKEN"
```

**To retrieve logs for a specific prompt:**

First set a variable for the prompt's ID.

```bash
PROMPT_ID="your-prompt-id-here" # Replace with a real prompt ID
```

Now send the request with the `promptId` query parameter.

```bash
curl -X GET "$BASE_URL/logs?promptId=$PROMPT_ID" \
  -H "Authorization: Bearer $TOKEN"
```

</details>

<details>
<summary>Postman</summary>

If you've imported the PromptCrafter Postman Collection, sending the request is simple. 

- **To retrieve all logs:** In the **Logs** folder, select the **Retrieve all logs** request and click **Send**.
- **To retrieve logs for a specific prompt:**
    1. In the **Logs** folder, select the **Retrieve logs by prompt** request.
    2. In the **Params** tab, replace the `{{promptId}}` variable with the ID of the prompt you want to see logs for.
    3. Click **Send**. The collection automatically uses the `{{token}}` variable set during login, so you don't need to configure authorization headers manually.

</details>

## Response

A successful request returns a `200 OK` status and a JSON array of log objects.

If you request all logs, the response looks like this:

```json
[
  {
    "_id": "log104",
    "promptId": "prompt104",
    "output": "The Industrial Revolution transformed how people lived and worked...",
    "modelUsed": "Claude 3 Sonnet",
    "notes": "Clear, accessible explanation for students. Good tone.",
    "score": 8,
    "createdAt": "2024-06-20T13:13:00Z"
  },
  {
    "_id": "log105",
    "promptId": "prompt102",
    "output": "The e-bike is fantastic! Its long battery life and smooth pedal assist make my commute a joy. I especially love the integrated lights for safety.",
    "modelUsed": "Grok-3 Beta",
    "notes": "Good review, meets all prompt criteria.",
    "score": 9,
    "createdAt": "2024-06-20T14:25:00Z"
  }
]
```

If you filter by `promptId`, the array contains only logs matching that ID. If there's no log for that prompt, the API returns an empty array `[]`.

## What to do if the request doesn't work

Here are issues you might encounter when retrieving logs and how to resolve them.

| Status | Example response | Cause | Solution |
|--------|------------------|--------|----------|
| **400 Bad Request** | `{"error": "`promptId` missing or malformed"}` | The `promptId` query parameter was included but had no value, or the value was invalid. | Ensure the `promptId` is a valid string. If you don't want to filter, remove the parameter entirely. |
| **401 Unauthorized** | `{"error": "Authentication token is missing"}` | The `Authorization` header is missing. | Include `Authorization: Bearer {your_token}` in your request headers with a valid token. |
| **401 Unauthorized** | `{"error": "Authentication token is expired or invalid"}` | The Bearer token is expired or malformed. | Log in again to obtain a new token and update your request. |
| **403 Forbidden** | `{"error": "User is not authorized to view logs for this prompt."}` | You are filtering by a `promptId` that belongs to another user. | Verify the `promptId` is correct and belongs to a prompt you own. |
| **404 Not Found** | `{"error": "No logs found for the specified prompt ID."}` | The prompt ID does not exist, or it exists but has no associated logs. | Double-check the `promptId`. If it is correct, create a log for it first. |
| **500 Internal Server Error** | `{"error": "An unexpected server error occurred"}` | An error occurred on the server. | Retry the request after a short wait. If the error persists, contact support. |

## Next steps

Now that you can review your test history, use that data to improve your prompts.

- Learn how to [Update a prompt](tutorials/update-prompt.md) based on insights from your logs.
- Use the [Search prompts tutorial](tutorials/search-prompts.md) to find related prompts to test.
- For complete details on parameters and endpoints, see the [log resource](reference/resources/log.md) and the [`GET /logs`](reference/endpoints/get-logs.md) endpoint documentation.
