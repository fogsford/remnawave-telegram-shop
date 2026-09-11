# Technology Stack

## Language & Runtime

- Go 1.25.3
- Standard library with minimal external dependencies

## Core Dependencies

### Telegram Bot
- `github.com/go-telegram/bot` - Telegram Bot API client

### Database
- PostgreSQL 17
- `github.com/jackc/pgx/v4` - PostgreSQL driver and connection pooling
- `github.com/golang-migrate/migrate/v4` - Database migrations
- `github.com/Masterminds/squirrel` - SQL query builder

### External APIs
- `github.com/Jolymmiles/remnawave-api-go/v2` - Remnawave VPN panel client

### Utilities
- `github.com/joho/godotenv` - Environment variable management
- `github.com/robfig/cron/v3` - Scheduled tasks (invoice checking, notifications)
- `github.com/google/uuid` - UUID handling
- `golang.org/x/text` - Internationalization

## Infrastructure

- Docker & Docker Compose for containerization
- PostgreSQL database with health checks
- Optional ngrok for webhook tunneling (commented out in docker-compose)

## Build System

### Development Build
```bash
./build-dev.sh
```

### Release Build
```bash
./build-release.sh
```

### Docker Deployment
```bash
# Initial setup
docker compose up -d

# Update to latest version
docker compose pull
docker compose down && docker compose up -d
```

## Common Commands

### Running the Application
```bash
# Start with Docker Compose
docker compose up -d

# View logs
docker compose logs -f bot

# Stop services
docker compose down
```

### Database Migrations
Migrations run automatically on startup. Located in `db/migrations/` with up/down scripts.

### Testing
```bash
# Run specific test
go test ./internal/database/purchase_test.go

# Run all tests
go test ./...
```

## Configuration

All configuration is managed through environment variables (`.env` file). See `.env.sample` for required variables.

## API Endpoints

The bot exposes a web server on `HEALTH_CHECK_PORT`:
- `/healthcheck` - Health status endpoint (checks DB and Remnawave connectivity)
- `/${TRIBUTE_PAYMENT_URL}` - Webhook for Tribute payments
