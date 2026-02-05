# order-service

> AI Agent context for understanding this repository

## 📋 Overview

Order processing microservice. Handles order creation, tracking, and aggregated order details with shipment info.

## 🏗️ Architecture

```
order-service/
├── cmd/main.go
├── config/config.go
├── db/migrations/sql/
├── internal/
│   ├── core/
│   │   ├── database.go
│   │   └── domain/
│   ├── logic/v1/service.go
│   └── web/v1/handler.go
├── middleware/
└── Dockerfile
```

## 🔌 API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/api/v1/orders` | List user orders |
| `GET` | `/api/v1/orders/:id` | Get order by ID |
| `GET` | `/api/v1/orders/:id/details` | **Aggregated** order + shipment |
| `POST` | `/api/v1/orders` | Create new order |

## 📐 3-Layer Architecture

| Layer | Location | Responsibility |
|-------|----------|----------------|
| **Web** | `internal/web/v1/handler.go` | HTTP, validation, **aggregation** |
| **Logic** | `internal/logic/v1/service.go` | Business rules (❌ NO SQL) |
| **Core** | `internal/core/` | Domain models, repositories |

**Aggregation:** `/orders/:id/details` combines order + shipment (HTTP call to shipping-service).

## 🗄️ Database

| Component | Value |
|-----------|-------|
| **Cluster** | transaction-db (shared with cart-service) |
| **PostgreSQL** | 18 |
| **HA** | 3 instances (1 primary + 2 replicas) |
| **Pooler** | PgCat HA (2 replicas) |
| **Endpoint** | `pgcat.cart.svc.cluster.local:5432` |
| **Database Name** | `order` (separate from `cart` database) |
| **Replication** | **Synchronous** (zero data loss) |

**Shared Cluster:** Same PostgreSQL cluster as cart-service, different database.

## 🚀 Graceful Shutdown

**VictoriaMetrics Pattern:**
1. `/ready` → 503 when shutting down
2. Drain delay (5s)
3. Sequential: HTTP → Database → Tracer

## 🔧 Tech Stack

| Component | Technology |
|-----------|------------|
| **Framework** | Gin |
| **Database** | PostgreSQL 18 via pgx/v5 |
| **Tracing** | OpenTelemetry |

## 🛠️ Development

```bash
go mod download && go test ./... && go build ./cmd/main.go
```
