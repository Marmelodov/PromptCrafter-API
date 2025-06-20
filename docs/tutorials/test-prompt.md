---
layout: page
title: Test a Prompt
---

# Tutorial: Log a generated output

This tutorial shows how to test a saved prompt by logging its output. The tutorial takes about 5-10 minutes.

## Before you start

Before sending the API request, make sure you have:

- A PromptCrafter user account  
- A bearer token from logging in  
- Either cURL or Postman installed  

If you need help creating a user or prompt, see [Create a user](create-user.md) or [Create a prompt](create-a-prompt.md).

## Choose a prompt to test

To get your saved prompts, send a `GET /prompts` request:

```bash
curl -H "Authorization: Bearer {your_token}" \
  https://promptcrafter-production.up.railway.app/prompts
```

Find the `_id` of the prompt you want to test.

## Prepare the log data

The request body for a log includes the prompt ID and the output you want to save. You can also add optional notes and a score (1-10).

- `promptId`: the `_id` of the prompt being tested  
- `output`: the generated text or image description  
- `modelUsed`: the model that created the output  
- `notes`: (optional) your comments on the result  
- `score`: (optional) your rating of the output  

Example:

```json
{
  "promptId": "prompt2",
  "output": "A dreamlike mountain range under a violet sky.",
  "notes": "Great visual balance and creativity.",
  "modelUsed": "DALL·E",
  "score": 9
}
```

## Send the request

Use this endpoint:  
`https://promptcrafter-production.up.railway.app/logs`

Include these headers:

- `Authorization: Bearer {your_token}`
- `Content-Type: application/json`

### Using cURL

```bash
curl -X POST https://promptcrafter-production.up.railway.app/logs \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "promptId": "prompt2",
    "output": "A dreamlike mountain range under a violet sky.",
    "notes": "Great visual balance and creativity.",
    "modelUsed": "DALL·E",
    "score": 9
  }'
```

### Using Postman

1. Open Postman  
2. Set method to `POST`  
3. Set URL to:  
   `https://promptcrafter-production.up.railway.app/logs`
4. In the **Authorization** tab:  
   - Type: Bearer Token  
   - Token: `{your_token}`  
5. In the **Headers** tab:  
   - Key: `Content-Type`  
   - Value: `application/json`  
6. In the **Body** tab:  
   - Choose `raw` and `JSON`  
   - Paste the JSON from the example above  
7. Click **Send**

## Check the response

If request is successful, the server returns a `201 Created` response like this:

```json
{
  "_id": "log4",
  "promptId": "prompt2",
  "output": "A dreamlike mountain range under a violet sky.",
  "notes": "Great visual balance and creativity.",
  "modelUsed": "DALL·E",
  "score": 9,
  "createdAt": "2024-03-02T12:00:00Z"
}
```

To confirm that the log saved correctly, retrieve all logs:

```bash
curl -H "Authorization: Bearer {your_token}" \
  https://promptcrafter-production.up.railway.app/logs
```

## What to do if the request doesn't work

Use this table to check what went wrong:

| Status Code                | What it means                  | What to check                                               |
|---------------------------|--------------------------------|-------------------------------------------------------------|
| 400 Bad Request           | The request wasn't valid        | Make sure you included `promptId`, `output`, and `modelUsed` |
| 401 Unauthorized          | You're not logged in            | Check your bearer token for typos or expiration             |
| 403 Forbidden             | Not allowed                     | You may not own the prompt you're testing                   |
| 404 Not Found             | Prompt doesn't exist            | Check that the prompt ID is correct                         |
| 500 Internal Server Error | Server problem                  | Try again later or contact support                          |

## Next steps

Learn how to [view all logs](view-logs.md) or [search prompts](search-prompts.md) to build on your testing process.

## Related

- [Log](../reference/resources/log.md)
- [Prompt](../reference/resources/prompt.md)
- [Log a generated output](../reference/endpoints/post-logs.md): `POST /logs`  
- [Retrieve all logs](../reference/endpoints/get-logs.md): `GET /logs`  
