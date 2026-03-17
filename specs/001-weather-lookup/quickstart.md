# Quickstart: Modern Weather Lookup Website

## Prerequisites
- Node.js 22 LTS
- npm 10+
- Internet access (weather provider calls)

## 1. Install dependencies
```bash
npm install
```

## 2. Start backend API
```bash
npm run dev:backend
```
- Expected base URL: `http://localhost:3000`
- Health check: `GET /api/health` returns `200 { status: "ok" }`

## 3. Start frontend app
```bash
npm run dev:frontend
```
- Expected URL: `http://localhost:5173`

## 4. Validate primary user flow
1. Open frontend.
2. Enter a valid location (`New York`) and submit.
3. Confirm weather appears in <=5 seconds.
4. Toggle units and verify values/labels update.
5. Deny geolocation prompt and confirm manual search still works.

## 5. Validate non-happy paths
1. Submit invalid location and confirm user-visible error.
2. Simulate backend/provider failure and confirm no stale data is shown.
3. Submit >10 searches within 60 seconds and confirm rate-limit message:
   `Too many searches, try again in 1 minute`.

## 6. Run tests
```bash
npm run test:unit
npm run test:integration
npm run test:e2e
```

## 7. Performance and accessibility checks
```bash
npm run test:lighthouse
npm run test:a11y
```
- Page load target on 4G profile: <3 seconds
- Accessibility baseline: WCAG 2.1 AA issues triaged to zero blockers
