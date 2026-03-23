# Fineract Source Study — Digital Wallet Upgrade

> Reading through the [Apache Fineract](https://github.com/apache/fineract) source code to understand
> how a production-grade financial backend is structured, and applying the patterns to my own
> digital-wallet project.

---

## What I Want to Understand by the End of the Week

- TDD practices in a real financial codebase
- Idempotency design for payment requests
- Transaction boundary decisions and rollback strategy
- Concurrency control and locking approaches
- Reliable event publishing with the Outbox pattern
- Ledger and double-entry accounting fundamentals

---

## Day 1 — Project Structure

**Goal**: Understand how Fineract is organized before reading any implementation code.

Tasks:
- Read `README.md` and `settings.gradle`
- Identify the roles of `fineract-provider`, `fineract-core`, `fineract-command`, `fineract-accounting`, `fineract-security`
- Run one unit test to confirm the build works

Code to read:
- `README.md`
- `settings.gradle`

Notes to write:
- What is Fineract, and what problem does it solve?
  Apache Fineract is an open-source core banking platform built with Java and Spring Boot.
  It provides backend infrastructure for managing savings accounts, loans, and transactions.
  It is designed around reliability patterns such as idempotent command handling,
  double-entry accounting, and transactional batch processing.
- How does its module structure relate to my digital-wallet project?
  My digital-wallet shares the same core concerns: account management, fund transfers,
  and balance consistency. The fineract-savings module is most similar to my wallet,
  and fineract-command handles the idempotency patterns I want to apply.
---

## Day 2 — TDD in Practice

**Goal**: See how Fineract approaches test-first development, then apply it in my own project.

Tasks:
- Trace the Red-Green-Refactor cycle in `SQLBuilderTest`
- Add one test-first change to my wallet project

Code to read:
- `fineract-core/src/main/java/org/apache/fineract/infrastructure/security/utils/SQLBuilder.java`
- `fineract-core/src/test/java/org/apache/fineract/infrastructure/security/utils/SQLBuilderTest.java`

Notes to write:
- My understanding of Red-Green-Refactor after seeing it in a real codebase
  Red-Green-Refactor means writing the test first, watching it fail (Red), then implementing the code to make it pass (Green), then cleaning up (Refactor).

Wallet project output:
- Commit one test written before the implementation

---

## Day 3 — Idempotency

**Goal**: Understand how Fineract prevents duplicate command execution, then apply the same thinking to wallet transfers.

Tasks:
- Study `CommandEntity` — how is the idempotency key stored and checked?
- Design how the wallet should handle a duplicate transfer request

Code to read:
- `fineract-command/README.md`
- `fineract-command/src/main/java/org/apache/fineract/command/persistence/domain/CommandEntity.java`
- `fineract-command/src/main/java/org/apache/fineract/command/persistence/domain/CommandRepository.java`

Notes to write:
- How to design idempotent transfer requests?
  On every incoming request, check whether the idempotency key already exists in the store. If it does, the request is a duplicate and should not be processed again.
- What should a duplicate request return, and why?
  It should return the same response as the first successful request. This ensures the client gets a consistent result without triggering side effects like double charging or double transfer.
- Redis vs DB tradeoff?
  DB storage is persistent and supports audit trails, making it suitable for regulated financial systems. Redis is lightweight and faster for lookups, but data is lost after TTL expires — which makes it unsuitable as the sole store for financial audit history.

Wallet project output:
- Commit `IdempotencyRecord.java` — modelled after `CommandEntity`
  ```java
  // Fields: idempotency_key (unique), status, response_body, created_at
  ```

---

## Day 4 — Transaction Boundaries

**Goal**: Learn how Fineract decides where to draw transaction boundaries in batch operations.

Tasks:
- Study `BatchApiServiceImpl` — one big transaction vs step-by-step
- Review and annotate the transaction boundaries in my own `TransferService`

Code to read:
- `fineract-core/src/main/java/org/apache/fineract/batch/api/BatchApiResource.java`
- `fineract-core/src/main/java/org/apache/fineract/batch/service/BatchApiServiceImpl.java`

Notes to write:
- How to decide where a transaction boundary should be
- Retry and rollback tradeoffs

Wallet project output:
- Commit annotated `TransferService.java` with comments explaining boundary decisions
  ```java
  // Are debit and credit inside the same transaction?
  // Does event publishing happen after the commit?
  ```

---

## Day 5 — Concurrency and Locking

**Goal**: Understand Fineract's lock table pattern for concurrent loan processing, compare with optimistic locking.

Tasks:
- Study `LoanLockingServiceImpl` and `LoanAccountLock`
- Write a concrete concurrent withdrawal scenario for the wallet
- Decide between lock table and `@Version` optimistic locking

Code to read:
- `fineract-provider/src/main/java/org/apache/fineract/cob/service/InlineLoanCOBExecutorServiceImpl.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/loan/LoanLockingServiceImpl.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/domain/LoanAccountLock.java`

Notes to write:
- How to prevent double spending in a wallet
- When to use a lock table vs optimistic locking

Wallet project output:
- Commit locking strategy applied to `WalletAccount.java`
  ```java
  // Option A: @Version (optimistic)
  // Option B: @Lock(PESSIMISTIC_WRITE) on repository method
  ```

---

## Day 6 — Reliable Event Publishing (Outbox Pattern)

**Goal**: Understand how Fineract's Reliable Event Framework guarantees that events are not lost when a transaction rolls back.

Tasks:
- Trace how events are persisted to DB before being sent to Kafka
- Compare with `@TransactionalEventListener(phase = AFTER_COMMIT)`
- Apply the Outbox pattern to the wallet's transfer flow

Code to read:
- `README.md` — Kafka / ActiveMQ / Reliable Event Framework section
- `fineract-provider` — `ExternalEventService` related classes

Notes to write:
- What is the Outbox pattern and what problem does it solve?
- How does it differ from `@TransactionalEventListener`?

Wallet project output:
- Commit `OutboxEvent.java` + `OutboxEventRepository.java`
  ```java
  // Fields: aggregate_type, aggregate_id, event_type, payload, status, created_at
  ```

---

## Day 7 — Ledger and Journal Entries

**Goal**: Understand why a financial system needs a ledger, not just a balance column.

Tasks:
- Study `AccountingValidations` and the `journalentry` package
- Design how a single wallet transfer maps to journal entries

Code to read:
- `fineract-accounting/src/main/java/org/apache/fineract/accounting/common/AccountingValidations.java`
- `fineract-accounting/src/main/java/org/apache/fineract/accounting/journalentry/`

Notes to write:
- Why balance alone is not enough for a financial system
- Why double-entry bookkeeping matters

Wallet project output:
- Commit `LedgerEntry.java`
  ```java
  // One transfer = two rows
  // DEBIT  | sender account   | -10,000
  // CREDIT | receiver account | +10,000
  ```

---

## Day 8 (Weekend) — Messaging and Event Handling

**Goal**: Round out the event-driven design picture with duplicate event handling.

Tasks:
- Study strategies for handling duplicate Kafka events
- Summarize how idempotency, the Outbox pattern, and event deduplication work together

Notes to write:
- End-to-end flow: transfer request → DB commit → Outbox → Kafka → consumer
- How to handle a duplicate event safely at the consumer side

---

## Wallet Project Commits This Week

```
feat: add idempotency record entity          # Day 3
feat: annotate transaction boundaries        # Day 4
feat: add optimistic locking to account      # Day 5
feat: add outbox event entity                # Day 6
feat: add ledger entry entity                # Day 7
```

---

*Study notes based on reading the [Apache Fineract](https://github.com/apache/fineract) open-source codebase.*
