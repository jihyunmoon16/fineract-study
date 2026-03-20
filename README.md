# Fineract Source Study

A structured reading of the [Apache Fineract](https://github.com/apache/fineract) codebase,
focused on understanding how a production financial backend handles reliability, consistency,
and correctness — and applying those patterns to my own [digital-wallet](../digital-wallet) project.

---

## Structure

- `ROADMAP.md` — week-by-week reading plan and progress tracker
- `topics/` — notes on specific patterns and concepts, connected to wallet implementation
- `templates/` — reusable templates for daily reading sessions

---

## How To Use

1. Start from `ROADMAP.md`.
2. Each day, read the target Fineract code and update one topic file.
3. At the end of the session, create a short daily note from `templates/daily-note-template.md`.
4. For each topic, always capture:
    - What the code does
    - Why it matters for a financial system
    - How the same idea applies to the wallet project
    - What breaks if this pattern is missing

---

## Study Rules

- Prefer short focused notes over long summaries.
- Tie every concept to one concrete wallet example.
- If a topic feels abstract, add a failure scenario: *"What goes wrong if this is missing?"*

---

## Reading Order

1. `topics/01-idempotency.md`
2. `topics/02-transaction-and-batch.md`
3. `topics/03-concurrency-and-locking.md`

---

*Based on reading the [Apache Fineract](https://github.com/apache/fineract) open-source codebase.*
