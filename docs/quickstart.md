# Quickstart

This guide shows you how to sign up, log in, and save and retrieve your first prompt in about five minutes. These steps teach you the core pattern of the PromptCrafter API and prepare you to build a powerful, searchable library of reusable prompts for any task that generative AI can tackle.

## What you accomplish

The four steps of this guide take about **5 minutes**.

| Step               | Purpose                                  | Outcome                                   |
|--------------------|------------------------------------------|-------------------------------------------|
| 1. Create your account         | Create your private API account          | You have a user account and can log in    |
| 2. Log in to get your token          | Get your authentication token (JWT)      | You have a bearer token for API requests  |
| 3. Save your first prompt   | Save your first prompt to your library  | You receive a unique prompt ID            |
| 4. Retrieve your prompt | Fetch the prompt to confirm it saved | You've verified the prompt saved    |

## Prerequisites

Use one of these HTTP clients:

* **cURL.**
* **Postman.** Import the [PromptCrafter Postman Collection](postman.md). It contains pre-built requests for every step in this guide, so you can simply replace the pre-filled values with your own and press **Send**.

If you prefer to work in code, skip the cURL and Postman steps and check out the end-to-end [SDK quickstart scripts](#end-to-end-sdk-scripts) for Python, JavaScript, Go, Ruby, and Java.

## Step 1. Create your account

This step establishes your identify and private workspace in the system. Every prompt you save in PromptCrafter is tied to your account so only you can access it.

**Endpoint**: `POST /auth/signup`

<!-- tabs:start -->

#### **cURL**

To keep the cURL commands tidy, first set a shell variable for the base URL.

```bash
BASE_URL="https://promptcrafter-production.up.railway.app"
```

Now, create your account. Replace the `name`, `email`, and `password` values with your own.

```bash
curl -X POST $BASE_URL/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Alice",
    "email": "alice@example.com",
    "password": "password123"
  }'
```

#### **Postman**

In the Postman Collection, use the **Sign up** request in the `Auth` folder of the PromptCrafter API Collection:

1. Click the **Body** tab to see the request's content.
2. In the JSON object, replace the placeholder values `name`, `email`, and `password` with your own.
3. Click **Send**.

<!-- tabs:end -->

A `201 Created` response with the message `"User account created successfully"` confirms your account is ready. (If you receive a `409 Conflict` error, the email address is already in use.)

## Step 2. Log in to get your token

Authentication in PromptCrafter requires a JWT (JSON Web Token), which is your key to access protected resources. After logging in and receiving your token, include it in the `Authorization` header of every subsequent request using the `Bearer` scheme, like this: `Authorization: Bearer {your_token}`.

**Endpoint**: `POST /auth/login`

<!-- tabs:start -->

#### **cURL**

Replace the `email` and `password` values with the credentials you created in **Step 1**.

```bash
curl -X POST $BASE_URL/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "alice@example.com",
    "password": "password123"
  }'
```

A successful request returns a `200 OK` response with a token in the response body, which looks like this:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

Copy the token and save it to a shell variable.

```bash
# Replace the placeholder with the actual token returned by the request.
TOKEN="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

#### **Postman**

In the Postman Collection, expand the `Auth` folder and select the **Log in** request.

1. Click the **Body** tab.
2. Replace the `email` and `password` values with the credentials you created in **Step 1**.
3. Click **Send**. A test script in the collection automatically saves the returned authentication token to a collection variable (`{{token}}`), which handles authentication for all subsequent requests.

<!-- tabs:end -->

## Step 3. Save your first prompt

Now comes the core value of PromptCrafter: storing and organizing your prompts. This step demonstrates the operation you'll use to build your prompt library over time. Each saved prompt includes the prompt text (what you feed into the AI model) and metadata like the intended `model`, descriptive `tags`, and timestamps.  

<details>
  <summary><em>Why does the metadata matter?</em></summary>

> The metadata is what enables you to build an organized, searchable library of prompts instead of a messy folder full of text files. Having your prompts organized in one place becomes invaluable when you're working on multiple projects or need to find that one perfect prompt you wrote months ago. With `tags`, for example, you can:
>
> * **Group by project:** Tag prompts for a particular marketing campaign with `campaign-q3-launch`.
> * **Group by task:** Tag prompts that generate Python code with `python-code-gen`.
> * **Group by purpose:** Tag prompts for summarizing customer feedback with `customer-feedback-summary`.

</details>

**Endpoint**: `POST /prompts`

<!-- tabs:start -->

#### **cURL**

Use the `$TOKEN` variable you set in the previous step.

```bash
curl -X POST $BASE_URL/prompts \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
    "model": "GPT-4o",
    "tags": ["review", "product", "writing"]
  }'
```

#### **Postman**

In the Postman Collection, expand the `Prompts` folder and select the **Save a prompt** request.

1. Click the **Body** tab. The fields are pre-filled with an example prompt. You can either send the request as is or modify the `title`, `content`, `model`, and `tags` values to whatever you'd like.
2. Click **Send**. The collection automatically saves the new prompt's `_id` from the response to a variable (`{{promptId}}`). This saves you from having to manually copy and paste the `_id` for the next step.

<!-- tabs:end -->

A successful request returns the full prompt object, which looks like this:

```json
{
    "_id": "prompt482",
    "ownerId": "user67",
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle...",
    "model": "GPT-4o",
    "tags": [
        "review",
        "product",
        "writing"
    ],
    "createdAt": "2024-06-20T13:20:12.000Z",
    "updatedAt": "2024-06-20T13:20:12.000Z"
}
```

**Important:** If you're using cURL, copy the server-generated `_id` from the response. You need it for the next step to verify that your prompt saved correctly.

## Step 4. Retrieve your prompt

This final step confirms your prompt saved correctly by retrieving it with its unique ID. This operation is the method you use to integrate your saved prompts into tools and application.

<details>
  <summary><em>Where do you use this operation in the real world?</em></summary>

> Retrieving a prompt by its ID is the key to building tools like:
>
> - A **content generation app** where clicking a "Summarize Article" button triggers a `GET /prompts/{summary-prompt-id}` request to fetch the appropriate prompt.
> - A **prompt evaluation suite** that programmatically loops through a list of prompt IDs to test and compare the outputs from each one.

</details><p></p>

**Endpoint**: `GET /prompts/{id}`

<!-- tabs:start -->

#### **cURL**

First, copy the `_id` from the previous step's response and save it to a variable.

```bash
# Replace the placeholder with the actual _id from the previous response.
PROMPT_ID="prompt482"
```

Now, use the variables for `BASE_URL`, `PROMPT_ID`, and `TOKEN`.

```bash
curl -X GET $BASE_URL/prompts/$PROMPT_ID \
  -H "Authorization: Bearer $TOKEN"
```

#### **Postman**

In the Postman Collection, expand the `Prompts` folder and select the **Retrieve a prompt by ID** request.

1. Notice that the request URL already includes the `{{promptId}}` variable that was automatically saved in the previous step.
2. You don't need to modify anything. Simply click **Send** to fetch the prompt you just created and verify it saved correctly.

<!-- tabs:end -->

The response should return the same prompt object you created in **Step 3**, confirming that it's safely stored.

### What if a request doesn't work?

<details>
  <summary><strong>Click to troubleshoot</strong></summary>

Use this table to diagnose and fix errors you might encounter. The most common issues involve an incorrect token or ID, so check those first.

| Status                 | Example response                                       | Cause                                                                                                                               | Solution                                                                                                                                                              |
| ---------------------- | ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **409 Conflict**       | `{"error": "Email already in use."}`                   | **During signup (Step 1):** The email address you provided is already registered.                                                   | Use a different email address, or if the account is yours, skip to **Step 2** and log in with your existing credentials.                                              |
| **401 Unauthorized**   | `{"error": "Incorrect email or password."}`            | **During login (Step 2):** The credentials you provided are incorrect.                                                              | Double-check your email and password and try logging in again.                                                                                                        |
| **401 Unauthorized**   | `{"error": "Authentication token is missing"}`         | **During Steps 3 & 4:** The `Authorization` header was missing, or the `$TOKEN` shell variable was not set or copied correctly after login. | Make sure you copied the full token from the login response and set the `TOKEN` variable correctly. Verify your cURL command includes `-H "Authorization: Bearer $TOKEN"`. |
| **404 Not Found**      | `{"error": "No prompt found with the specified ID."}`    | **During Step 4:** The `PROMPT_ID` variable was not set, or the `_id` from Step 3 was copied incorrectly.                             | Copy the exact `_id` string from the response you received in Step 3 and ensure the `PROMPT_ID` variable is set correctly before sending the `GET` request.            |
| **400 Bad Request**    | Varies; may include `{"error": "Submitted data is invalid."}` | **During Step 3:** The JSON in your cURL command is malformed. | Carefully check the JSON in the `-d` block of your cURL command for syntax errors and typos.                                     |

</details>

### End-to-end SDK scripts

<!-- tabs:start -->

#### **Overview**

These complete, runnable scripts for the five PromptCrafter SDKs perform all four steps of the Quickstart: sign up, log in, save a prompt, and retrieve it. Simply replace the placeholder credentials in the code  with your own, and execute.

#### **Python**

Save this script as `quickstart.py` and run it from your terminal.

```python
# quickstart.py
from promptcrafter import PromptCrafterClient, PromptCrafterAPIError, ConflictError

def main():
    """Runs the complete PromptCrafter API quickstart workflow."""
    # --- IMPORTANT ---
    # Replace these placeholders with your own credentials.
    # For a new account, choose any valid email and a secure password.
    user_name = "Python Quickstart User"
    user_email = "YOUR_EMAIL_HERE"
    user_password = "YOUR_PASSWORD_HERE"

    if user_email == "YOUR_EMAIL_HERE" or user_password == "YOUR_PASSWORD_HERE":
        print("Please replace 'YOUR_EMAIL_HERE' and 'YOUR_PASSWORD_HERE' in the script.")
        return

    print("--- Using your credentials ---")
    print(f"Email: {user_email}\n")

    client = PromptCrafterClient()

    # Step 1: Attempt to sign up. If the user already exists, continue to login.
    try:
        print("Step 1: Signing up...")
        client.signup(name=user_name, email=user_email, password=user_password)
        print("-> Signup successful!")
    except ConflictError:
        print(f"-> Account with email '{user_email}' already exists. Proceeding to login.")
    except PromptCrafterAPIError as e:
        print(f"\nAn API error occurred during signup: {e}")
        return

    # All subsequent steps are wrapped in a single try block.
    try:
        # Step 2: Log in with the credentials to get a token.
        print("\nStep 2: Logging in...")
        token = client.login(email=user_email, password=user_password)
        client.set_token(token)
        print("-> Login successful. Token is set on the client.")

        # Step 3: Create a new prompt.
        print("\nStep 3: Creating a new prompt...")
        prompt_data = {
            "title": "Python SDK Test Prompt",
            "content": "Generate a Python script to automate a boring task.",
            "model": "GPT-4o",
            "tags": ["python", "sdk", "automation"]
        }
        new_prompt = client.create_prompt(**prompt_data)
        prompt_id = new_prompt["_id"]
        print(f"-> Prompt created! ID: {prompt_id}")

        # Step 4: Retrieve the prompt by its ID to verify.
        print(f"\nStep 4: Retrieving prompt '{prompt_id}'...")
        retrieved_prompt = client.get_prompt(prompt_id=prompt_id)
        print("-> Prompt retrieved successfully!")
        print(f"   Title: {retrieved_prompt['title']}")
        print(f"   Model: {retrieved_prompt['model']}")
        
        print("\n--- Quickstart Complete! ---")

    except PromptCrafterAPIError as e:
        print(f"\nAn API error occurred: {e}")

if __name__ == "__main__":
    main()
```

#### **JavaScript**

Save this as `quickstart.js` and run it with `node quickstart.js`.

```javascript
// quickstart.js
import PromptCrafterClient, { PromptCrafterError } from './javascript_sdk.js';

async function main() {
    // --- IMPORTANT ---
    // Replace these placeholders with your own credentials.
    const userName = "JavaScript Quickstart User";
    const userEmail = "YOUR_EMAIL_HERE";
    const userPassword = "YOUR_PASSWORD_HERE";

    if (userEmail === "YOUR_EMAIL_HERE" || userPassword === "YOUR_PASSWORD_HERE") {
        console.log("Please replace 'YOUR_EMAIL_HERE' and 'YOUR_PASSWORD_HERE' in the script.");
        return;
    }

    console.log("--- Using your credentials ---");
    console.log(`Email: ${userEmail}\n`);

    const client = new PromptCrafterClient({});

    // Step 1: Attempt to sign up.
    try {
        console.log("Step 1: Signing up...");
        await client.signup({ name: userName, email: userEmail, password: userPassword });
        console.log("-> Signup successful!");
    } catch (error) {
        if (error.status === 409) { // 409 Conflict
            console.log(`-> Account with email '${userEmail}' already exists. Proceeding to login.`);
        } else {
            console.error(`\nAn API error occurred during signup:`, error);
            return;
        }
    }
    
    // All subsequent steps are wrapped in a single try block.
    try {
        // Step 2: Log in to get the authentication token.
        console.log("\nStep 2: Logging in...");
        const { token } = await client.login({ email: userEmail, password: userPassword });
        client.setToken(token);
        console.log("-> Login successful. Token is set.");

        // Step 3: Create a new prompt.
        console.log("\nStep 3: Creating a new prompt...");
        const newPrompt = await client.createPrompt({
            title: "JavaScript SDK Test Prompt",
            content: "Generate a JavaScript snippet to fetch data from a REST API.",
            model: "Claude 3 Sonnet",
            tags: ["javascript", "sdk", "api"]
        });
        const promptId = newPrompt._id;
        console.log(`-> Prompt created! ID: ${promptId}`);

        // Step 4: Retrieve the prompt by ID to verify.
        console.log(`\nStep 4: Retrieving prompt '${promptId}'...`);
        const retrievedPrompt = await client.getPromptById(promptId);
        console.log("-> Prompt retrieved successfully!");
        console.log(`   Title: ${retrievedPrompt.title}`);
        
        console.log("\n--- Quickstart Complete! ---");

    } catch (error) {
        if (error instanceof PromptCrafterError) {
            console.error(`\nAPI Error (${error.status}): ${error.message}`);
        } else {
            console.error("\nAn unexpected error occurred:", error);
        }
    }
}

main();
```

#### **Go**

Save this as `quickstart.go` and run with `go run quickstart.go`.

```go
// quickstart.go
package main

import (
  "context"
  "fmt"
  "log"
  "time"

  promptcrafter "path/to/your/promptcrafter" // <-- UPDATE THIS IMPORT PATH
)

func main() {
    // --- IMPORTANT ---
    // Replace these placeholders with your credentials.
    const userName = "Go Quickstart User"
    const userEmail = "YOUR_EMAIL_HERE"
    const userPassword = "YOUR_PASSWORD_HERE"

    if userEmail == "YOUR_EMAIL_HERE" || userPassword == "YOUR_PASSWORD_HERE" {
        log.Fatal("Please replace 'YOUR_EMAIL_HERE' and 'YOUR_PASSWORD_HERE' in the script.")
    }

  fmt.Println("--- Using your credentials ---")
  fmt.Printf("Email: %s\n\n", userEmail)

  client := promptcrafter.NewClient("")
  ctx, cancel := context.WithTimeout(context.Background(), 20*time.Second)
  defer cancel()

  // Step 1: Attempt to sign up.
  fmt.Println("Step 1: Signing up...")
  _, err := client.Signup(ctx, &promptcrafter.AuthSignupRequest{
    Name:     userName,
    Email:    userEmail,
    Password: userPassword,
  })
  if err != nil {
    if apiErr, ok := err.(*promptcrafter.Error); ok && apiErr.StatusCode == 409 {
      fmt.Printf("-> Account with email '%s' already exists. Proceeding to login.\n", userEmail)
    } else {
      log.Fatalf("Signup failed: %v", err)
    }
  } else {
    fmt.Println("-> Signup successful!")
  }

  // Step 2: Log in to get the token.
  fmt.Println("\nStep 2: Logging in...")
  loginRes, err := client.Login(ctx, &promptcrafter.AuthLoginRequest{
    Email:    userEmail,
    Password: userPassword,
  })
  if err != nil {
    log.Fatalf("Login failed: %v", err)
  }
  client.BearerToken = loginRes.Token
  fmt.Println("-> Login successful. Token is set.")

  // Step 3: Create a prompt.
  fmt.Println("\nStep 3: Creating a new prompt...")
  newPrompt, err := client.CreatePrompt(ctx, &promptcrafter.PromptCreateRequest{
    Title:   "Go SDK Test Prompt",
    Content: "Generate a Go function for concurrent HTTP requests.",
    Model:   "GPT-4",
    Tags:    []string{"go", "sdk", "concurrency"},
  })
  if err != nil {
    log.Fatalf("Failed to create prompt: %v", err)
  }
  fmt.Printf("-> Prompt created! ID: %s\n", newPrompt.ID)

  // Step 4: Retrieve the prompt.
  fmt.Printf("\nStep 4: Retrieving prompt '%s'...\n", newPrompt.ID)
  retrievedPrompt, err := client.GetPrompt(ctx, newPrompt.ID)
  if err != nil {
    log.Fatalf("Failed to retrieve prompt: %v", err)
  }
  fmt.Println("-> Prompt retrieved successfully!")
  fmt.Printf("   Title: %s\n", retrievedPrompt.Title)
  
  fmt.Println("\n--- Quickstart Complete! ---")
}
```

#### **Ruby**

Save this file as `quickstart.rb` and run with `ruby quickstart.rb`.

```ruby
# quickstart.rb
require_relative 'lib/promptcrafter' # Adjust path to your SDK file

def main
  # --- IMPORTANT ---
  # Replace these placeholders with your own credentials.
  user_name = "Ruby Quickstart User"
  user_email = "YOUR_EMAIL_HERE"
  user_password = "YOUR_PASSWORD_HERE"

  if user_email == "YOUR_EMAIL_HERE" || user_password == "YOUR_PASSWORD_HERE"
    puts "Please replace 'YOUR_EMAIL_HERE' and 'YOUR_PASSWORD_HERE' in the script."
    return
  end
  
  puts "--- Using your credentials ---"
  puts "Email: #{user_email}\n\n"

  client = PromptCrafter::Client.new

  # Step 1: Attempt to sign up.
  puts "Step 1: Signing up..."
  begin
    client.signup(name: user_name, email: user_email, password: user_password)
    puts "-> Signup successful!"
  rescue PromptCrafter::ConflictError
    puts "-> Account with email '#{user_email}' already exists. Proceeding to login."
  rescue PromptCrafter::Error => e
    puts "\nAn API error occurred during signup: #{e.message}"
    return
  end
  
  # All subsequent steps are wrapped in a single try block.
  begin
    # Step 2: Log in to get the token.
    puts "\nStep 2: Logging in..."
    token = client.login(email: user_email, password: user_password)
    client.config.access_token = token
    puts "-> Login successful. Token is set."

    # Step 3: Create a new prompt.
    puts "\nStep 3: Creating a new prompt..."
    new_prompt = client.create_prompt(
      title: 'Ruby SDK Test Prompt',
      content: 'Generate a Ruby script for parsing a CSV file.',
      model: 'Claude 3 Opus',
      tags: ['ruby', 'sdk', 'csv']
    )
    prompt_id = new_prompt['id']
    puts "-> Prompt created! ID: #{prompt_id}"

    # Step 4: Retrieve the prompt to verify.
    puts "\nStep 4: Retrieving prompt '#{prompt_id}'..."
    retrieved_prompt = client.get_prompt(id: prompt_id)
    puts "-> Prompt retrieved successfully!"
    puts "   Title: #{retrieved_prompt['title']}"

    puts "\n--- Quickstart Complete! ---"
  rescue PromptCrafter::Error => e
    puts "\nAn API error occurred: #{e.message}"
  end
end

main
```

#### **Java**

Save this as `Quickstart.java` and compile/run.

```java
// Quickstart.java
import com.promptcrafter.PromptCrafterClient;
import com.promptcrafter.model.*;
import com.promptcrafter.exception.PromptCrafterApiException;
import java.util.List;

public class Quickstart {

    public static void main(String[] args) {
        // --- IMPORTANT ---
        // Replace these placeholders with your own credentials.
        String userName = "Java Quickstart User";
        String userEmail = "YOUR_EMAIL_HERE";
        String userPassword = "YOUR_PASSWORD_HERE";

        if (userEmail.equals("YOUR_EMAIL_HERE") || userPassword.equals("YOUR_PASSWORD_HERE")) {
            System.out.println("Please replace 'YOUR_EMAIL_HERE' and 'YOUR_PASSWORD_HERE' in the script.");
            return;
        }

        System.out.println("--- Using your credentials ---");
        System.out.println("Email: " + userEmail + "\n");
        
        PromptCrafterClient client = PromptCrafterClient.builder().build();

        // Step 1: Attempt to sign up.
        try {
            System.out.println("Step 1: Signing up...");
            client.signup(new SignupRequest(userName, userEmail, userPassword));
            System.out.println("-> Signup successful!");
        } catch (PromptCrafterApiException e) {
            if (e.getStatusCode() == 409) { // 409 Conflict
                System.out.println("-> Account with email '" + userEmail + "' already exists. Proceeding to login.");
            } else {
                System.err.println("\nAn API error occurred during signup: " + e.getMessage());
                return;
            }
        } catch (Exception e) {
            System.err.println("\nAn unexpected error occurred during signup: " + e.getMessage());
            return;
        }

        // All subsequent steps are wrapped in a single try block.
        try {
            // Step 2: Log in to get the token and build an authenticated client.
            System.out.println("\nStep 2: Logging in...");
            LoginResponse loginRes = client.login(new LoginRequest(userEmail, userPassword));
            
            PromptCrafterClient authedClient = PromptCrafterClient.builder()
                .apiToken(loginRes.getToken())
                .build();
            
            System.out.println("-> Login successful.");
            
            // Step 3: Create a new prompt.
            System.out.println("\nStep 3: Creating a new prompt...");
            PromptCreate promptToCreate = new PromptCreate(
                "Java SDK Test Prompt",
                "Generate a Java class to interact with a database using JDBC.",
                "Gemini Pro",
                List.of("java", "sdk", "database")
            );
            Prompt newPrompt = authedClient.createPrompt(promptToCreate);
            String promptId = newPrompt.getId();
            System.out.println("-> Prompt created! ID: " + promptId);

            // Step 4: Retrieve the prompt to verify.
            System.out.println("\nStep 4: Retrieving prompt '" + promptId + "'...");
            Prompt retrievedPrompt = authedClient.getPromptById(promptId);
            System.out.println("-> Prompt retrieved successfully!");
            System.out.println("   Title: " + retrievedPrompt.getTitle());
            
            System.out.println("\n--- Quickstart Complete! ---");

        } catch (PromptCrafterApiException e) {
            System.err.println("\nAPI Error (" + e.getStatusCode() + "): " + e.getMessage());
        } catch (Exception e) {
            System.err.println("\nAn unexpected error occurred: " + e.getMessage());
        }
    }
}
```

<!-- tabs:end -->

## You have now...
   
  ✔️ Created your account and obtained authentication  
  ✔️ Saved a reusable prompt with metadata  
  ✔️ Retrieved your prompt by ID

## Ready to dive deeper?

Here's what to explore next:
* **[Log generated outputs](tutorials/test-prompt.md):** Learn to test and score the outputs of your prompts.
* **[Search your library](tutorials/search-prompts.md):** Discover how to find prompts by keyword, tags, or content as your collection grows.
* **[Explore the full API Reference](reference/index.md):** Detailed documentation for all endpoints.
