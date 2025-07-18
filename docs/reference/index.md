# API reference

The reference pages below describe the resources and operations in PromptCrafter API. All API requests use the `https://promptcrafter-production.up.railway.app` base URL and require your bearer token in the `Authorization: Bearer` header. See the [Quickstart](../quickstart.md) for help obtaining your bearer token.

## Resource definitions

- [Prompt](resources/prompt.md)  
- [Log](resources/log.md)

## Prompt operations

- [Retrieve all prompts](endpoints/get-prompts.md): `GET /prompts`  
- [Save a prompt](endpoints/post-prompts.md): `POST /prompts`  
- [Retrieve a prompt by ID](endpoints/get-prompts-id.md): `GET /prompts/:id`  
- [Update a prompt](endpoints/patch-prompts-id.md): `PATCH /prompts/:id`  
- [Delete a prompt](endpoints/delete-prompts-id.md): `DELETE /prompts/:id`  
- [Search prompts](endpoints/get-search.md): `GET /search?q=`  

## Log operations

- [Retrieve all logs](endpoints/get-logs.md): `GET /logs`  
- [Log a generated output](endpoints/post-logs.md): `POST /logs`  
- [Retrieve logs for a specific prompt](endpoints/get-logs-by-prompt.md): `GET /logs?promptId=`  
- [Delete a log](endpoints/delete-logs-id.md): `DELETE /logs/:id`  

## Authentication

- [Sign up](endpoints/post-auth-signup.md): `POST /auth/signup`  
- [Log in](endpoints/post-auth-login.md): `POST /auth/login`  
