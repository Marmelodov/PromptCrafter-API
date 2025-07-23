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

<!-- tabs:start -->

#### **cURL**

To make the cURL commands cleaner, set shell variables for the base URL and your token.

```bash
# Set shell variables for convenience
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token

# Search for prompts related to a single term ("python")
curl -G --data-urlencode "q=python" "$BASE_URL/search" \
  -H "Authorization: Bearer $TOKEN"

# Search for prompts related to multiple terms ("product marketing")
curl -G --data-urlencode "q=product marketing" "$BASE_URL/search" \
  -H "Authorization: Bearer $TOKEN"
```

#### **Postman**

If you've imported the PromptCrafter Postman Collection, sending the request is simple. The collection automatically handles authentication for you.

1.  In the **Search** folder, select the **Search prompts** request.
2.  In the **Params** tab, find the key `q`. In the **VALUE** column next to it, enter your search term (e.g., `python`).
3.  Click **Send**. The collection automatically uses the `{{token}}` variable set during login.

#### **Python**

<!-- tabs:start -->

##### **SDK**

```python
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError

# Replace with your actual JWT token
token = "your-jwt-goes-here"
client = PromptCrafterClient(token=token)

search_term = "python"

try:
    results = client.search_prompts(query=search_term)
    print(f"Found {len(results)} prompt(s) matching '{search_term}':")
    for prompt in results:
        print(f"  - ID: {prompt['_id']}, Title: {prompt['title']}")
except PromptCrafterAPIError as e:
    print(f"An API error occurred: {e}")
```

##### **Requests**

```python
import requests

# Replace with your actual JWT token
token = "your-jwt-goes-here"
base_url = "https://promptcrafter-production.up.railway.app"
search_term = "python"

# Set up headers and query parameters
headers = {"Authorization": f"Bearer {token}"}
params = {"q": search_term}

try:
    response = requests.get(f"{base_url}/search", headers=headers, params=params)
    response.raise_for_status() # Raise an exception for bad status codes

    results = response.json()
    print(f"Found {len(results)} prompt(s) matching '{search_term}':")
    for prompt in results:
        print(f"  - ID: {prompt['_id']}, Title: {prompt['title']}")
except requests.exceptions.RequestException as e:
    print(f"An HTTP error occurred: {e}")
```

<!-- tabs:end -->

#### **JavaScript**

<!-- tabs:start -->

##### **SDK**

```javascript
import PromptCrafterClient from './promptcrafter-client.js';

async function searchPrompts() {
    // Replace with your actual JWT token
    const token = 'your-jwt-goes-here';
    const client = new PromptCrafterClient({ token });
    const searchTerm = 'python';

    try {
        const results = await client.searchPrompts(searchTerm);
        console.log(`Found ${results.length} prompt(s) matching '${searchTerm}':`);
        results.forEach(prompt => {
            console.log(`  - ID: ${prompt._id}, Title: ${prompt.title}`);
        });
    } catch (error) {
        console.error("Failed to search prompts:", error.message);
    }
}

searchPrompts();
```

##### **Fetch**

```javascript
async function searchPrompts() {
    // Replace with your actual JWT token
    const token = "your-jwt-goes-here";
    const searchTerm = "python";

    // Construct the URL with the search query
    const url = new URL("https://promptcrafter-production.up.railway.app/search");
    url.searchParams.append('q', searchTerm);

    const headers = { 'Authorization': `Bearer ${token}` };

    try {
        const response = await fetch(url, { headers });
        const results = await response.json();

        if (!response.ok) {
            throw new Error(results.error || `HTTP error! Status: ${response.status}`);
        }

        console.log(`Found ${results.length} prompt(s) matching '${searchTerm}':`);
        results.forEach(prompt => {
            console.log(`  - ID: ${prompt._id}, Title: ${prompt.title}`);
        });
    } catch (error) {
        console.error("Failed to search prompts:", error.message);
    }
}

searchPrompts();
```

<!-- tabs:end -->

#### **Go**

```go
package main

import (
  "context"
  "fmt"
  "log"
  "time"
  "github.com/your-org/promptcrafter" // Replace with your actual import path
)

func main() {
    // Replace with your actual JWT token
  token := "your-jwt-goes-here"
  client := promptcrafter.NewClient(token)

    // Using a context with a timeout is a best practice for network requests.
  ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
  defer cancel()

  searchTerm := "python"
  results, err := client.SearchPrompts(ctx, searchTerm)
  if err != nil {
    log.Fatalf("Error searching prompts: %v", err)
  }

  fmt.Printf("Found %d prompt(s) matching '%s':\n", len(results), searchTerm)
  for _, prompt := range results {
    fmt.Printf("  - ID: %s, Title: %s\n", prompt.ID, prompt.Title)
  }
}
```

#### **Ruby**

```ruby
require 'promptcrafter'

# Replace with your actual JWT token
token = 'your-jwt-goes-here'
client = PromptCrafter::Client.new(access_token: token)

search_term = 'python'

begin
  results = client.search_prompts(q: search_term)
  puts "Found #{results.length} prompt(s) matching '#{search_term}':"
  results.each do |prompt|
    puts "  - ID: #{prompt['_id']}, Title: #{prompt['title']}"
  end
rescue PromptCrafter::Error => e
  puts "An API error occurred: #{e.message}"
end
```

#### **Java**

```java
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Prompt;
import java.util.List;

public class SearchPromptsExample {
    public static void main(String[] args) {
        // Replace with your actual JWT token
        String token = "your-jwt-goes-here";
        String searchTerm = "python";

        // Initialize the client with your API token
        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();

        try {
            // Use the client to search for prompts
            List<Prompt> results = client.searchPrompts(searchTerm);

            System.out.println("Found " + results.size() + " prompt(s) matching '" + searchTerm + "':");
            for (Prompt prompt : results) {
                System.out.println("  - ID: " + prompt._id + ", Title: " + prompt.title);
            }
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            System.err.println("API Error: " + e.getMessage());
        }
    }
}
```

<!-- tabs:end -->

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

## Next steps

Now that you can find prompts in your library, you are ready to use them to build and test solutions.

- Try the [Log a generated output tutorial](tutorials/log-output.md) to test a prompt you found.
- Learn how to [update a prompt](tutorials/update-prompt.md) with new content or tags.
- For complete details on parameters and endpoints, see the [Prompt resource](reference/resources/prompt.md) and the [`GET /search`](reference/endpoints/get-search.md) endpoint documentation.
