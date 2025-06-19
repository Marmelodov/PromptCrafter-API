---
layout: page
title: View Logs
---

# View logs

This tutorial shows how to view your saved logs using the PromptCrafter API. A log records the result of testing a prompt. Each log stores the generated output, the AI model used, and any notes or score you've added. Reviewing logs allows you to analyze and compare results of different tests. The tutorial takes 5-10 minutes to complete.

## Before you start

Before sending the API request, make sure you have:

- A PromptCrafter user account  
- A bearer token from logging in  
- At least one log saved using the `POST /logs` endpoint  
- Either cURL or Postman installed on your machine  

If you need help creating an account or logging in, see the [Quickstart](../quickstart.md) or the [Create a user](create-user.md) tutorial.

## Make the request

Use the endpoint `https://promptcrafter-production.up.railway.app/logs` and include the following request header:

- `Authorization: Bearer {your_token}`

### Using cURL

```bash
curl -H "Authorization: Bearer {your_token}" \
  https://promptcrafter-production.up.railway.app/logs
```

Replace `{your_token}` with your actual bearer token.

### Using Postman

1. Open Postman  
2. Set method to `GET`  
3. Set URL to:  
   `https://promptcrafter-production.up.railway.app/logs`  
4. In the **Authorization** tab:  
   - Type: Bearer Token  
   - Token: `{your_token}`  
5. Click **Send**

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
  }
  {
    "_id": "log87",
    "promptId": "prompt34",
    "output": "I am the ancient apple queen.",
    "notes": "This is a William Morris line.",
    "modelUsed": "GPT-4.1",
    "score": 4,
    "createdAt": "2025-03-03T13:14:00Z"
  }
]
```

Each object in the array is a log entry. The `promptId` field shows which prompt the log links to.

## What to do if the request doesn't work

The table below lists common errors and how to resolve them.

| Status Code                | What it means                     | What to check                                                 |
|---------------------------|-----------------------------------|---------------------------------------------------------------|
| 401 Unauthorized          | You're not logged in              | Make sure your bearer token is included and correct           |
| 403 Forbidden             | You don't have access             | You may be trying to view logs that belong to another user    |
| 404 Not Found             | The URL doesn't exist             | Check the URL for typos |
| 500 Internal Server Error | Something went wrong on the server | Try again later or contact support                            |

## Next steps

Now that you can view logs, learn how to [test a prompt](test-prompt.md) or [delete a log](../reference/endpoints/delete-log.md).

## Related

- [Retrieve logs](../reference/endpoints/get-logs-id.md)  
- [Test a prompt](../reference/endpoints/post-logs.md)  
- [Delete a log](../reference/endpoints/delete-logs-id.md)  
- [Log resource](../reference/resources/log.md)
