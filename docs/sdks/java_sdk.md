# PromptCrafter Java SDK

[Download Java SDK](link)  
[Technical addendum](link)  

## Overview

The **PromptCrafter Java SDK** provides a simple and robust interface for interacting with the [PromptCrafter API](https://promptcrafter-production.up.railway.app). It enables Java applications to automate prompt management, perform full-text search, and log AI completions for generative AI workflows.

**Key features:**
- Full prompt CRUD operations (create, retrieve, update, delete)
- Log output and feedback for any prompt
- Search prompts by keywords/tags/content
- Simple authentication using JWT Bearer tokens
- Rich Java POJOs for resource modeling
- Synchronous and asynchronous (CompletableFuture) methods
- Configurable HTTP timeouts and base URLs
- Minimal external dependencies (Java 11+)

---

## Installation

### Prerequisites

- **Java 11** or higher.
- (Recommended) [Jackson Databind](https://github.com/FasterXML/jackson-databind) for JSON parsing (SDK uses Jackson for deserialization.)

### Maven

```xml
<dependency>
  <groupId>com.promptcrafter</groupId>
  <artifactId>promptcrafter-java-sdk</artifactId>
  <version>{LATEST_VERSION}</version>
</dependency>
```

### Gradle

```groovy
implementation 'com.promptcrafter:promptcrafter-java-sdk:{LATEST_VERSION}'
```

*Replace `{LATEST_VERSION}` with the latest release.*

### JAR

Download the JAR and add it to your project’s classpath. (Manual installation; see [Releases](https://github.com/promptcrafter/sdk-java/releases) if available.)

### Dependencies

- Java 11+
- Jackson Databind (at runtime)

---

## Authentication

PromptCrafter requires all API requests to be authenticated using a **JWT Bearer token** in the `Authorization` header.

### Obtain a Bearer Token

- **Sign up:**  
  Register a new account and receive a JWT token.

- **Log in:**  
  Log in with your email and password to receive a JWT token.

```java
// Sign up (returns AuthResponse with token)
PromptCrafterClient.AuthResponse signupResponse = PromptCrafterClient.signUp(
   "https://promptcrafter-production.up.railway.app", 
   "Your Name", 
   "you@example.com", 
   "yourPassword"
);
String jwt = signupResponse.token;

// Log in (returns AuthResponse with token)
PromptCrafterClient.AuthResponse loginResponse = PromptCrafterClient.login(
   "https://promptcrafter-production.up.railway.app", 
   "you@example.com", 
   "yourPassword"
);
String jwt = loginResponse.token;
```

### Supplying the Token

When building your client:

```java
PromptCrafterClient client = PromptCrafterClient.builder()
    .apiToken(jwt)
    .build();
```

You may customize the base URL or timeout as needed:

```java
PromptCrafterClient client = PromptCrafterClient.builder()
    .apiToken(jwt)
    .apiBaseUrl("https://promptcrafter-production.up.railway.app")
    .timeoutSeconds(15)
    .build();
```

---

## Usage Examples

> All examples assume you have obtained a valid JWT token.

### Retrieve All Prompts

**Synchronous**
```java
List<PromptCrafterClient.Prompt> prompts = client.getAllPrompts();
```

**Asynchronous**
```java
CompletableFuture<List<PromptCrafterClient.Prompt>> futurePrompts = client.getAllPromptsAsync();
futurePrompts.thenAccept(prompts -> {
    prompts.forEach(prompt -> System.out.println(prompt.title));
});
```

### Create a New Prompt

```java
PromptCrafterClient.PromptCreate promptToCreate = new PromptCrafterClient.PromptCreate(
    "Newsletter Subject Lines",
    "Generate subject lines for a tech product launch email.",
    "Claude 4.0 Sonnet",
    Arrays.asList("email", "marketing", "tech")
);
PromptCrafterClient.Prompt createdPrompt = client.createPrompt(promptToCreate);
System.out.println("New prompt ID: " + createdPrompt._id);
```

### Retrieve a Prompt by ID

```java
PromptCrafterClient.Prompt prompt = client.getPromptById("prompt112");
```

### Update a Prompt

```java
Map<String, Object> updateFields = new HashMap<>();
updateFields.put("model", "GPT-4 Turbo");
updateFields.put("tags", Arrays.asList("email", "customer", "apology"));
PromptCrafterClient.Prompt updatedPrompt = client.updatePrompt("prompt112", updateFields);
```

### Delete a Prompt

```java
client.deletePrompt("prompt112");
```

### Search Prompts

```java
List<PromptCrafterClient.Prompt> searchResults = client.searchPrompts("email subject");
```

### Log a Model Output

```java
PromptCrafterClient.LogCreate logEntry = new PromptCrafterClient.LogCreate(
    "prompt112", // promptId
    "Generated subject line output", // output
    "Clear and creative.",           // notes (optional)
    "GPT-4o",                        // modelUsed
    9                                // score (optional)
);
PromptCrafterClient.Log newLog = client.createLog(logEntry);
```

### Retrieve All Logs

```java
List<PromptCrafterClient.Log> logs = client.getAllLogs();
```

### Retrieve Logs for a Specific Prompt

```java
List<PromptCrafterClient.Log> promptLogs = client.getLogsByPromptId("prompt112");
```

### Delete a Log

```java
client.deleteLog("log104");
```

---

## SDK Reference

### PromptCrafterClient

#### Builder

| Method                         | Type                 | Description                                            | Required | Default                         |
|--------------------------------|----------------------|--------------------------------------------------------|----------|---------------------------------|
| `apiBaseUrl(String url)`       | Builder              | Set the API base URL.                                  | No       | https://promptcrafter-production.up.railway.app |
| `apiToken(String token)`       | Builder              | Set JWT Bearer token for authentication.               | Yes      |                                 |
| `timeoutSeconds(int seconds)`  | Builder              | Set the request timeout (in seconds).                  | No       | 30                              |
| `build()`                      | PromptCrafterClient  | Construct the configured client instance.              | Yes      |                                 |

#### Static Methods for Auth

| Method | Params | Returns | Description |
|--------|--------|---------|-------------|
| `signUp(String apiBaseUrl, String name, String email, String password)` | All String | `AuthResponse` | Sign up for an account; returns token and user info |
| `login(String apiBaseUrl, String email, String password)` | All String | `AuthResponse` | Log in with credentials; returns token and user info |

#### Instance Methods

**Prompts**

| Method | Params | Returns | Description |
|--------|--------|---------|-------------|
| `getAllPrompts()` |  | `List<Prompt>` | List all prompts for the authenticated user |
| `getAllPromptsAsync()` |  | `CompletableFuture<List<Prompt>>` | List all prompts (async) |
| `getPromptById(String promptId)` | `promptId: String (required)` | `Prompt` | Get a single prompt by ID |
| `createPrompt(PromptCreate promptCreate)` | `PromptCreate (required)` | `Prompt` | Create a new prompt |
| `updatePrompt(String promptId, Map<String, Object> update)` | `promptId: String (required), update: fields to update` | `Prompt` | Update fields for a prompt |
| `deletePrompt(String promptId)` | `promptId: String (required)` | `void` | Delete a prompt by ID |
| `searchPrompts(String query)` | `query: String (required)` | `List<Prompt>` | Full-text search prompts |

**Logs**

| Method | Params | Returns | Description |
|--------|--------|---------|-------------|
| `getAllLogs()` |  | `List<Log>` | List all logs for your account |
| `getLogsByPromptId(String promptId)` | `promptId: String (required)` | `List<Log>` | Logs for the given prompt |
| `createLog(LogCreate logCreate)` | `LogCreate (required)` | `Log` | Log new model output/completion |
| `deleteLog(String logId)` | `logId: String (required)` | `void` | Delete a log by ID |

**Error Handling**

| Exception Type | Description | Best Practice |
|---|---|---|
| PromptCrafterClient.PromptCrafterApiException | All checked errors from API interaction | Catch on all SDK calls (sync/async) and inspect status/message |

---

## Error Handling

All SDK methods throw `PromptCrafterApiException` (checked) for API errors or IO issues.

**Error Example:**

```java
try {
    client.deletePrompt("prompt112");
} catch (PromptCrafterClient.PromptCrafterApiException ex) {
    System.err.println("Failed: " + ex.getMessage());
    int status = ex.getStatusCode(); // HTTP code if available
}
```

**Common error codes and meanings:**

| HTTP Status | Meaning | Recommended Handling |
|-------------|---------|---------------------|
| 400         | Invalid request (missing/invalid fields, malformed JSON) | Check parameters and request shape |
| 401         | Unauthorized (missing/invalid/expired token) | Refresh/log in again |
| 403         | Forbidden (no access to resource) | Check resource ownership/perms |
| 404         | Not found (resource does not exist) | Verify ID or existence |
| 409         | Conflict (signup: email already used) | Use a unique email / handle conflicts |
| 415         | Unsupported Media (bad Content-Type) | Ensure Content-Type is `application/json` |
| 500         | Server error | Retry or contact support |

Full error response bodies are parsed when possible, and you may inspect `PromptCrafterApiException.getStatusCode()`.

---

## Data Models

| Class                       | Field              | Type                         | Required | Description                                    |
|-----------------------------|--------------------|------------------------------|----------|------------------------------------------------|
| **Prompt**                  | `_id`              | String                       | Yes      | Unique identifier for prompt                   |
|                             | `ownerId`          | String                       | Yes      | Owner user ID                                  |
|                             | `title`            | String                       | Yes      | Prompt title                                   |
|                             | `content`          | String                       | Yes      | Prompt text                                    |
|                             | `model`            | String                       | Yes      | Model name                                     |
|                             | `tags`             | List&lt;String&gt;           | No       | User-defined tags                              |
|                             | `createdAt`        | String (ISO8601)             | Yes      | Creation timestamp                             |
|                             | `updatedAt`        | String (ISO8601)             | Yes      | Last update timestamp                          |
| **PromptCreate**            | `title`            | String                       | Yes      | Title                                          |
|                             | `content`          | String                       | Yes      | Content                                        |
|                             | `model`            | String                       | Yes      | Model name                                     |
|                             | `tags`             | List&lt;String&gt;           | No       | Tags                                           |
| **Log**                     | `_id`              | String                       | Yes      | Unique log ID                                  |
|                             | `promptId`         | String                       | Yes      | Prompt ID linked to this log                   |
|                             | `output`           | String                       | Yes      | Model output                                   |
|                             | `notes`            | String                       | No       | User notes                                     |
|                             | `modelUsed`        | String                       | Yes      | Model name used for output                     |
|                             | `score`            | Integer                      | No       | User score (1-10)                              |
|                             | `createdAt`        | String (ISO8601)             | Yes      | Timestamp                                      |
| **LogCreate**               | `promptId`         | String                       | Yes      | Prompt ID for reference                        |
|                             | `output`           | String                       | Yes      | Output text                                    |
|                             | `notes`            | String                       | No       | Notes                                          |
|                             | `modelUsed`        | String                       | Yes      | Model name                                     |
|                             | `score`            | Integer                      | No       | Score                                          |
| **AuthResponse**            | `token`            | String                       | Yes      | JWT authentication token                       |
|                             | `name`             | String                       | No       | Name of user (optional in response)            |
|                             | `email`            | String                       | No       | User email (optional in response)              |
|                             | `_id`              | String                       | No       | User ID (optional in response)                 |

---

## Advanced Usage

### Custom HTTP Client or Timeouts

You may specify the API base URL and the request timeout (in seconds):

```java
PromptCrafterClient client = PromptCrafterClient.builder()
    .apiToken(jwt)
    .apiBaseUrl("https://your-custom-base-url")
    .timeoutSeconds(10)
    .build();
```

### Async Operations

Use methods ending with `Async` for non-blocking, CompletableFuture-based async support.

### Pagination

Currently, all `getAllPrompts` and `getAllLogs` endpoints return all resources for the user account. Pagination options are not exposed in the current release. (Check for future API changes.)

### Using with Custom JSON Backend

The SDK internally uses Jackson for JSON serialization/deserialization, but you can substitute Jackson with your own logic if desired by editing the `Json` utility class in the SDK.

---

## FAQ

### How do I get my JWT Bearer token?

Sign up or log in with the static methods `PromptCrafterClient.signUp()` or `PromptCrafterClient.login()`, as shown above. The returned `AuthResponse` will contain your token.

### Can I use this SDK behind a proxy or with a custom HttpClient?

The SDK constructs its own `HttpClient` with the specified timeout and does not externalize proxy/custom client support. You may fork the SDK to configure custom networking.

### What is the default timeout?

30 seconds. Override via `.timeoutSeconds(int)` in the builder.

### How do I handle errors and status codes?

All checked exceptions are thrown as `PromptCrafterApiException`. Inspect `getMessage()` and `getStatusCode()` to diagnose errors.

### What is the support status for third-party JSON?

The SDK is compatible with Jackson for JSON. You may switch to Gson or other libraries by editing the `Json` helper; see the SDK source for details.

---

## Contact/Support

- **Issues & Bugs:**  
  File an issue on [GitHub](https://github.com/promptcrafter/sdk-java/issues) or email [support@promptcrafter.com](mailto:support@promptcrafter.com).

- **Feature Requests:**  
  Open a ticket on the issues board or contact the team at [PromptCrafter](https://promptcrafter.com/contact).

- **Documentation:**  
  See the [PromptCrafter API reference](https://promptcrafter-production.up.railway.app).
