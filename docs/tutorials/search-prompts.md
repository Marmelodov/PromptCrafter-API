# Tutorial: Search prompts

Search prompts with a GET /search?q= request. This endpoint performs a full-text, case-insensitive scan of every prompt’s title, content, model, and tags and returns matching prompts ordered by relevance. This tutorial takes about 10 minutes to complete.

## Before you start

Make sure you have:

- A PromptCrafter user account and the bearer token you received when you logged in. For help signing up or logging in, see the [Quickstart](../quickstart.md).
- cURL or Postman.  

## Build the request

To search your prompts, send a `GET` request to `/search` and include your search terms in the `q` query parameter.

### Endpoint format

```text
GET https://promptcrafter-production.up.railway.app/search?q={search_terms}
```

- Replace `{search_terms}` with one or more words.  
- Separate multiple words with either a plus sign (`+`) or `%20` (URL-encoded space).

**Examples**

| Search goal                              | URL example                                                          |
|------------------------------------------|-----------------------------------------------------------------------|
| Single word (“marketing”)                | `/search?q=marketing`                                                |
| Multi-word phrase (“email subject”)      | `/search?q=email+subject` or `/search?q=email%20subject`             |
| Prompts containing both “cold” and “email” | `/search?q=cold+email`                                               |

### Required query parameter

| Parameter | Where  | Required | Description                                      |
|-----------|--------|----------|--------------------------------------------------|
| `q`       | Query  | Yes      | Words or phrase to search for (full-text, case-insensitive) |

### Required header

- `Authorization: Bearer {your_token}`

## Send the request

### Using Postman

1. Navigate to **Send a new API request** and click **New Request**.
2. Set method to `GET`
3. Set URL to:  
   `https://promptcrafter-production.up.railway.app/search?q=email+subject`
4. Click the **Authorization** tab:
   - In the **Type** dropdown, select `Bearer Token`.
   - In the **Token** field, enter the bearer token you received after logging in.
5. Click **Send** to submit the request.

### Using cURL

```bash
curl -H "Authorization: Bearer {your_token}" \
     "https://promptcrafter-production.up.railway.app/search?q=email+subject"
```

## Response

A successful search returns **200 OK** and a JSON array of prompt objects:

```json
[
  {
    "_id": "prompt112",
    "title": "Email Subject Line",
    "content": "Write a catchy subject line for a product-launch email.",
    "model": "GPT-4o",
    "tags": ["marketing", "email"],
    "createdAt": "2025-06-10T14:23:00Z"
  },
  {
    "_id": "prompt87",
    "title": "Cold-email opener",
    "content": "Draft the first line of a cold email that grabs attention.",
    "model": "Claude 3 Sonnet",
    "tags": ["sales", "email"],
    "createdAt": "2025-05-22T09:11:00Z"
  }
]
```

Prompts appear in order of relevance: the first item matches the most words in `q`.

## What to do if the request doesn't work

Here are error codes you might encounter, what each means, and what you can do to fix them.

| Status | Example response body | Meaning | How to fix |
|--------|----------------------|---------|------------|
| **400 Bad Request** | `{ "message": "q parameter is required" }` | The query string is missing or empty. | Add `?q={search_terms}` to the URL. |
| **401 Unauthorized** | `{ "message": "Invalid or missing bearer token" }` | Authentication failed. | Add `Authorization: Bearer {your_token}` and confirm the token hasn’t expired. |
| **500 Internal Server Error** | `{ "message": "Unexpected server error" }` | The server encountered an error while processing the request. | Retry later; if the error persists, contact support. |

## Next steps

Learn how to [save a prompt](tutorials/create-prompt.md) or [log a generated result](tutorials/test-prompt.md).

## Related

[Prompt](reference/resources/prompt.md)  
[Save a prompt](reference/endpoints/post-prompts.md): `POST /prompts`  
[Retrieve prompts](reference/endpoints/get-prompts.md): `GET /prompts/`  
[Retrieve prompts by id](reference/endpoints/get-prompts-id.md): `GET /prompts/:id`  
