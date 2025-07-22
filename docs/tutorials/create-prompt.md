# Tutorial: Save a prompt

Save a prompt by sending a `POST /prompts` request. Prompts are the API's core resource, storing reusable instructions for generative AI models like GPT-4o and Claude. Saving prompts allows you to build an organized, searchable prompt library with which you can:
- **Automate content creation**: Design prompts for a marketing app that draft blog posts in your brand’s voice, then retrieve them by ID to generate articles in the same style every time without rewriting instructions.  
- **Build a prompt evaluation suite**: Run systematic experiments to determine the best prompt for a job like sentiment analysis. Tag related prompts (e.g., `sentiment-v1` and `sentiment-v2`) to programmatically test them across AI models and measure which version produces the most accurate outputs.
- **Collaborate with teams**: Group prompts by topic in a shared library (e.g., "marketing" or "research"), so your team can reuse and update them with version history, eliminating the chaos of prompts buried in emails, docs, or personal notebooks.

This tutorial takes about ten minutes to complete.

## Prerequisites

To follow this tutorial, you need:

- **Your JWT token:** All requests require a Bearer token for authentication. If you don't have one, complete the authentication steps in the [Quickstart guide](../quickstart.md) first.
- **An HTTP client:** This tutorial provides examples for both cURL and Postman.
    - If you're using Postman, import the [PromptCrafter Postman Collection](postman.md) to follow along.

## Build the request

To save a new prompt, send a `POST` request to the following endpoint:

```text
https://promptcrafter-production.up.railway.app/prompts
```

The request includes two headers and a JSON-formatted request body.

### Headers

Add the following headers to your request:

- `Authorization: Bearer {your_token}` authenticates you as the user. Replace `{your_token}` with the bearer token you received after logging in.
- `Content-Type: application/json` tells the server to expect a JSON object in the request body.

### Request body

The request body contains the information that defines your prompt.

| Field    | Type             | Required | Description                                                            |
|----------|------------------|----------|------------------------------------------------------------------------|
| `title`  | string           | Yes      | Short label that helps you identify or sort the prompt.                |
| `content`| string           | Yes      | The text sent to the AI model.                                         |
| `model`  | string           | Yes      | Name of the intended model (for example `gpt-4o`, `Claude 3 Sonnet`).  |
| `tags`   | array&lt;string&gt;  | No       | Optional keywords for grouping by topic, task, project, or library.    |

**Example request body:**

```json
{
  "title": "Positive Product Review Writer",
  "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  "model": "Grok-3 Beta",
  "tags": ["review", "product", "writing"]
}
```

## Send the request

<details>
<summary>cURL</summary>

To make the cURL commands cleaner, set shell variables for the base URL and your token. This avoids repeating them in every request.

```bash
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token
```

Now send the request:

```bash
curl -X POST $BASE_URL/prompts \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
    "model": "Grok-3 Beta",
    "tags": ["review", "product", "writing"]
  }'
```

After a successful response, save the prompt's `_id` to a variable so you can verify it saved correctly:

```bash
PROMPT_ID="prompt102"  # Replace with the _id from your response
```

</details>

<details>
<summary>Postman</summary>

If you've imported the PromptCrafter Postman Collection, sending the request is simple.  

1. In the **Prompts** folder, select the **Save a prompt** request.
2. In the **Body** tab, modify the pre-filled JSON with your prompt's details.
3. Click **Send**. The collection automatically uses the `{{token}}` variable set during login, so you don't need to configure authorization headers manually.

</details>

A successful request returns a `201 Created` status and the full prompt object, including its unique server-generated `_id`. Use this ID to retrieve, update, or test the prompt later.

## Response

If your request is successful, the server returns a status code `201 Created` and a response body that looks like this:

```json
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
```

**Note:** the server adds the `_id`, `ownerId`, `createdAt`, and `updatedAt` fields. Don't include them in the request body.

## Verify the prompt

To confirm your prompt saved successfully, send a GET request using the prompt `_id` from the response:

<details>
<summary>cURL</summary>

Use the variables you set earlier.

```bash
curl -X GET $BASE_URL/prompts/$PROMPT_ID \
  -H "Authorization: Bearer $TOKEN"
```

</details>

<details>
<summary>Postman</summary>

Use the **Retrieve a prompt by ID** request in the `Prompts` folder. Update the `promptId` collection variable with your new `_id`, then click **Send**.

</details>

Expect a `200 OK` status with a response matching the object from the `POST` request, confirming your prompt is stored and ready to use. If you get `404 Not Found`, check that the `_id` is correct.  

## What to do if the request doesn't work

Here are issues you might encounter when saving prompts and what to do about them.

| Status | Example response | Cause | Solution |
|--------|------------------|--------|----------|
| **400 Bad Request** | `{"error": "Required fields are missing or invalid"}` | Missing required fields (`title`, `content`, `model`) or invalid JSON syntax. | Verify all required fields are present and JSON is properly formatted. |
| **401 Unauthorized** | `{"error": "Authentication token is missing"}` | Missing bearer token. | Include `Authorization: Bearer {your_token}` header with a valid token. |
| **401 Unauthorized** | `{"error": "Authentication token is expired or invalid"}` | Expired or invalid bearer token. | Log in again to obtain a new token and include it in the `Authorization` header. |
| **415 Unsupported Media Type** | `{"error": "Content-Type must be application/json"}` | Missing or incorrect Content-Type header. | Add `Content-Type: application/json` to request headers. |
| **404 Not Found** | `{"error": "Route not found"}` | Incorrect endpoint URL. | Verify the endpoint is `/prompts` with no trailing slash or typos. |
| **500 Internal Server Error** | `{"error": "An unexpected server error occurred"}` | Server-side processing error. | Retry the request; contact support if the error persists. |

## Next Steps

Now that you've saved your first prompt, you're ready to explore PromtCrafter further.

- Try the [Search prompts tutorial](tutorials/search-prompts.md) to find prompts by keyword.
- See the [Log a generated output tutorial](tutorials/test-prompt.md) to track prompt performance.
- For complete details on parameters and endpoints, see the [Prompt resource](reference/resources/prompt.md) and the [`POST /prompts`](reference/endpoints/post-prompts.md) endpoint documentation.
