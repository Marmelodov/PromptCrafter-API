# Tutorial: Update a prompt

Update an existing prompt by sending a `PATCH /prompts/{promptId}` request. This method allows you to modify specific fields of a prompt—like its `title`, `content`, `model`, or `tags`—without rewriting the entire resource. Keeping your prompts current is essential for iterative development and library maintenance. You can:

- **Refine prompt instructions:** After testing, you might find a small change to a prompt's `content` significantly improves output quality. You can update the text while leaving the title and tags unchanged.
- **Upgrade to a new model:** When a more advanced AI model is released, update the `model` field on your best prompts to take advantage of the latest technology.
- **Reorganize your library:** As projects evolve, change a prompt's `tags` to better reflect its current use case (e.g., changing a tag from `draft` to `production-ready`).

This tutorial takes about ten minutes to complete.

## Prerequisites

To follow this tutorial, you need:

- **Your JWT token:** All requests require a Bearer token for authentication. If you don't have one, complete the authentication steps in the [Quickstart guide](../quickstart.md) first.
- **A saved prompt's ID:** You need the `_id` of a prompt you've already saved. If you don't have one, complete the [Save a prompt tutorial](create-prompt.md) first.
- **An HTTP client:** This tutorial provides examples for both cURL and Postman.
    - If you're using Postman, import the [PromptCrafter Postman Collection](postman.md) to follow along.

## Build the request

To update a prompt, send a `PATCH` request to the following endpoint, replacing `{promptId}` with the ID of the prompt you want to modify:

```text
https://promptcrafter-production.up.railway.app/prompts/{promptId}
```

The request includes headers, a path parameter, and a JSON-formatted request body with only the fields you want to change.

### Headers

Add the following headers to your request:

- `Authorization: Bearer {your_token}` authenticates you as the user. Replace `{your_token}` with the bearer token you received after logging in.
- `Content-Type: application/json` tells the server to expect a JSON object in the request body.

### Request body

The request body should only contain the fields you want to update. All fields are optional. Any fields you omit remain unchanged.

| Field    | Type             | Required | Description                                                            |
|----------|------------------|----------|------------------------------------------------------------------------|
| `title`  | string           | No       | A new label to help identify or sort the prompt.                       |
| `content`| string           | No       | The revised text to be sent to the AI model.                           |
| `model`  | string           | No       | The name of a different model (e.g., `gpt-4o`, `Claude 3 Sonnet`).     |
| `tags`   | array&lt;string&gt;  | No       | A new set of keywords for organization.  |

**Example request body (updating the model and tags):**

```json
{
  "model": "GPT-4o",
  "tags": ["review", "product", "writing", "e-commerce"]
}
```

## Send the request

<details>
<summary>cURL</summary>

To make the cURL commands cleaner, set shell variables for the base URL, your token, and the prompt's ID.

```bash
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token
PROMPT_ID="your-prompt-id-here" # Replace with the ID of the prompt you want to update
```

Now send the `PATCH` request. This example updates the `model` and `tags` for the specified prompt.

```bash
curl -X PATCH "$BASE_URL/prompts/$PROMPT_ID" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "GPT-4o",
    "tags": ["review", "product", "writing", "e-commerce"]
  }'
```

</details>

<details>
<summary>Postman</summary>

If you've imported the PromptCrafter Postman Collection, sending the request is simple.  

1. In the **Prompts** folder, select the **Update a prompt** request.
2. In the **Variables** tab, paste the `_id` of the prompt you want to update into the `CURRENT VALUE` field for the `promptId` variable. The request URL automatically uses this value.
3. In the **Body** tab of the request, edit the JSON to include only the fields you wish to change.
4. Click **Send**. The collection automatically uses the `{{token}}` variable set during login, so you don't need to configure authorization headers manually.

</details>

## Response

A successful request returns a `200 OK` status and the full, updated prompt object. Notice that the `updatedAt` timestamp has changed to reflect when the modification occurred.

```json
{
  "_id": "prompt102",
  "ownerId": "user5",
  "title": "Positive Product Review Writer",
  "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  "model": "GPT-4o",
  "tags": ["review", "product", "writing", "e-commerce"],
  "createdAt": "2024-06-20T13:01:00Z",
  "updatedAt": "2025-07-21T10:30:00Z"
}
```

## Verify the update

To confirm your prompt was updated successfully, send a `GET` request to retrieve the prompt using its `_id`.

<details>
<summary>cURL</summary>

Use the variables you set earlier.

```bash
curl -X GET "$BASE_URL/prompts/$PROMPT_ID" \
  -H "Authorization: Bearer $TOKEN"
```

</details>

<details>
<summary>Postman</summary>

Use the **Retrieve a prompt by ID** request in the `Prompts` folder. The `promptId` variable should already be set from the previous step. Click **Send** and inspect the response to confirm your changes were applied.

</details>

The response shows the fields with their new values, confirming the update was successful.

## What to do if the request doesn't work

Here are issues you might encounter when updating prompts and how to resolve them.

| Status | Example response | Cause | Solution |
|--------|------------------|--------|----------|
| **400 Bad Request** | `{"error": "Submitted data is invalid."}` | The data in the request body is malformed (e.g., `tags` is not an array) or violates validation rules. | Check that your JSON is valid and that all field values meet the required format. |
| **401 Unauthorized** | `{"error": "Authentication token is expired or invalid."}` | The bearer token is missing, malformed, or has expired. | Include a valid `Authorization: Bearer {your_token}` header. If needed, log in again to get a new token. |
| **403 Forbidden** | `{"error": "Prompt does not belong to the authenticated user."}` | You are trying to update a prompt that you do not own. | Verify you are using the correct `promptId` for a prompt associated with your account. |
| **404 Not Found** | `{"error": "No prompt found with the specified ID."}` | The `promptId` in the URL does not exist in the database. | Double-check that the `promptId` is correct and has not been deleted. |
| **500 Internal Server Error** | `{"error": "An unexpected server error occurred"}` | An error occurred on the server. | Retry the request after a short wait. If the error persists, contact support. |

## Next Steps

Now that you can modify your prompts, you can manage your library and iterate on your work.

- Try the [Log a generated output tutorial](tutorials/test-prompt.md) to test your updated prompt and see if its performance has improved.
- Learn how to [View logs](tutorials/view-logs.md) to compare test results before and after your update.
- For complete details on parameters and endpoints, see the [prompt resource](reference/resources/prompt.md) and the [`PATCH /prompts/{promptId}`](reference/endpoints/patch-prompts-id.md) endpoint documentation.
