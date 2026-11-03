# Existing HTML → secure API migration map

The original file `Nexa_BillSuite_FINAL(4).html` (~396 KB, 1,448 text lines) must be maintained as a **private local reference**. Do not directly publish it.

| Legacy functionality | Existing implementation | Recommended Cloud operation | Status in foundation |
|---|---|---|---|
| Login | `db.users` and `sessionStorage`, passwords compared in browser | `/api/login`; `account`, `app_session`, salted hash | Backend + simple UI implemented |
| New / edit account | Array `db.users`, plaintext password | `/api/accounts`, hashed passwords, backend roles | Backend + create UI; edit API only |
| Products | `trx(d => d.products...)` | `/api/products`; DB constraints | Basic API/UI |
| Customers | Browser `db.customers` | `/api/customers` | Basic create/read |
| Suppliers | Browser `db.suppliers` | `/api/suppliers` | Basic create/read |
| POS | `cartCalc`, `saveSale`, mutable stock + local seq | `/api/sales`: atomic line, invoice and stock postings | Basic posting only |
| Purchase/GRN | `createPurchase`, cost lot changes | `/api/purchases`, lock stock, supplier payments | Basic weighted-average only |
| Customer payments | `receivePayment` | `/api/sales/:id/payments` | Implemented |
| Returns | `db.returns`, cost lots | `/api/returns` + refund/credit-note ledger | Only basic stock-return API; accounting incomplete |
| Quotations, proforma, custom | Multiple `db` arrays | `/api/documents` | Basic create/read API |
| Expenses | `db.expenses` | `/api/expenses` | Implemented |
| Stock adjustments | `createStockAdjust` | `/api/stock/adjust` | Implemented |
| Cashier shifts | `db.shifts` | `cash_shift` and shift-specific APIs | SQL table only |
| Browser drafts | `nexaSaveDraft`, `localStorage` | `/api/draft` and `account_draft` | API only |
| Settings | `db.settings` | `/api/settings` | API only |
| Cost lots | `db.costLots` | Cost lot + issue allocation schema and RPC | Not implemented |
| Product discounts | `db.productDiscounts` | Discount policies + controlled price approvals | Not implemented |
| Barcode studio, queue | `db.printQueue`, HTML print | Server print jobs + device print pipeline | Not implemented |
| Excel bulk upload | Browser reads XLSX and overwrites arrays | Validated staging/import and transactional batch API | Not implemented |
| Reports | Computed from browser arrays | SQL views/materialized read APIs | Basic dashboard only |
| Backups and restore | Browser JSON file import/export | Server-side encrypted backups + verify restore | Not implemented |

**Why not directly replace `save()` with `fetch()`?** Legacy `trx()` mutates a complete client-side object synchronously. A mere asynchronous mirror could acknowledge/print an invoice before the database commits; concurrent clients could overwrite each other. Each business action should become a validated server-side transaction and UI success should happen only after HTTP 2xx.

**Original file findings:** `localStorage` holds `hardwareflow_studio_v2_local_db`; separate `nexa_billSuite_v4_shift_drafts` uses `localStorage`; `sessionStorage` retains current user ID; code contains static demo credentials and recovery values; account and cost approval are compared in JavaScript. These need complete removal when porting the actual UI.
