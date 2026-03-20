# 06. Messaging And Events

## Why This Matters

Not every follow-up action belongs in the request-response path.
Messaging helps move slow or downstream work out of the hot path.

## Fineract References

- `README.md` section on Kafka and ActiveMQ
- `docker-compose-postgresql-kafka.yml`
- `docker-compose-postgresql-activemq.yml`

## What To Look For

- what events are emitted
- which jobs are distributed
- how workers are triggered
- what happens if delivery is duplicated

## Questions For My Wallet

- Which flows should stay synchronous?
- Which flows should emit an event after commit?
- How will consumers handle duplicate delivery?
- Do I need an outbox pattern?

## Interview Answer Seed

`I would keep the money movement decision synchronous, but move secondary actions such as notification, settlement sync, analytics, or webhook delivery to async consumers. Since brokers are usually at-least-once, consumers should also be idempotent.`
