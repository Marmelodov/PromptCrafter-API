# Quickstart

This guide shows you how to create an account, authenticate, and save and retrieve your first prompt, all in about five minutes. These steps teach you the core pattern of PromptCrafter API and prepare you to build a searchable library of prompts and track their performance across AI models.

Make sure you have one of the following tools:

* [cURL](https://curl.se/docs/install.html)
* [Postman](https://www.postman.com/downloads/)

If you're using Postman, [import the PromptCrafter API Collection](postman.md). It includes all the API requests described in this guide, so you can replace their pre-filled values with your own and click **Send**.

## 1. Create your account

This step establishes your identity in the system, creating a private workspace. It ensures that every prompt you save is tied to your account, so only you can access your library.

**Endpoint**: `POST /auth/signup`

<details>
<summary>cURL</summary>

To make the cURL commands cleaner and easier to use, first set a shell variable for the base URL. This avoids having to type it repeatedly.

```bash
BASE_URL="https://promptcrafter-production.up.railway.app"
```

Now, create your account. Replace the `name`, `email`, and `password` values with your own credentials.

```bash
curl -X POST $BASE_URL/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Alice",
    "email": "alice@example.com",
    "password": "password123"
  }'
```

</details>

<details>
<summary>Postman</summary>

Use the **Sign up** request in the `Auth` folder of the PromptCrafter API Collection:

1.  **Replace the example data** in the request body with your own information:
    *   Change `"email": "john@example.com"` to your real email address
    *   Change `"password": "password123"` to your preferred password
    *   Change `"name": "John Doe"` to your actual name

2.  Click **Send** to create your account.

</details>

A `201 Created` response confirms your account is ready. You can now use these credentials to authenticate. (If you receive a `409 Conflict` error, the email address is already in use.)

## 2. Log in to get your token

Authentication in PromptCrafter requires a JWT (JSON Web Token), which acts as your temporary key to access protected resources. You must include this token in the `Authorization` header of every subsequent request using the `Bearer` scheme, like this: `Authorization: Bearer {your_token}`.

**Endpoint**: `POST /auth/login`

<details>
<summary>cURL</summary>

Replace the `email` and `password` values with your own credentials.

```bash
curl -X POST $BASE_URL/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "alice@example.com",
    "password": "password123"
  }'
```

After logging in, save your token to a shell variable. This prevents you from having to copy and paste it in every subsequent request.

```bash
TOKEN="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

</details>

<details>
<summary>Postman</summary>

Use the **Log in** request in the `Auth` folder of the Postman Collection. In the request body, replace the pre-filled `email` and `password` values with your own credentials.

> **Note:**
> The Postman Collection handles authentication for you. After you log in, a test script saves the token to a collection variable (`{{token}}`). This variable is then automatically included in the authorization header of every other request, so you don't need to copy and paste it.

</details>

Save the token from the response body, which looks like this:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

This token is your authentication credential for all protected endpoints. (If you get a `401 Unauthorized` error, your email or password may be incorrect. Try the log-in step again.)

## 3. Save your first prompt

Now comes the core value of PromptCrafter: storing and organizing your prompts. This step demonstrates how to build your prompt library over time. Each saved prompt includes not just the prompt text (what you actually feed into the AI model), but metadata like the intended `model`, descriptive `tags` for grouping, and timestamps. These make it easy to find and manage prompts as your collection grows.

**Why the metadata matters:** The metadata is what enables you to build an organized, searchable library of prompts instead of a chaotic folder full of text files. Having your prompts organized in one place becomes invaluable when you're working on multiple projects or need to find that one prompt you wrote months ago that worked perfectly. With `tags`, for example, you can:
- **Group by project:** Tag prompts for a particular marketing campaign with `campaign-q3-launch`.
- **Group by task:** Tag prompts that generate Python code with `python-code-gen`.
- **Group by purpose:** Tag prompts for summarizing customer feedback with `customer-feedback-summary`.


**Endpoint**: `POST /prompts`

<details>
<summary>cURL</summary>

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

</details>

<details>
<summary>Postman</summary>

Use the **Save a prompt** request in the `Prompts` folder. The request body is pre-filled, and your authorization token is already included as a variable. Just click **Send**.

</details>

A successful request returns the full prompt object, including its unique server-generated `_id`. This ID is crucial: you'll use it to retrieve, update, or delete this specific prompt. (If you receive a `401 Unauthorized` error, your token may be invalid or expired. Try the log-in step again to get a new token.)

**Example Response**

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

**Important:** Copy the `_id` from the response. You'll need it for the next step to verify your prompt was saved correctly. For cURL users, save it to a variable:

```bash
PROMPT_ID="prompt482"
```

## 4. Retrieve your prompt

This final step confirms your prompt was saved correctly and shows how to retrieve it using its unique ID. This is the fundamental pattern for using your saved prompts in an application.

**How to use this in the real world:**

Retrieving a prompt by its ID is the key to building tools like:
- A **content generation app** where clicking a "Summarize Article" button triggers a `GET /prompts/{summary-prompt-id}` request to fetch the appropriate prompt.
- A **prompt evaluation suite** that programmatically loops through a list of prompt IDs to test and compare the outputs from each one.

**Endpoint**: `GET /prompts/{id}`

<details>
<summary>cURL</summary>

Use the variables you set previously for the `BASE_URL`, `PROMPT_ID`, and `TOKEN`.

```bash
curl -X GET $BASE_URL/prompts/$PROMPT_ID \
  -H "Authorization: Bearer $TOKEN"
```

</details>

<details>
<summary>Postman</summary>

1.  In the collection's **Variables** tab, paste the `_id` from the previous step into the `CURRENT VALUE` field for the `promptId` variable.
2.  Run the **Retrieve a prompt by ID** request. It uses the `{{promptId}}` variable in the URL to fetch your specific prompt.

</details>

The response should return the exact same prompt object you created, confirming your prompt is safely stored and accessible. (If you get a `404 Not Found` error, make sure you copied the `_id` correctly into the `promptId` variable or URL path.)

## Next steps

You've successfully authenticated with the PromptCrafter API and learned its core pattern: saving and retrieving a prompt. You now have the foundation to build more sophisticated AI applications with organized, trackable prompts.

**Ready to dive deeper?** Here's what to explore next:

**[Log generated outputs](tutorials/test-prompt.md):** Learn to test and score the outputs of your prompts.  
**[Search your library](tutorials/search-prompts.md):** Discover how to find prompts by keyword, tags, or content as your collection grows.  
**[Explore the full API Reference](reference/index.md):** Detailed documentation for all endpoints.
