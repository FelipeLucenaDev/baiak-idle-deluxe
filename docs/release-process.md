# Release Process

## Overview

Baiak Idle Deluxe is maintained as a production software product.

Releases are not treated as simple file updates. Changes pass through a structured workflow involving implementation, validation, browser testing, release preparation and production verification.

The objective of this process is to reduce regressions and maintain compatibility with the Baiak Idle web application.

---

## Release Lifecycle

A typical release follows this flow:

```text
Feature / Bug / Compatibility Issue
                │
                ▼
        Technical Analysis
                │
                ▼
          Implementation
                │
                ▼
         Local Validation
                │
                ▼
        Automated Testing
                │
                ▼
        Manual Browser QA
                │
                ▼
       Release Candidate
                │
                ▼
       Production Deployment
                │
                ▼
      Post-Release Validation
                │
                ▼
       User Feedback / Support
```

Not every change requires the same depth of validation, but production-impacting updates are expected to pass through the complete process.

---

# 1. Requirement or Issue Identification

A release may begin from several different sources:

- New product feature
- User request
- Bug report
- Compatibility issue
- Game update
- Backend requirement
- Security improvement
- UI/UX improvement
- Operational requirement

The first step is understanding the actual problem before changing the implementation.

---

# 2. Technical Analysis

Before implementation, the affected system areas are identified.

Examples include:

```text
Extension UI
Background runtime
Content-side integration
Backend API
Database
Licensing
Checkout
Website
Deployment configuration
```

The objective is to determine the smallest practical change that solves the problem without unnecessarily affecting unrelated functionality.

---

# 3. Implementation

Changes are implemented incrementally whenever possible.

This is particularly important in areas that interact directly with the host game.

A smaller technical scope improves:

- Reviewability
- Debugging
- Regression isolation
- Rollback capability
- Testing confidence

Large changes are divided into smaller steps when the problem allows it.

---

# 4. Local Validation

Before broader testing, the modified component is validated locally.

Depending on the change, validation may include:

- Syntax checks
- Runtime inspection
- Browser console verification
- API response validation
- Database behavior validation
- UI behavior inspection
- Configuration review

The goal is to catch obvious failures before progressing to broader QA.

---

# 5. Automated Testing

Automated tests are used where predictable behavior can be validated reliably.

Typical areas include:

- Backend API behavior
- Business rules
- License-related workflows
- Checkout behavior
- Request validation
- Rate limiting
- Administrative endpoints
- Data persistence
- Regression gates

Automated validation is especially valuable for backend functionality because it can verify repeatable behavior before deployment.

---

# 6. Manual Browser QA

Because Baiak Idle Deluxe integrates directly with a live web application, automated testing alone is not sufficient.

Manual QA is performed in the browser for user-facing and compatibility-sensitive behavior.

Examples include:

- Control Center behavior
- Extension initialization
- Game reload behavior
- HUD integration
- Window positioning
- Drag and resize behavior
- Theme compatibility
- Premium feature access
- Native game interaction
- Visual regressions

---

## Compatibility Testing

Compatibility testing is particularly important after game updates.

A previously valid assumption may stop being true if the host application changes:

```text
DOM Structure
CSS
Grid Layout
Events
Panel Containers
Lifecycle Behavior
Native Features
```

When this happens, runtime behavior is inspected before a correction is released.

---

# 7. Release Candidate

Once implementation and validation are complete, the change can be treated as a Release Candidate.

The Release Candidate represents a version intended for production but still subject to final verification.

Typical RC checks include:

- Required functionality works
- Known regressions are resolved
- No development-only files remain in the distribution package
- Production configuration is correct
- Sensitive information is not included
- Manifest configuration is valid
- Version information is correct
- Browser QA has been completed

---

# 8. Production Package Review

Before store deployment, the extension package is reviewed separately from the development workspace.

Development files that are not required at runtime should not be included in the production package.

Examples may include:

```text
Internal documentation
Development notes
Temporary files
Debug artifacts
Test-only resources
Local backups
Unused assets
```

The goal is to publish only what the extension requires in production.

---

# 9. Extension Deployment

The production extension is distributed through the Chrome Web Store.

The deployment process includes:

1. Prepare production package
2. Verify extension version
3. Review manifest and permissions
4. Upload release package
5. Complete store release information
6. Submit the update
7. Wait for store processing/review when applicable

The public store version and the internal development version are treated as separate stages of the release lifecycle.

---

# 10. Backend Deployment

Backend services can be deployed independently of the browser extension.

This is useful because backend corrections do not always require a new store release.

Typical backend deployment considerations include:

- Automated test status
- Environment variables
- Secrets
- Database compatibility
- API contracts
- Migration requirements
- External service integrations

---

# 11. Database Changes

Database changes require additional care because they affect persisted production data.

When schema evolution is required, migrations are used instead of manually rebuilding production state.

A simplified process is:

```text
Schema Requirement
       │
       ▼
Migration Design
       │
       ▼
Local Validation
       │
       ▼
Backup / Safety Check
       │
       ▼
Migration Execution
       │
       ▼
Application Validation
```

Backward compatibility is considered when different product components may temporarily operate on different release versions.

---

# 12. Post-Release Validation

Deployment does not automatically mean the release is complete.

After publication, production behavior is checked again.

Validation may include:

- Extension installation/update
- Product initialization
- Backend availability
- License validation
- Premium access
- Critical feature behavior
- Browser console errors
- Store distribution state

This step helps detect problems that may only appear in the production environment.

---

# 13. Monitoring User Feedback

Real users are an important source of production information.

Feedback may reveal:

- Edge cases
- Compatibility problems
- UI confusion
- Missing functionality
- Unexpected runtime behavior

Reports are investigated as technical inputs rather than treated only as support requests.

---

# 14. Hotfixes

Some production problems require a faster release cycle.

A hotfix still follows the principles of:

```text
Reproduce
   ↓
Diagnose
   ↓
Implement focused fix
   ↓
Validate
   ↓
Release
```

The urgency of the issue should not eliminate validation completely.

The scope of the fix should remain as narrow as practical.

---

# 15. Versioning

Extension releases use explicit version numbers.

Version changes help distinguish:

- Development state
- Release Candidates
- Production versions

Version information is also important when diagnosing user reports because behavior can differ between releases.

---

# Release Responsibilities

The release process includes responsibilities across multiple technical areas.

## Development

- Implement changes
- Review affected components
- Maintain compatibility

## Quality Assurance

- Run automated tests
- Perform manual browser QA
- Verify regressions

## Production Preparation

- Clean release package
- Validate configuration
- Verify version information
- Check permissions

## Deployment

- Publish extension
- Deploy backend changes when required
- Apply database migrations when necessary

## Maintenance

- Validate production behavior
- Investigate reports
- Prepare fixes and future releases

---

# Why This Process Matters

Baiak Idle Deluxe operates in an environment where several independent systems interact:

```text
Browser
+
Game
+
Extension
+
Backend
+
Database
+
External Services
+
Chrome Web Store
```

A change that works in isolation may still cause problems elsewhere.

The release process therefore focuses on reducing uncertainty before production deployment.

---

# Current Release Philosophy

The project follows several practical principles:

- Prefer incremental changes
- Test critical business logic automatically
- Validate user-facing behavior manually
- Keep production packages clean
- Separate client and backend deployments
- Protect sensitive production configuration
- Treat compatibility as an ongoing responsibility
- Validate the product again after deployment

---

# Lessons from Production Releases

Maintaining a production extension introduced practical experience beyond implementation itself.

The project requires consideration of:

- Deployment risk
- Regression management
- Third-party compatibility
- Store distribution
- Production configuration
- Database migration safety
- User feedback
- Hotfix prioritization
- Release discipline

These processes continue to evolve with the product.

---

## Related Documentation

- [System Architecture](architecture.md)
- [Engineering Decisions](engineering-decisions.md)

---

## Author

**Felipe Lucena Marcos**

Software Developer  
Software Engineering Student
