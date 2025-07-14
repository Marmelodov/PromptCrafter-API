# PromptCrafter API Specification

This OpenAPI specification is a portfolio piece designed to demonstrate advanced skills in API documentation, developer experience (DX), and technical communication.

The document describes the PromptCrafter API, a REST service for managing, organizing, and evaluating generative AI prompts. It's written to show best practices in clear, comprehensive, and developer-centric documentation.

## Key documentation features

- **Practical use-case examples:** Provides scenario-specific examples of API request and response bodies, helping developers quickly grasp how to use the API for various tasks.
- **Clear error handling guidance:** Simplifies debugging not just by documenting error codes, but by providing detailed descriptions and specific JSON examples for each type of error.
- **Precise data schemas and validation:** Defines data models with field constraints, helping developers submit valid data and avoid common errors.
- **Proactive performance notes:** Includes contextual notes on potential performance issues, such as the lack of pagination on list endpoints, so developers know upfront what to expect.
- **Secure and well-documented authentication:** Offers a clear, step-by-step guide to implementing the API's token-based authentication scheme.

## How to view the specification

- **Preview in an interactive UI:** Import the `openapi.yaml` file into a viewer like [Swagger Editor](https://editor.swagger.io/) or [Redocly](https://redocly.github.io/redoc/) for an interactive experience.
- **View the published documentation site:** A fully rendered version of the documentation is available at [https://marmelodov.github.io/PromptCrafter-API/](https://marmelodov.github.io/PromptCrafter-API/).

## API Overview

- **/auth**: Endpoints for signing up, logging in, and managing token.
- **/prompts**: Full CRUD (Create, Read, Update, Delete) endpoints for managing prompts.
- **/logs**: Endpoints for tracking and retrieving logged outputs.
- **/search**: A dedicated endpoint for full-text search across prompts.

## Related Resources

- [GitHub repository](https://github.com/Marmelodov/PromptCrafter-API)
- [Published documentation](https://marmelodov.github.io/PromptCrafter-API/)
- [Postman Collection](https://github.com/Marmelodov/PromptCrafter-API/tree/main/postman)
- SDKs: Available in the SDKs directory of the GitHub repository
