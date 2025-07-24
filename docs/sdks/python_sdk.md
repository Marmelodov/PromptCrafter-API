# PromptCrafter API Python SDK Documentation

[Download Python SDK](path)  
[Technical Addendum: Automated SDK Generation](../sdk/README.md)

## 1. Overview

Welcome to the official Python SDK for the PromptCrafter API. This library provides a convenient and idiomatic Python interface for developers to interact with the PromptCrafter backend service.

The PromptCrafter API is designed to help developers, prompt engineers, and AI enthusiasts store, organize, test, and evaluate generative AI prompts[1]. This SDK simplifies integration by handling authentication, request signing, and response parsing, allowing you to focus on building your application.

Key features of the SDK include:
*   **Full CRUD Operations:** Create, retrieve, update, and delete prompts and performance logs[1][2].
*   **Simplified Authentication:** Easy-to-use methods for user signup and login to manage JWT tokens[2].
*   **Robust Search:** A dedicated method to perform full-text searches across your entire prompt library[1][2].
*   **Comprehensive Error Handling:** A full suite of custom exceptions to handle API errors gracefully[2].
*   **Pythonic Interface:** Interact with API resources as Python objects and dictionaries.

## 2. Installation

### Prerequisites
*   Python 3.x
*   The `requests` library (will be installed automatically as a dependency)

### Installation with pip
The SDK can be installed directly from PyPI using pip. We recommend using a virtual environment to manage your project's dependencies.

```bash
pip install requests
# Note: The SDK is not yet published to PyPI. 
# For now, you can use the python_sdk.py file directly in your project.
# A pip install command will be provided here once the package is available.
# pip install promptcrafter-sdk 
```

### Verifying the Installation
To ensure the SDK is installed correctly and accessible in your environment, run the following Python code:

```python
try:
    from python_sdk import PromptCrafterClient
    print("PromptCrafter SDK imported successfully!")
except ModuleNotFoundError:
    print("Error: The PromptCrafter SDK is not installed or not in the Python path.")
    print("Please ensure you have placed python_sdk.py in your project directory or installed it via pip.")
```

If you encounter a `ModuleNotFoundError`, it typically means:
1.  The package was installed in a different Python environment than the one you are running your script with.
2.  Your system's `pip` is not linked to the `python` executable you are using. Ensure both are from the same installation.

## 3. Quick Start (5 Minutes)

This end-to-end example demonstrates the complete workflow for a new user: signing up, creating a prompt, logging a test output for that prompt, and then cleaning up the created resources.

This script is production-ready and includes full error handling.

```python
import os
from python_sdk import (
    PromptCrafterClient,
    AuthenticationError,
    BadRequestError,
    ConflictError,
    NotFoundError,
)

# --- 1. Initialize the Client ---
# The client manages your authentication token and all API requests.
client = PromptCrafterClient()

# --- 2. Authenticate ---
# For a first-time user, we'll sign up for a new account.
# The API returns a JWT token, which the client's `signup` method automatically
# stores for all subsequent requests.
try:
    print("Attempting to sign up a new user...")
    # Use environment variables or a secrets manager for credentials in a real app.
    user_name = "Jane Doe"
    user_email = "jane.doe@example.com"
    user_password = "a-very-secure-password-123"
    
    # The signup method returns a token, but also sets it on the client instance.
    token = client.signup(name=user_name, email=user_email, password=user_password)
    print("Sign up successful! Client is now authenticated.")

except ConflictError:
    # If the email is already in use, we'll log in instead.
    print("User already exists. Attempting to log in...")
    try:
        token = client.login(email=user_email, password=user_password)
        print("Login successful! Client is now authenticated.")
    except AuthenticationError:
        print("Login failed. Please check your credentials.")
        exit() # Exit if authentication fails
except BadRequestError as e:
    print(f"Sign up failed due to invalid data: {e}")
    exit()

# --- 3. Create a New Prompt ---
# Let's create our first prompt. The `create_prompt` method returns a dictionary
# representing the newly created prompt, including its unique `_id`.
new_prompt = None
try:
    print("\nCreating a new prompt...")
    new_prompt = client.create_prompt(
        title="E-commerce Slogan Generator",
        content="Generate ten catchy slogans for a new online store selling handmade pottery.",
        model="GPT-4o",
        tags=["e-commerce", "writing", "marketing"]
    )
    prompt_id = new_prompt['_id']
    print(f"Prompt created successfully with ID: {prompt_id}")

except BadRequestError as e:
    print(f"Failed to create prompt due to invalid input: {e}")
    exit()
except AuthenticationError:
    print("Authentication failed. Your token may be invalid or expired.")
    exit()

# --- 4. Log a Test Output ---
# Now, we'll log a generated output for the prompt we just created.
# This is useful for tracking performance and comparing results.
new_log = None
if new_prompt:
    try:
        print("\nLogging a test output for the new prompt...")
        new_log = client.create_log(
            prompt_id=new_prompt['_id'],
            output="1. 'Crafted by Hand, Designed for Your Home.' 2. 'The Art of Clay, Delivered to You.'",
            model_used="GPT-4o",
            notes="Initial test, output looks good but a bit generic.",
            score=8
        )
        log_id = new_log['_id']
        print(f"Log created successfully with ID: {log_id}")

    except NotFoundError:
        print(f"Failed to create log: Prompt with ID {new_prompt['_id']} not found.")
    except BadRequestError as e:
        print(f"Failed to create log due to invalid input: {e}")

# --- 5. Retrieve and Verify ---
# Let's fetch the prompt and its logs to confirm they were saved correctly.
if new_prompt:
    try:
        print(f"\nRetrieving prompt '{new_prompt['_id']}'...")
        retrieved_prompt = client.get_prompt(new_prompt['_id'])
        print("Retrieved Prompt Content:", retrieved_prompt['content'])

        print(f"\nRetrieving logs for prompt '{new_prompt['_id']}'...")
        retrieved_logs = client.get_logs_by_prompt(new_prompt['_id'])
        print(f"Found {len(retrieved_logs)} log(s).")
        for log in retrieved_logs:
            print(f"- Log ID: {log['_id']}, Score: {log['score']}")

    except NotFoundError as e:
        print(f"Verification failed. Could not retrieve resources: {e}")

# --- 6. Clean Up ---
# It's good practice to clean up created resources after testing.
if new_prompt:
    try:
        print(f"\nCleaning up... Deleting prompt '{new_prompt['_id']}'.")
        client.delete_prompt(new_prompt['_id'])
        print("Prompt deleted successfully.")
    except NotFoundError:
        print("Cleanup failed: Prompt was already deleted or not found.")

if new_log:
    try:
        print(f"Deleting log '{new_log['_id']}'.")
        client.delete_log(new_log['_id'])
        print("Log deleted successfully.")
    except NotFoundError:
        print("Cleanup failed: Log was already deleted or not found.")
```

## 4. Authentication

The PromptCrafter API uses JWT Bearer tokens for authentication[1]. All requests to protected endpoints must include an `Authorization: Bearer <token>` header[1]. The `PromptCrafterClient` handles this for you automatically.

There are three ways to authenticate:
1.  **Sign Up:** Create a new account. The `signup` method will automatically configure the client with the new token[2].
2.  **Log In:** Authenticate an existing user. The `login` method will fetch a new token and configure the client[2].
3.  **Manual Token:** Initialize the client directly with a pre-existing token[2].

### Sign Up for a New Account
Use this method if you don't have an account.

```python
from python_sdk import PromptCrafterClient, ConflictError, BadRequestError

client = PromptCrafterClient()
try:
    token = client.signup(
        name="Your Name",
        email="your.email@example.com",
        password="your-secure-password"
    )
    print("Signup successful. Client is ready to use.")
except ConflictError:
    print("This email is already in use. Please log in instead.")
except BadRequestError as e:
    print(f"Signup failed: {e}")
```

### Log In to an Existing Account
If you already have an account, use `login` to get a new session token.

```python
from python_sdk import PromptCrafterClient, AuthenticationError

client = PromptCrafterClient()
try:
    token = client.login(
        email="your.email@example.com",
        password="your-secure-password"
    )
    print("Login successful. Client is ready to use.")
except AuthenticationError:
    print("Login failed. Incorrect email or password.")
```

### Using an Existing Token
You can also initialize the client directly if you have a valid token.

```python
from python_sdk import PromptCrafterClient

# A JWT token you have previously saved
existing_token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." 

# Initialize the client with the token
client = PromptCrafterClient(token=existing_token)

# Or, set the token on an existing client instance
client.set_token(existing_token)
```

## 5. Usage Examples

Below are concise, production-ready examples for common operations. All examples assume the `client` has been initialized and authenticated as shown in the **Authentication** section.

### Create a Prompt
```python
try:
    new_prompt = client.create_prompt(
        title="Python Docstring Generator",
        content="Given a Python function, write a complete Google-style docstring for it.",
        model="Claude 3 Sonnet",
        tags=["python", "code-generation", "developer"]
    )
    print(f"Prompt created with ID: {new_prompt['_id']}")
except BadRequestError as e:
    print(f"Error: The prompt data was invalid. {e}")
```

### Retrieve a Prompt by ID
```python
try:
    prompt_id = "prompt104" # From your prompt library
    prompt = client.get_prompt(prompt_id)
    print(f"Title: {prompt['title']}")
    print(f"Content: {prompt['content']}")
except NotFoundError:
    print(f"Error: No prompt found with ID '{prompt_id}'.")
```

### Update a Prompt
The `update_prompt` method performs a PATCH operation, so you only need to provide the fields you want to change[1].

```python
try:
    prompt_id = "prompt103" # From your prompt library
    updated_prompt = client.update_prompt(
        prompt_id=prompt_id,
        model="GPT-4 Turbo",
        tags=["email", "apology", "customer-service"]
    )
    print(f"Prompt '{prompt_id}' updated successfully.")
    print(f"New Model: {updated_prompt['model']}")
except NotFoundError:
    print(f"Error: No prompt found with ID '{prompt_id}'.")
except BadRequestError as e:
    print(f"Error: The update data was invalid. {e}")
```

### Delete a Prompt
This action is permanent and cannot be undone[1].

```python
try:
    prompt_id = "prompt_to_delete" # ID of the prompt to delete
    client.delete_prompt(prompt_id)
    print(f"Prompt '{prompt_id}' was deleted successfully.")
except NotFoundError:
    print(f"Error: No prompt found with ID '{prompt_id}'.")
except ForbiddenError:
    print("Error: You do not have permission to delete this prompt.")
```

### Search Prompts
Perform a case-insensitive, full-text search across the `title`, `content`, and `tags` of all your prompts[1].

```python
try:
    search_query = "python"
    results = client.search_prompts(search_query)
    print(f"Found {len(results)} prompt(s) matching '{search_query}':")
    for prompt in results:
        print(f"- ID: {prompt['_id']}, Title: {prompt['title']}")
except BadRequestError:
    print("Error: The search query was missing or malformed.")
```

### Create a Log for a Prompt
```python
try:
    prompt_id = "prompt101" # ID of the associated prompt
    new_log = client.create_log(
        prompt_id=prompt_id,
        output="The report shows a 15% increase in quarterly revenue.",
        model_used="GPT-4o",
        notes="Output was concise and accurate.",
        score=9
    )
    print(f"Log created with ID: {new_log['_id']}")
except NotFoundError:
    print(f"Error: Cannot create log. Prompt ID '{prompt_id}' does not exist.")
```

## 6. SDK Reference

### `PromptCrafterClient` Class
The main class for interacting with the API.

`PromptCrafterClient(token: Optional[str] = None)`
*   **token** (`Optional[str]`): An optional JWT Bearer token. If provided, the client is initialized with this token.

### Authentication Methods

**`signup(name: str, email: str, password: str) -> str`**[2]
Creates a new user account and authenticates the client.
*   **Arguments:**
    *   `name` (`str`, required): The user's full name.
    *   `email` (`str`, required): The user's email address.
    *   `password` (`str`, required): The desired password.
*   **Returns:** `str` - The JWT authentication token.

**`login(email: str, password: str) -> str`**[2]
Authenticates an existing user and configures the client.
*   **Arguments:**
    *   `email` (`str`, required): The user's email address.
    *   `password` (`str`, required): The user's password.
*   **Returns:** `str` - The JWT authentication token.

**`set_token(token: str)`**[2]
Manually sets the authentication token on the client instance.
*   **Arguments:**
    *   `token` (`str`, required): The JWT token.

### Prompt Endpoints

**`create_prompt(title: str, content: str, model: str, tags: Optional[List[str]] = None) -> Dict[str, Any]`**[2]
Adds a new prompt to your library.
*   **Arguments:**
    *   `title` (`str`, required): The title of the prompt[1].
    *   `content` (`str`, required): The full text of the prompt[1].
    *   `model` (`str`, required): The target generative model (e.g., "GPT-4o")[1].
    *   `tags` (`Optional[List[str]]`): A list of strings for organization[1].
*   **Returns:** `Dict[str, Any]` - The created prompt object.

**`get_prompt(prompt_id: str) -> Dict[str, Any]`**[2]
Retrieves a single prompt by its unique ID.
*   **Arguments:**
    *   `prompt_id` (`str`, required): The unique identifier of the prompt[1].
*   **Returns:** `Dict[str, Any]` - The requested prompt object.

**`get_prompts() -> List[Dict[str, Any]]`**[2]
Retrieves all prompts belonging to the authenticated user.
*   **Returns:** `List[Dict[str, Any]]` - A list of all prompt objects.

**`update_prompt(prompt_id: str, **fields) -> Dict[str, Any]`**[2]
Updates one or more fields of an existing prompt.
*   **Arguments:**
    *   `prompt_id` (`str`, required): The unique identifier of the prompt[1].
    *   `**fields`: Keyword arguments for the fields to update. Allowed fields are `title`, `content`, `model`, and `tags`[1].
*   **Returns:** `Dict[str, Any]` - The updated prompt object.

**`delete_prompt(prompt_id: str)`**[2]
Permanently deletes a prompt.
*   **Arguments:**
    *   `prompt_id` (`str`, required): The unique identifier of the prompt[1].
*   **Returns:** `None`. A successful deletion returns a 204 No Content status, which the method handles gracefully.

**`search_prompts(query: str) -> List[Dict[str, Any]]`**[2]
Performs a full-text search across your prompt library.
*   **Arguments:**
    *   `query` (`str`, required): The search term(s)[1].
*   **Returns:** `List[Dict[str, Any]]` - A list of matching prompt objects.

### Log Endpoints

**`create_log(prompt_id: str, output: str, model_used: str, notes: Optional[str] = None, score: Optional[int] = None) -> Dict[str, Any]`**[2]
Records a generated output for a specific prompt.
*   **Arguments:**
    *   `prompt_id` (`str`, required): The ID of the prompt this log is associated with[1].
    *   `output` (`str`, required): The text generated by the model[1].
    *   `model_used` (`str`, required): The name of the model that generated the output[1].
    *   `notes` (`Optional[str]`): Any personal notes about the test run[1].
    *   `score` (`Optional[int]`): A numerical score (e.g., 1-10) to rate the output quality[1].
*   **Returns:** `Dict[str, Any]` - The created log object.

**`get_logs() -> List[Dict[str, Any]]`**[2]
Retrieves all logs belonging to the authenticated user.
*   **Returns:** `List[Dict[str, Any]]` - A list of all log objects.

**`get_logs_by_prompt(prompt_id: str) -> List[Dict[str, Any]]`**[2]
Retrieves all logs associated with a specific prompt ID.
*   **Arguments:**
    *   `prompt_id` (`str`, required): The unique ID of the prompt[1].
*   **Returns:** `List[Dict[str, Any]]` - A list of matching log objects.

**`delete_log(log_id: str)`**[2]
Permanently deletes a log.
*   **Arguments:**
    *   `log_id` (`str`, required): The unique identifier of the log[1].
*   **Returns:** `None`. A successful deletion returns a 204 No Content status.

## 7. Error Handling

The SDK uses a set of custom exceptions to report errors from the API. All exceptions inherit from the base `PromptCrafterAPIError` class[2]. It is best practice to catch specific exceptions to handle different error scenarios gracefully.

| Exception             | HTTP Status | Description                                                                        |
| --------------------- | ----------- | ---------------------------------------------------------------------------------- |
| `BadRequestError`     | 400         | The request was malformed, or required fields were missing/invalid[1].               |
| `AuthenticationError` | 401         | The provided JWT token is missing, invalid, or expired[1].                           |
| `ForbiddenError`      | 403         | You do not have permission to access or modify the requested resource[1].             |
| `NotFoundError`       | 404         | The requested resource (e.g., a prompt or log) could not be found[1].                |
| `ConflictError`       | 409         | The request could not be completed due to a conflict, e.g., signing up with an existing email[1]. |
| `ServerError`         | 5xx         | An unexpected error occurred on the PromptCrafter API server[1].                      |

### Example Error Handling
```python
from python_sdk import PromptCrafterClient, NotFoundError, ForbiddenError, PromptCrafterAPIError

client = PromptCrafterClient(token="your-token")

try:
    prompt = client.get_prompt("some-id-that-might-not-exist")
except NotFoundError:
    print("The prompt could not be found. Please check the ID.")
except ForbiddenError:
    print("You are not authorized to view this prompt.")
except AuthenticationError:
    print("Your session has expired. Please log in again.")
except PromptCrafterAPIError as e:
    # Catch any other API-related errors
    print(f"An unexpected API error occurred: {e}")
```

## 8. Data Models

### Prompt Resource
The structure of a Prompt object returned by the API.

| Field Name | Data Type      | Description                                          |
| ---------- | -------------- | ---------------------------------------------------- |
| `_id`      | string         | The unique identifier for the prompt.                |
| `ownerId`  | string         | The ID of the user who owns the prompt.              |
| `title`    | string         | The title of the prompt.                             |
| `content`  | string         | The full text content of the prompt.                 |
| `model`    | string         | The target generative model for the prompt.          |
| `tags`     | array[string]  | A list of tags for organization.                     |
| `createdAt`| string (date)  | The ISO 8601 timestamp when the prompt was created.  |
| `updatedAt`| string (date)  | The ISO 8601 timestamp when the prompt was last updated. |

### Log Resource
The structure of a Log object returned by the API[1].

| Field Name  | Data Type     | Description                                               |
| ----------- | ------------- | --------------------------------------------------------- |
| `_id`       | string        | The unique identifier for the log.                        |
| `promptId`  | string        | The ID of the prompt this log is associated with.         |
| `output`    | string        | The generated text output.                                |
| `notes`     | string        | Optional user notes about the test run.                   |
| `modelUsed` | string        | The name of the model that generated the output.          |
| `score`     | integer       | An optional numerical score to rate the output quality.   |
| `createdAt` | string (date) | The ISO 8601 timestamp when the log was created.          |

## 9. Advanced Usage

### Filtering and Searching
The PromptCrafter API provides two primary ways to filter resources:

1.  **Full-Text Search for Prompts:** The `search_prompts(query)` method provides a powerful way to find prompts. The search is case-insensitive and covers the `title`, `content`, and `tags` fields[1].

2.  **Filtering Logs by Prompt:** The `get_logs_by_prompt(prompt_id)` method is the designated way to retrieve all test logs for a single prompt, which is essential for comparing outputs and performance over time[1].

### Pagination
Based on the provided API documentation and SDK, endpoints that return lists (`get_prompts`, `get_logs`, `search_prompts`) do not currently support pagination parameters like `limit` or `offset`[1][2]. They will return all resources matching the request in a single response. Please plan accordingly for a large number of resources.

## 10. FAQ

**Q: How do I handle an expired `AuthenticationError`?**
A: An expired token will raise an `AuthenticationError`. Your application should catch this error and prompt the user to re-authenticate using the `client.login()` method to obtain a new token.

**Q: What is the difference between `get_logs()` and `get_logs_by_prompt(prompt_id)`?**
A: `get_logs()` retrieves a complete list of every log associated with your user account. `get_logs_by_prompt(prompt_id)` retrieves only the logs for one specific prompt, which is more efficient for analyzing a single task[1][2].

**Q: Why am I getting a `ForbiddenError` (403)?**
A: This error means you are trying to access or modify a resource that does not belong to your authenticated user account. This can happen if you try to `get`, `update`, or `delete` a prompt or log using an ID from another user's account[1].

**Q: Can I update just one field of a prompt, like its tags?**
A: Yes. The `update_prompt()` method accepts keyword arguments for any combination of the updatable fields (`title`, `content`, `model`, `tags`). Any fields you do not provide in the call will remain unchanged[1][2].

## 11. Contact & Support

For bugs, feature requests, or issues with the Python SDK, please open an issue on the official GitHub repository. This is the primary channel for support.

*   **GitHub Repository:** [https://github.com/Marmelodov/PromptCrafter-API](https://github.com/Marmelodov/PromptCrafter-API)[1]
*   **Live API Documentation:** [https://marmelodov.github.io/PromptCrafter-API/](https://marmelodov.github.io/PromptCrafter-API/)[1]
