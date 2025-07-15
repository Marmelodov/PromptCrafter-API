# PromptCrafter API: Postman Collection

This repository contains the Postman Collection for the **PromptCrafter API**, a RESTful service designed for organizing, storing, and evaluating generative AI prompts.

> **Note: API server offline**
> The PromptCrafter API server that this collection communicates with is no longer running. This documentation reflects the API as it was implemented and tested. Live requests made from this collection will fail.

### What this Collection provides

This Postman Collection is a complete, interactive reference for every endpoint in the PromptCrafter API. It demonstrates best practices for creating thorough, user-friendly, and interactive documentation.

*   **Interactive "Getting Started" guide:** The main collection description includes a step-by-step tutorial for authenticating and performing a basic workflow.
*   **Pre-configured requests:** Every endpoint is set up as a request, complete with headers, example bodies, and path variables.
*   **Automated authentication:** A test script on the `Log in` request automatically captures the JWT and saves it as a collection variable, allowing you to make authenticated calls without manually copying tokens.
*  **Pre-configured Collection variables**: Key values such as the {{baseUrl}}, authentication {{token}}, and resource IDs ({{promptId}}) are managed as variables. This makes the collection easy to configure for different environments and keeps individual requests clean and easy to read.

### A focus on developer empathy

This collection was built with a deep focus on developer experience (DX), especially around error handling.

*   **Rigorous error documentation:** Every endpoint includes a comprehensive set of saved example responses for potential errors. This collection provides distinct examples for different failure scenarios, such as `Error: Missing Required Fields`, `Error: Invalid ID Format`, or `Error: Access Forbidden`. This removes ambiguity and helps developers troubleshoot issues quickly and efficiently.

### Project context

This Postman Collection is a part of the broader **PromptCrafter API Documentation Portfolio**, designed to be a full suite of professional-grade API documentation.

*   **[View the live API documentation](https://marmelodov.github.io/PromptCrafter-API/)**
*   **[Explore the project on GitHub](https://github.com/Marmelodov/PromptCrafter-API)**

The full portfolio includes complete Markdown-based API documentation, an OpenAPI 3.0 specification, and several documented SDKs. The Markdown API documentation served as the single source of truth for this Collection, the OpenAPI spec, and the SDKs.
