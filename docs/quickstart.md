---
layout: page
---

# Quickstart

This Quickstart shows how to create an account, log in, save a prompt, and retrieve it using the PromptCrafter API. Each step includes both cURL and Postman examples.

## 1. Sign up

Create a user account.

**Endpoint**: `POST https://promptcrafter-production.up.railway.app/auth/signup`  
**Headers**:

- `Content-Type: application/json`

### In cURL

```shell
curl -X POST https://promptcrafter-production.up.railway.app/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Alice",
    "email": "alice@example.com",
    "password": "password123"
  }'
```

### In Postman

- **Method**: `POST`  
- **URL**: `https://promptcrafter-production.up.railway.app/auth/signup`  
- **Headers**:
    - `Content-Type`: `application/json`
- **Body** (raw JSON):

```json
{
  "name": "Alice",
  "email": "alice@example.com",
  "password": "password123"
}
```

## 2. Log in

Obtain a JWT token to authenticate your requests.

**Endpoint**: `POST https://promptcrafter-production.up.railway.app/auth/login`  
**Headers**:

- `Content-Type: application/json`

### In cURL

```shell
curl -X POST https://promptcrafter-production.up.railway.app/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "alice@example.com",
    "password": "password123"
  }'
```

### In Postman

- **Method**: `POST`  
- **URL**: `https://promptcrafter-production.up.railway.app/auth/login`  
- **Headers**:
    - `Content-Type`: `application/json`
- **Body** (raw JSON):

```json
{
  "email": "alice@example.com",
  "password": "password123"
}
```

The response body contains your JWT token:

```json
{
  "token": "{your_token}"
}
```

## 3. Save a prompt

Use the JWT token you received to create a new prompt object.

**Endpoint**: `POST https://promptcrafter-production.up.railway.app/prompts`  
**Headers**:  

- `Authorization`: `Bearer {your_token}`  
- `Content-Type`: `application/json`

Note: replace `{your_token}` with the bearer token returned by your login request.

### In cURL

```shell
curl -X POST https://promptcrafter-production.up.railway.app/prompts \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Positive Product Review Writer",
    "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
    "model": "Grok-3 Beta",
    "tags": ["review", "product", "writing"]
  }'
```

### In Postman

- **Method**: `POST`  
- **URL**: `https://promptcrafter-production.up.railway.app/prompts`  
- **Headers**:
    - `Authorization`: `Bearer {your_token}`
    - `Content-Type`: `application/json`
- **Body** (raw JSON):

```json
{
  "title": "Positive Product Review Writer",
  "content": "Imagine you are a satisfied customer. Write a friendly, detailed review for a new electric bicycle, mentioning at least three features you enjoyed and describing how it improved your daily commute.",
  "model": "Grok-3 Beta",
  "tags": ["review", "product", "writing"]
}
```

## 4. View your saved prompt

Replace `{prompt_id}` with the ID of the prompt you created.

**Endpoint**: `GET https://promptcrafter-production.up.railway.app/prompts/{prompt_id}`  
**Headers**:  

- `Authorization`: `Bearer {your_token}`

### In cURL

```shell
curl -X GET https://promptcrafter-production.up.railway.app/prompts/prompt2 \
  -H "Authorization: Bearer {your_token}"
```

### In Postman

- **Method**: `GET`  
- **URL**: `https://promptcrafter-production.up.railway.app/prompts/prompt2`  
- **Headers**:
    - `Authorization`: `Bearer {your_token}`

## 5. Next steps

- [Log a generated output](tutorials/test-prompt.md)  
- [Retrieve logs for a specific prompt](tutorials/view-logs.md)  
- [Update a prompt](reference/endpoints/patch-prompts-id.md)  
- [Search prompts](reference/endpoints/get-search.md)  
- [Explore all API operations](reference/index.md)
