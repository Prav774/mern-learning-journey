# Day 1 — Database Fundamentals & Why MongoDB

## Learning Objectives

By the end of this module, you should understand:

* What data is and its types
* What a database and DBMS are
* Difference between DBMS and RDBMS
* SQL and NoSQL databases
* ACID and BASE concepts
* CAP Theorem basics
* Why MongoDB is widely used in MERN applications
* Schema-less design
* Horizontal and Vertical Scaling

---

# What is Data?

Data refers to raw facts, values, or information that can be stored and processed.

## Types of Data

### 1. Structured Data

Data organized in a fixed format.

Examples:

* Student Records
* Employee Information
* Bank Transactions

```text
ID | Name   | Age
1  | John   | 21
2  | Alice  | 22
```

---

### 2. Semi-Structured Data

Data does not follow a strict table structure but contains organizational tags.

Examples:

* JSON
* XML

```json
{
  "name": "John",
  "age": 21
}
```

---

### 3. Unstructured Data

Data without any predefined format.

Examples:

* Images
* Videos
* Audio Files
* Social Media Posts

---

# What is a Database?

A database is an organized collection of data that can be stored, managed, and retrieved efficiently.

## Real-World Examples

* Banking Systems
* E-Commerce Websites
* Social Media Platforms
* College Management Systems

Without databases, managing large amounts of information becomes difficult and inefficient.

---

# What is DBMS?

DBMS (Database Management System) is software that helps users create, manage, and interact with databases.

## Responsibilities

* Data Storage
* Data Retrieval
* Data Security
* Backup and Recovery
* Data Management

## Examples

* MySQL
* Oracle
* Microsoft SQL Server
* PostgreSQL

---

# Components of DBMS

```text
Users
  |
  v
DBMS Software
  |
  v
Database
```

### Main Components

1. Hardware
2. Software
3. Data
4. Procedures
5. Database Access Language
6. Users

---

# What is RDBMS?

RDBMS (Relational Database Management System) stores data in the form of tables.

Each table consists of:

* Rows (Records)
* Columns (Fields)

## Example

### Students Table

| StudentID | Name  | Department |
| --------- | ----- | ---------- |
| 101       | John  | CSE        |
| 102       | Alice | IT         |

### Departments Table

| DepartmentID | DepartmentName |
| ------------ | -------------- |
| 1            | CSE            |
| 2            | IT             |

Relationships can be created between tables.

---

# Database Keys

## Primary Key

A primary key uniquely identifies each record.

### Example

| StudentID | Name  |
| --------- | ----- |
| 101       | John  |
| 102       | Alice |

StudentID is the Primary Key.

### Properties

* Unique
* Cannot be NULL
* One per table

---

## Foreign Key

A foreign key creates a relationship between two tables.

### Example

Orders Table references Users Table.

```text
Users
-----
UserID (PK)

Orders
------
OrderID (PK)
UserID (FK)
```

---

## Composite Key

A combination of multiple columns used as a primary key.

### Example

```text
StudentID + CourseID
```

Together they uniquely identify a record.

---

# SQL Basics

SQL (Structured Query Language) is used to interact with relational databases.

## SELECT

Retrieve data.

```sql
SELECT * FROM Students;
```

---

## JOIN

Combine data from multiple tables.

```sql
SELECT *
FROM Students
JOIN Departments
ON Students.DepartmentID = Departments.DepartmentID;
```

---

# What is NoSQL?

NoSQL means "Not Only SQL".

These databases are designed for large-scale, flexible, and distributed applications.

---

# Types of NoSQL Databases

## 1. Document Database

Stores data as documents.

Example:

```json
{
  "name": "Praveen",
  "skills": ["Java", "MongoDB"]
}
```

Examples:

* MongoDB
* CouchDB

---

## 2. Key-Value Database

Stores data as key-value pairs.

Example:

```text
user123 -> John
```

Examples:

* Redis
* DynamoDB

---

## 3. Column-Family Database

Stores data by columns instead of rows.

Examples:

* Cassandra
* HBase

---

## 4. Graph Database

Stores relationships between entities.

Examples:

* Neo4j
* Amazon Neptune

---

# SQL vs NoSQL

| Feature        | SQL      | NoSQL                  |
| -------------- | -------- | ---------------------- |
| Structure      | Tables   | Documents/Other Models |
| Schema         | Fixed    | Flexible               |
| Scaling        | Vertical | Horizontal             |
| Query Language | SQL      | Varies                 |
| Relationships  | Strong   | Limited                |
| ACID Support   | Strong   | Often BASE-Oriented    |

---

# When to Use SQL

## Use Cases

### Banking System

Requires strict transaction consistency.

### Hospital Management

Patient records must remain accurate.

### Airline Reservation System

Seat bookings require transactional integrity.

---

# When to Use NoSQL

## Use Cases

### Social Media Platform

Handles huge volumes of data.

### Chat Application

Stores rapidly changing messages.

### Real-Time Analytics

Processes large-scale event data.

---

# ACID Properties

ACID ensures reliable transactions.

---

## A — Atomicity

Either all operations succeed or none succeed.

### Example

Bank Transfer

```text
Deduct ₹1000
Add ₹1000
```

If one step fails, the entire transaction is rolled back.

---

## C — Consistency

Database remains valid before and after transactions.

### Example

Account balance rules remain correct.

---

## I — Isolation

Multiple transactions do not interfere with each other.

### Example

Two users booking tickets simultaneously.

---

## D — Durability

Committed data remains stored permanently.

### Example

Even after a power failure.

---

# BASE Model

BASE is commonly associated with distributed NoSQL systems.

## B — Basically Available

System remains available.

## A — Soft State

Data may temporarily change.

## E — Eventual Consistency

All copies eventually become consistent.

---

# CAP Theorem (Overview)

In a distributed system, only two of the following three can be fully achieved simultaneously.

## C — Consistency

All users see the same data.

## A — Availability

Every request gets a response.

## P — Partition Tolerance

System continues working despite network failures.

### Simple Formula

```text
CAP = Consistency + Availability + Partition Tolerance
```

Trade-offs are required in distributed systems.

---

# Why MongoDB for MERN?

MongoDB is a document-oriented NoSQL database.

## Reasons

### JSON-like Documents

MongoDB stores BSON documents similar to JavaScript objects.

```json
{
  "name": "Praveen",
  "email": "praveen@example.com"
}
```

---

### Easy Integration with JavaScript

Frontend, Backend, and Database all work naturally with JavaScript.

---

### Flexible Schema

Fields can change without redesigning tables.

---

### Horizontal Scaling

Can handle large-scale applications.

---

### Faster Development

Developers can iterate quickly without strict schemas.

---

# Schema-Based vs Schema-Less Databases

| Schema-Based        | Schema-Less        |
| ------------------- | ------------------ |
| Fixed Structure     | Flexible Structure |
| Difficult to Change | Easy to Change     |
| SQL Databases       | MongoDB            |

---

## Advantage of Schema-Less Design

Rapid development and flexibility.

## Disadvantage

Possible data inconsistency if validation is poor.

---

# Scaling Basics

## Vertical Scaling

Increase resources of one server.

### Example

```text
8 GB RAM → 32 GB RAM
```

### Pros

* Simple

### Cons

* Hardware Limitations

---

## Horizontal Scaling

Add more servers.

### Example

```text
Server 1
Server 2
Server 3
```

### Pros

* Better Scalability

### Cons

* More Complex

---

# Frequently Asked Interview Questions

## 1. DBMS vs RDBMS

DBMS manages databases, while RDBMS stores data in related tables and supports relationships.

---

## 2. Primary Key vs Foreign Key

Primary Key uniquely identifies a row.

Foreign Key creates a relationship between tables.

---

## 3. Why NoSQL Exists

To handle massive, flexible, and distributed data efficiently.

---

## 4. Explain ACID

Atomicity, Consistency, Isolation, and Durability ensure reliable transactions.

---

## 5. SQL or NoSQL for Chat Application?

NoSQL because chats generate huge volumes of rapidly changing data and require horizontal scaling.

---

## 6. CAP Theorem

* Consistency → Same data everywhere
* Availability → Always responds
* Partition Tolerance → Survives network failures

---

## 7. Why MongoDB Over MySQL in MERN?

* JSON-like documents
* Flexible schema
* Easy JavaScript integration
* Better horizontal scaling

---

## 8. What is Schema-Less Design?

Data structure can vary between records.

Advantage: Flexibility

Disadvantage: Potential inconsistency

---

## 9. Horizontal vs Vertical Scaling

Vertical → Upgrade one server.

Horizontal → Add more servers.

---

## 10. When is RDBMS Better?

Banking systems, payment gateways, and reservation systems where data consistency is critical.

---

# Day 1 Task

Create a one-page comparison document:

## SQL

### Use Cases

1. Banking System
2. Hospital Management System
3. Airline Reservation System

### Strengths

* Strong Consistency
* ACID Compliance
* Complex Queries

---

## NoSQL

### Use Cases

1. Social Media Platforms
2. Chat Applications
3. Real-Time Analytics Systems

### Strengths

* Flexible Schema
* Horizontal Scaling
* High Performance

---

# Summary

Today you learned:

* Database Fundamentals
* DBMS and RDBMS
* SQL and NoSQL
* ACID and BASE
* CAP Theorem Basics
* MongoDB Fundamentals
* Schema-less Design
* Scaling Concepts

These concepts form the foundation required before starting MongoDB installation, CRUD operations, and Mongoose in the MERN stack.
