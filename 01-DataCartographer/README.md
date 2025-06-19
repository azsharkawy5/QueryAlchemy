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
├── pagila-data/               # Contains the pagila data dump
└── README.md                  # This project’s documentation and challenges
```

---

### 🗺️ Schema Familiarization

Before writing any queries, it’s helpful to review the database schema and understand how the tables relate to each other.

> **Recommended:** Take a moment to explore the entity-relationship diagram (ERD) or schema layout so you can visualize the foreign key relationships and table structures.

🔗 **[View the Pagila Schema Diagram](https://github.com/devrimgunduz/pagila/blob/master/pagila-schema-diagram.png)**

Understanding how `customer`, `rental`, `inventory`, `film`, and `category` are connected will make complex `JOIN`s much more intuitive.

---

## 🚨 What is the N+1 Query Problem?

Before diving into multi-`JOIN`s, it's important to understand the **N+1 problem**—a common performance pitfall when accessing related data.

> **The N+1 Problem** happens when your code first runs **one query** to get a list of authors, then runs **one additional query per author** to fetch their books.

> So if you have 100 authors, you're running **1 query to get authors + 100 queries to get books**—a total of 101 queries.

This quickly becomes inefficient and slows down your app as the data grows.

**The solution?** Use a single SQL query with a `JOIN` to fetch all authors and their books in one go. Let the database do the heavy lifting.

---

## Part 1: The Multi-`JOIN` Chain

### The Challenge

> **Task:** Generate a list containing the first name, last name, and email of every customer who has ever rented a film in the 'Action' category. Each customer should only appear once.

### Mental Model Building: Thinking Like a Cartographer

Before writing any code, look at the database schema as a map. Your job is to find the route from your starting point (`customer`) to your destination (`category`). Don't proceed until you can trace the path of foreign keys:
`customer` -> `rental` -> `inventory` -> `film` -> `film_category` -> `category`.
This "map" becomes the blueprint for your `JOIN` clauses.

### The Solution Query

<details>
  <summary>💡 View Solution</summary>

```sql
SELECT DISTINCT -- A customer might rent multiple action films; this ensures they appear only once.
    c.first_name,
    c.last_name,
    c.email
FROM
    customer AS c
INNER JOIN
    rental AS r ON c.customer_id = r.customer_id
INNER JOIN
    inventory AS i ON r.inventory_id = i.inventory_id
INNER JOIN
    film AS f ON i.film_id = f.film_id
INNER JOIN
    film_category AS fc ON f.film_id = fc.film_id
INNER JOIN
    category AS cat ON fc.category_id = cat.category_id
WHERE
    cat.name = 'Action';
```

</details>

### 🧠 Brain Teaser

> **Challenge:** Generate the same report, but using a `GROUP BY` clause instead of `DISTINCT`. How does this change the query?

### Analysis

- **Readability:** `JOIN` syntax reads like a logical sentence, clearly stating how tables connect. Aliases (`c`, `r`, `i`) are essential for maintaining clarity in complex queries.
- **Performance:** This is the most direct and performant way to ask this question. Database query planners are highly optimized for resolving these chains.
- **Complexity:** The complexity is not in the SQL syntax, but in the initial problem-solving step of schema navigation.
