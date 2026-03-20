# 03. Concurrency And Locking

## Why This Matters

Wallet systems face concurrent writes on the same account or balance.
Without coordination, you can get lost updates or double spending.

## Fineract References

- `fineract-provider/src/main/java/org/apache/fineract/cob/service/InlineLoanCOBExecutorServiceImpl.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/loan/LoanLockingServiceImpl.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/domain/LoanAccountLock.java`

## What To Look For

- explicit lock records
- separate lock transaction
- save and flush timing
- failure behavior when lock cannot be acquired

## Questions For My Wallet

- How do I protect a wallet from concurrent withdrawals?
- Do I use optimistic locking, pessimistic locking, or a lock table?
- What should the API return when a lock conflict happens?
- How do I avoid deadlocks?

## Interview Answer Seed

`For concurrent withdrawals on the same wallet, I would protect the balance update with a locking strategy. The exact choice depends on write frequency, but the goal is the same: only one update path should commit for the same balance state.`

## Failure Scenario

If locking is missing:

- two withdrawal requests read the same balance
- both believe funds are sufficient
- both commit
- final balance is wrong
