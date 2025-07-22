# Tutorial: Save a prompt

Save a prompt by sending a `POST /prompts` request. Prompts are the API's core resource, storing reusable instructions for generative AI models like GPT-4o and Claude. Build an organized, searchable library of saved prompts where you can update versions, test outputs across models, log performance with scores and notes, and integrate prompts into tools for automation or experimenting.  

**Things you can do with saved prompts:**  
- **Automate content creation**: Store a prompt for drafting blog posts in your marketing app, then retrieve it by ID to generate articles in the same style every time without rewriting instructions.  
- **Test prompt variations**: Tag related prompts (e.g., `sentiment-v1` and `sentiment-v2`) to run side-by-side tests in different AI models, tracking which version produces the most accurate sentiment analysis.  
- **Collaborate with teams**: Group prompts by topic in a shared library (e.g., "marketing" or "research"), so your team can reuse and update them with version history, eliminating the chaos of prompts buried in emails, docs, or personal notebooks.

This tutorial takes about ten minutes to complete.

## Before you start

Make sure you have:

- A PromptCrafter user account and the Bearer token you received when you logged in. For help signing up or logging in, see the [Quickstart](../quickstart.md).
- cURL or Postman. If you're using Postman, download and import the [PromptCrafter Postman Collection](postman.md).

This tutorial assumes you're familiar with the basics of authentication from the Quickstart. If you haven't already, follow those steps to get your token, which you need for all requests in this tutorial.  

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
  "title": "Historical Explanation for Students",
  "content": "Explain the significance of the Industrial Revolution to high school students using clear, accessible language. Include at least two key inventions and describe how these changes affected daily life in Europe and America.",
  "model": "Claude 3 Sonnet",
  "tags": ["history", "education", "explanation"]
}
```

## Send the request

<details>
<summary>cURL</summary>

To make the cURL commands cleaner, set shell variables for the base URL and your token. This avoids repeating them in every request.

```bash
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."  # Replace with your actual token
```

Now send the request:

```bash
curl -X POST $BASE_URL/prompts \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Historical Explanation for Students",
    "content": "Explain the significance of the Industrial Revolution to high school students using clear, accessible language. Include at least two key inventions and describe how these changes affected daily life in Europe and America.",
    "model": "Claude 3 Sonnet",
    "tags": ["history", "education", "explanation"]
  }'
```

After a successful response, save the prompt's `_id` to a variable so you can verify it saved correctly:

```bash
PROMPT_ID="prompt104"  # Replace with the _id from your response
```

</details>

<details>
<summary>Postman</summary>

If using the PromptCrafter Postman Collection, select the 'Save a prompt' request, replace the pre-filled example body with the details of your new prompt, and send. The Collection handles authentication automatically by capturing and setting the JWT token in the `{{token}}` variable after login.

<details>
<summary>Manual Postman Setup</summary>

1. Navigate to **Send a new API request** and click **New Request**.
2. Set method to `POST`.
3. Set URL to:  
   `https://promptcrafter-production.up.railway.app/prompts`
4. Click the **Authorization** tab:
   - In the **Type** dropdown, select `Bearer Token`.
   - In the **Token** field, enter the bearer token you received after logging in.
5. Click the **Headers** tab. Add a new header with:
   - Key: `Content-Type`
   - Value: `application/json`
6. In the **Body** tab:
   - Choose `raw` and `JSON`.
   - Enter the request body.
7. Click **Send** to submit the request.

</details>

</details>

A successful request returns a `201 Created` status and the full prompt object, including its unique server-generated `_id`. Use this ID to retrieve, update, or test the prompt later.

## Response

If your request is successful, the server returns a status code `201 Created` and a response body that looks like this:

```json
{
  "_id": "prompt104",
  "ownerId": "user7",
  "title": "Historical Explanation for Students",
  "content": "Explain the significance of the Industrial Revolution to high school students using clear, accessible language. Include at least two key inventions and describe how these changes affected daily life in Europe and America.",
  "model": "Claude 3 Sonnet",
  "tags": ["history", "education", "explanation"],
  "createdAt": "2025-06-20T13:03:00Z",
  "updatedAt": "2025-06-20T13:03:00Z"
}
```

**Note:** the server adds the `_id`, `createdAt`, and `updatedAt` fields. Don't include them in the request body.

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

## Next steps

- Search your library: [Search prompts](tutorials/search-prompts.md)
- Test and log outputs: [Log generated outputs](tutorials/test-prompt.md)
- Update an existing prompt: [Update a prompt](tutorials/update-prompt.md)
- Explore the full reference: [Prompt resource](reference/resources/prompt.md)

## Related

[Prompt](reference/resources/prompt.md)  
[Save a prompt](reference/endpoints/post-prompts.md): `POST /prompts`  
[Retrieve prompts](reference/endpoints/get-prompts.md): `GET /prompts/:id`
