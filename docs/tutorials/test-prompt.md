# Tutorial: Log a generated output

Log a generated output by sending a `POST /logs` request. Logs are the API's primary resource for evaluating prompt performance, linking a specific prompt (`promptId`) to a model's generated `output` along with contextual `notes` and a `score`. Systematically logging outputs enables you to:

- **Refine a customer support chatbot:** Log chatbot responses to common user queries, then score them for accuracy and helpfulness to identify which prompts need improvement.
- **A/B test marketing copy:** Test two versions of a prompt that generates e-commerce product descriptions. Log the outputs from each, score them based on engagement metrics, and use the data to choose the better-performing prompt.
- **Create a regression test suite:** After updating a critical prompt, run it against a set of test cases and log the outputs. Compare new logs to previous ones to ensure the prompt's performance hasn't degraded.

This tutorial takes about ten minutes to complete.

## Prerequisites

To follow this tutorial, you need:

*   **Your JWT token:** All requests require a Bearer token for authentication. If you don't have one, complete the authentication steps in the [Quickstart](../quickstart.md) first.
*   **A saved prompt's ID:** You need the `_id` of a prompt you've already saved. If you don't have one, complete the [Save a prompt tutorial](create-prompt.md).
*   **An HTTP client:** This tutorial provides examples for both cURL and Postman.
    *   If you're using Postman, import the [PromptCrafter Postman Collection](postman.md) to follow along easily.

## Build the request

To log a new output, send a `POST` request to the following endpoint:

```text
https://promptcrafter-production.up.railway.app/logs
```

The request includes two headers and a JSON-formatted request body.

### Headers

Add the following headers to your request:

- `Authorization: Bearer {your_token}` authenticates you as the user. Replace `{your_token}` with the bearer token you received after logging in.
- `Content-Type: application/json` tells the server to expect a JSON object in the request body.

### Request body

The request body contains the data from your prompt test.

| Field       | Type    | Required | Description                                                                    |
|-------------|---------|----------|--------------------------------------------------------------------------------|
| `promptId`  | string  | Yes      | The unique `_id` of the prompt that generated this output.                       |
| `output`    | string  | Yes      | The text returned by the AI model.                                             |
| `modelUsed` | string  | Yes      | Name of the model that generated the output (e.g., `gpt-4o`, `Claude 3 Sonnet`). |
| `notes`     | string  | No       | Optional comments or context for later review.                                 |
| `score`     | integer | No       | Optional quality rating (e.g., on a scale of 0–10).                           |

**Example request body:**

```json
{
  "promptId": "prompt104",
  "output": "The Industrial Revolution transformed how people lived and worked by introducing inventions like the steam engine and the spinning jenny. These technologies allowed factories to produce goods faster, making everyday items cheaper and more accessible for families throughout Europe and America.",
  "modelUsed": "Claude 3 Sonnet",
  "notes": "Clear, accessible explanation for students. Good tone.",
  "score": 8
}
```

## Send the request

<details>
<summary>cURL</summary>

To make the cURL commands cleaner, set shell variables for the base URL, your token, and the prompt ID. This avoids repeating them in every request.

```bash
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token
PROMPT_ID="prompt104" # Replace with the ID of your prompt
```

Now send the request:

```bash
curl -X POST $BASE_URL/logs \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "promptId": "'"$PROMPT_ID"'",
    "output": "The Industrial Revolution transformed how people lived and worked by introducing inventions like the steam engine and the spinning jenny. These technologies allowed factories to produce goods faster, making everyday items cheaper and more accessible for families throughout Europe and America.",
    "modelUsed": "Claude 3 Sonnet",
    "notes": "Clear, accessible explanation for students. Good tone.",
    "score": 8
  }'
```

After a successful response, you can save the log's `_id` to a variable for future use:

```bash
LOG_ID="log104"  # Replace with the _id from your response
```

</details>

<details>
<summary>Postman</summary>

If you have imported the PromptCrafter Postman Collection, sending the request is simple. The collection is pre-configured to handle authentication for you.

1.  In the **Logs** folder, select the **Log a generated output** request.
2.  In the **Body** tab, modify the pre-filled JSON with your test data, ensuring you replace the example `promptId` with the ID of a prompt you own.
3.  Click **Send**. The collection automatically uses the `{{token}}` variable set during login, so you don't need to configure authorization headers manually.

</details>

A successful request returns a `201 Created` status and the full log object, including its unique server-generated `_id`.

## Response

If your request is successful, the server returns a `201 Created` status code and a response body that looks like this:

```json
{
  "_id": "log104",
  "promptId": "prompt104",
  "output": "The Industrial Revolution transformed how people lived and worked by introducing inventions like the steam engine and the spinning jenny. These technologies allowed factories to produce goods faster, making everyday items cheaper and more accessible for families throughout Europe and America.",
  "modelUsed": "Claude 3 Sonnet",
  "notes": "Clear, accessible explanation for students. Good tone.",
  "score": 8,
  "createdAt": "2024-06-20T13:13:00Z"
}
```

**Note:** The server adds the `_id` and `createdAt` fields. Do not include them in your request body.

## Verify the log

To confirm your log was saved successfully, send a `GET` request to retrieve all logs for that prompt using the `promptId`.

<details>
<summary>cURL</summary>

Use the variables you set earlier.

```bash
curl -X GET "$BASE_URL/logs?promptId=$PROMPT_ID" \
  -H "Authorization: Bearer $TOKEN"
```

</details>

<details>
<summary>Postman</summary>

Use the **Retrieve logs by prompt** request in the `Logs` folder. Ensure the `promptId` in the query parameters matches your prompt's `_id`, then click **Send**.

</details>

Expect a `200 OK` status with a JSON array containing the log object you just created. This confirms your test result is stored and ready for analysis.

## What to do if the request doesn't work

Here are issues you might encounter when logging outputs and how to resolve them.

| Status | Example response | Cause | Solution |
|--------|------------------|--------|----------|
| **400 Bad Request** | `{"error": "Required fields are missing or invalid"}` | Missing required fields (`promptId`, `output`, `modelUsed`) or invalid JSON syntax. | Verify all required fields are present and that the JSON is properly formatted. |
| **401 Unauthorized** | `{"error": "Authentication token is missing"}` | The `Authorization` header is missing. | Include `Authorization: Bearer {your_token}` in your request headers with a valid token. |
| **401 Unauthorized** | `{"error": "Authentication token is expired or invalid"}` | The bearer token is expired or malformed. | Log in again to obtain a new token and update your request. |
| **404 Not Found** | `{"error": "Prompt ID does not exist or is not accessible"}` | The `promptId` does not exist or belongs to another user. | Check that the `promptId` is correct and corresponds to a prompt you own. |
| **415 Unsupported Media Type** | `{"error": "Content-Type must be application/json"}` | The `Content-Type` header is missing or incorrect. | Add `Content-Type: application/json` to your request headers. |
| **500 Internal Server Error** | `{"error": "An unexpected server error occurred"}` | An error occurred on the server. | Retry the request after a short wait. If the error persists, contact support. |

## Next Steps

Now that you can log prompt outputs, you`re ready to build a data-driven evaluation process.

- Try the [Search prompts tutorial](tutorials/search-prompts.md) to find prompts by keyword.  
- Learn how to [Update a prompt](tutorials/update-prompt.md) based on the insights from your logged tests.  
- For complete details on endpoints and para, see the [Log resource](reference/resources/log.md) and the [`POST /logs`](reference/endpoints/post-logs.md) endpoint documentation.
