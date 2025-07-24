# PromptCrafter JavaScript SDK

[Download JavaScript SDK](link)  
[Technical addendum](link)  

## Overview

**PromptCrafter JavaScript SDK** is the official Node.js client library for the [PromptCrafter API](https://promptcrafter-production.up.railway.app). PromptCrafter is a backend platform for storing, organizing, and evaluating generative AI prompts. This SDK enables developers and prompt engineers to easily interact with the API, manage prompt libraries, log and evaluate model outputs, and perform prompt searches programmatically.

**Key Features:**
- Full CRUD operations for prompt resources
- Full-text prompt search (title, content, tags, model)
- Logging and evaluation of prompt outputs
- User account signup and login flow
- Strong error handling for API/network errors

## Installation

### Prerequisites

- Node.js v16+ recommended
- npm or yarn

### Install via npm

```sh
npm install promptcrafter-client
```

### Install via yarn

```sh
yarn add promptcrafter-client
```

## Authentication

All API operations (except signup/login) require an authenticated user with a Bearer token. Obtain a token by signing up or logging in, then set it on your `PromptCrafterClient` instance.

### Sign Up

```js
import PromptCrafterClient from 'promptcrafter-client';

const client = new PromptCrafterClient();
const { token } = await client.signup({
  name: 'John Doe',
  email: 'john@example.com',
  password: 'password123'
});
// Save the token for authenticated requests
client.setToken(token);
```

### Log In

```js
const { token } = await client.login({
  email: 'john@example.com',
  password: 'password123'
});
client.setToken(token);
```

**Note:** Use `.setToken(token)` to authorize all further requests.

## Usage Examples

### Create a Prompt

```js
const prompt = await client.createPrompt({
  title: 'Positive Product Review Writer',
  content: 'Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle...',
  model: 'Grok-3 Beta',
  tags: ['review', 'product', 'writing']
});
console.log(prompt._id);
```

### Retrieve All Prompts

```js
const prompts = await client.getPrompts();
console.log(prompts);
```

### Get a Prompt by ID

```js
const prompt = await client.getPromptById('prompt104');
console.log(prompt.title);
```

### Update a Prompt

```js
const updated = await client.updatePrompt('prompt104', {
  model: 'GPT-4o',
  tags: ['bicycle', 'review']
});
console.log(updated.model);
```

### Delete a Prompt

```js
await client.deletePrompt('prompt104');
```

### Search Prompts

```js
const results = await client.searchPrompts('email subject');
console.log(results.map(p => p.title));
```

### Log an Output

```js
const log = await client.createLog({
  promptId: 'prompt104',
  output: 'The Industrial Revolution transformed how people lived...',
  modelUsed: 'Claude 3 Sonnet',
  notes: 'Excellent coverage',
  score: 9
});
```

### Get All Logs

```js
const logs = await client.getLogs();
```

### Get Logs for a Prompt

```js
const logsForPrompt = await client.getLogsByPrompt('prompt104');
```

### Delete a Log

```js
await client.deleteLog('log106');
```

## SDK Reference

### Classes

#### `PromptCrafterClient`

##### Constructor

```js
new PromptCrafterClient([options])
```
- `options` (object, optional)
  - `baseUrl` (string): Override API base URL. Default: `https://promptcrafter-production.up.railway.app`
  - `token` (string): Bearer token for authentication (optional)

##### Methods

###### Authentication

- **`signup(data)`**
  - `data` (object): `{ name: string, email: string, password: string }` (all required)
  - Returns: `Promise<{ token: string }>`
  - Errors: If required fields are missing, throws `PromptCrafterError`

- **`login(data)`**
  - `data` (object): `{ email: string, password: string }` (all required)
  - Returns: `Promise<{ token: string }>`
  - Errors: If required fields are missing, throws `PromptCrafterError`

- **`setToken(token)`**
  - `token` (string): JWT bearer token for API requests
  - Returns: `void`

###### Prompt Operations

- **`getPrompts()`**
  - Returns: `Promise<Prompt[]>`
  - Auth: Requires bearer token
  - Errors: Throws `PromptCrafterError` if not authenticated

- **`createPrompt(promptBody)`**
  - `promptBody` (object):  
    - `title` (string, required)  
    - `content` (string, required)  
    - `model` (string, required)  
    - `tags` (string[], optional)
  - Returns: `Promise<Prompt>`
  - Errors: Throws if required fields missing

- **`getPromptById(id)`**
  - `id` (string, required): Prompt ID
  - Returns: `Promise<Prompt>`
  - Errors: Throws if `id` missing

- **`updatePrompt(id, updateFields)`**
  - `id` (string, required): Prompt ID
  - `updateFields` (object, at least one required): Any of `{ title, content, model, tags }`
  - Returns: `Promise<Prompt>`
  - Errors: Throws if `id` missing or empty updateFields

- **`deletePrompt(id)`**
  - `id` (string, required): Prompt ID
  - Returns: `Promise<void>`
  - Errors: Throws if `id` missing

- **`searchPrompts(query)`**
  - `query` (string, required): Search string (title/content/tags/model)
  - Returns: `Promise<Prompt[]>`
  - Errors: Throws if query missing/empty

###### Log Operations

- **`getLogs()`**
  - Returns: `Promise<Log[]>`
  - Auth: Requires bearer token

- **`createLog(logBody)`**
  - `logBody` (object):
    - `promptId` (string, required)
    - `output` (string, required)
    - `modelUsed` (string, required)
    - `notes` (string, optional)
    - `score` (number, optional)
  - Returns: `Promise<Log>`
  - Errors: Throws if required fields missing

- **`getLogsByPrompt(promptId)`**
  - `promptId` (string, required)
  - Returns: `Promise<Log[]>`
  - Errors: Throws if promptId is missing

- **`deleteLog(id)`**
  - `id` (string, required): Log ID
  - Returns: `Promise<void>`
  - Errors: Throws if id is missing

###### Error Class

- **`PromptCrafterError`**
  - Properties:
    - `name`: Always `"PromptCrafterError"`
    - `message`: Error message
    - `status`: HTTP status code (optional)
    - `body`: Response body (optional)

## Error Handling

All methods can throw `PromptCrafterError`. Inspect `error.message`, `error.status`, and `error.body` for details.

| Status | Example Response                    | Cause                                                   | Resolution                                     |
|--------|-------------------------------------|---------------------------------------------------------|------------------------------------------------|
| 400    | `{ "message": "...required..." }`   | Required fields missing or invalid input                | Check required fields & data types             |
| 401    | `{ "message": "Invalid or missing bearer token" }` | Token missing, expired, or invalid            | Log in again; ensure proper token              |
| 403    | `{ "message": "You do not own this prompt" }`      | Permission denied (not resource owner)         | Operate only on resources you own              |
| 404    | `{ "message": "Prompt not found" }`                | Resource not found                             | Verify resource ID (promptId/logId)            |
| 415    | `{ "message": "Content-Type must be application/json" }` | Wrong/missing header                    | Set `Content-Type: application/json`           |
| 409    | `{ "message": "Email already in use" }`            | Duplicate signup                               | Use a new email to register                    |
| 500    | `{ "message": "Unexpected server error" }`         | Server error                                   | Retry later or contact support                 |

**Best Practices:**
- Use `try/catch` around all client SDK calls
- Log `error.status` and `error.body` for debugging

## Data Models

### Prompt

| Field      | Type      | Required | Description                           |
|------------|-----------|----------|---------------------------------------|
| _id        | string    | Yes      | Unique prompt ID                      |
| ownerId    | string    | Yes      | Creator’s user ID                     |
| title      | string    | Yes      | Short title                           |
| content    | string    | Yes      | Prompt text/instructions              |
| model      | string    | Yes      | AI model for which prompt is targeted |
| tags       | string[]  | No       | Tags for grouping/search              |
| createdAt  | string    | Yes      | ISO8601 timestamp (created)           |
| updatedAt  | string    | Yes      | ISO8601 timestamp (updated)           |

### Log

| Field      | Type      | Required | Description                           |
|------------|-----------|----------|---------------------------------------|
| _id        | string    | Yes      | Unique log ID                         |
| promptId   | string    | Yes      | Associated prompt’s ID                |
| output     | string    | Yes      | AI model output text                  |
| notes      | string    | No       | Optional evaluation notes             |
| modelUsed  | string    | Yes      | Model that produced the output        |
| score      | number    | No       | User score for output quality (1–10)  |
| createdAt  | string    | Yes      | ISO8601 timestamp (created)           |

## Advanced Usage

### Searching and Filtering

- Use `.searchPrompts(query)` for full-text search (case-insensitive) over `title`, `content`, `tags`, and `model`.
- Example: `client.searchPrompts("marketing email")`

### Partial Updates

- Use `.updatePrompt(id, fields)` to update only specified fields. Leave out the fields you do not wish to modify.

### Pagination

- **[Documentation does not describe server-side pagination; all results are returned for each query.]**

### Extensibility

- Override the `baseUrl` in the constructor for self-hosted or staging environments.
  ```js
  const client = new PromptCrafterClient({ baseUrl: 'https://my-railway-hosted-api.app' });
  ```

### HTTP Options

- All requests are sent using [`node-fetch`](https://www.npmjs.com/package/node-fetch). There is currently no middleware or hook support.

### Streaming

- **[Streaming is not supported by this SDK or API as described.]**

## FAQ

**Q: How do I get my bearer token?**  
A: Sign up or log in with `.signup()`/`.login()`. Use `token` from the response with `.setToken(token)`.

**Q: What happens if my token expires?**  
A: A `401 Unauthorized` error is thrown. Log in again to get a new token.

**Q: Can I manage prompts or logs without authentication?**  
A: No, all prompt/log operations require a valid bearer token.

**Q: How do I update only one field on a prompt?**  
A: Pass only that field to `.updatePrompt()`. Others remain unchanged.

**Q: Can I access logs for prompts I do not own?**  
A: No. You may only view or log outputs for your own prompts.

**Q: Can I search prompts by tag only?**  
A: Use `.searchPrompts()` with a tag keyword—search is across title, content, model, and tags.

## Contact/Support

- **Report bugs or request features:** [Official Contact Page](https://promptcrafter-production.up.railway.app/contact)
- **API Docs:** [https://promptcrafter-production.up.railway.app/reference/index.md](https://promptcrafter-production.up.railway.app/reference/index.md)
- **Community/Support:**  
  If you encounter bugs, unexpected errors, or need developer help, please visit the [Contact page](https://promptcrafter-production.up.railway.app/contact) or open an issue (if using this as an open-source package).

