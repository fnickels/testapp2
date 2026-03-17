# Tasks: Modern Weather Lookup Website

**Input**: Design documents from `/specs/001-weather-lookup/`
**Prerequisites**: plan.md (required), spec.md (required), research.md, data-model.md, contracts/, quickstart.md

**Tests**: Included. Testing is required by project constitution (TDD + integration + user-facing validation).

**Organization**: Tasks are grouped by user story to support independent implementation and validation.

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Initialize monorepo tooling and baseline frontend/backend project structure.

- [ ] T001 Initialize npm workspaces in package.json for frontend and backend at package.json
- [ ] T002 Create backend TypeScript project scaffold and scripts in backend/package.json
- [ ] T003 [P] Create backend TypeScript config in backend/tsconfig.json
- [ ] T004 [P] Create frontend Vite React TypeScript scaffold in frontend/package.json
- [ ] T005 [P] Configure linting and formatting for workspace in .eslintrc.cjs and .prettierrc

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Build shared infrastructure required before any user story implementation.

**⚠️ CRITICAL**: No user story tasks should begin until this phase is complete.

- [ ] T006 Implement backend Express app bootstrap and routing mount in backend/src/api/server.ts
- [ ] T007 [P] Implement request logging and request-id middleware in backend/src/middleware/requestLogging.ts
- [ ] T008 [P] Implement centralized error mapping middleware in backend/src/middleware/errorHandler.ts
- [ ] T009 [P] Implement Zod query validation schemas for location and units in backend/src/models/weatherQuerySchema.ts
- [ ] T010 Implement session-based rate limiter (10 req/min) in backend/src/middleware/rateLimit.ts
- [ ] T011 Implement weather provider adapter and Open-Meteo client in backend/src/services/openMeteoClient.ts
- [ ] T012 [P] Implement frontend typed API client for backend weather endpoint in frontend/src/services/weatherApiClient.ts
- [ ] T013 [P] Implement shared frontend weather state store (idle/loading/success/error/rate-limited) in frontend/src/state/weatherStore.ts
- [ ] T014 Configure test runners (Vitest, Supertest, Playwright) and scripts in package.json

**Checkpoint**: Foundation complete. User stories can now be developed independently.

---

## Phase 3: User Story 1 - Search and Display Current Weather (Priority: P1) 🎯 MVP

**Goal**: Users search by location and view current weather facts with locale-default units + manual toggle.

**Independent Test**: Search for a valid city and coordinates, verify weather fields render within 5 seconds, verify unit toggle works, geolocation denial fallback works, and API failures show non-stale error state.

### Tests for User Story 1

- [ ] T015 [P] [US1] Add contract tests for GET /api/weather/current success and error responses in backend/tests/contract/weather-current.contract.test.ts
- [ ] T016 [P] [US1] Add backend integration tests for city, coordinates, invalid input, provider failure, and rate limit in backend/tests/integration/weather-current.integration.test.ts
- [ ] T017 [P] [US1] Add frontend component tests for search form, loading, weather card, and error states in frontend/tests/unit/weather-search-and-display.test.tsx
- [ ] T018 [P] [US1] Add Playwright E2E test for search and render workflow in frontend/tests/e2e/weather-search.e2e.spec.ts

### Implementation for User Story 1

- [ ] T019 [P] [US1] Implement current weather domain mapper from provider payload in backend/src/models/currentWeatherMapper.ts
- [ ] T020 [US1] Implement GET /api/weather/current controller logic in backend/src/api/weatherController.ts
- [ ] T021 [US1] Implement location search form and submission handling in frontend/src/components/LocationSearchForm.tsx
- [ ] T022 [US1] Implement current weather fact card UI in frontend/src/components/CurrentWeatherCard.tsx
- [ ] T023 [US1] Implement locale-default units detection and metric/imperial toggle in frontend/src/state/unitPreference.ts
- [ ] T024 [US1] Implement first-load geolocation permission flow with manual fallback in frontend/src/services/geolocationService.ts
- [ ] T025 [US1] Implement active-location auto-refresh timer (10 minutes) in frontend/src/services/autoRefreshService.ts
- [ ] T026 [US1] Implement no-cache provider-failure error state with retry action in frontend/src/components/WeatherErrorState.tsx
- [ ] T027 [US1] Integrate page-level weather flow orchestration in frontend/src/pages/HomePage.tsx

**Checkpoint**: US1 is fully functional and independently demoable (MVP).

---

## Phase 4: User Story 2 - Modern Responsive User Interface (Priority: P1)

**Goal**: Deliver a modern, responsive, accessible UI that works across desktop, tablet, and mobile.

**Independent Test**: Validate layout and usability at 1920px, 768px, and 375px; confirm touch targets, readability, and keyboard focus behavior.

### Tests for User Story 2

- [ ] T028 [P] [US2] Add frontend responsiveness and accessibility unit tests in frontend/tests/unit/responsive-layout.test.tsx
- [ ] T029 [P] [US2] Add Playwright viewport regression tests for desktop/tablet/mobile in frontend/tests/e2e/responsive-layout.e2e.spec.ts

### Implementation for User Story 2

- [ ] T030 [P] [US2] Implement design tokens (color, spacing, typography) in frontend/src/styles/tokens.css
- [ ] T031 [US2] Implement modern responsive page shell and weather layout in frontend/src/pages/HomePageLayout.tsx
- [ ] T032 [US2] Implement responsive component styles and breakpoints in frontend/src/styles/home.css
- [ ] T033 [US2] Implement accessibility improvements (ARIA labels, focus states, contrast-safe variants) in frontend/src/components/
- [ ] T034 [US2] Implement polished loading skeleton and transitions in frontend/src/components/WeatherLoadingState.tsx

**Checkpoint**: US2 independently validates on all target viewport sizes.

---

## Phase 5: User Story 3 - Save and Access Favorite Locations (Priority: P2)

**Goal**: Users can save favorite locations, persist them, and quickly reload weather by selecting favorites.

**Independent Test**: Save a location, reload the browser, confirm favorite persists, and click favorite to reload weather.

### Tests for User Story 3

- [ ] T035 [P] [US3] Add unit tests for favorites storage and deduplication in frontend/tests/unit/favorites-storage.test.ts
- [ ] T036 [P] [US3] Add Playwright E2E test for add favorite, reload, and quick-select in frontend/tests/e2e/favorites.e2e.spec.ts

### Implementation for User Story 3

- [ ] T037 [US3] Implement favorites localStorage repository in frontend/src/services/favoritesStorage.ts
- [ ] T038 [US3] Implement add/remove favorite actions from current weather view in frontend/src/components/FavoriteActions.tsx
- [ ] T039 [US3] Implement persisted favorites list UI with quick-select behavior in frontend/src/components/FavoritesList.tsx
- [ ] T040 [US3] Integrate favorites flow into page state and weather search pipeline in frontend/src/pages/HomePage.tsx
- [ ] T041 [US3] Implement duplicate-prevention by normalized location name in frontend/src/services/normalizeLocation.ts

**Checkpoint**: US3 independently functional without breaking US1/US2.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final quality improvements and cross-story hardening.

- [ ] T042 [P] Add structured operational logs for provider latency and rate-limit events in backend/src/middleware/requestLogging.ts
- [ ] T043 Add security hardening (input sanitization checks + production headers) in backend/src/api/server.ts
- [ ] T044 [P] Update developer runbook and verification steps in specs/001-weather-lookup/quickstart.md
- [ ] T045 Execute full validation suite (unit, integration, e2e, coverage, lighthouse, a11y) via scripts in package.json
- [ ] T046 Configure coverage thresholds (>=70% on critical paths) and CI enforcement in package.json and .github/workflows/ci.yml
- [ ] T047 Integrate production error tracking SDK and backend/frontend error capture in backend/src/api/server.ts and frontend/src/main.tsx
- [ ] T048 Configure alert routing and escalation target for tracked production errors (<=1 hour surfacing) in specs/001-weather-lookup/quickstart.md

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Setup)**: No dependencies.
- **Phase 2 (Foundational)**: Depends on Phase 1 completion and blocks all user stories.
- **Phase 3 (US1)**: Depends on Phase 2 completion.
- **Phase 4 (US2)**: Depends on Phase 2 completion; can run in parallel with US1 if staffed.
- **Phase 5 (US3)**: Depends on Phase 2 completion; can run in parallel with US1/US2 but typically follows MVP.
- **Phase 6 (Polish)**: Depends on completion of selected user stories.

### User Story Dependencies

- **US1 (P1)**: Independent after foundational phase.
- **US2 (P1)**: Independent after foundational phase.
- **US3 (P2)**: Independent after foundational phase; consumes shared weather state but no hard dependency on US2.

### Within Each User Story

- Tests must be created before implementation and fail first.
- Backend models/services before endpoint integration.
- Frontend state/services before page integration.
- Story completes before being marked done.

### Parallel Opportunities

- Setup tasks marked `[P]` can execute concurrently.
- Foundational middleware/schema/client tasks marked `[P]` can execute concurrently.
- US1 test tasks `T015-T018` can run in parallel.
- US2 tests and token/layout styling tasks (`T028-T030`) can run in parallel.
- US3 tests and storage service implementation (`T035-T037`) can run in parallel.

---

## Parallel Example: User Story 1

```bash
# Run US1 tests in parallel:
T015, T016, T017, T018

# Implement independent US1 modules in parallel:
T019, T021, T022, T023, T024, T025, T026
```

## Parallel Example: User Story 2

```bash
# Run US2 validation in parallel:
T028, T029

# Build independent styling foundations in parallel:
T030, T032
```

## Parallel Example: User Story 3

```bash
# Run US3 tests in parallel:
T035, T036

# Implement storage and UI modules in parallel:
T037, T038, T039, T041
```

---

## Implementation Strategy

### MVP First (US1)

1. Complete Phase 1 and Phase 2.
2. Deliver Phase 3 (US1) end-to-end.
3. Validate US1 independently against its test criteria.
4. Demo/release MVP.

### Incremental Delivery

1. Foundation complete (Phases 1-2).
2. Deliver US1 (core weather flow).
3. Deliver US2 (modern responsive UX enhancements).
4. Deliver US3 (favorites persistence and quick access).
5. Finalize with Phase 6 polish and full-system validation.

### Suggested MVP Scope

- **MVP = User Story 1 only** after foundational setup.
- US2 and US3 can follow as incremental releases.
