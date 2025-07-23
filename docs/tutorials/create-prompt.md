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

To make the cURL commands cleaner, set shell variables for the base URL, your token, and the JSON payload.

```bash
# Set shell variables for convenience
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token

# Define the JSON payload in a separate variable for readability
JSON_PAYLOAD=$(cat <<-END
{
  "title": "Positive Product Review Writer",
  "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  "model": "Grok-3 Beta",
  "tags": ["review", "product", "writing"]
}
END
)

# Send the POST request with the payload
curl -X POST "$BASE_URL/prompts" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d "$JSON_PAYLOAD"
```

#### **Postman**

If you've imported the PromptCrafter Postman Collection, sending the request is simple.

1.  In the **Prompts** folder, select the **Save a prompt** request.
2.  In the **Body** tab, modify the pre-filled JSON with your prompt's details.
3.  Click **Send**. The collection automatically uses the `{{token}}` variable set during login, so you don't need to configure authorization headers manually.

#### **Python**

<!-- tabs:start -->

##### **SDK**

```python
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError

# Replace with your actual JWT token
token = "your-jwt-goes-here"
client = PromptCrafterClient(token=token)

# Data for the new prompt, defined in a dictionary for clarity
prompt_data = {
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
    "model": "Grok-3 Beta",
    "tags": ["review", "product", "writing"]
}

try:
    # Pass the data to the create_prompt method using keyword arguments
    new_prompt = client.create_prompt(**prompt_data)
    print("Prompt created successfully:")
    print(f"  ID: {new_prompt['_id']}")
    print(f"  Title: {new_prompt['title']}")
except PromptCrafterAPIError as e:
    print(f"Failed to create prompt: {e}")
```

##### **Requests**

```python
import requests

# Replace with your actual JWT token
token = "your-jwt-goes-here"
url = "https://promptcrafter-production.up.railway.app/prompts"

headers = {
    "Authorization": f"Bearer {token}",
    "Content-Type": "application/json"
}

# The JSON payload for the new prompt
payload = {
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
    "model": "Grok-3 Beta",
    "tags": ["review", "product", "writing"]
}

try:
    response = requests.post(url, headers=headers, json=payload)
    response.raise_for_status() # Raise an exception for bad status codes
    
    new_prompt = response.json()
    print("Prompt created successfully:")
    print(f"  ID: {new_prompt['_id']}")
    print(f"  Title: {new_prompt['title']}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

<!-- tabs:end -->

#### **JavaScript**

<!-- tabs:start -->

##### **SDK**

```javascript
import PromptCrafterClient from './promptcrafter-client.js';

async function createPrompt() {
    // Replace with your actual JWT token
    const token = 'your-jwt-goes-here';
    const client = new PromptCrafterClient({ token });

    // Data for the new prompt, defined in an object for clarity
    const promptData = {
        title: "Positive Product Review Writer",
        content: "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
        model: "Grok-3 Beta",
        tags: ["review", "product", "writing"]
    };

    try {
        const newPrompt = await client.createPrompt(promptData);
        console.log('Prompt created successfully:');
        console.log(`  ID: ${newPrompt._id}`);
        console.log(`  Title: ${newPrompt.title}`);
    } catch (error) {
        console.error('Failed to create prompt:', error.message);
    }
}

createPrompt();
```

##### **Fetch**

```javascript
async function createPrompt() {
    // Replace with your actual JWT token
    const token = "your-jwt-goes-here";
    const url = "https://promptcrafter-production.up.railway.app/prompts";

    // The JSON payload for the new prompt
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
            throw new Error(data.error || `HTTP error! Status: ${response.status}`);
        }

        console.log("Prompt created successfully:");
        console.log(`  ID: ${data._id}`);
        console.log(`  Title: ${data.title}`);
    } catch (error) {
        console.error("Failed to create prompt:", error.message);
    }
}

createPrompt();
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
    ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
    defer cancel()

    // Define the data for the new prompt in a request struct
    promptRequest := &promptcrafter.PromptCreateRequest{
        Title:   "Positive Product Review Writer",
        Content: "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
        Model:   "Grok-3 Beta",
        Tags:    []string{"review", "product", "writing"},
    }

    newPrompt, err := client.CreatePrompt(ctx, promptRequest)
    if err != nil {
        log.Fatalf("Error creating prompt: %v", err)
    }

    fmt.Printf("Prompt created successfully:\n")
    fmt.Printf("  ID: %s\n", newPrompt.ID)
    fmt.Printf("  Title: %s\n", newPrompt.Title)
}
```

#### **Ruby**

```ruby
require 'promptcrafter'

# Replace with your actual JWT token
token = 'your-jwt-goes-here'
client = PromptCrafter::Client.new(access_token: token)

# Data for the new prompt, defined in a hash for clarity
prompt_data = {
  title: "Positive Product Review Writer",
  content: "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  model: "Grok-3 Beta",
  tags: ["review", "product", "writing"]
}

begin
  new_prompt = client.create_prompt(**prompt_data)
  puts "Prompt created successfully:"
  puts "  ID: #{new_prompt['_id']}"
  puts "  Title: #{new_prompt['title']}"
rescue PromptCrafter::Error => e
  puts "Failed to create prompt: #{e.message}"
end
```

#### **Java**

```java
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Prompt;
import com.promptcrafter.PromptCrafterClient.PromptCreate;
import java.util.List;

public class CreatePromptExample {
    public static void main(String[] args) {
        // Replace with your actual JWT token
        String token = "your-jwt-goes-here";

        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();

        // Data for the new prompt, encapsulated in a request object
        PromptCreate promptToCreate = new PromptCreate(
            "Positive Product Review Writer",
            "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
            "Grok-3 Beta",
            List.of("review", "product", "writing")
        );

        try {
            Prompt newPrompt = client.createPrompt(promptToCreate);
            System.out.println("Prompt created successfully:");
            System.out.println("  ID: " + newPrompt._id);
            System.out.println("  Title: " + newPrompt.title);
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            System.err.println("Failed to create prompt: " + e.getMessage());
        }
    }
}
```

<!-- tabs:end -->

## Response

If your request is successful, the server returns a `201 Created` status and the full prompt object, including its unique server-generated `_id`. Use this ID to retrieve, update, or test the prompt later.

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

**Note:** The server adds the `_id`, `ownerId`, `createdAt`, and `updatedAt` fields. Do not include them in your request body.

## Verify the prompt

To confirm your prompt was saved successfully, send a `GET` request using the prompt `_id` from the response. Each code sample below is self-contained and can be run independently.

<!-- tabs:start -->

#### **cURL**

```bash
# Replace with your token and the ID from the previous response
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here"
PROMPT_ID="your-new-prompt-id"

curl -X GET "$BASE_URL/prompts/$PROMPT_ID" \
  -H "Authorization: Bearer $TOKEN"
```

#### **Postman**

Use the **Retrieve a prompt by ID** request in the `Prompts` folder. First, copy the `_id` from the response of the **Save a prompt** request. Then, in the Collection's **Variables** tab, paste the ID into the `CURRENT VALUE` for the `promptId` variable. Click **Send** to verify.

#### **Python**

<!-- tabs:start -->

##### **SDK**

```python
# This snippet is self-contained and can be run independently.
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError

# Replace with your token and the ID of the prompt you just created
token = "your-jwt-goes-here"
prompt_id_to_verify = "your-new-prompt-id"

client = PromptCrafterClient(token=token)

try:
    retrieved_prompt = client.get_prompt(prompt_id=prompt_id_to_verify)
    print("Successfully retrieved prompt to verify it was saved:")
    print(f"  - ID: {retrieved_prompt['_id']}")
    print(f"  - Title: {retrieved_prompt['title']}")
except PromptCrafterAPIError as e:
    print(f"Failed to retrieve prompt: {e}")
```

##### **Requests**

```python
# This snippet is self-contained and can be run independently.
import requests

# Replace with your token and the ID from the POST response
token = "your-jwt-goes-here"
prompt_id_to_verify = "your-new-prompt-id"

url = f"https://promptcrafter-production.up.railway.app/prompts/{prompt_id_to_verify}"
headers = {"Authorization": f"Bearer {token}"}

try:
    response = requests.get(url, headers=headers)
    response.raise_for_status()
    retrieved_prompt = response.json()
    print("Successfully retrieved prompt:")
    print(f"  - ID: {retrieved_prompt['_id']}")
    print(f"  - Title: {retrieved_prompt['title']}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

<!-- tabs:end -->

#### **JavaScript**

<!-- tabs:start -->

##### **SDK**

```javascript
// This snippet is self-contained and can be run independently.
import PromptCrafterClient from './promptcrafter-client.js';

async function verifyPromptCreation() {
    // Replace with your token and the ID of the prompt you just created
    const token = 'your-jwt-goes-here';
    const promptIdToVerify = 'your-new-prompt-id';

    const client = new PromptCrafterClient({ token });
    
    try {
        const retrievedPrompt = await client.getPromptById(promptIdToVerify);
        console.log('Successfully retrieved prompt to verify it was saved:');
        console.log(`  - ID: ${retrievedPrompt._id}`);
        console.log(`  - Title: ${retrievedPrompt.title}`);
    } catch (error) {
        console.error("Failed to retrieve prompt:", error.message);
    }
}

verifyPromptCreation();
```

##### **Fetch**

```javascript
// This snippet is self-contained and can be run independently.
async function verifyPromptCreation() {
    // Replace with your token and the ID from the POST response
    const token = "your-jwt-goes-here";
    const promptIdToVerify = "your-new-prompt-id";

    const url = `https://promptcrafter-production.up.railway.app/prompts/${promptIdToVerify}`;

    try {
        const response = await fetch(url, {
            method: 'GET',
            headers: { 'Authorization': `Bearer ${token}` }
        });

        const data = await response.json();

        if (!response.ok) {
            throw new Error(data.error || `HTTP error! Status: ${response.status}`);
        }

        console.log('Successfully retrieved prompt:');
        console.log(`  - ID: ${data._id}`);
        console.log(`  - Title: ${data.title}`);
    } catch (error) {
        console.error("Failed to retrieve prompt:", error.message);
    }
}

verifyPromptCreation();
```

<!-- tabs:end -->

#### **Go**

```go
// This snippet is self-contained and can be run independently.
package main

import (
    "context"
    "fmt"
    "log"
    "time"
    "github.com/your-org/promptcrafter" // Replace with your actual import path
)

func main() {
    // Replace with your token and the ID of the prompt you just created
    token := "your-jwt-goes-here"
    promptIdToVerify := "your-new-prompt-id"

    client := promptcrafter.NewClient(token)
    ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
    defer cancel()

    retrievedPrompt, err := client.GetPrompt(ctx, promptIdToVerify)
    if err != nil {
        log.Fatalf("Error retrieving prompt to verify creation: %v", err)
    }

    fmt.Println("Successfully retrieved prompt to verify it was saved:")
    fmt.Printf("  - ID: %s\n", retrievedPrompt.ID)
    fmt.Printf("  - Title: %s\n", retrievedPrompt.Title)
}
```

#### **Ruby**

```ruby
# This snippet is self-contained and can be run independently.
require 'promptcrafter'

# Replace with your token and the ID of the prompt you just created
token = 'your-jwt-goes-here'
prompt_id_to_verify = 'your-new-prompt-id'

client = PromptCrafter::Client.new(access_token: token)

begin
  retrieved_prompt = client.get_prompt(id: prompt_id_to_verify)
  puts "Successfully retrieved prompt to verify it was saved:"
  puts "  - ID: #{retrieved_prompt['_id']}"
  puts "  - Title: #{retrieved_prompt['title']}"
rescue PromptCrafter::Error => e
  puts "Failed to retrieve prompt: #{e.message}"
end
```

#### **Java**

```java
// This snippet is self-contained and can be run independently.
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Prompt;

public class VerifyPromptCreationExample {
    public static void main(String[] args) {
        // Replace with your token and the ID of the prompt you just created
        String token = "your-jwt-goes-here";
        String promptIdToVerify = "your-new-prompt-id";

        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();
        
        try {
            Prompt retrievedPrompt = client.getPromptById(promptIdToVerify);
            System.out.println("Successfully retrieved prompt to verify it was saved:");
            System.out.println("  - ID: " + retrievedPrompt._id);
            System.out.println("  - Title: " + retrievedPrompt.title);
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            System.err.println("Failed to retrieve prompt: " + e.getMessage());
        }
    }
}
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

## Next steps

Now that you've saved your first prompt, you're ready to explore the other features of PromptCrafter.

- Try the [Search prompts tutorial](tutorials/search-prompts.md) to find prompts by keyword.
- See the [Log a generated output tutorial](tutorials/log-output.md) to track prompt performance.
- For complete details on parameters and endpoints, see the [Prompt resource](reference/resources/prompt.md) and the [`POST /prompts`](reference/endpoints/post-prompts.md) endpoint documentation.
