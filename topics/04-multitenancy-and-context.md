# 04. Multitenancy And Context Propagation

## Why This Matters

If one backend serves multiple tenants, every request must be isolated by tenant context.
Async execution adds risk because thread-local state may not move automatically.

## Fineract References

- `fineract-security/src/main/java/org/apache/fineract/infrastructure/security/filter/TenantAwareAuthenticationFilter.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/loan/ContextAwareTaskDecorator.java`
- `fineract-provider/src/main/java/org/apache/fineract/cob/service/AsyncLoanCOBExecutorServiceImpl.java`

## What To Look For

- where tenant is resolved
- where tenant is stored during request processing
- how context is copied for async execution
- where context is reset

## Questions For My Wallet

- Will my wallet ever be multi-merchant or multi-tenant?
- Do my cache keys include tenant information?
- What happens if async processing uses the wrong tenant context?
- Should I use ThreadLocal, explicit context passing, or both?

## Interview Answer Seed

`In a multitenant backend, tenant identity is not just an auth concern. It affects database reads, cache keys, job execution, and event handling. In async code, I would explicitly propagate context instead of assuming thread-local state survives thread switches.`
