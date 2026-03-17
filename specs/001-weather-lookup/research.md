# Research: Modern Weather Lookup Website

## Decision 1: Use frontend + backend split (React + Express)
- Decision: Implement a React frontend and Node/Express backend API.
- Rationale: Enforces constitution API-driven boundary, keeps provider-specific logic and validation server-side, and supports future auth/analytics expansion.
- Alternatives considered: Frontend-only app calling provider directly (rejected: exposes provider coupling/secrets risk, weakens business-logic boundary); monolithic server-rendered app (rejected: slower iteration for rich client UX).

## Decision 2: Use Open-Meteo as initial weather provider
- Decision: Integrate Open-Meteo via backend service adapter.
- Rationale: No API key requirement for MVP, strong global coverage, predictable response schema for current-weather metrics.
- Alternatives considered: OpenWeatherMap (rejected for MVP due to key management overhead); WeatherAPI.com (rejected due to plan limits for free tier).

## Decision 3: Validate all inbound inputs with Zod schemas
- Decision: Apply schema validation at API boundary for location query, coordinates, and unit toggle values.
- Rationale: Meets constitution security requirement for server-side validation/sanitization and gives deterministic error messages.
- Alternatives considered: Ad-hoc validator utilities (rejected: error-prone/inconsistent); class-validator (rejected: heavier setup for MVP).

## Decision 4: Enforce client-visible rate limit in backend middleware
- Decision: Implement a per-session key (session token header/cookie) with 10 requests/minute policy and standardized 429 payload.
- Rationale: Aligns with clarified requirement FR-015 and keeps enforcement authoritative.
- Alternatives considered: Frontend-only throttling (rejected: bypassable); external gateway-only limits (rejected for initial local/dev simplicity).

## Decision 5: Implement no-cache error behavior for provider/API failures
- Decision: Return explicit weather-unavailable error state; do not return stale weather snapshots.
- Rationale: Directly matches clarification and prevents misrepresenting outdated weather as current.
- Alternatives considered: 30-minute stale fallback (rejected by clarification); offline cache mode (out-of-scope for MVP).

## Decision 6: Testing stack and coverage strategy
- Decision: Use Vitest + RTL for frontend unit tests, Supertest-based backend integration tests, and Playwright for E2E flows.
- Rationale: Fast local feedback plus full user-path assurance; maps directly to constitution TDD and integration requirements.
- Alternatives considered: Cypress for E2E (rejected due to team preference for Playwright cross-browser controls); Jest-only for frontend (rejected due to slower Vite integration).

## Decision 7: Observability baseline
- Decision: Log structured JSON for request IDs, provider latency, rate-limit events, and error categories; include `/api/health` endpoint.
- Rationale: Satisfies observability principle with minimum operational overhead.
- Alternatives considered: Full telemetry stack in MVP (rejected as premature complexity); console-only logs (rejected: poor production diagnostics).
