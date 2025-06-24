---
layout: page
title: Log a generated output
---

# Tutorial: Log a generated output

Record the output of a prompt test with a `POST /logs` request. A log contains the model’s output, the prompt ID, optional notes, and a quality score, giving you a permanent history of each test. After logging an output you can review it and compare the results of different tests. This tutorials take about 10 minutes to complete.

## Before you start

Make sure you have:

- A PromptCrafter user account and the bearer token you received when you logged in. For help signing up or logging in, see the [Quickstart](../quickstart.md).
- cURL or Postman.  

## Choose a prompt to test

To retrieve your saved prompts, send a `GET /prompts` request:

```bash
curl -H "Authorization: Bearer {your_token}" \
  https://promptcrafter-production.up.railway.app/logs
```

Find the `_id` of the prompt you want to test.

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

The request body contains the data for the output you are logging.

| Field       | Type            | Required | Description                                                                                 |
|-------------|-----------------|----------|---------------------------------------------------------------------------------------------|
| `promptId`  | string          | Yes      | ID of the prompt that generated this output.                                                |
| `output`    | string          | Yes      | Text returned by the AI model.                                                              |
| `modelUsed`     | string          | Yes      | Name of the model that generated the output (for example `gpt-4o`, `Claude 3 Sonnet`).      |
| `notes`     | string          | No       | Optional analyst comments or context for later review.                                      |
| `score`     | integer         | No       | Optional quality rating (for example 0–10)

Example request body:

```json
{
  "promptId": "prompt104",
  "output": "The Industrial Revolution transformed how people lived and worked by introducing inventions like the steam engine and the spinning jenny. These technologies allowed factories to produce goods faster, making everyday items cheaper and more accessible for families throughout Europe and America.",
  "notes": "Clear, accessible explanation for students.",
  "modelUsed": "Claude 3 Sonnet",
  "score": 8
}
```

## Send the request

### Using Postman

1. Navigate to **Send a new API request** and click **New Request**.
2. Set method to `POST`
3. Set URL to:  
   `https://promptcrafter-production.up.railway.app/logs`
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
curl -X POST https://promptcrafter-production.up.railway.app/logs \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "promptId": "prompt104",
    "output": "The Industrial Revolution transformed how people lived and worked by introducing inventions like the steam engine and the spinning jenny. These technologies allowed factories to produce goods faster, making everyday items cheaper and more accessible for families throughout Europe and America.",
    "notes": "Clear, accessible explanation for students.",
    "modelUsed": "Claude 3 Sonnet",
    "score": 8
  }'
```

## Response

If request is successful, the server returns a `201 Created` response like this:

```json
{
  "_id": "log104",
  "promptId": "prompt104",
  "output": "The Industrial Revolution transformed how people lived and worked by introducing inventions like the steam engine and the spinning jenny. These technologies allowed factories to produce goods faster, making everyday items cheaper and more accessible for families throughout Europe and America.",
  "notes": "Clear, accessible explanation for students.",
  "modelUsed": "Claude 3 Sonnet",
  "score": 8,
  "createdAt": "2024-06-20T13:13:00Z"
}
```

Note: the server adds the `_id`, `createdAt`, and `updatedAt` fields. Don't include them in the request body.

## What to do if the request doesn't work

Here are error codes you might encounter, what each means, and what you can do to fix them.

| Status | Example response body | Meaning | How to fix |
|--------|----------------------|---------|------------|
| **400 Bad Request** | `{ "message": "promptId, output, and model are required" }` | One or more required fields are missing or the JSON is malformed. | Include `promptId`, `output`, and `model`, and make sure the body is valid JSON. |
| **401 Unauthorized** | `{ "message": "Invalid or missing bearer token" }` | Authentication failed. | Add `Authorization: Bearer {your_token}` and confirm the token hasn’t expired. |
| **403 Forbidden** | `{ "message": "You do not own this prompt" }` | You authenticated, but you lack permission to log against this prompt. | Verify that the prompt belongs to your account. |
| **404 Not Found** | `{ "message": "Prompt not found" }` | The server can’t find the prompt referenced by `promptId`. | Check that the `promptId` is correct and still exists. |
| **415 Unsupported Media Type** | `{ "message": "Content-Type must be application/json" }` | The server couldn’t parse the body because the header is wrong or absent. | Add `Content-Type: application/json` to the request headers. |
| **500 Internal Server Error** | `{ "message": "Unexpected server error" }` | The server encountered an error while processing the request. | Retry later; if the error persists, contact support. |

## Next steps

Learn how to [view all logs](view-logs.md) or [search prompts](search-prompts.md).

## Related

[Log](../reference/resources/log.md)  
[Prompt](../reference/resources/prompt.md)  
[Log a generated output](../reference/endpoints/post-logs.md): `POST /logs`  
[Retrieve all logs](../reference/endpoints/get-logs.md): `GET /logs`  
