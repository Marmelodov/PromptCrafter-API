# API Reference

Use this API reference to find the data models, required parameters, and request/response schemas for every resource and endpoint in the PromptCrafter API.

All API requests use the `https://promptcrafter-production.up.railway.app` base URL and require a bearer token. See the [Quickstart](../quickstart.md) for help with authentication.

## Prompts

The `prompt` resource is the core of the API, storing a set of reusable instructions for a generative AI model.

* **[Prompt resource](reference/resources/prompt.md)**: View the attributes, data types, and description of the prompt object.

#### Operations

* `GET /prompts`: [**Retrieve all prompts**](reference/endpoints/get-prompts.md)  
    Retrieves a list of all prompts owned by the authenticated user.
* `POST /prompts`: [**Save a prompt**](reference/endpoints/post-prompts.md)  
    Creates a new prompt in the user's library.
* `GET /prompts/{id}`: [**Retrieve a prompt by ID**](reference/endpoints/get-prompts-id.md)  
    Fetches a single prompt using its unique identifier.
* `PATCH /prompts/{id}`: [**Update a prompt**](reference/endpoints/patch-prompts-id.md)  
    Modifies one or more fields of an existing prompt.
* `DELETE /prompts/{id}`: [**Delete a prompt**](reference/endpoints/delete-prompts-id.md)  
    Permanently removes a prompt and its associated logs from the database.

## Logs

The `log` resource records the results of a prompt test, including the generated output and user-provided notes and performance score.

* **[Log resource](reference/resources/log.md)**: View the attributes, data types, and description of the log object.

#### Operations

* `GET /logs`: [**Retrieve logs**](reference/endpoints/get-logs.md)  
    Fetches a list of logs, optionally filtered by the parent `promptId`.
* `POST /logs`: [**Log a generated output**](reference/endpoints/post-logs.md)  
    Creates a new log to record the output and score of a prompt test.
* `DELETE /logs/{id}`: [**Delete a log**](reference/endpoints/delete-logs-id.md)  
    Permanently removes a single log from the database.

## Search

The search endpoint provides a dedicated method for finding prompts within your library.

#### Operations

* `GET /search`: [**Search prompts**](reference/endpoints/get-search.md)  
    Performs a case-insensitive search across the `title`, `content`, and `tags` of all prompts.

## Authentication

These endpoints create new user accounts and authenticate existing users, returning the JWT bearer token required for all subsequent API calls.

#### Operations

* `POST /auth/signup`: [**Sign up**](reference/endpoints/post-auth-signup.md)  
    Creates a new user account.
* `POST /auth/login`: [**Log in**](reference/endpoints/post-auth-login.md)  
    Authenticates a user and returns a JWT bearer token for all subsequent requests.
