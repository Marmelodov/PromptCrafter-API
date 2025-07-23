# Tutorial: Save a prompt

This guide walks through how to save a prompt to PromptCrafter API. Prompts are the API's core resource, storing reusable instructions for generative AI models like GPT-4o and Claude. Saving prompts allows you to build an organized, searchable prompt library with which you can:
- **Automate content creation**: Design prompts for a marketing app that draft blog posts in your brand’s voice, then retrieve them by ID to generate articles in the same style every time without rewriting instructions.  
- **Build a prompt evaluation suite**: Run systematic experiments to determine the best prompt for a job like sentiment analysis. Tag related prompts (e.g., `sentiment-v1` and `sentiment-v2`) to programmatically test them across AI models and measure which version produces the most accurate outputs.
- **Collaborate with teams**: Group prompts by topic in a shared library (e.g., "marketing" or "research"), so your team can reuse and update them with version history, eliminating the chaos of prompts buried in emails, docs, or personal notebooks.

This tutorial takes about ten minutes to complete.

## Prerequisites

To follow this tutorial, you need:

- **Your JWT token:** All requests require a Bearer token for authentication. If you don't have one, complete the authentication steps in the [Quickstart guide](../quickstart.md) first.
- **An HTTP client:** This tutorial provides examples for both cURL and Postman.
    - If you're using Postman, import the [PromptCrafter Postman Collection](postman.md) to follow along.

## Build the request

Save your new prompt by sending a `POST` request to this endpoint:

```text
https://promptcrafter-production.up.railway.app/prompts
```

The `POST` request includes two headers and a JSON-formatted request body.

### Headers

Both these headers are required:

- `Authorization: Bearer {your_token}` authenticates you as the user. Replace `{your_token}` with the bearer token you received after logging in.
- `Content-Type: application/json` tells the server to expect a JSON object in the request body.

### Request body

The request body contains the data that makes up your new prompt.

| Field    | Type             | Required | Description                                                            |
|----------|------------------|----------|------------------------------------------------------------------------|
| `title`  | string           | Yes      | A short label that helps you identify or sort the prompt.                |
| `content`| string           | Yes      | The text sent to the AI model.                                         |
| `model`  | string           | Yes      | The name of the intended AI model (e.g., `Claude 3 Sonnet`).  |
| `tags`   | array&lt;string&gt;  | No       | Optional keywords for grouping by topic, task, project, or library.    |

**Example request body:**

```json
{
  "title": "Positive Product Review Writer",
  "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  "model": "Grok-3 Beta",
  "tags": ["review", "product", "writing"]
}
```

## Send the request

<!-- tabs:start -->

#### **cURL**

```bash
# Define shell variables for convenience
base_url="https://promptcrafter-production.up.railway.app"
token="your-jwt-goes-here" # Replace with your actual token

# Send the POST request to save the prompt
curl -X POST $base_url/prompts \
  -H "Authorization: Bearer $token" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
    "model": "Grok-3 Beta",
    "tags": ["review", "product", "writing"]
  }'
```

#### **Postman**

If you've imported the PromptCrafter Postman Collection, sending the request is simple.

1. In the **Prompts** folder, select the **Save a prompt** request.
2. In the **Body** tab, modify the pre-filled JSON with your prompt's details.
3. Click **Send**. The collection automatically uses the `{{token}}` variable set during login, so you don't need to configure authorization headers manually.

#### **Python**

<!-- tabs:start -->

##### **SDK**
```python
# The PromptCrafter SDK is the recommended way to use the API in Python.
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError

# Replace with your actual JWT token
token = "your-jwt-goes-here"

# Initialize the client with your token
client = PromptCrafterClient(token=token)

try:
    # Create the prompt using the SDK
    new_prompt = client.create_prompt(
        title="Positive Product Review Writer",
        content="Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
        model="Grok-3 Beta",
        tags=["review", "product", "writing"]
    )
    # The new prompt's ID is needed to verify it was saved.
    print(f"Prompt created successfully with ID: {new_prompt['_id']}")
except PromptCrafterAPIError as e:
    print(f"Failed to create prompt: {e}")
```

##### **Requests**
```python
# You can also use the popular 'requests' library for raw HTTP calls.
import requests
import json

# Replace with your actual JWT token
token = "your-jwt-goes-here"

# API endpoint for saving prompts
url = "https://promptcrafter-production.up.railway.app/prompts"

# Request headers, including authorization
headers = {
    "Authorization": f"Bearer {token}",
    "Content-Type": "application/json"
}

# The data for the new prompt
payload = {
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
    "model": "Grok-3 Beta",
    "tags": ["review", "product", "writing"]
}

try:
    # Send the POST request using the requests library
    response = requests.post(url, headers=headers, json=payload)
    # Raise an exception for bad status codes (4xx or 5xx)
    response.raise_for_status()

    created_prompt = response.json()
    print(f"Prompt created successfully with ID: {created_prompt['_id']}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

<!-- tabs:end -->

#### **JavaScript**

<!-- tabs:start -->

##### **SDK**
```javascript
// The PromptCrafter SDK is the recommended way to use the API in JavaScript/Node.js.
import PromptCrafterClient from './promptcrafter-client.js'; // Adjust path as needed

async function savePromptWithSDK() {
    // Replace with your actual JWT token
    const token = 'your-jwt-goes-here';

    const client = new PromptCrafterClient({ token });
    
    try {
        const newPrompt = await client.createPrompt({
            title: "Positive Product Review Writer",
            content: "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
            model: "Grok-3 Beta",
            tags: ["review", "product", "writing"]
        });
        // The new prompt's ID is needed to verify it was saved.
        console.log(`Prompt saved successfully with ID: ${newPrompt._id}`);
    } catch (error) {
        console.error("Failed to save prompt:", error.message);
    }
}

savePromptWithSDK();
```

##### **Fetch**
```javascript
// You can also use the built-in Fetch API for raw HTTP calls.
async function savePrompt() {
    // Replace with your actual JWT token
    const token = "your-jwt-goes-here";

    // API endpoint for saving prompts
    const url = "https://promptcrafter-production.up.railway.app/prompts";

    // The data for the new prompt
    const payload = {
        title: "Positive Product Review Writer",
        content: "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
        model: "Grok-3 Beta",
        tags: ["review", "product", "writing"]
    };

    try {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Authorization': `Bearer ${token}`,
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(payload)
        });

        const data = await response.json();

        if (!response.ok) {
            // Use the server's error message if available
            throw new Error(data.error || `HTTP error! Status: ${response.status}`);
        }

        console.log(`Prompt saved successfully with ID: ${data._id}`);
    } catch (error) {
        console.error("Failed to save prompt:", error.message);
    }
}

// Call the function to execute the request
savePrompt();
```

<!-- tabs:end -->

#### **Go**

```go
package main

import (
    "context"
    "fmt"
    "log"
    "github.com/your-org/promptcrafter" // Replace with your actual import path
)

func main() {
    // Replace with your actual JWT token
    token := "your-jwt-goes-here"
    client := promptcrafter.NewClient(token)
    ctx := context.Background()

    prompt, err := client.CreatePrompt(ctx, &promptcrafter.PromptCreateRequest{
        Title:   "Positive Product Review Writer",
        Content: "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
        Model:   "Grok-3 Beta",
        Tags:    []string{"review", "product", "writing"},
    })

    if err != nil {
        log.Printf("Error creating prompt: %v", err)
        return
    }

    // The new prompt's ID is needed to verify it was saved.
    fmt.Printf("Prompt created successfully with ID: %s\n", prompt.ID)
}
```

#### **Ruby**

```ruby
require 'promptcrafter'

# Replace with your actual JWT token
token = 'your-jwt-goes-here'

# Initialize the client with the token
client = PromptCrafter::Client.new(access_token: token)

begin
  new_prompt = client.create_prompt(
    title: 'Positive Product Review Writer',
    content: 'Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.',
    model: 'Grok-3 Beta',
    tags: ['review', 'product', 'writing']
  )
  # The new prompt's ID is needed to verify it was saved.
  puts "Prompt created successfully with ID: #{new_prompt['_id']}"
rescue PromptCrafter::Error => e
  puts "Failed to create prompt: #{e.message}"
end
```

#### **Java**

```java
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Prompt;
import com.promptcrafter.PromptCrafterClient.PromptCreate;
import java.util.Arrays;

public class CreatePromptExample {
    public static void main(String[] args) {
        // Replace with your actual JWT token
        String token = "your-jwt-goes-here";

        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();

        PromptCreate promptToCreate = new PromptCreate(
            "Positive Product Review Writer",
            "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
            "Grok-3 Beta",
            Arrays.asList("review", "product", "writing")
        );

        try {
            Prompt createdPrompt = client.createPrompt(promptToCreate);
            // The new prompt's ID is needed to verify it was saved.
            System.out.println("Prompt created successfully with ID: " + createdPrompt._id);
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            System.err.println("Failed to create prompt: " + e.getMessage());
        }
    }
}
```

<!-- tabs:end -->


A successful request returns a `201 Created` status and the full prompt object, including its unique server-generated `_id`. Use this ID to retrieve, update, or test the prompt later.

## Response

If your request is successful, the server returns a status code `201 Created` and a response body that looks like this:

```json
{
  "_id": "prompt102",
  "ownerId": "user5",
  "title": "Positive Product Review Writer",
  "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  "model": "Grok-3 Beta",
  "tags": ["review", "product", "writing"],
  "createdAt": "2024-06-20T13:01:00Z",
  "updatedAt": "2024-06-20T13:01:00Z"
}
```

**Note:** the server adds the `_id`, `ownerId`, `createdAt`, and `updatedAt` fields. Don't include them in the request body.

## Verify the prompt

To confirm your prompt saved successfully, send a GET request using the prompt `_id` from the response:

<!-- tabs:start -->

#### **cURL**

```bash
# Set shell variables from the previous step
TOKEN="your-jwt-goes-here" # Replace with your actual token
PROMPT_ID="prompt102"      # Replace with the _id from the POST response

BASE_URL="https://promptcrafter-production.up.railway.app"

# Send the GET request to retrieve the prompt by its ID
curl -X GET $BASE_URL/prompts/$PROMPT_ID \
  -H "Authorization: Bearer $TOKEN"
```

#### **Postman**

Use the **Retrieve a prompt by ID** request in the `Prompts` folder. Update the `promptId` collection variable with your new `_id`, then click **Send**.

#### **Python**

<!-- tabs:start -->

##### **SDK**

```python
from promptcrafter import PromptCrafterClient, NotFoundError

# Replace with your actual JWT token and the ID of the prompt you created
token = "your-jwt-goes-here"
prompt_id = "prompt102"

# Initialize the client with your token
client = PromptCrafterClient(token=token)

try:
    # Retrieve the prompt using the SDK
    retrieved_prompt = client.get_prompt(prompt_id=prompt_id)
    print(f"Successfully retrieved prompt: '{retrieved_prompt['title']}'")
    # print(retrieved_prompt)

except NotFoundError:
    print(f"Error: Prompt with ID '{prompt_id}' not found.")
except Exception as e:
    print(f"An unexpected error occurred: {e}")
```

##### **Requests**

```python
import requests

# Replace with your actual JWT token and the ID of the prompt you created
token = "your-jwt-goes-here"
prompt_id = "prompt102"

base_url = "https://promptcrafter-production.up.railway.app"
url = f"{base_url}/prompts/{prompt_id}"

headers = { "Authorization": f"Bearer {token}" }

try:
    response = requests.get(url, headers=headers)
    response.raise_for_status()  # Raise an exception for bad status codes

    retrieved_prompt = response.json()
    print(f"Successfully retrieved prompt: '{retrieved_prompt['title']}'")

except requests.exceptions.HTTPError as e:
    if e.response.status_code == 404:
        print(f"Error: Prompt with ID '{prompt_id}' not found.")
    else:
        print(f"An HTTP error occurred: {e}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

<!-- tabs:end -->

#### **JavaScript**

<!-- tabs:start -->

##### **SDK**

```javascript
import PromptCrafterClient from './promptcrafter-client.js'; // Adjust path as needed

async function retrievePrompt() {
    // Replace with your actual JWT token and the ID of the prompt you created
    const token = 'your-jwt-goes-here';
    const promptId = 'prompt102';

    const client = new PromptCrafterClient({ token });
    
    try {
        const prompt = await client.getPromptById(promptId);
        console.log(`Successfully retrieved prompt: '${prompt.title}'`);
        // console.log(prompt);
    } catch (error) {
        if (error.status === 404) {
            console.error(`Error: Prompt with ID '${promptId}' not found.`);
        } else {
            console.error("Failed to retrieve prompt:", error.message);
        }
    }
}

retrievePrompt();
```

##### **Fetch**

```javascript
async function retrievePromptWithFetch() {
    // Replace with your actual JWT token and the ID of the prompt you created
    const token = "your-jwt-goes-here";
    const promptId = "prompt102";

    const baseUrl = "https://promptcrafter-production.up.railway.app";
    const url = `${baseUrl}/prompts/${promptId}`;

    try {
        const response = await fetch(url, {
            method: 'GET',
            headers: { 'Authorization': `Bearer ${token}` }
        });

        if (response.status === 404) {
            throw new Error(`Prompt with ID '${promptId}' not found.`);
        }
        if (!response.ok) {
            const errorData = await response.json().catch(() => ({})); // Try to parse error, else empty object
            throw new Error(errorData.error || `HTTP error! Status: ${response.status}`);
        }

        const prompt = await response.json();
        console.log(`Successfully retrieved prompt: '${prompt.title}'`);

    } catch (error) {
        console.error("Failed to retrieve prompt:", error.message);
    }
}

retrievePromptWithFetch();
```

<!-- tabs:end -->

#### **Java**

```java
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Prompt;

public class RetrievePromptExample {
    public static void main(String[] args) {
        // Replace with your actual JWT token and the ID of the prompt you created
        String token = "your-jwt-goes-here";
        String promptId = "prompt102";

        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();

        try {
            Prompt retrievedPrompt = client.getPromptById(promptId);
            System.out.println("Successfully retrieved prompt: '" + retrievedPrompt.title + "'");
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            if (e.getStatusCode() == 404) {
                System.err.println("Error: Prompt with ID '" + promptId + "' not found.");
            } else {
                System.err.println("Failed to retrieve prompt: " + e.getMessage());
            }
        }
    }
}
```

#### **Go**

```go
package main

import (
  "context"
  "fmt"
  "log"
  "github.com/your-org/promptcrafter" // Replace with your actual import path
)

func main() {
  // Replace with your actual JWT token and the ID of the prompt you created
  token := "your-jwt-goes-here"
  promptId := "prompt102"
    
  client := promptcrafter.NewClient(token)
  ctx := context.Background()

  prompt, err := client.GetPrompt(ctx, promptId)
  if err != nil {
    log.Printf("Error retrieving prompt: %v", err)
    return
  }

  fmt.Printf("Successfully retrieved prompt: '%s'\n", prompt.Title)
}
```

#### **Ruby**

```ruby
require 'promptcrafter'

# Replace with your actual JWT token and the ID of the prompt you created
token = 'your-jwt-goes-here'
prompt_id = 'prompt102'

# Initialize the client with the token
client = PromptCrafter::Client.new(access_token: token)

begin
  prompt = client.get_prompt(prompt_id)
  puts "Successfully retrieved prompt: '#{prompt['title']}'"
rescue PromptCrafter::NotFoundError => e
  puts "Error: Prompt with ID '#{prompt_id}' not found."
rescue PromptCrafter::Error => e
  puts "Failed to retrieve prompt: #{e.message}"
end
```

<!-- tabs:end -->

Expect a `200 OK` status with a response matching the object from the `POST` request, confirming your prompt is saved and ready to use. If you get `404 Not Found`, check that the `_id` is correct.  

## What to do if the request doesn't work

Here are issues you might encounter when saving prompts and what to do about them.

| Status | Example response | Cause | Solution |
|--------|------------------|--------|----------|
| **400 Bad Request** | `{"error": "Required fields are missing or invalid"}` | Missing required fields (`title`, `content`, `model`) or invalid JSON syntax. | Verify all required fields are present and JSON is properly formatted. |
| **401 Unauthorized** | `{"error": "Authentication token is missing"}` | Missing bearer token. | Include `Authorization: Bearer {your_token}` header with a valid token. |
| **401 Unauthorized** | `{"error": "Authentication token is expired or invalid"}` | Expired or invalid bearer token. | Log in again to obtain a new token and include it in the `Authorization` header. |
| **415 Unsupported Media Type** | `{"error": "Content-Type must be application/json"}` | Missing or incorrect Content-Type header. | Add `Content-Type: application/json` to request headers. |
| **404 Not Found** | `{"error": "Route not found"}` | Incorrect endpoint URL. | Verify the endpoint is `/prompts` with no trailing slash or typos. |
| **500 Internal Server Error** | `{"error": "An unexpected server error occurred"}` | Server-side processing error. | Retry the request; contact support if the error persists. |

## Next Steps

Now that you've saved your first prompt, you're ready to explore the other features of PromptCrafter.

- Try the [Search prompts tutorial](tutorials/search-prompts.md) to find prompts by keyword.
- See the [Log a generated output tutorial](tutorials/test-prompt.md) to track prompt performance.
- For complete details on parameters and endpoints, see the [Prompt resource](reference/resources/prompt.md) and the [`POST /prompts`](reference/endpoints/post-prompts.md) endpoint documentation.


