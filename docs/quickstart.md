---
layout: page
---

# Quickstart Guide

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

- **Method**: POST  
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

- **Method**: POST  
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

Note: Replace `{your_token}` with the bearer token returned by your login request.

### In cURL

```shell
curl -X POST https://promptcrafter-production.up.railway.app/prompts \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "AI image prompt",
    "content": "Create a prompt for a surreal mountain landscape painting.",
    "model": "DALL·E",
    "tags": ["art", "surreal", "landscape"]
  }'
```

### In Postman

- **Method**: POST  
- **URL**: `https://promptcrafter-production.up.railway.app/prompts`  
- **Headers**:
    - `Authorization`: `Bearer {your_token}`
    - `Content-Type`: `application/json`
- **Body** (raw JSON):

```json
{
  "title": "AI image prompt",
  "content": "Create a prompt for a surreal mountain landscape painting.",
  "model": "DALL·E",
  "tags": ["art", "surreal", "landscape"]
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

- **Method**: GET  
- **URL**: `https://promptcrafter-production.up.railway.app/prompts/prompt2`  
- **Headers**:
    - `Authorization`: `Bearer {your_token}`

## 5. Next steps

- [Test a prompt and save the output](tutorials/test-prompt.md)  
- [View logs of your tested prompts](tutorials/view-logs.md)  
- [Update a prompt](reference/endpoints/patch-prompts-id.md)  
- [Search prompts](reference/endpoints/get-search.md)  
- [Explore all API operations](reference/index.md)

