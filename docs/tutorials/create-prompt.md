---
layout: page
title: Create a Prompt
---

# Tutorial: Save a prompt

This tutorial walks through how to save a prompt to the PromptCrafter API with a POST request. Prompts are the core resource of the API: they store reusable instructions for generative AI models used in applications like ChatGPT, Gemini, and Claude. After your prompt is saved, you can test it, log its outputs, and update it as needed. The tutorial takes about 10-15 minutes to complete.

## Before you start

Before sending the API request, make sure you have:

- A PromptCrafter user account  
- A bearer token from logging in  
- Either cURL or Postman installed on your machine  

If you need help creating an account or logging in, see the [Quickstart](../quickstart.md) or the [Create a user](create-user.md) tutorial.

## Write the request body

The request body holds the information that defines your prompt. It includes four fields:

- `title`: A short name to help identify or organize the prompt  
- `content`: The prompt text sent to the AI model to generate a response  
- `model`: The name of the AI model to use (for example, `gpt-4` or `dall-e-3`)  
- `tags`: Optional keywords for grouping the prompt by topic, task, or project. Tags can also help organize prompts into libraries

Here's an example request body:

```json
{
  "title": "Historical Explanation for Students",
  "content": "Explain the significance of the Industrial Revolution to high school students using clear, accessible language. Include at least two key inventions and describe how these changes affected daily life in Europe and America.",
  "model": "Claude 3 Sonnet",
  "tags": ["history", "education", "explanation"]
}
```

## Make the request

Use the endpoint `https://promptcrafter-production.up.railway.app/prompts` and include the following headers in your request:
- `Authorization: Bearer {your_token}`
- `Content-Type: application/json`

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

### Using Postman

1. Open Postman
2. Set method to `POST`
3. Set URL to:  
   `https://promptcrafter-production.up.railway.app/prompts`
4. In the **Authorization** tab:
   - Type: Bearer Token
   - Token: `{your_token}`
5. In the **Headers** tab:
   - Key: `Content-Type`
   - Value: `application/json`
6. In the **Body** tab:
   - Choose `raw` and `JSON`
   - Paste the JSON from step 1
7. Click **Send**

## Check the response

If your request is successful, the server returns a status code `201 Created` and a response body like this:

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

> Note: the `_id`, `createdAt`, and `updatedAt` fields are added by the server. Do not include them in the request body.

To confirm that the prompt saved correctly, use the `_id` from the `201 Created` response to retrieve the prompt with a GET request:

```bash
curl -H "Authorization: Bearer {your_token}" \
  https://promptcrafter-production.up.railway.app/prompts/{_id}
```

## What to do if the request doesn't work

The table below lists common errors and how to resolve them.

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

- [Prompt](../resources/prompt.md)
- [Save a prompt](../references/post-prompts.md): `POST /prompts`
- [Retrieve prompts](../references/get-prompts.md): `GET /prompts/:id`
