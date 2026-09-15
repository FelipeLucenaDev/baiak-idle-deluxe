# Engineering Decisions

## Overview

Baiak Idle Deluxe evolved from a browser extension into a production software product with multiple interconnected components.

As the project grew, technical decisions increasingly needed to balance:

- Maintainability
- Compatibility
- Product security
- User experience
- Operational simplicity
- Production reliability
- Speed of iteration

This document describes selected engineering decisions and the reasoning behind them.

It intentionally avoids exposing proprietary implementation details.

---

## 1. Separating Client and Server Responsibilities

### Problem

Browser extensions execute in a client-controlled environment.

Any logic distributed with the extension can potentially be inspected, modified or reverse engineered.

This creates limitations when dealing with:

- Licensing
- Premium entitlement
- Payment-related workflows
- Administrative operations
- Sensitive credentials
- Business rules

### Decision

Sensitive or privileged operations are handled by backend services whenever appropriate.

The extension remains responsible for:

- User interface
- Game integration
- Local product functionality
- User interaction
- API communication

The backend is responsible for operations that require stronger control over business rules and sensitive data.

### Reasoning

This provides a clearer security boundary:

```text
Browser
   │
   │ Untrusted client environment
   ▼
Extension
   │
   │ Validated HTTPS requests
   ▼
Backend
   │
   │ Controlled execution environment
   ▼
Sensitive Operations
```

### Trade-off

This increases system complexity because the product is no longer fully self-contained inside the extension.

It introduces:

- Network dependency
- API maintenance
- Backend deployment
- Error handling for unavailable services

The trade-off is acceptable because it provides better separation of responsibility and reduces exposure of sensitive operations.

---

## 2. Keeping the Commercial Source Code Private

### Problem

The product contains commercial logic, licensing mechanisms and production-specific implementation details.

Publishing the complete production source code would expose information that is not necessary for demonstrating the engineering work behind the product.

### Decision

The production source code remains private.

A separate public repository is used as a technical case study.

### Reasoning

The public repository can demonstrate:

- Architecture
- Engineering decisions
- Technologies
- Product lifecycle
- Production challenges
- Development practices

without exposing:

- Production credentials
- Proprietary business logic
- Sensitive licensing implementation
- Internal administrative tools
- Security-sensitive configuration

### Result

The portfolio remains technically useful while preserving the commercial and security boundaries of the product.

---

## 3. Treating the Host Application as an External Dependency

### Problem

Baiak Idle Deluxe integrates with an application that is developed independently.

The extension does not control changes made to:

- DOM structure
- CSS
- UI containers
- Native event handlers
- Game panels
- Layout systems

An update to the game can therefore cause previously working extension functionality to fail.

### Decision

The host application is treated as an external dependency whose behavior may change.

Compatibility is considered a continuous maintenance requirement rather than a one-time integration problem.

### Engineering Impact

The maintenance process includes:

```text
Host Application Update
        │
        ▼
Runtime Observation
        │
        ▼
Behavior Difference
        │
        ▼
Compatibility Diagnosis
        │
        ▼
Targeted Correction
        │
        ▼
Regression Testing
        │
        ▼
Manual Browser QA
```

### Reasoning

Trying to assume that the game's DOM and internal UI structure are permanently stable would make the extension fragile.

Recognizing the host as an external dependency leads to more defensive development and more deliberate compatibility testing.

---

## 4. Avoiding Broad Fixes for Compatibility Problems

### Problem

When an external game update causes UI or runtime incompatibility, a large change can potentially fix the immediate problem while creating regressions elsewhere.

### Decision

Compatibility problems are addressed through isolated and targeted corrections whenever possible.

### Preferred Process

```text
Observe
   ↓
Reproduce
   ↓
Identify exact affected behavior
   ↓
Isolate component
   ↓
Apply smallest practical fix
   ↓
Validate surrounding behavior
```

### Reasoning

This reduces the risk of unrelated regressions.

It also makes future debugging easier because each compatibility correction has a narrower technical scope.

---

## 5. Combining Automated Validation with Manual Browser QA

### Problem

Automated tests can verify business logic, API behavior and predictable execution paths.

However, browser extensions that interact with a live game UI also depend heavily on:

- Layout
- DOM timing
- Drag-and-drop behavior
- Visual integration
- Runtime browser state
- Native game events

These behaviors cannot always be fully represented through automated tests alone.

### Decision

The project uses both automated validation and manual browser QA.

### Automated Validation

Useful for areas such as:

- Backend business rules
- API behavior
- Data validation
- License-related workflows
- Rate limiting
- Checkout behavior
- Regression gates

### Manual QA

Used for areas such as:

- UI positioning
- Game integration
- Visual compatibility
- Extension lifecycle
- Browser reload behavior
- Native/extension interaction
- User-facing workflows

### Reasoning

Neither testing approach is sufficient by itself.

Together they provide broader confidence before release.

---

## 6. Maintaining Independent Deployable Components

### Problem

The product contains multiple components:

- Browser extension
- Backend services
- Public website

These components do not always need to change at the same time.

### Decision

They are treated as independently deployable systems.

### Example

```text
Extension Update
     │
     └── Chrome Web Store

Backend Update
     │
     └── Cloud Deployment

Website Update
     │
     └── Independent Web Deployment
```

### Reasoning

This allows:

- Backend fixes without waiting for a store review
- Website updates without modifying the extension
- Extension releases only when client functionality changes

### Trade-off

Independent deployment requires stronger version awareness and compatibility between components.

---

## 7. Keeping Secrets Outside Distributed Client Code

### Problem

Browser extension code is distributed to users and should not be considered a secure place for secrets.

### Decision

Sensitive credentials and privileged configuration are managed outside the extension.

Examples include:

- Server credentials
- Private API credentials
- Payment credentials
- Administrative secrets
- Infrastructure configuration

### Reasoning

Client-side applications must be considered inspectable.

Anything embedded directly in distributed code may eventually become visible to users.

---

## 8. Building Premium Features Around Entitlements

### Problem

The extension includes both free and premium functionality.

The system needs a way to determine whether a user is authorized to access premium features.

### Decision

Premium access is modeled around product entitlement rather than purely local client configuration.

### Simplified Concept

```text
User
  │
  ▼
Extension
  │
  ▼
Entitlement Validation
  │
  ▼
Backend
  │
  ▼
Access State
  │
  ▼
Premium Feature Availability
```

### Reasoning

A purely local flag would provide weak control over premium functionality.

Remote entitlement allows business rules to remain under backend control.

---

## 9. Keeping the UI Integrated but Isolated

### Problem

The extension needs to visually integrate into the game while avoiding unnecessary interference with native game functionality.

### Decision

Extension interfaces are designed to integrate with the host visually while maintaining their own behavioral boundaries where appropriate.

This became especially important for:

- HUD positioning
- Extension windows
- Drag behavior
- Theme application
- Native game panels

### Reasoning

Direct integration improves user experience, but uncontrolled coupling to native game behavior creates maintenance problems.

The objective is therefore:

```text
Visual Integration
        +
Behavioral Isolation
        =
More Predictable Extension UI
```

---

## 10. Designing for Continuous Product Evolution

### Problem

A production product does not stop changing after its first release.

New requirements emerge from:

- Users
- Game updates
- Bugs
- Compatibility issues
- New premium functionality
- Business needs

### Decision

The project is maintained as an evolving product rather than a finished static extension.

### Development Model

```text
Release
  ↓
Production Usage
  ↓
Feedback
  ↓
New Requirement / Bug
  ↓
Technical Analysis
  ↓
Implementation
  ↓
Validation
  ↓
Next Release
```

### Reasoning

Real-world usage continuously exposes new technical requirements.

The architecture and development process therefore need to support maintenance and incremental evolution.

---

# Decision-Making Principles

Across the project, several principles guide technical decisions.

## Prefer Controlled Complexity

Additional architecture should solve a real problem.

Complexity is accepted when it provides clear benefits in areas such as:

- Security
- Maintainability
- Product reliability
- Operational control

---

## Prefer Small, Reversible Changes

When dealing with compatibility-sensitive code, smaller changes are generally easier to:

- Test
- Review
- Revert
- Diagnose

---

## Preserve Clear System Boundaries

Client-side UI, backend business logic and production infrastructure should have clearly defined responsibilities.

---

## Treat Production Feedback as Engineering Input

User reports are not handled only as support issues.

They can also provide information about:

- Missing edge cases
- Compatibility failures
- UX problems
- Unexpected runtime behavior

This feedback becomes part of the engineering cycle.

---

## Validate Before Release

A feature is not considered finished only because the implementation is complete.

The release process also includes:

- Testing
- Regression checks
- Browser validation
- Production readiness review

---

# What I Learned from These Decisions

Developing Baiak Idle Deluxe required moving beyond isolated feature implementation.

The project introduced practical experience with:

- System boundaries
- Production maintenance
- Security considerations
- Client/server architecture
- External application compatibility
- Release management
- Regression prevention
- Real user feedback
- Technical trade-offs

These decisions continue to evolve as the product grows.

---

## Related Documentation

- [System Architecture](architecture.md)
- [Release Process](release-process.md)

---

## Author

**Felipe Lucena Marcos**

Software Developer  
Software Engineering Student
