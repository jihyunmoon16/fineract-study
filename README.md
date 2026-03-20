# Interview Study Notes

This folder is a standalone study repository scaffold for:

- organizing what to study from Apache Fineract
- mapping those ideas to `digital-wallet`
- building interview-ready answers from actual code reading

## Structure

- `ROADMAP.md`
  1-week study plan and progress tracker
- `topics/`
  topic-by-topic notes connected to wallet/backend interview prep
- `templates/`
  reusable note templates for daily study

## How To Use

1. Start from `ROADMAP.md`.
2. Each day, read the target code and update one topic file.
3. At the end of the day, create one short daily note from `templates/daily-note-template.md`.
4. Always write down:
   - what the code does
   - why it matters for a wallet system
   - how you would explain it in an interview

## Publish As Separate Repo

This folder is designed to work as an independent git repository.

Typical flow:

1. `cd study-notes`
2. `git init -b main`
3. `git add .`
4. `git commit -m "Initial study notes"`
5. `git remote add origin <your-repo-url>`
6. `git push -u origin main`

## Study Rules

- Prefer short notes over long summaries.
- Turn every concept into one concrete wallet example.
- Turn every code reading session into one interview answer.
- If a topic feels vague, add one failure scenario:
  `What goes wrong if this is missing?`

## First Target

Start with:

1. `topics/01-idempotency.md`
2. `topics/02-transaction-and-batch.md`
3. `topics/03-concurrency-and-locking.md`
