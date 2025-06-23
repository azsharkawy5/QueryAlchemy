# Project 2: SchemaSculptor - Designing for Integrity

Welcome to the second project in the QueryAlchemy series. In the [first project, DataCartographer](./DataCartographer/), we learned to query an existing database. Now, we take on the role of the architect: we will design a database schema from the ground up, translating a set of business requirements into a robust, normalized, and logical structure.

This is the art of building the container, not just manipulating what's inside it. The goal is to enforce data integrity at the lowest level, creating a foundation that prevents bad data from ever entering the system.

### The Mission: From Business Rules to Bulletproof Schema

We will design the database for a simple project management application, "TaskHub." Our task is to create the Data Definition Language (DDL) script that defines the tables, columns, data types, and—most importantly—the constraints that govern their relationships.

---

## Part 1: The Theory - What is Normalization?

Before we can build, we must understand the blueprint's principles. Normalization is a process of organizing tables to reduce data redundancy and improve data integrity. Think of it like organizing your home: instead of keeping duplicate items scattered throughout every room, you create designated places where each type of item belongs.

Let's explore this through a realistic scenario that everyone can relate to: **managing a university course registration system**. We'll see how poor design creates chaos, and how normalization principles solve real problems.

## The Problem: A Messy, Unnormalized System

Imagine your university stores all course registration data in a single, massive spreadsheet that looks like this:

| Student_Name | Student_Email   | Student_Phone | Course_Code | Course_Name          | Instructor_Name | Instructor_Email | Credits | Semester    |
| ------------ | --------------- | ------------- | ----------- | -------------------- | --------------- | ---------------- | ------- | ----------- |
| Sarah Chen   | sarah@email.com | 555-0123      | CS101       | Intro to Programming | Dr. Smith       | smith@uni.edu    | 3       | Fall 2024   |
| Sarah Chen   | sarah@email.com | 555-0123      | MATH200     | Calculus II          | Prof. Johnson   | johnson@uni.edu  | 4       | Fall 2024   |
| Mike Torres  | mike@email.com  | 555-0456      | CS101       | Intro to Programming | Dr. Smith       | smith@uni.edu    | 3       | Fall 2024   |
| Sarah Chen   | sarah@email.com | 555-0789      | CS101       | Intro to Programming | Dr. Smith       | smith@uni.edu    | 3       | Spring 2025 |

**What's wrong with this picture?** Let's say Sarah changes her phone number. You'd have to update it in multiple rows, and if you miss one, you now have conflicting information. Worse yet, if Sarah drops all her courses, you lose her contact information entirely. If Dr. Smith gets married and changes her name, you'd need to update dozens of rows across multiple semesters.

## First Normal Form (1NF): Making Data Atomic

**The Rule:** Each column must hold a single value, and each row must be unique.

**The Problem:** Let's say we tried to be "efficient" and stored multiple phone numbers like this:

| Student_Name | Phone_Numbers      | Course_Code |
| ------------ | ------------------ | ----------- |
| Sarah Chen   | 555-0123, 555-0124 | CS101       |

**Why This Breaks:** How do you search for students with a specific phone number? How do you update just one phone number? You'd need to parse the comma-separated string every time, which is messy and error-prone.

**The Solution:** Create separate rows for each phone number, or better yet, create a separate phone numbers table. Each piece of data gets its own dedicated space.

## Second Normal Form (2NF): Eliminating Partial Dependencies

**The Rule:** Every non-key column must depend on the entire primary key, not just part of it.

**The Problem in Our Example:** Suppose we use a composite primary key of (Student_ID, Course_Code) to uniquely identify each enrollment. But look at this problem:

| Student_ID | Course_Code | Student_Name | Student_Email   | Course_Name          | Instructor_Name |
| ---------- | ----------- | ------------ | --------------- | -------------------- | --------------- |
| 1001       | CS101       | Sarah Chen   | sarah@email.com | Intro to Programming | Dr. Smith       |
| 1001       | MATH200     | Sarah Chen   | sarah@email.com | Calculus II          | Prof. Johnson   |

**The Issue:** Student_Name and Student_Email only depend on Student_ID (part of the key), while Course_Name and Instructor_Name only depend on Course_Code (the other part of the key). This creates redundancy and update problems.

**The Real-World Impact:** When Sarah changes her email, you have to update multiple rows. If she drops all courses, you lose her information. If a course name changes, you have to update every enrollment record for that course.

**The Solution:** Split this into separate tables where each piece of information has a single, authoritative location:

- **Students table:** Student_ID (primary key), Student_Name, Student_Email
- **Courses table:** Course_Code (primary key), Course_Name, Instructor_Name
- **Enrollments table:** Student_ID, Course_Code (composite primary key), enrollment date, grade

## Third Normal Form (3NF): Eliminating Transitive Dependencies

**The Rule:** Non-key columns should not depend on other non-key columns.

**The Problem:** Even after our 2NF improvements, we still have issues in the Courses table:

| Course_Code | Course_Name          | Instructor_Name | Instructor_Email | Instructor_Department |
| ----------- | -------------------- | --------------- | ---------------- | --------------------- |
| CS101       | Intro to Programming | Dr. Smith       | smith@uni.edu    | Computer Science      |
| CS201       | Data Structures      | Dr. Smith       | smith@uni.edu    | Computer Science      |

**The Issue:** Instructor_Email and Instructor_Department depend on Instructor_Name, not on Course_Code. This means Dr. Smith's information is duplicated across every course she teaches.

**The Real-World Impact:** If Dr. Smith changes her email or moves to a different department, you have to update multiple course records. If she stops teaching temporarily, you might lose her contact information.

**The Solution:** Create separate tables for instructors:

- **Instructors table:** Instructor_ID (primary key), Instructor_Name, Instructor_Email, Instructor_Department
- **Courses table:** Course_Code (primary key), Course_Name, Instructor_ID (foreign key)

## The Beautiful Result: A Normalized System

After applying all three normal forms, we end up with a clean, logical structure:

**Students Table:** Each student exists exactly once **Instructors Table:** Each instructor exists exactly once  
**Courses Table:** Each course exists exactly once, referencing its instructor **Enrollments Table:** Links students to courses without duplicating information

**The Payoff:** Now when Sarah changes her phone number, you update one record in one place. When Dr. Smith gets promoted to department head, you update her information once, and it's automatically reflected across all her courses. When a course gets renamed, you change it in one location, and every student's transcript is automatically accurate.

This is the power of normalization: it transforms a chaotic, error-prone system into an elegant, maintainable foundation where every piece of information has exactly one authoritative home. Just like organizing your house, it might take more initial effort to set up proper storage systems, but it saves countless hours of frustration and prevents serious problems down the road.

---

## Part 2: The Challenge - The Business Requirements

Here is the set of requirements for our "TaskHub" application. Your challenge is to think about what tables and columns would be needed to satisfy these rules.

- **Users:** We need to store users. Each user has a unique username, a required and unique email, a password hash, and a creation date.
- **Projects:** Users can create projects. A project has a name and is owned by a single user. A user cannot have two projects with the same name.
- **Tasks:** A project contains tasks. Each task has a title, a status ('To Do', 'In Progress', or 'Done'), and must belong to a project.
- **Assignments:** A task can be assigned to multiple users. A user can be assigned to multiple tasks.

**Before looking at the solution, try sketching out the tables on a piece of paper. How would you connect them? What constraints would you need?**

---

## Part 3: The Solution - Your DDL Script

After analyzing the requirements and applying the principles of normalization, here is the final, robust DDL script. We will break down the key architectural decisions below.

<details> <summary>💡 View Solution</summary>

```sql
-- Creates a custom type to ensure status can only be one of the allowed values.
CREATE TYPE task_status AS ENUM (
    'To Do',
    'In Progress',
    'Done'
);

-- Table for storing user information
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(256) NOT NULL UNIQUE,
    hashed_password VARCHAR(256) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- Table for projects, owned by a user
CREATE TABLE projects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    owner_id UUID NOT NULL REFERENCES users(id) ON DELETE RESTRICT,
    -- This constraint is critical for the business rule
    UNIQUE (owner_id, name)
);

-- Table for tasks, which must belong to a project
CREATE TABLE tasks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    status task_status NOT NULL DEFAULT 'To Do',
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE
);

-- Junction table for the many-to-many relationship between tasks and users
CREATE TABLE task_assignments (
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    -- This composite primary key is the heart of this table's design
    PRIMARY KEY (task_id, user_id)
);
```

</details>
