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

---

## Part 2: The `LEFT JOIN` and the "Zero-Count" Trap

### The Challenge

> **Task:** Generate a report of all film titles and the number of times each has been rented. **Crucially, you must include films that have never been rented, showing a count of 0.**

### Mental Model Building: Enrichment vs. Filtering

Think of joins in two ways:

- `INNER JOIN` is a **filter**. It only keeps rows that have a match on both sides. It reduces your dataset.
- `LEFT JOIN` is an **enricher**. It keeps everything from the "left" table and adds information from the "right" table if it exists.
  Since we must keep all films, we must start with the `film` table and use `LEFT JOIN` to _enrich_ it with rental data.

### The Solution Query

<details>
  <summary>💡 View Solution</summary>

```sql
SELECT
    f.title,
    COUNT(r.rental_id) AS rental_count -- COUNT() ignores NULLs, automatically giving us 0 for non-rented films.
FROM
    film AS f
LEFT JOIN
    inventory AS i ON f.film_id = i.film_id
LEFT JOIN -- This must also be a LEFT JOIN to preserve films with inventory but no rentals.
    rental AS r ON i.inventory_id = r.inventory_id
GROUP BY
    f.title
ORDER BY
    rental_count DESC;
```

</details>

### Analysis

- **Readability:** `LEFT JOIN` explicitly signals the query's intent: "start with all films and add rental data if it exists."
- **Performance:** While `LEFT JOIN` can process more rows than an `INNER JOIN`, it is the **only correct and performant way** to solve this specific problem, as the alternative (many individual queries) would be far slower.
- **Complexity:** The conceptual leap is understanding that a single `INNER JOIN` in the chain will negate the "keep all" effect of preceding `LEFT JOIN`s.

## Part 3: Filtering on Aggregates with `HAVING`

### The Challenge

> **Task:** The marketing team wants to reward loyal customers. Produce a list of "power users" by finding all customers who have rented **more than 35 films** in total.

### Mental Model Building: The SQL Order of Operations

A query is not executed in the order it's written. The database follows a logical processing order. Understanding this is key to knowing where to filter.

1.  `FROM` and `JOIN`s assemble the initial dataset.
2.  `WHERE` filters individual rows.
3.  `GROUP BY` collapses rows into groups.
4.  Aggregate functions (`COUNT`, `SUM`, etc.) calculate values for each group.
5.  `HAVING` filters the _groups_ based on the aggregate results.
6.  `SELECT` determines the final output columns.
7.  `ORDER BY` sorts the final result.
    Since we need to filter on a `COUNT()`, the filter must come _after_ `GROUP BY`. That means we must use `HAVING`.

### The Solution Query

<details>
  <summary>💡 View Solution</summary>

```sql
SELECT
    c.first_name,
    c.last_name,
    COUNT(r.rental_id) AS rental_count
FROM
    customer AS c
INNER JOIN
    rental AS r ON c.customer_id = r.customer_id
GROUP BY
    c.customer_id -- Group by the primary key for accuracy and correctness.
HAVING
    COUNT(r.rental_id) > 35
ORDER BY
    rental_count DESC;
```

</details>

### 🧠 Brain Teaser

> **Question:** Why can't we use COUNT(rental_count) rather than COUNT(r.rental_id) in the `HAVING` clause?

### Analysis

- **Readability:** The `GROUP BY ... HAVING` structure is a standard, highly readable pattern for analytical reporting.
- **Performance:** This is the standard, optimized way to perform this operation. Shifting this logic to the application layer would be drastically less performant.
- **Complexity:** The difficulty lies in memorizing the logical order of operations and the fundamental difference between `WHERE` and `HAVING`.

---
