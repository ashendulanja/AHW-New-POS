# API summary (Phase 1)

All `/api/*` calls except `/api/login` and `/api/health` require an authenticated, HTTP-only session cookie. Mutating methods must include `X-Nexa-Request: 1` and be same-origin. JSON is used for request/response bodies. Backend validates permissions and business constraints.

| Method | Endpoint | Use / authorization |
|---|---|---|
| POST | `/api/login` | Authenticate using `account` table |
| GET | `/api/me` | Current account, no password hash |
| POST | `/api/change-password` | Initial password change (12+ characters) |
| POST | `/api/logout` | Revoke session |
| GET/POST/PATCH | `/api/accounts`, `/api/accounts/:id` | Admin manages accounts |
| GET/POST/PATCH | `/api/products`, `/api/products/:id` | Products (cost hidden from unauthorized users) |
| GET/POST | `/api/customers` | Customer list/create |
| GET/POST | `/api/suppliers` | Supplier list/create |
| GET/POST | `/api/sales` | Invoice list/create, transactional stock deduction |
| GET | `/api/sales/:id` | Invoice detail and lines |
| POST | `/api/sales/:id/payments` | Record credit collections |
| GET/POST | `/api/purchases` | GRN and stock receipt |
| POST | `/api/stock/adjust` | Stock correction with reason |
| GET | `/api/stock/movements` | Stock ledger |
| POST | `/api/returns` | Basic stock return entry; **refund accounting incomplete** |
| GET/POST | `/api/expenses` | Expense records |
| GET/POST | `/api/documents` | Non-posting quotation/proforma/custom records |
| GET/PUT | `/api/settings` | General settings (admin updates) |
| GET/PUT | `/api/draft` | Per-account drafts saved in PostgreSQL |
| GET | `/api/dashboard` | Basic KPIs |
| GET | `/api/changes` | Database change timestamp |
| GET | `/api/health` | Database connectivity |

## Example: POST /api/sales

```json
{
  "idempotencyKey": "3f2eb3ee-4c47-43ea-9bce-713f8a09ae27",
  "customerId": null,
  "kind": "retail",
  "items": [{"productId": "UUID_OF_PRODUCT", "qty": 2, "price": 1250, "discount": 0}],
  "discount": 0,
  "paid": 2500,
  "method": "cash",
  "notes": ""
}
```

The server does **not** trust the product's displayed stock/cost from the browser. It reads current stock from SQL under row locks. The invoice, stock deduction, stock ledger and payment record commit atomically.
