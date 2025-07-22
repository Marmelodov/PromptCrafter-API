# PromptCrafter API Documentation Portfolio

**Note:** this repository shows the complete developer documentation, reference, and tooling for PromptCrafter API, a backend service for organizing, storing, and evaluating generative AI prompts. While the live server is now offline, this project presents a production-ready documentation suite developed for a fully functional API.

## Table of contents

- [PromptCrafter API Documentation Portfolio](#promptcrafter-api-documentation-portfolio)
  - [Table of contents](#table-of-contents)
  - [Overview](#overview)
  - [Features](#features)
  - [Documentation](#documentation)
  - [Postman Collection](#postman-collection)
  - [OpenAPI Specification](#openapi-specification)
  - [SDKs](#sdks)
  - [How to use this repository](#how-to-use-this-repository)
  - [Project status](#project-status)
  - [Background and credits](#background-and-credits)
  - [Contact](#contact)

## Overview

PromptCrafter is a backend REST API for storing, organizing, and evaluating generative AI prompts. It enables developers, prompt engineers, and others working with generative AI to maintain a structured prompt library, track changes and performance over time, and integrate prompt data into external tools and applications.

This repository contains the full API documentation, reference materials, and supporting tools, demonstrating best practices in API documentation, developer experience, and technical communication. It is intended as a portfolio sample for technical writing roles.

View the published [PromptCrafter API documentation](https://Marmelodov.github.io/PromptCrafter-API/) site.

## Features

- **Comprehensive API reference:** clear endpoint descriptions, detailed schema documentation, and multiple usage examples.
- **Postman Collection:** ready-to-import collection for exploring all endpoints interactively (see [Postman Collection](./postman/)).
- **OpenAPI Specification:** industry-standard OpenAPI (Swagger) file for code generation and integration (see [OpenAPI Specification](./openapi/)).
- **Multi-language SDKs:** documentation for generated SDKs in Python, JavaScript, Go, Java, and Ruby.
- **Tutorials and guides:** step-by-step usage examples and instructions for all major API operations.
- **Realistic resource examples:** rich, real-world example payloads for all resources.

## Documentation

The complete documentation, located in the [`/docs`](./docs/) folder, provides a comprehensive guide for developers. It's organized to take a user from initial setup to advanced API usage.

Key documentation includes:

- **Getting started:** instructions for authentication (sign-up, login, JWT usage), core request/response patterns, and initial setup.
- **Endpoints reference:** detailed breakdowns of all API resources, including **prompts**, **logs**, and **users**. Each reference provides endpoint descriptions, parameters, and error handling guidance.
- **Resource schemas:** full descriptions of the main data objects and their fields.
- **Usage examples:** realistic API call request/response examples for all major operations.
- **Troubleshooting:** explanations of common errors and guidance for resolving them.

See the complete documentation in the [**docs folder**](./docs/).

## Postman Collection

A complete Postman Collection is included to allow testing of all endpoints in an interactive environment.

- **File:** [`/postman/PromptCrafter.postman_collection.json`](./postman/PromptCrafter.postman_collection.json)
- **How to use:**  
  1. Open Postman and import the collection file.
  2. Review request examples, parameters, and sample responses.
  3. Note: The live API server is currently offline; requests do not return live data, but example responses and documentation remain available.

## OpenAPI Specification

The OpenAPI (Swagger) file provides a machine-readable description of the API to be used with tools, code generators, and developer portals.

- **File:** [`/openapi/openapi.yaml`](./openapi/openapi.yaml)
- **Use cases:**  
    - Generate client libraries.
    - Integrate with documentation tools.
    - Validate and test against schema.

<img src="./Screenshot%202025-07-15%20205113.png" alt="alt text" width="76%">

## SDKs

Documentation for generated SDKs are provided in the following languages:

- **Python** (`/sdk/python/`)
- **JavaScript** (`/sdk/javascript/`)
- **Go** (`/sdk/go/`)
- **Ruby** (`/sdk/ruby/`)
- **Java** (`/sdk/java/`)

Documentation for each SDK accounts for the idiom and particular challenges of the language.

## How to use this repository

Although the live API server is not currently available, you can:

- **Review the documentation** to see examples of API reference structure, usage examples, and schema descriptions.
- **Import the Postman Collection** to explore example requests, payloads, and responses.
- **Examine the OpenAPI Spec** for schema details and integration patterns.
- **Use SDK samples** to see idiomatic usage in multiple programming languages.
- **Assess technical writing quality** and communication clarity throughout the project.

## Project status

- **Current state:** documentation, reference files, and sample SDKs are complete and production-quality.  
- **API server:** currently offline (see below).
- **Demo/testing:** example responses and usage patterns are included throughout the documentation and Postman Collection.

> **About the API server:**  
> The PromptCrafter API was fully functional and used for documentation development, validation, and testing. While the hosted server is now offline, the documentation is designed according to industry standards and reflects best practices for REST API documentation.

## Background and credits

This project originated as the capstone for the University of Washington Specialization in API Documentation. The project used the same tools, workflows, and developer-centered approach found in professional API documentation projects.

> **Why this matters:**  
> The documentation demonstrates command of developer experience, reference design, API tooling, and technical communication. It also reflects experience working from product requirements to final deliverables.

## Contact

**Author:** Robert Norrell  
**LinkedIn:** [linkedin.com/in/robert-norrell-268b249b](https://linkedin.com/in/robert-norrell-268b249b)  
**Email:** robertjnorrell@gmail.com

