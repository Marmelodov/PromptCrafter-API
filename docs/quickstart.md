# Quickstart

This guide walks you through the essential workflow of creating an account, authenticating, and saving your first prompt. Follow these steps to make your first authenticated API calls and store a prompt in your library.

The entire process takes about 5 minutes.

## Before you begin

Make sure you have one of the following tools:

* [cURL](https://curl.se/docs/install.html)
* [Postman](https://www.postman.com/downloads/)

## 1. Create your account

First, create a user account. A successful request confirms that your account was created. You then use your new credentials to log in.

**Endpoint**: `POST /auth/signup`

<details>
<summary>cURL</summary>

```bash
curl -X POST https://promptcrafter-production.up.railway.app/auth/signup \
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

1. Set the method to `POST`.
2. Enter the URL: `https://promptcrafter-production.up.railway.app/auth/signup`.
3. In the **Body** tab, select **raw** and **JSON**, then paste the following:

```
{
  "name": "Alice",
  "email": "alice@example.com",
  "password": "password123"
}
```

</details>

A successful request returns a `201 Created` status with an empty response body, confirming the account is ready.

## 2. Log in to get your token

Now, use your new credentials to log in. This endpoint authenticates you and returns a new JWT token for your session. All protected API endpoints require this token for authentication.

**Endpoint**: `POST /auth/login`

<details>
<summary>cURL</summary>

```bash
curl -X POST https://promptcrafter-production.up.railway.app/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "alice@example.com",
    "password": "password123"
  }'
```
</details>

<details>
<summary>Postman</summary>
Use the **Log in** request in the `Auth` folder of the Postman Collection. The request body is pre-filled with example credentials.

> **✨ Note:**
> The Postman Collection is configured to automatically save your token to a collection variable (`{{token}}`) upon successful login. This variable is automatically included in the authorization header of every other request, so you don't need to copy and paste it.

</details>

Save the `token` from the response body. You need it for the next step.

## 3. Save your first prompt

It's time to create your first prompt. This request sends your token in an `Authorization` header and includes the details of your prompt in the request body.

**Endpoint**: `POST /prompts`

<details>
<summary>cURL</summary>

Replace `{your_token}` with the token you received from the login step.

```bash
curl -X POST https://promptcrafter-production.up.railway.app/prompts \
  -H "Authorization: Bearer {your_token}" \
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

A successful request returns the full prompt object, including its unique server-generated `_id`.

**✅ Example Response**

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

Copy the `_id` from the response. Use it in the final step to retrieve your new prompt.

## 4. Retrieve your prompt

Finally, fetch the prompt you just created using its unique ID. This step confirms that your prompt saved correctly.

**Endpoint**: `GET /prompts/{id}`

<details>
<summary>cURL</summary>

Replace `{your_token}` and `{prompt_id}` with your values.

```bash
curl -X GET https://promptcrafter-production.up.railway.app/prompts/{prompt_id} \
  -H "Authorization: Bearer {your_token}"
```

</details>

<details>
<summary>Postman</summary>

1. In the collection's **Variables** tab, paste the `_id` from the previous step into the `CURRENT VALUE` field for the `promptId` variable.
2. Run the **Retrieve a prompt by ID** request. It uses the `{{promptId}}` variable in the URL to fetch your specific prompt.
</details>

## Next steps

You've successfully authenticated with the PromptCrafter API and managed your first prompt.

You're now ready to explore more features:
* **Test a prompt**: [Log a generated output](tutorials/test-prompt.md) and score its performance.
* **Organize your library**: [Search your prompts](tutorials/search-prompts.md) by keyword or tag.
* **Dive deeper**: Explore the full [API reference](reference/index.md) for more endpoints.
