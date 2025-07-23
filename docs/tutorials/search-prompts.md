# Tutorial: Search for prompts

This guide shows you how to search your prompt library. Use `GET /search` to performs a case-insensitive search across the `title`, `content`, and `tags` of all prompts you own, making it easy to find the exact instructions you need. Systematically searching your library allows you to:

- **Find and reuse existing work:** A teammate needs to draft an apology email. They can search for `customer apology email` to find and adapt a high-quality prompt someone else has already perfected.
- **Discover prompts for new tasks:** You're building a new feature that summarizes articles. Search your team's library for `summary` or `tldr` to find relevant prompts to use as a starting point.
- **Manage prompt versions:** Your team uses version tags like `v1` and `v2`. Search for prompts tagged with `v1` to quickly identify older versions that may need to be updated or archived as part of your maintenance process.

This tutorial takes about five minutes to complete.

## Prerequisites

To follow this tutorial, you need:

- **Your JWT token:** All requests require a Bearer token for authentication. If you don't have one, complete the authentication steps in the [Quickstart guide](../quickstart.md) first.
- **Saved prompts:** You should have at least one saved prompt in your library. If you need to create one, complete the [Save a prompt tutorial](create-prompt.md).
- **An HTTP client:** This tutorial provides examples for both cURL and Postman.
    - If you're using Postman, import the [PromptCrafter Postman Collection](postman.md) to follow along easily.

## Build the request

Search your prompts by sending a `GET` request to this endpoint:

```text
https://promptcrafter-production.up.railway.app/search
```

The `GET` request includes an authorization header and a query parameter with your search term.

### Headers

This header is required:

- `Authorization: Bearer {your_token}` authenticates you as the user. Replace `{your_token}` with the bearer token you received after logging in.

### Query parameters

The request requires a single query parameter to specify what you're looking for.

| Parameter | Type   | Required | Description                                                            |
|-----------|--------|----------|------------------------------------------------------------------------|
| `q`       | string | Yes      | The search term to match against prompt titles, content, and tags.     |

The search is case-insensitive and matches partial words; for example, a query for "email" finds prompts containing "emails" or "emailing." To search for multiple words, separate them with `+` or `%20` (e.g., `?q=product+review`). Multiple words are treated as separate search terms that can appear anywhere in the prompt's searchable fields.

## Send the request

<details>
<summary>cURL</summary>

To make the cURL commands cleaner, set shell variables for the base URL and your token.

```bash
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token
```

Now send a request with your search term in the `q` query parameter.

```bash
# Search for prompts related to a single term ("python")
curl -X GET "$BASE_URL/search?q=python" \
  -H "Authorization: Bearer $TOKEN"

# Search for prompts related to multiple terms ("product marketing")
curl -X GET "$BASE_URL/search?q=product+marketing" \
  -H "Authorization: Bearer $TOKEN"
```

</details>

<details>
<summary>Postman</summary>

If you've imported the PromptCrafter Postman Collection, sending the request is simple.  

1. In the **Search** folder, select the **Search prompts** request.
2. In the **Params** tab, find the key `q`. In the **VALUE** column next to it, enter your search term (e.g., `python`).
3. Click **Send**. The collection automatically uses the `{{token}}` variable set during login, so you don't need to configure authorization headers manually.

</details>

## Response

A successful request returns a `200 OK` status and a JSON array of prompt objects that match your query, sorted by relevance.

```json
[
  {
    "_id": "prompt314",
    "ownerId": "user980",
    "title": "Python Function Generator",
    "content": "Write a Python function that takes a list of dictionaries representing users and returns a list of emails.",
    "model": "GPT-4.1",
    "tags": ["python", "code-generation", "developer"],
    "createdAt": "2025-06-20T13:22:12.000Z",
    "updatedAt": "2025-06-20T13:22:12.000Z"
  }
]
```

> **Understanding results ordering:** Prompts appear in order of relevance. The search algorithm considers matches in titles more relevant than matches in content or tags, so prompts with titles that match your query appear higher in the results.

If no prompts match your search term, the request returns a `200 OK` status with an empty array `[]`.

## What to do if the request doesn't work

Here are issues you might encounter when searching for prompts and how to resolve them.

| Status | Example response | Cause | Solution |
|--------|------------------|--------|----------|
| **400 Bad Request** | `{"error": "Missing or malformed query string"}` | The `q` query parameter is missing from the URL. | Add `?q=` followed by your search term to the end of the request URL. |
| **401 Unauthorized** | `{"error": "Authentication token is missing"}` | The `Authorization` header is missing. | Include `Authorization: Bearer {your_token}` in your request headers with a valid token. |
| **401 Unauthorized** | `{"error": "Authentication token is expired or invalid"}` | The bearer token is expired or malformed. | Log in again to obtain a new token and update your request. |
| **500 Internal Server Error** | `{"error": "An unexpected server error occurred"}` | An error occurred on the server. | Retry the request after a short wait. If the error persists, contact support. |

## Next Steps

Now that you can find prompts in your library, you are ready to use them to build and test solutions.

- Try the [Log a generated output tutorial](tutorials/test-prompt.md) to test a prompt you found.
- Learn how to [update a prompt](tutorials/update-prompt.md) with new content or tags.
- For complete details on parameters and endpoints, see the [Prompt resource](reference/resources/prompt.md) and the [`GET /search`](reference/endpoints/get-search.md) endpoint documentation.
