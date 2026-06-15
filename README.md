# Football Ticket Booking System - Database Design & SQL Queries

**Author:** Arpan Sarkar  
**Assignment:** Relational Database Management Systems (RDMS)

##  Project Overview
This project involves the design, creation, and querying of a relational database for a Football Ticket Booking System.
The database manages user profiles, match fixtures, and ticket transactions. It is built using standard SQL (PostgreSQL) and strictly 
enforces data integrity through primary keys, foreign keys, and specific `CHECK` constraints to mirror real-world business rules.

## 🔗 Entity-Relationship Diagram (ERD)
The schema architecture was mapped using Crow's Foot notation to clearly define the cardinality between entities.

* **[View the Complete ERD Diagram Here](https://drawsql.app/teams/arpan-workspace/diagrams/erd-relationship-football-ticketing-system)** (Hosted on DrawSQL)

---

## Business Logic & Schema Architecture

The database is structured around three primary entities. The business logic dictates that a **User** can make multiple bookings, 
and a **Match** can have multiple bookings, but a single **Booking** record links exactly one user to one match.

### 1. Users Table
Stores information regarding the fans and system administrators.
* **Primary Key:** `user_id`
* **Constraints:** `email` is explicitly set to `UNIQUE`.
* **Business Logic:** The `role` column uses a `CHECK` constraint to restrict entries strictly to `'Ticket Manager'` or `'Football Fan'`.

### 2. Matches Table
Stores scheduling and financial details for football fixtures.
* **Primary Key:** `match_id`
* **Constraints:** `base_ticket_price` enforces a `CHECK (base_ticket_price >= 0)` to prevent negative pricing.
* **Business Logic:** The `match_status` tracks the lifecycle of the event using a `CHECK` constraint limited to: `'Available'`, `'Selling Fast'`, `'Sold Out'`, or `'Postponed'`.

### 3. Bookings Table
Acts as the transactional table linking Users and Matches.
* **Primary Key:** `booking_id`
* **Foreign Keys:** `user_id` (references Users) and `match_id` (references Matches).
* **Relationships:** * **1-to-Many:** Users to Bookings
  * **1-to-Many:** Matches to Bookings
* **Business Logic:** `payment_status` uses a `CHECK` constraint limited to `'Pending'`, `'Confirmed'`, `'Cancelled'`, or `'Refunded'`. `total_cost` cannot be negative.

---

##  SQL Query Challenges

The `QUERY.sql` file in this repository contains the schema creation, data seeding, and the solutions to the following seven analytical queries:

1. **Basic Filtering:** Retrieve all upcoming football matches belonging to the 'Champions League' where the match status is 'Available'.
2. **Pattern Matching (`ILIKE` & `%`):** Search for all users whose full names start with 'Tanvir' or contain the phrase 'Haque' (case-insensitive).
3. **Null Handling (`COALESCE`):** Retrieve all booking records where the payment status is missing (`NULL`), replacing the empty result with the text 'Action Required'.
4. **Relational Joins (`INNER JOIN`):** Retrieve match booking details along with the User's full name and the scheduled Match fixture teams.
5. **Outer Joins (`FULL JOIN` / `LEFT JOIN`):** Display a comprehensive list of all users and their booking IDs, ensuring that fans who have never bought a ticket are still listed.
6. **Subqueries:** Find all ticket bookings where the total cost is strictly higher than the average cost of all ticket bookings.
7. **Sorting & Pagination (`LIMIT` & `OFFSET`):** Retrieve the top 2 most expensive matches sorted by base ticket price, skipping the absolute highest premium match.

---

## How to Run
1. Clone this repository.
2. Load the `QUERY.sql` file into your preferred PostgreSQL environment.
3. Execute the script to drop/create tables, insert the seed data, and run the 7 analytical queries.
