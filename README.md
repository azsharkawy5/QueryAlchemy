## 🧪 QueryAlchemy: The SQL Core Concepts 🧪

> **“A database is not just a storage engine. It’s the _mind_ of your application.”**  
> This project is my way of mastering that mind — and helping others do the same.

This project documents a focused journey: the transformation of an ORM-reliant developer into a high-impact database engineer. It's not just a collection of scripts; it's a structured curriculum built around hands-on projects, designed to forge deep, first-principles knowledge of relational databases.

Built entirely in the open, this initiative serves both as a personal learning journal and a practical resource for others. It's a space to revisit and reinforce essential SQL and data modeling concepts, while also exploring new techniques and insights along the way.

## 🎯 Who is this Repository For?

Whether you're a developer tired of abstracted ORM layers or a student seeking real-world SQL fluency, **QueryAlchemy** is designed to help you master the fundamentals that power serious backend and data systems.

It’s especially useful for:

- **Backend & Full-Stack Developers (Python, Django, Rails, Node.js, etc.):**  
  If the ORM feels like a "black box" and you want to write performant queries, design robust schemas, and debug with confidence, this is for you.

- **Aspiring Data Engineers & Analysts:**  
  Lay the groundwork in SQL, schema design, and analytical querying — essential skills for any data-centric role.

- **Computer Science Students:**  
  Go beyond theory with hands-on, portfolio-worthy projects that solve realistic challenges.

- **Self-Taught Programmers:**  
  Follow a structured, project-based path to mastering one of software engineering’s most durable and in-demand skills.

## 🗺️ The Curriculum: A Project-Based Journey

Theory without application fades fast. This curriculum is built around real, practical projects — each one designed to tackle a specific SQL concept or database engineering skill.

Each folder is a standalone learning unit: complete with code, write-ups, and reflections. Together, they form a clear and progressive path from fundamentals to advanced use cases.

### 🔹 Core Mini-Projects

| Project                                                    | Pitch                                                                                                                                                                    | Core Focus                                    |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------- |
| **[01-DataCartographer](./01-DataCartographer/README.md)** | A showcase of advanced data retrieval, answering complex business questions with a fluency that surpasses standard ORM capabilities.                                     | JOINs, GROUP BY, HAVING, Subqueries           |
| **[02-SchemaSculptor](./02-SchemaSculptor/README.md)**     | Demonstrates robust database architecture by designing a normalized (3NF) schema from scratch, enforcing data integrity at the database level.                           | DDL, Normalization, Constraints               |
| **[03-InsightEngine](./03-InsightEngine/README.md)**       | A deep dive into analytical SQL, using CTEs and Window Functions to perform sophisticated calculations like ranking and running totals.                                  | CTEs, Window Functions                        |
| **[04-LedgerGuard](./04-LedgerGuard/README.md)**           | A practical demonstration of transactional control, ensuring data consistency and atomicity in critical, multi-statement operations.                                     | ACID, BEGIN/COMMIT/ROLLBACK, Isolation Levels |
| **[05-QueryCharger](./05-QueryCharger/README.md)**         | A lab in performance engineering: analyzing slow query plans (EXPLAIN ANALYZE), identifying bottlenecks, and applying strategic indexes for dramatic speed improvements. | Indexes, Execution Plans, Optimization        |
| **[06-LogicVault](./06-LogicVault/README.md)**             | Treats the database as a powerful processing engine by encapsulating business logic in Stored Procedures and enforcing rules with Triggers.                              | Stored Procedures, Triggers, UDFs             |

---

### 🧱 Capstone Projects

These projects synthesize all the skills from the mini-projects into a single, cohesive, enterprise-grade solution.

| Project                                                                  | Pitch                                                                                                                                                                             |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[07-SupplyChainCore](./07-SupplyChainCore/README.md)** _(Coming Soon)_ | The complete data layer for a mission-critical supply chain platform, from schema design to transactional logic and optimized reporting. An OLTP system built from the ground up. |
| **[08-AnalyticsPrism](./08-AnalyticsPrism/README.md)** _(Coming Soon)_   | Bridges the gap between transactional and analytical systems by designing and building a denormalized OLAP data warehouse (star schema) from a production OLTP database.          |

---

### 🛠️ Core Skills & Technologies

- **Primary Database:** PostgreSQL (chosen for its feature-richness and strict standards compliance)
- **Scripting:** Python (using libraries like psycopg2 for transactional scripts)
- **Key Concepts:**
  - **Querying:** SELECT, JOINs, Aggregations, Subqueries, CTEs, Window Functions
  - **Design:** Normalization (1NF, 2NF, 3NF), Constraints, Data Types, OLTP vs. OLAP, Star Schemas
  - **Transactions:** ACID Properties, Isolation Levels, BEGIN/COMMIT/ROLLBACK
  - **Performance:** EXPLAIN ANALYZE, Indexing Strategies (B-Tree)
  - **In-Database Logic:** Stored Procedures, UDFs, Triggers

---

### 📂 How to Use This Repository

Each project is a standalone module with its own `README.md` — your starting point for understanding the goals, steps, and key takeaways.

The folders are numbered for a reason: they’re structured to build knowledge progressively, but you’re free to jump around based on your interest.

---

### 🚧 Project Status: Gradual Rollout in Progress

This repository is being developed progressively. New projects and documentation are added over time to expand and refine the content.

> Feel free to ⭐ star the repo to stay notified of future releases.

---

## 🚀 How to Get Started

```bash
git clone https://github.com/azsharkawy5/QueryAlchemy.git
cd QueryAlchemy/01-DataCartographer/
# Follow README instructions in each folder
```

✅ Each folder is self-contained and ready to run.  
✅ While the curriculum is progressive, feel free to explore topics in any order.  
✅ Best experienced using **PostgreSQL** with tools like **DataGrip**, **DBeaver**, or **pgAdmin**.

---

## 🌍 License & Contribution

**License**: MIT — use, fork, or remix freely.  
**Contributions**: Coming soon — this will open as a collaborative learning space for SQL learners and backend engineers.

---

## 🧠 About the Creator

**Ahmad Zakaria** — Enthusiast Backend Engineer | TA @ Faculty of CS at O6U

I created QueryAlchemy to bridge a gap I personally experienced: going from depending on ORMs to truly understanding relational databases. This project is as much a learning journal as it is a resource for others walking a similar path.

> Connect on [LinkedIn](https://www.linkedin.com/in/azsharkawy5/) • Explore more on [GitHub](https://github.com/azsharkawy5)

---

## 📚 Resources & Credits

This repository was built on a blend of deep-dive documentation, interactive tutorials, and creative learning tools. These resources shaped the journey and are recommended for anyone looking to strengthen their SQL and database skills:

- **📘 [PostgreSQL Docs](https://www.postgresql.org/docs/)** — The official, gold-standard reference for everything PostgreSQL.
- **🧠 [Mode SQL Tutorial](https://mode.com/sql-tutorial/)** — A visual guide to SQL fundamentals with an analytics focus.
- **⚡ [Use The Index, Luke!](https://use-the-index-luke.com/)** — Master SQL performance through indexing strategies.
- **🎓 [Khan Academy SQL](https://www.khanacademy.org/computing/computer-programming/sql)** — Beginner-friendly, visual, and practical lessons.
- **🧩 [SQLBolt](https://sqlbolt.com/)** — Quick, interactive lessons for practicing core SQL operations.
- **🌟 [Select Star SQL](https://selectstarsql.com/)** — A fun and narrative-driven way to solidify SQL basics.
- **🕵️‍♂️ [SQL Murder Mystery](https://mystery.knightlab.com/)** — A game-like challenge to sharpen your query-building and debugging instincts.

> _If any resource creators want further attribution or have feedback, feel free to contact me._

---
