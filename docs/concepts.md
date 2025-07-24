# Concepts

PromptCrafter API helps developers, prompt engineers, and AI practitioners organize, test, and improve their generative AI prompts. This page explains the key ideas and mental models you need to understand before using the API.

## What PromptCrafter Does

PromptCrafter solves a common problem: as you experiment with generative AI, you accumulate dozens or hundreds of prompts scattered across different files, tools, and conversations. You lose track of what works, what doesn't, and why. PromptCrafter provides a structured way to store your prompts, record their outputs, and track their performance over time.

Think of it as a specialized database for prompt engineering, designed around the unique needs of working with AI models.

## Core Data Model

PromptCrafter organizes information around three main entities:

### Prompts

A **prompt** is the text you send to an AI model to generate output. In PromptCrafter, each prompt includes:

- **Content**: The actual text of your prompt
- **Title**: A human-readable name for easy identification
- **Model**: Which AI model you intend to use (GPT-4, Claude, etc.)
- **Tags**: Labels for categorization and search

Prompts are your reusable templates. You might have a prompt for "writing product descriptions" or "explaining technical concepts to beginners."

### Logs

A **log** captures what happened when you tested a prompt. Each log records:

- **Output**: What the AI model actually generated
- **Model Used**: Which model generated this output (may differ from the prompt's intended model)
- **Score**: Your rating of the output's quality (0-10)
- **Notes**: Your observations about the result

Logs let you compare different attempts with the same prompt or track how a prompt performs across different models.

### The Prompt-Log Relationship

Every log belongs to exactly one prompt, but each prompt can have many logs. This relationship captures the reality of prompt engineering: you write a prompt once, then test it multiple times under different conditions.

For example, you might test your "product description" prompt with GPT-4, Claude, and a local model, creating three separate logs to compare their outputs.

## User Accounts and Data Ownership

Each user has a completely separate library. Your prompts and logs are private—no other user can see or access them. This isolation ensures you can experiment freely without worrying about exposing sensitive or proprietary prompts.

When you create an account, PromptCrafter generates a unique identifier for you. All your prompts and logs are linked to this identifier, ensuring clean separation between users.

## Authentication and Security

PromptCrafter uses JSON Web Tokens (JWT) for authentication. Here's how it works:

1. **Sign up** to create your account
2. **Log in** to receive a JWT token
3. **Include the token** in the `Authorization` header for all subsequent requests

The token acts like a temporary key that proves your identity. Tokens eventually expire, requiring you to log in again—this limits the damage if a token is compromised.

All API endpoints except signup and login require authentication. There are no "public" prompts or logs in PromptCrafter.

## Search and Discovery

As your prompt library grows, finding specific prompts becomes challenging. PromptCrafter's search examines three fields:

- **Title**: The name you gave the prompt
- **Content**: The full text of the prompt
- **Tags**: All labels you've applied

Search is case-insensitive and matches partial words. Searching for "photo" will find prompts containing "photosynthesis."

This approach reflects how you actually think about prompts—sometimes you remember the topic, sometimes a specific phrase, sometimes just a tag you used.

## Error Handling Philosophy

PromptCrafter follows standard HTTP conventions for errors:

- **400 Bad Request**: Your request is malformed (missing fields, invalid data)
- **401 Unauthorized**: You're not authenticated or your token expired
- **403 Forbidden**: You're trying to access someone else's data
- **404 Not Found**: The resource doesn't exist
- **500 Internal Server Error**: Something broke on the server

Each error includes a human-readable message explaining what went wrong. The API prioritizes clear error messages over terse technical codes.

## Data Persistence and Reliability

Your prompts and logs are permanently stored unless you explicitly delete them. PromptCrafter doesn't automatically expire or clean up your data—your prompt library persists indefinitely.

Updates to prompts modify the existing record and update the `updatedAt` timestamp. This preserves the creation date while tracking when changes occurred.

Deletions are permanent and cannot be undone. When you delete a prompt, all associated logs are also deleted to maintain data consistency.

## Resource Relationships and Constraints

Several important rules govern how prompts and logs relate:

- **Logs require valid prompts**: You cannot create a log without specifying a prompt that exists and belongs to you
- **Prompt deletion cascades**: Deleting a prompt removes all its logs
- **Ownership is immutable**: You cannot transfer prompts or logs between users

These constraints prevent orphaned data and maintain clear ownership boundaries.

## API Versioning and Stability

PromptCrafter API uses a single version (1.0.0) for now. The API prioritizes stability—existing endpoints won't change in ways that break your code.

Future versions will be additive (new endpoints, new optional fields) rather than breaking (removing endpoints, changing required fields).

## Performance Considerations

The API returns complete, unpaginated lists for both prompts and logs. This works well for individual users but means response times increase as your library grows.

For applications managing large prompt libraries (hundreds of prompts or thousands of logs), consider caching frequently accessed data on the client side.

## Integration Patterns

PromptCrafter works best as a complement to your existing AI development tools. Common integration patterns include:

- **Prompt storage**: Save successful prompts from your development environment
- **Testing automation**: Programmatically create logs when running prompt tests
- **Performance tracking**: Build dashboards using your prompt scores and notes
- **Team coordination**: Share prompt IDs with teammates (though the prompts themselves remain private)

The API's simplicity makes it easy to integrate into existing scripts, applications, and development processes.

## Model Flexibility

PromptCrafter doesn't integrate directly with AI model APIs—it's model-agnostic. You specify which model you intend to use (stored in the prompt) and which model you actually used (stored in the log), but you handle the actual AI model interactions in your own code.

This separation keeps PromptCrafter focused on organization and tracking rather than trying to wrap every possible AI service.

## Glossary

- **Prompt:** A user-authored input for a generative AI model.
- **Log:** A saved record of a model’s output for a prompt, with optional notes and score.
- **Model:** The AI system used to generate output (e.g., GPT-4o).
- **Tag:** A keyword used to organize or find prompts.
- **JWT (JSON Web Token):** The authentication mechanism for securing API requests.

---

For detailed API endpoints, parameters, and usage, see the [Reference](./reference.md) section.
