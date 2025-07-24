# PromptCrafter Ruby SDK Documentation

[Download Ruby SDK](link)  
[Technical addendum](link)  

## Table of Contents

1. [Overview](#overview)
2. [Installation](#installation)
3. [Authentication](#authentication)
4. [Usage Examples](#usage-examples)
5. [SDK Reference](#sdk-reference)
6. [Error Handling](#error-handling)
7. [Data Models](#data-models)
8. [Advanced Usage](#advanced-usage)
9. [FAQ](#faq)
10. [Contact/Support](#contactsupport)

---

## Overview

The **PromptCrafter Ruby SDK** provides idiomatic, easy-to-use access to the PromptCrafter API, a backend service for storing, organizing, and evaluating generative AI prompts.

**Key Features:**
- Full CRUD support for Prompt resources
- Logging and scoring of AI model outputs
- Powerful full-text prompt search
- Comprehensive error handling
- Supports user signup, login, and authenticated sessions
- Customizable networking (timeouts, base URL)

Designed for developers, prompt engineers, and anyone managing large prompt libraries or evaluating generative AI workflow results.

---

## Installation

### Prerequisites

- Ruby 2.7 or greater recommended
- Bundler (for Gemfile usage)

### Via RubyGems

```shell
gem install promptcrafter
```

### With Bundler/Gemfile

Add to your `Gemfile`:

```ruby
gem 'promptcrafter'
```

Then run:

```shell
bundle install
```

---

## Authentication

All API requests require user authentication via a bearer token.

### How to Obtain an Access Token

You must sign up for a new account or log in with your existing credentials to obtain an access token.

#### 1. Sign Up

```ruby
client = PromptCrafter::Client.new
token = client.signup(
  name: 'Your Name',
  email: 'your.email@example.com',
  password: 'YourPassword123'
)
# token is a string JWT, save it securely!
```

#### 2. Log In

```ruby
client = PromptCrafter::Client.new
token = client.login(
  email: 'your.email@example.com',
  password: 'YourPassword123'
)
# token is a string JWT for use with API requests
```

### Setting the Access Token

You can set the access token globally or per client:

**A. Globally via Configuration**

```ruby
PromptCrafter::Client.configure do |config|
  config.access_token = 'your_jwt_token'
end
client = PromptCrafter::Client.new
```

**B. Per Client via Constructor**

```ruby
client = PromptCrafter::Client.new(access_token: 'your_jwt_token')
```

**C. Updating Client After Initialization**

```ruby
client = PromptCrafter::Client.new
client.config.access_token = 'your_jwt_token'
```

> **Note:** All methods that require authentication will raise an error if a token is not provided.

---

## Usage Examples

### Creating a Prompt

```ruby
client = PromptCrafter::Client.new(access_token: 'your_jwt_token')
prompt = client.create_prompt(
  title:   'Positive Product Review Writer',
  content: 'Imagine you are a satisfied customer. Write a friendly review of an electric bicycle...',
  model:   'Grok-3 Beta',
  tags:    ['review', 'product', 'writing']
)
puts prompt['title'] # => "Positive Product Review Writer"
```

### Listing All Prompts

```ruby
prompts = client.list_prompts
prompts.each do |p|
  puts "#{p['title']}: #{p['content']}"
end
```

### Get a Prompt by ID

```ruby
prompt = client.get_prompt('prompt104')
puts prompt['content']
```

### Update a Prompt

```ruby
updated = client.update_prompt(
  id:     'prompt104',
  model:  'GPT-4 Turbo',
  tags:   ['email', 'apology', 'customer']
)
puts updated['model'] # => "GPT-4 Turbo"
```

### Delete a Prompt

```ruby
client.delete_prompt('prompt104') # returns nil if successful
```

### Search Prompts

```ruby
results = client.search_prompts(q: 'email subject')
results.each { |r| puts r['title'] }
```

### Log a Model Output

```ruby
log = client.create_log(
  prompt_id:  'prompt104',
  output:     'The Industrial Revolution transformed how people lived...',
  model_used: 'Claude 3 Sonnet',
  notes:      'Clear, accessible explanation.',
  score:      8
)
puts log['_id']
```

### List All Logs

```ruby
logs = client.list_logs
logs.each do |log|
  puts "#{log['modelUsed']}: #{log['output']}"
end
```

### Get Logs for a Prompt

```ruby
logs = client.get_logs_by_prompt(prompt_id: 'prompt104')
puts logs.size
```

### Delete a Log

```ruby
client.delete_log('log104') # returns nil if successful
```

---

## SDK Reference

### Module: `PromptCrafter`

#### Exception Classes

- **PromptCrafter::Error** - Base exception for all SDK errors.
- **PromptCrafter::AuthenticationError** - Raised for token or login issues.
- **PromptCrafter::BadRequestError** - Raised for invalid parameters or request format.
- **PromptCrafter::NotFoundError** - Raised for missing resources or access denial.
- **PromptCrafter::ForbiddenError** - Raised for access denial due to permissions.
- **PromptCrafter::ConflictError** - Raised for resource conflicts (e.g., duplicate signup).
- **PromptCrafter::ServerError** - Raised for internal server errors (HTTP 5xx).

---

### Class: `PromptCrafter::Configuration`

| Attribute      | Type    | Description                                 | Default             |
| -------------- | ------- | ------------------------------------------- | ------------------- |
| base_url       | String  | Base URL for the API                        | `https://promptcrafter-production.up.railway.app` |
| access_token   | String  | Bearer authentication token                 | `nil`               |
| open_timeout   | Numeric | Timeout (sec) for opening HTTP connections  | `5`                 |
| read_timeout   | Numeric | Timeout (sec) for reading response          | `10`                |

---

### Class: `PromptCrafter::Client`

#### Constructor

```ruby
PromptCrafter::Client.new(
  base_url: nil,
  access_token: nil,
  open_timeout: nil,
  read_timeout: nil
)
```

All options are optional and override global configuration.

#### Global Configuration

```ruby
PromptCrafter::Client.configure { |cfg| ... }
PromptCrafter::Client.config # => returns global Configuration
```

#### Methods

##### Authentication

- **signup(name:, email:, password:) → String**
  - Registers a new user and returns a JWT token.
  - Required: `name` (String), `email` (String), `password` (String)
  - Errors: BadRequestError, ConflictError, ServerError

- **login(email:, password:) → String**
  - Authenticates user, returns a JWT token.
  - Required: `email` (String), `password` (String)
  - Errors: BadRequestError, AuthenticationError, ServerError

##### Prompts

- **list_prompts → Array<Hash>**
  - Returns all prompts for the authenticated user.
  - Errors: AuthenticationError, BadRequestError, ServerError

- **get_prompt(id) → Hash**
  - Returns a prompt by ID.
  - Required: `id` (String)
  - Errors: NotFoundError, AuthenticationError, ForbiddenError, BadRequestError, ServerError

- **create_prompt(title:, content:, model:, tags: nil) → Hash**
  - Creates a new prompt.
  - Required: `title` (String), `content` (String), `model` (String)
  - Optional: `tags` (Array<String>)
  - Errors: BadRequestError, AuthenticationError, ServerError

- **update_prompt(id:, title: nil, content: nil, model: nil, tags: nil) → Hash**
  - Updates fields on a prompt (any/all optional).
  - Required: `id` (String)
  - Optional: `title` (String), `content` (String), `model` (String), `tags` (Array<String>)
  - Errors: BadRequestError, NotFoundError, ForbiddenError, ServerError

- **delete_prompt(id) → nil**
  - Deletes a prompt by ID.
  - Required: `id` (String)
  - Errors: BadRequestError, NotFoundError, ForbiddenError, ServerError

- **search_prompts(q:) → Array<Hash>**
  - Full-text search over prompts.
  - Required: `q` (String)
  - Errors: AuthenticationError, BadRequestError, ServerError

##### Logs

- **list_logs → Array<Hash>**
  - Lists all logs for user.
  - Errors: AuthenticationError, BadRequestError, ServerError

- **create_log(prompt_id:, output:, model_used:, notes: nil, score: nil) → Hash**
  - Creates a new log record (output of a prompt run).
  - Required: `prompt_id` (String), `output` (String), `model_used` (String)
  - Optional: `notes` (String), `score` (Integer)
  - Errors: BadRequestError, AuthenticationError, NotFoundError, ServerError

- **get_logs_by_prompt(prompt_id:) → Array<Hash>**
  - Returns logs for a specific prompt.
  - Required: `prompt_id` (String)
  - Errors: AuthenticationError, BadRequestError, NotFoundError, ForbiddenError, ServerError

- **delete_log(log_id) → nil**
  - Deletes a log by its ID.
  - Required: `log_id` (String)
  - Errors: BadRequestError, NotFoundError, ForbiddenError, ServerError

---

## Error Handling

### Exception Classes

All SDK errors inherit from `PromptCrafter::Error`. Common subclasses:

| Exception                      | When it occurs                                            |
| ------------------------------ | -------------------------------------------------------- |
| `AuthenticationError`          | Invalid/missing token or unauthorized resource access    |
| `BadRequestError`              | Input validation, malformed request, missing required parameters |
| `NotFoundError`                | Resource does not exist, or user not authorized          |
| `ForbiddenError`               | User lacks permission for a resource                     |
| `ConflictError`                | Duplicate resource (e.g., signup with existing email)    |
| `ServerError`                  | 5xx errors — server errors                              |

### Example: Handling errors

```ruby
begin
  client.delete_prompt('bad_id')
rescue PromptCrafter::NotFoundError
  puts "Prompt does not exist."
rescue PromptCrafter::AuthenticationError => e
  puts "Authentication failed: #{e.message}"
rescue PromptCrafter::Error => e
  puts "Other API error: #{e.class}: #{e.message}"
end
```

### Best Practices

- Always rescue `PromptCrafter::Error` at the top-level for reliability.
- For user input, rescue more granular exceptions to inform user of specific actions to take (e.g., check fields or re-authenticate).
- Inspect `.message` on exceptions for API-supplied error detail.

---

## Data Models

### Prompt

| Field       | Type             | Required | Description                                         |
| ----------- | ---------------- | -------- | --------------------------------------------------- |
| _id         | String           | Yes      | Unique prompt identifier                            |
| ownerId     | String           | Yes      | User who created the prompt                         |
| title       | String           | Yes      | Prompt title                                        |
| content     | String           | Yes      | Prompt text/content                                 |
| model       | String           | Yes      | Target AI model (e.g., GPT-4, DALL·E)               |
| tags        | Array<String>    | No       | Tags for categorization                             |
| createdAt   | String (ISO 8601)| Yes      | Creation timestamp                                  |
| updatedAt   | String (ISO 8601)| Yes      | Last update timestamp                               |

### Log

| Field       | Type             | Required | Description                                         |
| ----------- | ---------------- | -------- | --------------------------------------------------- |
| _id         | String           | Yes      | Unique log identifier                               |
| promptId    | String           | Yes      | Prompt this log is attached to                      |
| output      | String           | Yes      | Output generated by AI model                        |
| notes       | String           | No       | Optional user notes                                 |
| modelUsed   | String           | Yes      | Model that produced output                          |
| score       | Integer          | No       | Optional quality score                              |
| createdAt   | String (ISO 8601)| Yes      | Creation timestamp                                  |

---

## Advanced Usage

### Customizing HTTP Timeouts

```ruby
PromptCrafter::Client.configure do |config|
  config.open_timeout = 10
  config.read_timeout = 30
end
client = PromptCrafter::Client.new
```

Or per client:

```ruby
client = PromptCrafter::Client.new(open_timeout: 10, read_timeout: 30)
```

### Base URL Overrides

Useful for sandboxing or QA.

```ruby
client = PromptCrafter::Client.new(base_url: 'https://custom-url')
```

### Pagination

> **Note:** As of this version, list endpoints return full arrays without pagination. If paginated endpoints become available, update accordingly.

---

## FAQ

**Q: Where do I get my token?**

A: After signing up (`signup`) or logging in (`login`), use the returned token as your `access_token`.

**Q: Why do I get `AuthenticationError`?**

A: The bearer token is missing, expired, or invalid. Set your token via `config.access_token` or pass `access_token:` to the client.

**Q: What data does search match against?**

A: Search matches prompt `title`, `content`, and `tags` fields.

**Q: How are errors formatted?**

A: All errors raise a subclass of `PromptCrafter::Error` with an informative message; refer to error handling.

**Q: Are timestamps set automatically?**

A: Yes, `createdAt` and `updatedAt` are set by the server.

**Q: Can I use the SDK in multi-threaded environments?**

A: The SDK is thread-safe as long as you do not share mutable clients across threads.

**Q: Is there support for resource pagination?**

A: Not at this time; all prompts/logs are returned in a single array.

---

## Contact/Support

- **Bug reports:** [GitHub Issues](https://github.com/your_org/promptcrafter-ruby/issues)
- **API support:** Use the contact form at [PromptCrafter website](https://promptcrafter-production.up.railway.app/contact) or reach out at [support@promptcrafter.app](mailto:support@promptcrafter.app)
- **Feature requests:** Use GitHub Issues or the web contact form
