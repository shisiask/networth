# Personal Finance Management System

The Personal Finance Management System is a comprehensive SQL-based platform designed to help college students manage and understand their financial lives. It centralizes bank accounts, transactions, budgets, investments, bills, loans, credit cards, credit score history, and more in a single relational database with a clean, scalable architecture.

**Features**
- **Scope:** Centralized storage for accounts, transactions, budgets, goals, investments, bills, loans, and credit records.
- **Tracking:** Multi-account balances, transaction history, recurring payments, and digital receipt uploads.
- **Budgeting:** Monthly budgets, spend categories, savings goals, and progress evaluation.
- **Credit:** Credit card usage tracking, minimum payment reminders, and historical credit score log.
- **Security & Auditing:** Session tracking, login attempts log, and activity audit trails.
- **Notifications:** Alerts for bill due dates, low balances, and goal milestones.

**Why this project**
- Provides a realistic database foundation for a full personal-finance application.
- Teaches relational modeling, data integrity, secure activity logging, and analytical queries.
- Targets college students to improve financial literacy and credit-building habits.

**Schema & Architecture (Overview)**
- **Core entities:** `users`, `accounts`, `transactions`, `categories`, `budgets`, `budget_items`, `savings_goals`, `investments`, `positions`, `bills`, `recurring_payments`, `loans`, `loan_payments`, `credit_cards`, `credit_card_transactions`, `credit_scores`.
- **Security & audit:** `sessions`, `login_attempts`, `audit_logs`, `file_uploads` (for receipts).
- **Relationships:** Users have many accounts; accounts have many transactions; budgets link to categories; investments contain positions and trades; credit score history is time-series per user.
- **Design goals:** Normalized schema to avoid duplication, FK constraints for integrity, indexed fields for efficient reporting, and temporal tables where historical tracking is needed (e.g., `credit_scores`).

**Getting Started (development)**
1. Install a SQL server: e.g., `PostgreSQL 14+`.
2. Create a database and user for development:

```bash
createdb networth_dev
psql -d networth_dev -c "CREATE USER networth_user WITH PASSWORD 'change_me';"
psql -d networth_dev -c "GRANT ALL PRIVILEGES ON DATABASE networth_dev TO networth_user;"
```

3. Run migrations or apply the schema SQL files (placeholders in `sql/schema/`):

```bash
psql -d networth_dev -f sql/schema/00_tables.sql
psql -d networth_dev -f sql/schema/01_constraints.sql
psql -d networth_dev -f sql/schema/02_indexes.sql
```

4. (Optional) Seed with sample data for testing: `sql/seeds/sample_seed.sql`.

**Example Queries**
- Get current balances across a user’s accounts:
	- `SELECT a.id, a.name, SUM(t.amount) AS balance FROM accounts a JOIN transactions t ON t.account_id = a.id WHERE a.user_id = $1 GROUP BY a.id, a.name;`
- Monthly spending by category:
	- `SELECT c.name, SUM(t.amount) FROM transactions t JOIN categories c ON t.category_id = c.id WHERE t.user_id = $1 AND date_trunc('month', t.posted_at) = date_trunc('month', now()) GROUP BY c.name;`

**Testing & Validation**
- Write SQL unit/integration tests using tools like `pgTAP` or run automated checks in your CI pipeline that execute sample reports and constraints.

**Extending the Project**
- Add an API layer (REST or GraphQL) to expose read/write operations.
- Build a frontend for dashboards, visualizations, and interactive budgeting tools.
- Integrate with external bank APIs (Plaid, OpenBanking) for automated transaction import.
- Add scheduled jobs for recurring payments and alert delivery.

**Contributing**
- Use feature branches and open pull requests against `main`.
- Document schema changes in `sql/migrations/` and include accompanying tests or data migration scripts.

**Next steps**
- Provide schema SQL files in `sql/schema/` and a small sample dataset in `sql/seeds/`.
- Add a migration tool (e.g., `Flyway`, `Sqitch`, or `node-pg-migrate`) and CI checks to run schema validation.

If you want, I can scaffold `sql/schema/` and `sql/seeds/` files, add a migration tool configuration, or create a small API starter — tell me which to do next.
