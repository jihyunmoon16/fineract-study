# 02. Transaction And Batch

## Why This Matters

A wallet transfer usually changes multiple pieces of data:

- debit side
- credit side
- transaction history
- event/outbox record

These updates need a clear transaction boundary.

## Fineract References

- `fineract-core/src/main/java/org/apache/fineract/batch/api/BatchApiResource.java`
- `fineract-core/src/main/java/org/apache/fineract/batch/service/BatchApiServiceImpl.java`

## What To Look For

- enclosing transaction behavior
- rollback behavior
- retry behavior
- isolation level

## Questions For My Wallet

- Which operations must succeed or fail together?
- Should transfer posting and notification be in one transaction?
- Which part can be retried safely?
- Which failures require compensation instead of rollback?

## Interview Answer Seed

`For financial operations, I define the transaction boundary around state changes that must stay consistent together, such as balance update and ledger record creation. Side effects like notifications should be pushed outside that boundary through events.`

## Failure Scenario

If transaction boundaries are weak:

- debit succeeds
- credit fails
- system enters inconsistent state
