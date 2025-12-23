# Project Structure

## Root Directory

- `cmd/app/main.go` - Application entry point, initialization, and server setup
- `go.mod` / `go.sum` - Go module dependencies
- `docker-compose.yaml` - Docker services configuration
- `Dockerfile` - Container image definition
- `.env.sample` - Environment variable template
- `build-dev.sh` / `build-release.sh` - Build scripts

## Internal Packages (`internal/`)

### Core Business Logic

- `handler/` - Telegram bot handlers and middleware
  - Command handlers (`start.go`, `connect.go`, `sync.go`)
  - Callback handlers (`payment_handlers.go`, `referral.go`, `trial.go`)
  - Middleware (`middleware.go`) - Auth, user filtering, customer creation
  - Callback types (`callback_type.go`) - Constants for callback data

- `payment/` - Payment processing orchestration
  - Coordinates between payment providers and VPN provisioning

- `notification/` - Subscription notifications
  - Expiration warnings and renewal reminders

- `sync/` - User synchronization with Remnawave panel

### Data Layer

- `database/` - PostgreSQL repositories and migrations
  - `persistance.go` - Migration runner
  - `customer.go` - Customer repository
  - `purchase.go` - Purchase repository
  - `referal.go` - Referral repository
  - Tests: `*_test.go`

### External Integrations

- `cryptopay/` - CryptoPay API client
  - `client.go` - API methods
  - `models.go` - Request/response types

- `yookasa/` - YooKassa API client
  - `client.go` - API methods
  - `models.go` - Request/response types

- `remnawave/` - Remnawave VPN panel client
  - `client.go` - API wrapper

- `tribute/` - Tribute payment service
  - `tribute.go` - Webhook handler
  - `models.go` - Payment models

- `moynalog/` - Moynalog analytics (optional)
  - `client.go` - API client
  - `models.go` - Analytics models

### Supporting Services

- `config/` - Configuration management
  - `config.go` - Environment variable parsing and validation

- `translation/` - Internationalization
  - `translation.go` - Translation manager (singleton pattern)

- `cache/` - In-memory caching
  - `cache.go` - Simple cache implementation

## Utilities (`utils/`)

- `text_sanitizer.go` - Text sanitization for Telegram HTML
- `utils.go` - General utility functions

## Database (`db/`)

- `migrations/` - SQL migration files
  - Numbered migrations with `.up.sql` and `.down.sql` pairs
  - Schema changes, table creation, and data migrations

## Translations (`translations/`)

- `en.json` - English translations
- `ru.json` - Russian translations

## Architecture Patterns

### Dependency Injection
Services are initialized in `main.go` and injected into handlers. No global state except configuration.

### Repository Pattern
Database access is abstracted through repository interfaces (`CustomerRepository`, `PurchaseRepository`, `ReferralRepository`).

### Middleware Chain
Telegram handlers use middleware for cross-cutting concerns (auth, user creation, filtering).

### Cron Jobs
Background tasks run on schedules:
- Invoice checking (every 5 seconds for pending payments)
- Subscription notifications (daily at 16:00 UTC)

### Client Wrappers
External APIs are wrapped in client structs with methods for each endpoint.
