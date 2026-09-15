# Baiak Idle Deluxe

> Production browser extension for Baiak Idle, developed as a complete software product and currently used by real customers.

## Overview

Baiak Idle Deluxe is a browser extension designed to expand and improve the experience of Baiak Idle players through additional tools, interface improvements, customization features and productivity-oriented functionality.

The project evolved from an extension prototype into a production software product involving frontend development, backend services, licensing, payments, cloud infrastructure, testing, deployment and ongoing maintenance.

This repository is a public technical case study of the project.

The commercial source code and sensitive infrastructure are not publicly distributed.

---

## Project Status

**Status:** Production  
**Platform:** Chromium-based browsers  
**Distribution:** Chrome Web Store  
**Development:** Active maintenance and continuous improvement

The extension has already been publicly released and is being used by real customers.

---

## My Role

I am responsible for the product's development and technical evolution, including:

- Product planning
- Browser extension development
- UI/UX implementation
- Backend integration
- Licensing architecture
- Payment integration
- Database-related workflows
- Cloud deployment
- Testing and QA
- Production debugging
- Release preparation
- Maintenance and compatibility updates
- User feedback implementation

The project has required decisions across the complete software lifecycle, from feature design to production support.

---

## Main Features

Baiak Idle Deluxe includes functionality such as:

- Extended in-game tools
- Custom Control Center
- HUD customization
- Interface themes
- Premium functionality
- License-based feature access
- Product update workflows
- Changelog system
- Support and feedback functionality
- Gameplay-oriented utilities
- UI customization
- Compatibility handling with game updates

The feature set continues to evolve according to technical requirements and user feedback.

---

## Technical Areas

The project involves multiple software engineering areas:

### Browser Extension Development

- Chromium extension architecture
- Content scripts
- Background logic
- DOM integration
- Runtime messaging
- Browser storage
- Page lifecycle handling

### Frontend

- JavaScript
- HTML
- CSS
- Dynamic UI injection
- Responsive interface components
- Theme systems
- Game-interface integration

### Backend

- REST APIs
- License validation
- User entitlement management
- Checkout workflows
- Support/feedback processing
- Administrative operations

### Infrastructure

- Cloudflare Workers
- Cloud deployment
- Environment configuration
- Production secrets
- Database migrations
- API integrations

### Quality Assurance

- Automated tests
- Regression testing
- Compatibility testing
- Manual browser QA
- Production validation
- Release candidate validation

---

## High-Level Architecture

```text
┌──────────────────────────┐
│       Baiak Idle         │
│        Web Game          │
└─────────────┬────────────┘
              │
              │ DOM / Browser Integration
              ▼
┌──────────────────────────┐
│   Baiak Idle Deluxe      │
│   Browser Extension      │
│                          │
│ • UI                     │
│ • Tools                  │
│ • Themes                 │
│ • Premium Features       │
│ • Game Integration       │
└─────────────┬────────────┘
              │
              │ HTTPS / API
              ▼
┌──────────────────────────┐
│     Backend Services     │
│                          │
│ • Licensing              │
│ • Checkout               │
│ • Support                │
│ • Product Services       │
└─────────────┬────────────┘
              │
              ▼
┌──────────────────────────┐
│   Cloud Infrastructure   │
│                          │
│ • Database               │
│ • External APIs          │
│ • Deployment             │
└──────────────────────────┘
