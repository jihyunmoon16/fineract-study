# 05. Ledger And Accounting

## Why This Matters

A wallet that only stores current balance is fragile.
Financial systems usually need ledger thinking:

- transaction history
- debit and credit meaning
- reconciliation
- auditability

## Fineract References

- `fineract-accounting/src/main/java/org/apache/fineract/accounting/common/AccountingValidations.java`
- `fineract-accounting/src/main/java/org/apache/fineract/accounting/journalentry/`
- `fineract-accounting/src/main/java/org/apache/fineract/accounting/rule/`

## What To Look For

- debit and credit validation
- journal entry rules
- account mapping
- business events around accounting entries

## Questions For My Wallet

- What is my source of truth: balance table or ledger?
- How do I reconstruct balance from transactions?
- How do I reconcile internal ledger with external payment provider data?
- What is my audit trail for manual adjustments?

## Interview Answer Seed

`For a finance product, I prefer ledger-first thinking. Balance is important for fast reads, but ledger entries are what make the system auditable and recoverable. If the balance looks wrong, the ledger should let us explain and rebuild it.`
