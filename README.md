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

```bash
go mod download
go test ./...
go run cmd/main.go
```

## License

MIT
