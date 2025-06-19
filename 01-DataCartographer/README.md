# Project 1: DataCartographer - A Journey into Advanced SQL Reporting

Hello! This project documents my first step in evolving from a developer who relies on an ORM into an engineer who is fluent in raw SQL. If you've ever felt that the database is a "black box," this guide is for you.

The goal is not just to show final queries, but to walk through the _mental models_ needed to construct them, present challenges for you to try, and then analyze the solutions in detail.

## Project Setup: Personal SQL Lab

To follow along and run these queries yourself, you'll need your own instance of the `pagila` database. The easiest and most reproducible way to do this is with Docker.

**Prerequisites:**

- [Docker](https://www.docker.com/get-started) is installed on your machine.

**Steps:**

1.  **Download the Database Dump:** You'll need the `pagila-schema.sql` and `pagila-data.sql` database files, which contains both the schema (the table structure) and the data.

    - **Direct Download Link:**
      - [pagila-schema.sql](https://github.com/devrimgunduz/pagila/blob/master/pagila-schema.sql)
      - [pagila-data.sql](https://github.com/devrimgunduz/pagila/blob/master/pagila-data.sql)
    - Save `pagila-schema.sql` file as `00-pagila-schema.sql` and `pagila-data.sql` file as `01-pagila-data.sql` in this project directory (`01-DataCartographer`).

2.  **Start the Database:** Open your terminal, navigate to this project directory, and run:

    ```bash
    docker-compose up -d
    ```

3.  **Connect:** You can now connect to your PostgreSQL server using any SQL client (DBeaver, DataGrip, Postico, `psql`) with these credentials:
    - **Host:** `localhost`, **Port:** `5432`, **User:** `postgres`, **Password:** `postgres`

---

### 🗂️ Project Directory Structure

After completing the above steps, your `01-DataCartographer/` folder should look like this:

```plaintext
01-DataCartographer/
├── 00-pagila-schema.sql       # PostgreSQL schema (tables, indexes, etc.)
├── 01-pagila-data.sql         # Sample data for the schema
├── docker-compose.yml         # Spins up the PostgreSQL container
└── README.md                  # This project’s documentation and challenges
```

---

### 🗺️ Schema Familiarization

Before writing any queries, it’s helpful to review the database schema and understand how the tables relate to each other.

> **Recommended:** Take a moment to explore the entity-relationship diagram (ERD) or schema layout so you can visualize the foreign key relationships and table structures.

🔗 **[View the Pagila Schema Diagram](https://github.com/devrimgunduz/pagila/blob/master/pagila-schema-diagram.png)**

Understanding how `customer`, `rental`, `inventory`, `film`, and `category` are connected will make complex `JOIN`s much more intuitive.

---

```

```
