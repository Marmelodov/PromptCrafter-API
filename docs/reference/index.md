# API reference

Available and upcoming reference topics for the PromptCrafter API.

## Prompt endpoints

- [Retrieve all prompts](endpoints/get-prompts.md): `GET /prompts`
- [Create a new prompt](endpoints/post-prompts.md): `POST /prompts`
- [Retrieve a prompt by ID](endpoints/get-prompts-id.md): `GET /prompts/:id`
- [Update a prompt by ID](endpoints/patch-prompts-id.md): `PATCH /prompts/:id`
- [Delete a prompt by ID](endpoints/delete-prompts-id.md): `DELETE /prompts/:id`
- [Search prompts](endpoints/get-search.md): `GET /search?q=...` *(not yet written)*

## Log endpoints

- [Retrieve all logs](endpoints/get-logs.md): `GET /logs`
- [Create a new log](endpoints/post-logs.md): `POST /logs`
- [Retrieve logs for a specific prompt](endpoints/get-logs-by-prompt.md): `GET /logs?promptId=...` *(not yet written)*
- [Delete a log by ID](endpoints/delete-logs-id.md): `DELETE /logs/:id`

## Resource definitions

- [Prompt resource](resources/prompt.md)
- [Log resource](resources/log.md)

## Authentication

- [Sign up](endpoints/post-auth-signup.md): `POST /auth/signup` *(not yet written)*
- [Log in](endpoints/post-auth-login.md): `POST /auth/login` *(not yet written)*
