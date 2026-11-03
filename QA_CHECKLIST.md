# QA / Go-live checklist

## Initial database and authentication
- [ ] SQL Editor schema completes without error.
- [ ] Direct browser requests with the publishable key cannot read `public.account` or `public.product`.
- [ ] `account` table contains only the seeded admin; password_hash differs from plaintext.
- [ ] Login requires username and password; incorrect passwords cannot authenticate.
- [ ] Initial account must change password before business APIs respond.
- [ ] HTTP-only, Secure, SameSite cookie is set over HTTPS.
- [ ] Account creation, reset, disable and permission changes are verified server-side.
- [ ] Admin/secret/database passwords do not occur in public GitHub source.

## Stock and billing transactions
- [ ] Same product sale from device A reduces visible stock on device B after refresh.
- [ ] Concurrent sales for the final item result in exactly one successful posting.
- [ ] Failed sale produces no sale lines, no payment, no stock change and no invoice number committed.
- [ ] Replaying an idempotency key does not post a second invoice.
- [ ] Unpaid walk-in invoice is rejected; valid customer credit limit is enforced.
- [ ] Non-admin below-cost sale is blocked by backend, including bill-level discounts.
- [ ] GRN stock + free quantities, payment and weighted cost reconcile.
- [ ] Duplicate product in GRN is rejected or merged correctly.
- [ ] Return quantity never exceeds sold quantity; actual refund/credit-note accounting implemented before go-live.

## Operational controls
- [ ] Full original POS/print/barcode functionality is migrated and regression-tested.
- [ ] All report totals reconcile with SQL.
- [ ] Account permissions, show-cost restrictions, shift handling, bulk import, tax rules tested.
- [ ] Desktop thermal receipt, barcode labels and A4 invoices print with correct measurements.
- [ ] Backup made, restored on separate test database, and checksum/reconciliation verified.
- [ ] Browser shows no business data in localStorage, sessionStorage, IndexedDB or browser cache after logout.
- [ ] Security review, monitoring, production hosting, database PITR and alerting configured.
