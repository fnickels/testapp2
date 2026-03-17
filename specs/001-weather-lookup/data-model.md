# Data Model: Modern Weather Lookup Website

## Entity: Location
- Description: User-specified geographic target used to request current weather.
- Fields:
  - `query` (string, required): raw user input (city, postal code, or `lat,long`).
  - `normalizedName` (string, required): canonical display name (e.g., `New York, US`).
  - `latitude` (number, required): range -90 to 90.
  - `longitude` (number, required): range -180 to 180.
  - `countryCode` (string, optional): ISO-3166 alpha-2.
  - `timezone` (string, required): IANA timezone.
- Validation Rules:
  - `query` trimmed and non-empty.
  - Coordinate query must parse as two decimals within valid ranges.

## Entity: CurrentWeather
- Description: Current atmospheric snapshot for a location.
- Fields:
  - `locationId` (string, required): deterministic key from normalized location.
  - `observedAt` (string, required): ISO 8601 timestamp from provider.
  - `temperature` (number, required).
  - `feelsLike` (number, required).
  - `humidityPercent` (integer, required, 0-100).
  - `windSpeed` (number, required).
  - `uvIndex` (number, required, >=0).
  - `conditionCode` (string, required): provider-independent weather code.
  - `conditionLabel` (string, required): user-friendly weather summary.
  - `units` (enum, required): `metric` or `imperial`.
- Validation Rules:
  - All numeric values must be finite numbers.
  - `observedAt` cannot be in future beyond 5 minutes tolerance.

## Entity: UserPreference
- Description: Session/user settings affecting weather rendering.
- Fields:
  - `unitsPreference` (enum, required): `metric`, `imperial`, or `locale-default`.
  - `locale` (string, required): browser locale used for defaults.
  - `autoRefreshIntervalMs` (integer, required): fixed at `600000`.
- Validation Rules:
  - Units preference must map to supported enum values.

## Entity: Favorite
- Description: Saved location entry persisted in local storage.
- Fields:
  - `id` (string, required): stable unique identifier.
  - `location` (Location, required).
  - `savedAt` (string, required): ISO 8601 timestamp.
- Validation Rules:
  - No duplicate `normalizedName` entries per user storage.

## Entity: SearchRateWindow
- Description: Session-level rate limiting state.
- Fields:
  - `sessionId` (string, required).
  - `windowStart` (string, required): ISO 8601 timestamp.
  - `requestCount` (integer, required).
- Validation Rules:
  - `requestCount` <= 10 inside any rolling 60-second window.

## Relationships
- One `Location` can map to many `CurrentWeather` observations over time.
- One user/session can have many `Favorite` entries.
- One user/session has exactly one active `UserPreference` record.
- One `SearchRateWindow` belongs to one session.

## State Transitions
- Search flow:
  1. `Idle` -> `Searching` (valid query submitted)
  2. `Searching` -> `ShowingWeather` (provider/API success)
  3. `Searching` -> `Error` (validation/provider/network failure)
- Refresh flow:
  1. `ShowingWeather` -> `Refreshing` (10-minute timer)
  2. `Refreshing` -> `ShowingWeather` (success)
  3. `Refreshing` -> `Error` (failure, no stale fallback)
- Rate-limit flow:
  1. `Searching` -> `RateLimited` (11th request in 60s)
  2. `RateLimited` -> `Idle` (cooldown expires)
