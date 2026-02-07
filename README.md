# order-service

Order processing microservice for creating and tracking orders.

## Features

- Order creation
- Order status tracking
- Aggregated order details (with shipment)
- Order history

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/api/v1/orders` | List user orders |
| `GET` | `/api/v1/orders/:id` | Get order by ID |
| `GET` | `/api/v1/orders/:id/details` | Aggregated with shipment |
| `POST` | `/api/v1/orders` | Create new order |

## Tech Stack

- Go + Gin framework
- PostgreSQL 18 (transaction-db cluster, shared with cart)
- PgCat connection pooling
- OpenTelemetry tracing

## Development

### Prerequisites

- Go 1.25+
- [golangci-lint](https://golangci-lint.run/welcome/install/) v2+

### Local Development

```bash
# Install dependencies
go mod tidy
go mod download

# Build
go build ./...

# Test
go test ./...

# Lint (must pass before PR merge)
golangci-lint run --timeout=10m

# Run locally (requires .env or env vars)
go run cmd/main.go
```

### Pre-push Checklist

```bash
go build ./... && go test ./... && golangci-lint run --timeout=10m
```

## License

MIT
