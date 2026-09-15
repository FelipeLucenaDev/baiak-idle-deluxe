# System Architecture

## Overview

Baiak Idle Deluxe is composed of a browser extension integrated with the Baiak Idle web application and a set of backend services responsible for product-related operations.

The architecture separates client-side game integration from sensitive server-side operations.

This document intentionally describes only the public, high-level architecture of the product.

Implementation details that could expose proprietary logic, credentials, security mechanisms or production infrastructure are intentionally omitted.

---

## Architectural Goals

The architecture was designed around several main requirements:

- Integrate additional functionality into an existing web application
- Preserve compatibility with a third-party interface that may change over time
- Keep sensitive operations outside the browser whenever appropriate
- Support free and premium functionality
- Validate product licenses remotely
- Support checkout and entitlement workflows
- Allow independent backend evolution
- Provide production diagnostics and maintenance capabilities
- Minimize exposure of sensitive implementation details in the client

---

## High-Level Architecture

```text
┌──────────────────────────────────────────┐
│               Baiak Idle                 │
│              Web Application             │
└───────────────────┬──────────────────────┘
                    │
                    │ DOM Integration
                    │ Runtime Interaction
                    ▼
┌──────────────────────────────────────────┐
│          Baiak Idle Deluxe               │
│          Browser Extension               │
│                                          │
│  • Control Center                        │
│  • Game tools                            │
│  • HUD integration                       │
│  • Theme system                          │
│  • Premium features                      │
│  • Local preferences                     │
│  • Backend communication                 │
└───────────────────┬──────────────────────┘
                    │
                    │ HTTPS / REST
                    ▼
┌──────────────────────────────────────────┐
│             Backend Services             │
│                                          │
│  • License validation                    │
│  • Entitlement management                │
│  • Checkout workflows                    │
│  • Support / feedback                    │
│  • Administrative operations             │
└───────────────────┬──────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────┐
│          Cloud Infrastructure            │
│                                          │
│  • Persistent data                       │
│  • Environment configuration             │
│  • External service integrations         │
│  • Deployment infrastructure             │
└──────────────────────────────────────────┘
```

---

# Browser Extension Layer

The browser extension is the client-facing component of the product.

It runs inside Chromium-based browsers and integrates with the Baiak Idle interface.

Its responsibilities include:

- Injecting extension interfaces into the game environment
- Managing extension UI components
- Providing additional gameplay-oriented tools
- Handling HUD-related functionality
- Applying theme customization
- Storing local user preferences
- Communicating with background extension logic
- Communicating with backend APIs
- Controlling access to premium functionality

---

## Content-Side Integration

Part of the extension operates directly inside the game's page environment.

This layer is responsible for interacting with the existing DOM and responding to changes in the game interface.

Typical responsibilities include:

```text
Game DOM
   │
   ├── Detect supported interface elements
   ├── Add extension components
   ├── Observe interface lifecycle changes
   ├── Maintain extension UI state
   └── Adapt to compatible game updates
```

Because the extension integrates with software that is developed independently, this layer must be resilient to changes in the host application.

---

# Extension Runtime Communication

Different extension components communicate through the browser extension runtime.

A simplified communication model is:

```text
User Interaction
       │
       ▼
Injected / Content UI
       │
       │ Extension Messaging
       ▼
Background Logic
       │
       ├── Local extension operations
       │
       └── Backend API requests
```

Separating responsibilities reduces unnecessary coupling between page integration and backend-related operations.

---

# Backend Layer

Backend services handle operations that should not depend entirely on client-side execution.

These services include product-related functionality such as:

- License validation
- Product entitlement verification
- Checkout-related workflows
- Support and feedback processing
- Administrative operations
- Integration with external services

The browser extension communicates with these services using HTTPS APIs.

---

## Simplified Request Flow

A typical backend interaction can be represented as:

```text
Extension
    │
    │ HTTPS Request
    ▼
API Endpoint
    │
    ├── Request validation
    ├── Business rules
    ├── Data access
    └── External integrations
    │
    ▼
API Response
    │
    ▼
Extension
```

Client-side applications are treated as untrusted environments.

Sensitive credentials and privileged server operations are therefore kept outside the distributed extension whenever possible.

---

# Licensing Architecture

Premium functionality requires remote product entitlement validation.

At a high level:

```text
Extension
    │
    ▼
License Validation Request
    │
    ▼
Backend
    │
    ├── Validate request
    ├── Check entitlement
    ├── Apply business rules
    └── Return authorization state
    │
    ▼
Extension
    │
    ▼
Enable / Restrict Premium Features
```

Exact validation mechanisms are intentionally not documented publicly.

---

# Checkout Integration

Purchase flows are separated from extension UI logic.

The extension initiates the appropriate product workflow while sensitive checkout processing remains server-side.

Simplified flow:

```text
Extension
    │
    ▼
Checkout Request
    │
    ▼
Backend
    │
    ▼
Payment Provider
    │
    ▼
Purchase Confirmation
    │
    ▼
Backend
    │
    ▼
Product Entitlement
```

This separation helps reduce exposure of payment-related logic inside the browser extension.

---

# Support and Feedback

The product also contains a support workflow integrated with backend services.

At a high level:

```text
Extension Support UI
        │
        ▼
Support API
        │
        ├── Validate request
        ├── Apply service limits
        ├── Process message
        └── Forward support data
        │
        ▼
Developer Support Channel
```

---

# Data and Configuration

Different categories of information are handled according to their purpose.

## Client-side

Examples include:

- UI preferences
- Extension configuration
- Non-sensitive user settings

## Server-side

Examples include:

- Product-related records
- License-related information
- Checkout state
- Administrative data

Sensitive configuration is managed through production environment configuration rather than embedded directly into public source code.

---

# Compatibility Strategy

One of the main architectural challenges is integration with a live third-party web application.

A game update can alter:

- DOM structure
- CSS behavior
- UI containers
- Event handling
- Component lifecycle
- Native game features

The extension therefore requires a compatibility workflow:

```text
Game Update
     │
     ▼
Runtime Observation
     │
     ▼
Compatibility Issue Detected
     │
     ▼
Technical Diagnosis
     │
     ▼
Isolated Fix
     │
     ▼
Regression Validation
     │
     ▼
Browser QA
     │
     ▼
Release
```

This has become an important part of the product's maintenance architecture.

---

# Deployment Model

The system contains independently deployable components.

```text
Browser Extension
       │
       ├── Versioned build
       └── Chrome Web Store deployment

Backend Services
       │
       ├── Server-side deployment
       └── Environment configuration

Public Website
       │
       └── Independent web deployment
```

This separation allows individual product components to evolve without requiring every part of the system to be released simultaneously.

---

# Security Boundaries

The architecture follows several basic security principles:

- Production secrets are not embedded in public documentation
- Sensitive credentials are not stored directly in distributed client code
- Privileged operations are handled server-side where appropriate
- External requests are validated by backend services
- Public documentation excludes sensitive licensing details
- Payment-related credentials remain outside the browser extension

This repository intentionally documents architecture without exposing security-sensitive implementation details.

---

# Architectural Evolution

Baiak Idle Deluxe began as a smaller browser extension and evolved into a product containing multiple interconnected systems.

Its architecture now supports:

```text
Browser Extension
        +
Backend Services
        +
Licensing
        +
Checkout
        +
Support
        +
Cloud Infrastructure
        +
Production Deployment
        +
Continuous Maintenance
```

The architecture continues to evolve as new product requirements and compatibility challenges emerge.

---

## Related Documentation

- [Engineering Decisions](engineering-decisions.md)
- [Release Process](release-process.md)

These documents will be added as the public technical case study evolves.

---

## Author

**Felipe Lucena Marcos**

Software Developer  
Software Engineering Student
