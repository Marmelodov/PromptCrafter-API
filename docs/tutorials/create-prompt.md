---
layout: page
title: Save a prompt
---

# Tutorial: Save a prompt

This tutorial walks through how to save a prompt to PromptCrafter API with a POST request. Prompts are the core resource of the API: they store reusable instructions for generative AI models used in applications like ChatGPT, Gemini, and Claude. After saving your prompt, you can test it, log its outputs, and update it as needed. The tutorial takes about 5-15 minutes to complete.

## Before you start

Make sure you have:

- A PromptCrafter user account and the bearer token you received when you logged in. For help signing up or logging in, see the [Quickstart](../quickstart.md).
- cURL or Postman.  

## Build the request

To save a new prompt, send a POST request to the following endpoint:

```text
https://promptcrafter-production.up.railway.app/prompts
```

The request must include two headers and a JSON-formatted request body.

### Headers

Add the following headers to your request:

- `Authorization: Bearer {your_token}` authenticates you as the user. Replace `{your_token}` with the bearer token you received after logging in.  
- `Content-Type: application/json` tells the server to expect a JSON object in the request body.

### Request body

The request body holds the information that defines your prompt. It includes four fields:

- `title`: A short name to help identify or organize the prompt  
- `content`: The prompt text sent to the AI model to generate a response  
- `model`: The name of the AI model to use (for example, `gpt-4` or `dall-e-3`)  
- `tags`: Optional keywords for grouping the prompt by topic, task, or project. Tags can also help organize prompts into libraries

Example:

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
   - In the **Token** field, enter your bearer token (the one you received after logging in).
5. Click the **Headers** tab.  Add a new header with:
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

Note: The server adds the `_id`, `createdAt`, and `updatedAt` fields. Don't include them in the request body.

Congratulations, you've successfully saved your prompt. If you want to verify that you saved it, send a GET request using the prompt `_id` from the response body:

```bash
curl -H "Authorization: Bearer {your_token}" \
  https://promptcrafter-production.up.railway.app/prompts/{_id}
```

## What to do if the request doesn't work

The table below shows the error codes you might encounter, what each means, and what you can do to fix them.

| Status Code                | What it means                  | What to check                                             |
|---------------------------|--------------------------------|-----------------------------------------------------------|
| 400 Bad Request           | The request wasn't valid      | Make sure you included `title`, `content`, and `model` in the request body |
| 401 Unauthorized          | You're not logged in           | Check that your bearer token is valid and spelled correctly |
| 403 Forbidden             | Not allowed                    | This action isn't allowed for your account      |
| 404 Not Found             | The URL doesn't exist | Check the URL for typos                              |
| 415 Unsupported Media Type | The request format is wrong   | Make sure you set `Content-Type: application/json`        |
| 500 Internal Server Error | Something went wrong on the server | Try again later or contact support                        |

## Next steps

Now that you can save prompts, learn how to [search prompts](search-prompts.md) or [test prompts](test-prompt.md).

## Related

[Prompt](../resources/prompt.md)  
[Save a prompt](../references/post-prompts.md): `POST /prompts`  
[Retrieve prompts](../references/get-prompts.md): `GET /prompts/:id`
