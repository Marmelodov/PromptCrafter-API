---
layout: page
title: Save a prompt
---

# Tutorial: Save a prompt

Save a prompt by sending a `POST /prompts` request. Prompts are the API’s core resource: reusable instructions for generative-AI models such as GPT-4o, Gemini, and Claude. After saving a prompt, you can update it, test it, and log its outputs. This tutorial takes about 10 minutes to complete.

## Before you start

Make sure you have:

- A PromptCrafter user account and the bearer token you received when you logged in. For help signing up or logging in, see the [Quickstart](../quickstart.md).
- cURL or Postman.  

## Build the request

To save a new prompt, send a POST request to the following endpoint:

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
| `model`  | string           | Yes      | Name of the intended model (for example `gpt-4o`, `Claude 3 Sonnet`).     |
| `tags`   | array\<string\>  | No       | Optional keywords for grouping by topic, task, project, or library.    |

Example request body:

```json
{
  "title": "Historical Explanation for Students",
  "content": "Explain the significance of the Industrial Revolution to high school students using clear, accessible language. Include at least two key inventions and describe how these changes affected daily life in Europe and America.",
  "model": "Claude 3 Sonnet",
  "tags": ["history", "education", "explanation"]
}
```

## Send the request

### Using Postman

1. Navigate to **Send a new API request** and click **New Request**.
2. Set method to `POST`
3. Set URL to:  
   `https://promptcrafter-production.up.railway.app/prompts`
4. Click the **Authorization** tab:
   - In the **Type** dropdown, select `Bearer Token`.
   - In the **Token** field, enter the bearer token you received after logging in.
5. Click the **Headers** tab. Add a new header with:
   - Key: `Content-Type`
   - Value: `application/json`
6. In the **Body** tab:
   - Choose `raw` and `JSON`
   - Enter the request body
7. Click **Send** to submit the request.

### Using cURL

```bash
curl -X POST https://promptcrafter-production.up.railway.app/prompts \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Historical Explanation for Students",
    "content": "Explain the significance of the Industrial Revolution to high school students using clear, accessible language. Include at least two key inventions and describe how these changes affected daily life in Europe and America.",
    "model": "Claude 3 Sonnet",
    "tags": ["history", "education", "explanation"]
  }'
```

Replace `{your_token}` with your actual bearer token.

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

Note: the server adds the `_id`, `createdAt`, and `updatedAt` fields. Don't include them in the request body.

## Verify the prompt

If you want to verify that your prompt saved successfully, send a GET request using the prompt `_id` from the response body:

```bash
curl -H "Authorization: Bearer {your_token}" \
  https://promptcrafter-production.up.railway.app/prompts/{_id}
```

## What to do if the request doesn't work

Here are error codes you might encounter, what each means, and what you can do to fix them.

| Status | Example response body | Meaning | How to fix |
|--------|----------------------|---------|------------|
| **400 Bad Request** | `{ "message": "title, content, and model are required" }` | The request body is missing one or more required fields or contains malformed JSON. | Ensure `title`, `content`, and `model` are present and the JSON is valid. |
| **401 Unauthorized** | `{ "message": "Invalid or missing bearer token" }` | Authentication failed. | Pass `Authorization: Bearer {your_token}` and confirm the token has not expired. |
| **415 Unsupported Media Type** | `{ "message": "Content-Type must be application/json" }` | The server could not parse the body because the header is wrong or absent. | Add `Content-Type: application/json` to the request headers. |
| **404 Not Found** | `{ "message": "Route not found" }` | The path is incorrect. | Verify the endpoint (`/prompts`) and avoid trailing slashes or typos. |
| **500 Internal Server Error** | `{ "message": "Unexpected server error" }` | The server encountered an error while processing the request. | Retry later; if the error persists, contact support. |

## Next steps

Now that you can save prompts, learn how to [search prompts](search-prompts.md) or [log generated outputs](test-prompt.md).

## Related

[Prompt](../resources/prompt.md)  
[Save a prompt](../reference/endpoints/post-prompts.md): `POST /prompts`  
[Retrieve prompts](../reference/endpoints/get-prompts.md): `GET /prompts/:id`
