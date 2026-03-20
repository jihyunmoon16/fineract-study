# 1-Week Roadmap

## Goal

By the end of this week, be able to explain and apply:

- TDD
- idempotency
- transaction boundaries
- concurrency and locking
- multitenancy and context propagation
- ledger/accounting basics
- messaging and event-driven design

## Day 1

- [ ] Read root `README.md`
- [ ] Identify the roles of `fineract-provider`, `fineract-core`, `fineract-command`, `fineract-accounting`, `fineract-security`
- [ ] Run one small unit test
- [ ] Write a 10-line summary of what Fineract is

Code to read:

- `README.md`
- `settings.gradle`

Output:

- [ ] `What is Fineract?`
- [ ] `How is this relevant to my digital-wallet?`

## Day 2

- [ ] Review TDD flow with one small utility test
- [ ] Add one tiny test-first change
- [ ] Write down red-green-refactor in your own words

Code to read:

- `fineract-core/src/main/java/org/apache/fineract/infrastructure/security/utils/SQLBuilder.java`
- `fineract-core/src/test/java/org/apache/fineract/infrastructure/security/utils/SQLBuilderTest.java`

Output:

- [ ] `I can explain TDD in 1 minute`
- [ ] `I completed one test-first change`

## Day 3

- [ ] Study command handling
- [ ] Study idempotency persistence
- [ ] Design how wallet transfers should prevent duplicate processing

Code to read:

- `fineract-command/README.md`
- `fineract-command/src/main/java/org/apache/fineract/command/persistence/domain/CommandEntity.java`
- `fineract-command/src/main/java/org/apache/fineract/command/persistence/domain/CommandRepository.java`

Output:

- [ ] `How to design idempotent transfer requests`
- [ ] `What response should repeated requests return?`

## Day 4

- [ ] Study batch execution and enclosing transactions
- [ ] Understand rollback behavior
- [ ] Compare one-big-transaction vs step-by-step execution

Code to read:

- `fineract-core/src/main/java/org/apache/fineract/batch/api/BatchApiResource.java`
- `fineract-core/src/main/java/org/apache/fineract/batch/service/BatchApiServiceImpl.java`

Output:

- [ ] `I can explain transaction boundary choices`
- [ ] `I can explain retry + rollback tradeoffs`

## Day 5

- [ ] Study locking and concurrent processing
- [ ] Write one race condition example for a wallet
- [ ] Compare lock table vs optimistic locking

Code to read:

- `fineract-provider/src/main/java/org/apache/fineract/cob/service/InlineLoanCOBExecutorServiceImpl.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/loan/LoanLockingServiceImpl.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/domain/LoanAccountLock.java`

Output:

- [ ] `How to prevent double spending`
- [ ] `When to use lock table`

## Day 6

- [ ] Study tenant handling and thread-local context
- [ ] Study async context propagation
- [ ] Note what breaks when async code loses tenant/auth context

Code to read:

- `fineract-security/src/main/java/org/apache/fineract/infrastructure/security/filter/TenantAwareAuthenticationFilter.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/loan/ContextAwareTaskDecorator.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/service/AsyncLoanCOBExecutorServiceImpl.java`

Output:

- [ ] `I can explain multitenancy in a backend system`
- [ ] `I can explain why ThreadLocal is dangerous in async code`

## Day 7

- [ ] Study accounting and journal concepts
- [ ] Review messaging and external events
- [ ] Prepare interview answers for production incidents

Code to read:

- `fineract-accounting/src/main/java/org/apache/fineract/accounting/common/AccountingValidations.java`
- `fineract-accounting/src/main/java/org/apache/fineract/accounting/journalentry/`
- `README.md` section on Kafka and ActiveMQ

Output:

- [ ] `Why balance alone is not enough`
- [ ] `Why ledger and journal entries matter`
- [ ] `How I would handle a duplicate event in production`

## End Of Week Check

- [ ] I can explain idempotency using my wallet example
- [ ] I can explain transaction boundaries using a transfer example
- [ ] I can explain locking using concurrent withdrawal example
- [ ] I can explain multitenancy and async context propagation
- [ ] I can explain why a finance backend should have ledger thinking
