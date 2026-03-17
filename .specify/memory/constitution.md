<!--
SYNC IMPACT REPORT
- Version: 1.0.0 (initial)
- Ratification: 2026-03-17
- Changes: Constitution created for dynamic website project with 5 core principles
- Dependent artifacts: plan-template.md, spec-template.md, tasks-template.md (require review for alignment)
-->

# testapp2 Constitution

## Core Principles

### I. User-Centric Interface
The frontend MUST prioritize user experience and accessibility. Every UI
component must be tested with real users or validated against accessibility
standards (WCAG 2.1 AA minimum). Responsive design is mandatory—site must
function on desktop, tablet, and mobile devices.

### II. API-Driven Architecture
Backend functionality MUST be exposed via RESTful or GraphQL APIs. APIs must
be versioned, documented, and support client applications. All business logic
lives in APIs, never in frontend—frontend only handles presentation and
user interaction.

### III. Test-Driven Development
Tests MUST be written before implementation. Unit tests required for all
business logic; integration tests required for API endpoints; user-facing
features require end-to-end tests. Minimum 70% code coverage for critical
paths.

### IV. Performance & Security
Page load times MUST be under 3 seconds on 4G networks. All user input
validated and sanitized server-side. HTTPS required for all communication.
Regular security audits mandatory before production releases.

### V. Observability & Monitoring
Application MUST log errors and key events with timestamps and context.
Error tracking (e.g., Sentry) required in production. Uptime and performance
metrics tracked continuously. Errors surface to development team within
1 hour.

## Technical Stack

- **Frontend**: HTML5, CSS3, JavaScript/TypeScript (framework: TBD)
- **Backend**: TBD (server-side language/framework)
- **Database**: TBD (relational or document store)
- **Deployment**: TBD (cloud provider, containerization)
- All choices require written justification in design docs.

## Development Workflow

- Feature work starts with acceptance criteria approval (user/product)
- Tests written first; then implementation; then code review
- Code review must verify principle compliance (especially testing &
  security)
- Commits atomic, with clear messages referencing issue IDs
- Merges to main require: passing tests, approved review, and passing
  security checks

## Governance

All contributors MUST follow this constitution. Violations block merges.
Amendments require consensus from core team. Breaking changes to APIs or
database schema require new MAJOR version and migration plan. This document
supersedes ad-hoc decisions.

**Version**: 1.0.0 | **Ratified**: 2026-03-17 | **Last Amended**: 2026-03-17
