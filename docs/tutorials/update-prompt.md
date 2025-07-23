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

<!-- tabs:start -->

#### **cURL**

To make the cURL commands cleaner, set shell variables for the base URL, your token, and the prompt's ID.

```bash
# Set shell variables for convenience
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token
PROMPT_ID="your-prompt-id-here" # Replace with the ID of the prompt you want to update

# Define the JSON payload for clarity
JSON_PAYLOAD='{
  "model": "GPT-4o",
  "tags": ["review", "product", "writing", "e-commerce"]
}'

# Send the PATCH request
curl -X PATCH "$BASE_URL/prompts/$PROMPT_ID" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d "$JSON_PAYLOAD"
```

#### **Postman**

If you've imported the PromptCrafter Postman Collection, sending the request is simple.

1.  In the **Prompts** folder, select the **Update a prompt** request.
2.  In the path variables for the request, replace `{{promptId}}` with the `_id` of the prompt you want to update.
3.  In the **Body** tab of the request, edit the JSON to include only the fields you wish to change.
4.  Click **Send**. The collection automatically uses the `{{token}}` variable set during login.

#### **Python**

<!-- tabs:start -->

##### **SDK**

```python
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError

# Replace with your actual JWT token and prompt ID
token = "your-jwt-goes-here"
prompt_id_to_update = "your-prompt-id-here"

client = PromptCrafterClient(token=token)

# Define the fields you want to change in a dictionary
update_data = {
    "model": "GPT-4o",
    "tags": ["review", "product", "writing", "e-commerce"]
}

try:
    # Pass the prompt ID and the update data to the SDK method
    updated_prompt = client.update_prompt(prompt_id_to_update, **update_data)
    print("Prompt updated successfully:")
    print(updated_prompt)
except PromptCrafterAPIError as e:
    print(f"Failed to update prompt: {e}")
```

##### **Requests**

```python
import requests

# Replace with your actual JWT token and prompt ID
token = "your-jwt-goes-here"
prompt_id_to_update = "your-prompt-id-here"

base_url = "https://promptcrafter-production.up.railway.app"
url = f"{base_url}/prompts/{prompt_id_to_update}"

headers = {
    "Authorization": f"Bearer {token}",
    "Content-Type": "application/json"
}

# The request body should only contain the fields you want to update
payload = {
    "model": "GPT-4o",
    "tags": ["review", "product", "writing", "e-commerce"]
}

try:
    response = requests.patch(url, headers=headers, json=payload)
    response.raise_for_status()

    updated_prompt = response.json()
    print("Prompt updated successfully:")
    print(updated_prompt)
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

<!-- tabs:end -->

#### **JavaScript**

<!-- tabs:start -->

##### **SDK**

```javascript
import PromptCrafterClient from './promptcrafter-client.js';

async function updatePrompt() {
    // Replace with your actual JWT token and prompt ID
    const token = 'your-jwt-goes-here';
    const promptIdToUpdate = 'your-prompt-id-here';
    
    const client = new PromptCrafterClient({ token });

    // Define an object with only the fields you want to change
    const fieldsToUpdate = {
        model: 'GPT-4o',
        tags: ['review', 'product', 'writing', 'e-commerce']
    };

    try {
        const updatedPrompt = await client.updatePrompt(promptIdToUpdate, fieldsToUpdate);
        console.log('Prompt updated successfully:');
        console.dir(updatedPrompt, { depth: null });
    } catch (error) {
        console.error('Failed to update prompt:', error.message);
    }
}

updatePrompt();
```

##### **Fetch**

```javascript
async function updatePrompt() {
    // Replace with your actual JWT token and prompt ID
    const token = "your-jwt-goes-here";
    const promptIdToUpdate = "your-prompt-id-here";

    const url = `https://promptcrafter-production.up.railway.app/prompts/${promptIdToUpdate}`;
    
    // The request body contains only the fields to be updated
    const payload = {
        "model": "GPT-4o",
        "tags": ["review", "product", "writing", "e-commerce"]
    };

    try {
        const response = await fetch(url, {
            method: 'PATCH',
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

        console.log("Prompt updated successfully:");
        console.log(data);
    } catch (error) {
        console.error("Failed to update prompt:", error.message);
    }
}

updatePrompt();
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
    // Replace with your actual JWT token and prompt ID
  token := "your-jwt-goes-here"
  promptIdToUpdate := "your-prompt-id-here"
    
  client := promptcrafter.NewClient(token)
  ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
  defer cancel()

  // For PATCH updates, fields must be pointers to distinguish
  // between a field being absent and a field being intentionally set to a zero value.
  newModel := "GPT-4o"
  newTags := []string{"review", "product", "writing", "e-commerce"}

  updateRequest := &promptcrafter.PromptUpdateRequest{
    Model: &newModel,
    Tags:  &newTags,
  }

  updatedPrompt, err := client.UpdatePrompt(ctx, promptIdToUpdate, updateRequest)
  if err != nil {
    log.Fatalf("Error updating prompt: %v", err)
  }

  fmt.Printf("Prompt updated successfully:\n%+v\n", updatedPrompt)
}
```

#### **Ruby**

```ruby
require 'promptcrafter'

# Replace with your actual JWT token and prompt ID
token = 'your-jwt-goes-here'
prompt_id_to_update = 'your-prompt-id-here'

client = PromptCrafter::Client.new(access_token: token)

# Define the fields to change in a hash
update_data = {
  model: 'GPT-4o',
  tags: ['review', 'product', 'writing', 'e-commerce']
}

begin
  updated_prompt = client.update_prompt(id: prompt_id_to_update, **update_data)
  puts "Prompt updated successfully:"
  puts updated_prompt
rescue PromptCrafter::Error => e
  puts "Failed to update prompt: #{e.message}"
end
```

#### **Java**

```java
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Prompt;
import com.promptcrafter.PromptCrafterClient.PromptUpdate;
import java.util.List;

public class UpdatePromptExample {
    public static void main(String[] args) {
        // Replace with your actual JWT token and prompt ID
        String token = "your-jwt-goes-here";
        String promptIdToUpdate = "your-prompt-id-here";

        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();

        // Use the PromptUpdate builder to specify only the fields to change
        PromptUpdate updatePayload = PromptUpdate.builder()
            .model("GPT-4o")
            .tags(List.of("review", "product", "writing", "e-commerce"))
            .build();

        try {
            Prompt updatedPrompt = client.updatePrompt(promptIdToUpdate, updatePayload);
            System.out.println("Prompt updated successfully:");
            System.out.println(updatedPrompt);
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            System.err.println("API Error: " + e.getMessage());
        }
    }
}
```

<!-- tabs:end -->

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

To confirm your prompt was updated successfully, send a `GET` request to retrieve the prompt using its `_id`. Each code sample below is self-contained and can be run independently.

<!-- tabs:start -->

#### **cURL**

```bash
# Ensure these shell variables are set
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here"
PROMPT_ID="your-prompt-id-here"

# Send a GET request to verify the update
curl -X GET "$BASE_URL/prompts/$PROMPT_ID" \
  -H "Authorization: Bearer $TOKEN"
```

#### **Postman**

Use the **Retrieve a prompt by ID** request in the `Prompts` folder. In the path variables, replace `{{promptId}}` with the ID of the prompt you just updated, then click **Send**. Inspect the response to confirm your changes were applied.

#### **Python**

<!-- tabs:start -->

##### **SDK**

```python
# This snippet is self-contained and can be run independently.
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError

# Replace with your actual JWT token and the ID of the prompt you updated
token = "your-jwt-goes-here"
prompt_id_to_verify = "your-prompt-id-here"

client = PromptCrafterClient(token=token)

try:
    retrieved_prompt = client.get_prompt(prompt_id_to_verify)
    print("Retrieved prompt to verify update:")
    print(f"  - ID: {retrieved_prompt['_id']}")
    print(f"  - Model: {retrieved_prompt['model']}")
    print(f"  - Tags: {retrieved_prompt['tags']}")
except PromptCrafterAPIError as e:
    print(f"Failed to retrieve prompt: {e}")
```

##### **Requests**

```python
# This snippet is self-contained and can be run independently.
import requests

# Replace with your actual JWT token and the ID of the prompt you updated
token = "your-jwt-goes-here"
prompt_id_to_verify = "your-prompt-id-here"

url = f"https://promptcrafter-production.up.railway.app/prompts/{prompt_id_to_verify}"
headers = {"Authorization": f"Bearer {token}"}

try:
    response = requests.get(url, headers=headers)
    response.raise_for_status()

    prompt = response.json()
    print("Retrieved prompt to verify update:")
    print(f"  - ID: {prompt['_id']}")
    print(f"  - Model: {prompt['model']}")
    print(f"  - Tags: {prompt['tags']}")
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

async function verifyUpdate() {
    // Replace with your actual JWT token and the ID of the prompt you updated
    const token = 'your-jwt-goes-here';
    const promptIdToVerify = 'your-prompt-id-here';
    
    const client = new PromptCrafterClient({ token });

    try {
        const prompt = await client.getPromptById(promptIdToVerify);
        console.log('Retrieved prompt to verify update:');
        console.log(`  - ID: ${prompt._id}`);
        console.log(`  - Model: ${prompt.model}`);
        console.log(`  - Tags: [${prompt.tags.join(', ')}]`);
    } catch (error) {
        console.error("Failed to retrieve prompt:", error.message);
    }
}

verifyUpdate();
```

##### **Fetch**

```javascript
// This snippet is self-contained and can be run independently.
async function verifyUpdate() {
    // Replace with your actual JWT token and the ID of the prompt you updated
    const token = "your-jwt-goes-here";
    const promptIdToVerify = "your-prompt-id-here";

    const url = `https://promptcrafter-production.up.railway.app/prompts/${promptIdToVerify}`;

    try {
        const response = await fetch(url, {
            method: 'GET',
            headers: { 'Authorization': `Bearer ${token}` }
        });

        const prompt = await response.json();

        if (!response.ok) {
            throw new Error(prompt.error || `HTTP error! Status: ${response.status}`);
        }

        console.log('Retrieved prompt to verify update:');
        console.log(`  - ID: ${prompt._id}`);
        console.log(`  - Model: ${prompt.model}`);
        console.log(`  - Tags: [${prompt.tags.join(', ')}]`);
    } catch (error) {
        console.error("Failed to retrieve prompt for verification:", error.message);
    }
}

verifyUpdate();
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
    // Replace with your actual JWT token and the ID of the prompt you updated
  token := "your-jwt-goes-here"
  promptIdToVerify := "your-prompt-id-here"
    
  client := promptcrafter.NewClient(token)
  ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
  defer cancel()

  prompt, err := client.GetPrompt(ctx, promptIdToVerify)
  if err != nil {
    log.Fatalf("Error retrieving prompt for verification: %v", err)
  }

  fmt.Println("Retrieved prompt to verify update:")
  fmt.Printf("  - ID: %s\n", prompt.ID)
  fmt.Printf("  - Model: %s\n", prompt.Model)
  fmt.Printf("  - Tags: %v\n", prompt.Tags)
}
```

#### **Ruby**

```ruby
# This snippet is self-contained and can be run independently.
require 'promptcrafter'

# Replace with your actual JWT token and the ID of the prompt you updated
token = 'your-jwt-goes-here'
prompt_id_to_verify = 'your-prompt-id-here'

client = PromptCrafter::Client.new(access_token: token)

begin
  prompt = client.get_prompt(id: prompt_id_to_verify)
  puts "Retrieved prompt to verify update:"
  puts "  - ID: #{prompt['_id']}"
  puts "  - Model: #{prompt['model']}"
  puts "  - Tags: #{prompt['tags']}"
rescue PromptCrafter::Error => e
  puts "Failed to retrieve prompt for verification: #{e.message}"
end
```

#### **Java**

```java
// This snippet is self-contained and can be run independently.
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Prompt;

public class VerifyPromptUpdateExample {
    public static void main(String[] args) {
        // Replace with your actual JWT token and the ID of the prompt you updated
        String token = "your-jwt-goes-here";
        String promptIdToVerify = "your-prompt-id-here";

        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();

        try {
            Prompt prompt = client.getPromptById(promptIdToVerify);
            System.out.println("Retrieved prompt to verify update:");
            System.out.println("  - ID: " + prompt._id);
            System.out.println("  - Model: " + prompt.model);
            System.out.println("  - Tags: " + prompt.tags);
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            System.err.println("Failed to retrieve prompt for verification: " + e.getMessage());
        }
    }
}
```
<!-- tabs:end -->

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

## Next steps

Now that you can update your prompts, you can manage your library and iterate on your work.

- Try the [Log a generated output tutorial](log-output.md) to test your updated prompt and see if its performance has improved.
- Learn how to [View logs](view-logs.md) to compare test results before and after your update.
- For complete details on parameters and endpoints, see the [prompt resource](reference/resources/prompt.md) and the [`PATCH /prompts/{promptId}`](reference/endpoints/patch-prompts-id.md) endpoint documentation.
