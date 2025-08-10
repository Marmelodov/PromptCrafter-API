# Tutorial: View logs

View test logs by sending a `GET /logs` request. Each log records a model's `output`, linking it to the `promptId` that produced it. Reviewing logs allows you to analyze and compare results across tests, identify performance patterns, and systematically improve your prompts. You can:

- **Analyze chatbot performance:** Retrieve logs for a specific `promptId` used by a chatbot to see a complete history of its responses to users, then analyze the scores and notes to find areas for improvement.
- **Compare A/B test results:** Fetch logs for two different prompt versions (e.g., `prompt-A` and `prompt-B`) to compare their average scores and determine which is more effective.
- **Monitor for quality degradation:** After modifying a critical prompt, retrieve its most recent logs to ensure that its performance hasn't declined unexpectedly.

This tutorial takes about five minutes to complete.

## Prerequisites

To follow this tutorial, you need:

- **Your JWT token:** All requests require a Bearer token for authentication. If you don't have a token, complete the authentication steps in the [Quickstart guide](../quickstart.md) first.
- **At least one saved log:** You need to have logged at least one output. If you haven't, complete the [Log a generated output tutorial](log-output.md) first.
- **An HTTP client:** This tutorial includes examples for cURL, Postman, the PromptCrafter SDKs (Python, JavaScript, Go, Ruby, and Java), and raw HTTP using Python’s `requests` library and JavaScript’s `fetch` API.
    - If you're using Postman, import the [PromptCrafter Postman Collection](postman.md) to follow along.

## Build the request

View logs by sending a `GET` request to the `/logs` endpoint. You can either retrieve all logs associated with your account or filter them to see only the logs for a specific prompt.

### Headers

Add the following header to your request:

- `Authorization: Bearer {your_token}` authenticates you as the user. Replace `{your_token}` with your Bearer token.

### Query parameters

To retrieve logs for a single prompt, include the following optional query parameter:

| Field      | Type   | Required | Description                                               |
|------------|--------|----------|-----------------------------------------------------------|
| `promptId` | string | No       | The unique `_id` of a prompt to filter the logs by. |

If you omit the `promptId` parameter, the API returns all logs for your account.

## Retrieve all logs for your account

This is the default behavior when you send a `GET` request to `/logs` without any query parameters.

<!-- tabs:start -->

#### **cURL**

To make the cURL commands cleaner, set shell variables for the base URL and your token.

```bash
# Set shell variables for convenience
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token

# Send the request to get all logs
curl -X GET $BASE_URL/logs \
  -H "Authorization: Bearer $TOKEN"
```

#### **Postman**

In the **Logs** folder of the Postman Collection, select the **Retrieve all logs** request and click **Send**. The collection automatically uses the `{{token}}` variable set during login.

#### **Python**

<!-- tabs:start -->

##### **SDK**

```python
# This snippet uses the PromptCrafter Python SDK
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError

# Replace with your actual JWT token
token = "your-jwt-goes-here"
client = PromptCrafterClient(token=token)

try:
    all_logs = client.get_logs()
    print(f"Retrieved {len(all_logs)} total log(s).")
    if all_logs:
        # Print details of the first log as an example
        first_log = all_logs[0]
        print(f"Example Log ID: {first_log['_id']}, Prompt ID: {first_log['promptId']}")
except PromptCrafterAPIError as e:
    print(f"An API error occurred: {e}")
```

##### **`requests`**

```python
# This snippet uses Python's requests library
import requests

# Replace with your actual JWT token
token = "your-jwt-goes-here"
url = "https://promptcrafter-production.up.railway.app/logs"
headers = {"Authorization": f"Bearer {token}"}

try:
    response = requests.get(url, headers=headers)
    response.raise_for_status()  # Raise an exception for bad status codes
    all_logs = response.json()
    
    print(f"Retrieved {len(all_logs)} total log(s).")
    if all_logs:
        first_log = all_logs[0]
        print(f"Example Log ID: {first_log['_id']}, Prompt ID: {first_log['promptId']}")
except requests.exceptions.RequestException as e:
    print(f"An HTTP error occurred: {e}")
```
<!-- tabs:end -->

#### **JavaScript**

<!-- tabs:start -->

##### **SDK**

```javascript
// This snippet uses the PromptCrafter JavaScript SDK
import PromptCrafterClient from './promptcrafter-client.js';

async function getAllLogs() {
    // Replace with your actual JWT token
    const token = 'your-jwt-goes-here';
    const client = new PromptCrafterClient({ token });

    try {
        const allLogs = await client.getLogs();
        console.log(`Retrieved ${allLogs.length} total log(s).`);
        if (allLogs.length > 0) {
            const firstLog = allLogs[0];
            console.log(`Example Log ID: ${firstLog._id}, Prompt ID: ${firstLog.promptId}`);
        }
    } catch (error) {
        console.error("Failed to retrieve logs:", error.message);
    }
}

getAllLogs();
```

##### **`fetch`**

```javascript
// This snippet uses JavaScript's fetch API
async function getAllLogs() {
    // Replace with your actual JWT token
    const token = "your-jwt-goes-here";
    const url = "https://promptcrafter-production.up.railway.app/logs";
    const headers = { 'Authorization': `Bearer ${token}` };

    try {
        const response = await fetch(url, { headers });
        const allLogs = await response.json();

        if (!response.ok) {
            throw new Error(allLogs.error || `HTTP error! Status: ${response.status}`);
        }
        
        console.log(`Retrieved ${allLogs.length} total log(s).`);
        if (allLogs.length > 0) {
            const firstLog = allLogs[0];
            console.log(`Example Log ID: ${firstLog._id}, Prompt ID: ${firstLog.promptId}`);
        }
    } catch (error) {
        console.error("Failed to retrieve logs:", error.message);
    }
}

getAllLogs();
```
<!-- tabs:end -->

#### **Go**

```go
// This snippet uses the PromptCrafter Go SDK
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

  allLogs, err := client.ListLogs(ctx)
  if err != nil {
    log.Fatalf("Error retrieving all logs: %v", err)
  }
    
  fmt.Printf("Retrieved %d total log(s).\n", len(allLogs))
  if len(allLogs) > 0 {
    fmt.Printf("Example Log ID: %s, Prompt ID: %s\n", allLogs[0].ID, allLogs[0].PromptID)
  }
}
```

#### **Ruby**

```ruby
# This snippet uses the PromptCrafter Ruby SDK
require 'promptcrafter'

# Replace with your actual JWT token
token = 'your-jwt-goes-here'
client = PromptCrafter::Client.new(access_token: token)

begin
  all_logs = client.list_logs
  puts "Retrieved #{all_logs.length} total log(s)."
  if all_logs.any?
    first_log = all_logs.first
    puts "Example Log ID: #{first_log['_id']}, Prompt ID: #{first_log['promptId']}"
  end
rescue PromptCrafter::Error => e
  puts "An API error occurred: #{e.message}"
end
```

#### **Java**

```java
// This snippet uses the PromptCrafter Java SDK
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Log;
import java.util.List;

public class GetAllLogsExample {
    public static void main(String[] args) {
        // Replace with your actual JWT token
        String token = "your-jwt-goes-here";

        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();

        try {
            List<Log> allLogs = client.getAllLogs();
            System.out.println("Retrieved " + allLogs.size() + " total log(s).");
            if (!allLogs.isEmpty()) {
                Log firstLog = allLogs.get(0);
                System.out.println("Example Log ID: " + firstLog._id + ", Prompt ID: " + firstLog.promptId);
            }
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            System.err.println("API Error: " + e.getMessage());
        }
    }
}
```

<!-- tabs:end -->

## Retrieve logs for a specific prompt

To filter logs by prompt, add the `promptId` query parameter to your `GET` request.

<!-- tabs:start -->

#### **cURL**

To make the cURL commands cleaner, set shell variables for the base URL, your token, and the `promptID`.

```bash
# Set shell variables for convenience
BASE_URL="https://promptcrafter-production.up.railway.app"
TOKEN="your-jwt-goes-here" # Replace with your actual token
PROMPT_ID="your-prompt-id-here" # Replace with the ID of the prompt to filter by

# Send the request with the promptId query parameter
curl -X GET "$BASE_URL/logs?promptId=$PROMPT_ID" \
  -H "Authorization: Bearer $TOKEN"
```

#### **Postman**

1. In the **Logs** folder, select the **Retrieve logs by prompt** request.
2. In the **Params** tab, set the value of the `promptId` key to the ID of the prompt you want to see logs for.
3. Click **Send**.

#### **Python**

<!-- tabs:start -->

##### **SDK**

```python
# This snippet uses the PromptCrafter Python SDK
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError

# Replace with your actual JWT token and a prompt ID from your account
token = "your-jwt-goes-here"
prompt_id_to_filter = "prompt104"

client = PromptCrafterClient(token=token)

try:
    prompt_logs = client.get_logs_by_prompt(prompt_id=prompt_id_to_filter)
    print(f"Found {len(prompt_logs)} log(s) for prompt ID '{prompt_id_to_filter}'.")
    for log in prompt_logs:
        print(f"  - Log ID: {log['_id']}, Score: {log.get('score', 'N/A')}")
except PromptCrafterAPIError as e:
    print(f"An API error occurred: {e}")
```

##### **`requests`**

```python
# This snippet uses Python's requests library
import requests

# Replace with your token and the prompt ID to filter by
token = "your-jwt-goes-here"
prompt_id_to_filter = "prompt104"

url = "https://promptcrafter-production.up.railway.app/logs"
headers = {"Authorization": f"Bearer {token}"}
params = {"promptId": prompt_id_to_filter}

try:
    response = requests.get(url, headers=headers, params=params)
    response.raise_for_status()
    prompt_logs = response.json()
    
    print(f"Found {len(prompt_logs)} log(s) for prompt ID '{prompt_id_to_filter}'.")
    for log in prompt_logs:
        print(f"  - Log ID: {log['_id']}, Score: {log.get('score', 'N/A')}")
except requests.exceptions.RequestException as e:
    print(f"An HTTP error occurred: {e}")
```
<!-- tabs:end -->

#### **JavaScript**

<!-- tabs:start -->

##### **SDK**

```javascript
// This snippet uses the PromptCrafter JavaScript SDK
import PromptCrafterClient from './promptcrafter-client.js';

async function getLogsByPrompt() {
    // Replace with your token and the prompt ID to filter by
    const token = 'your-jwt-goes-here';
    const promptIdToFilter = 'prompt104';
    
    const client = new PromptCrafterClient({ token });

    try {
        const promptLogs = await client.getLogsByPrompt(promptIdToFilter);
        console.log(`Found ${promptLogs.length} log(s) for prompt ID '${promptIdToFilter}'.`);
        promptLogs.forEach(log => {
            console.log(`  - Log ID: ${log._id}, Score: ${log.score || 'N/A'}`);
        });
    } catch (error) {
        console.error("Failed to retrieve logs:", error.message);
    }
}


getLogsByPrompt();
```

##### **Fetch**

```javascript
// This snippet uses JavaScript's fetch API
async function getLogsByPrompt() {
    // Replace with your token and the prompt ID to filter by
    const token = "your-jwt-goes-here";
    const promptIdToFilter = "prompt104";

    const url = new URL("https://promptcrafter-production.up.railway.app/logs");
    url.searchParams.append('promptId', promptIdToFilter);
    const headers = { 'Authorization': `Bearer ${token}` };

    try {
        const response = await fetch(url, { headers });
        const promptLogs = await response.json();
        
        if (!response.ok) {
            throw new Error(promptLogs.error || `HTTP error! Status: ${response.status}`);
        }

        console.log(`Found ${promptLogs.length} log(s) for prompt ID '${promptIdToFilter}'.`);
        promptLogs.forEach(log => {
            console.log(`  - Log ID: ${log._id}, Score: ${log.score || 'N/A'}`);
        });
    } catch (error) {
        console.error("Failed to retrieve logs:", error.message);
    }
}

getLogsByPrompt();
```
<!-- tabs:end -->

#### **Go**

```go
// This snippet uses PromptCrafter's Go SDK
package main

import (
  "context"
  "fmt"
  "log"
  "time"
  "github.com/your-org/promptcrafter" // Replace with your actual import path
)

func main() {
    // Replace with your token and a real prompt ID from your account
  token := "your-jwt-goes-here"
  promptIDToFilter := "prompt104"

  client := promptcrafter.NewClient(token)
  ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
  defer cancel()

  promptLogs, err := client.ListLogsByPrompt(ctx, promptIDToFilter)
  if err != nil {
    log.Fatalf("Error retrieving logs for prompt %s: %v", promptIDToFilter, err)
  }

  fmt.Printf("Found %d log(s) for prompt ID '%s'.\n", len(promptLogs), promptIDToFilter)
  for _, lg := range promptLogs {
    score := "N/A"
    if lg.Score != nil {
      score = fmt.Sprintf("%d", *lg.Score)
    }
    fmt.Printf("  - Log ID: %s, Score: %s\n", lg.ID, score)
  }
}
```

#### **Ruby**

```ruby
# This snippet uses the PromptCrafter Ruby SDK
require 'promptcrafter'

# Replace with your token and a real prompt ID from your account
token = 'your-jwt-goes-here'
prompt_id_to_filter = 'prompt104'
client = PromptCrafter::Client.new(access_token: token)

begin
  prompt_logs = client.get_logs_by_prompt(prompt_id: prompt_id_to_filter)
  puts "Found #{prompt_logs.length} log(s) for prompt ID '#{prompt_id_to_filter}'."
  prompt_logs.each do |log|
    puts "  - Log ID: #{log['_id']}, Score: #{log['score'] || 'N/A'}"
  end
rescue PromptCrafter::Error => e
  puts "An API error occurred: #{e.message}"
end
```

#### **Java**

```java
// This snippet uses the PromptCrafter Java SDK
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.PromptCrafterClient.Log;
import java.util.List;

public class GetLogsByPromptExample {
    public static void main(String[] args) {
        // Replace with your token and a real prompt ID from your account
        String token = "your-jwt-goes-here";
        String promptIdToFilter = "prompt104";

        PromptCrafterClient client = PromptCrafterClient.builder()
            .apiToken(token)
            .build();

        try {
            List<Log> promptLogs = client.getLogsByPrompt(promptIdToFilter);
            System.out.println("Found " + promptLogs.size() + " log(s) for prompt ID '" + promptIdToFilter + "'.");
            for (Log log : promptLogs) {
                String score = (log.score != null) ? log.score.toString() : "N/A";
                System.out.println("  - Log ID: " + log._id + ", Score: " + score);
            }
        } catch (PromptCrafterClient.PromptCrafterApiException e) {
            System.err.println("API Error: " + e.getMessage());
        }
    }
}
```

<!-- tabs:end -->

## Response

A successful request returns a `200 OK` status and a JSON array of log objects.

If you request all logs, the response looks like this:

```json
[
  {
    "_id": "log104",
    "promptId": "prompt104",
    "output": "The Industrial Revolution transformed how people lived and worked...",
    "modelUsed": "Claude 3 Sonnet",
    "notes": "Clear, accessible explanation for students. Good tone.",
    "score": 8,
    "createdAt": "2024-06-20T13:13:00Z"
  },
  {
    "_id": "log105",
    "promptId": "prompt102",
    "output": "The e-bike is fantastic! Its long battery life and smooth pedal assist make my commute a joy. I especially love the integrated lights for safety.",
    "modelUsed": "Grok-3 Beta",
    "notes": "Good review, meets all prompt criteria.",
    "score": 9,
    "createdAt": "2024-06-20T14:25:00Z"
  }
]
```

If you filter by `promptId`, the array contains only logs matching that ID. If there are no logs for that prompt, the response is an empty array `[]`.

## What to do if the request doesn't work

Here are issues you might encounter when retrieving logs and how to resolve them.

| Status | Example response | Cause | Solution |
|--------|------------------|--------|----------|
| **400 Bad Request** | `{"error": "`promptId` missing or malformed"}` | The `promptId` query parameter was included but had no value, or the value was invalid. | Ensure the `promptId` is a valid string. If you don't want to filter, remove the parameter entirely. |
| **401 Unauthorized** | `{"error": "Authentication token is missing"}` | The `Authorization` header is missing. | Include `Authorization: Bearer {your_token}` in your request headers with a valid token. |
| **401 Unauthorized** | `{"error": "Authentication token is expired or invalid"}` | The Bearer token is expired or malformed. | Log in again to obtain a new token and update your request. |
| **403 Forbidden** | `{"error": "User is not authorized to view logs for this prompt."}` | You are filtering by a `promptId` that belongs to another user. | Verify the `promptId` is correct and belongs to a prompt you own. |
| **404 Not Found** | `{"error": "No logs found for the specified prompt ID."}` | The prompt ID does not exist, or it exists but has no associated logs. | Double-check the `promptId`. If it is correct, create a log for it first. |
| **500 Internal Server Error** | `{"error": "An unexpected server error occurred"}` | An error occurred on the server. | Retry the request after a short wait. If the error persists, contact support. |

## Next steps

Now that you can review your test history, use that data to improve your prompts.

- Learn how to [Update a prompt](tutorials/update-prompt.md) based on insights from your logs.
- Use the [Search prompts tutorial](tutorials/search-prompts.md) to find related prompts to test.
- For complete details on parameters and endpoints, see the [log resource](reference/resources/log.md) and the [`GET /logs`](reference/endpoints/get-logs.md) endpoint documentation.
