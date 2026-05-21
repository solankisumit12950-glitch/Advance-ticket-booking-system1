# Advanced Ticket Booking System - Database Project

This repository contains the database design and SQL scripts for a high-concurrency **Advanced Ticket Booking System**, developed as a practical mini-project for the MCA Advanced Database Management System course.

## 🎯 Problem Statement
Design and implement a high-concurrency ticket booking system where multiple users can simultaneously book seats. The system must ensure no double booking, maintain consistency, and properly handle transactions under heavy load.

## 📁 Repository Structure

The project is broken down into specific SQL files demonstrating various advanced database concepts. **PostgreSQL** syntax is used throughout to leverage its powerful concurrency control mechanisms (`SKIP LOCKED`, `NOWAIT`, etc.).

1. **`01_schema_and_seed.sql`** (Q1 Database Design)
   - Creates the robust schema (`Users`, `Shows`, `Seats`, `Bookings`).
   - Applies Primary Keys, Foreign Keys, and Constraints.
   - Includes seed data to populate the tables for testing.

2. **`02_advanced_booking.sql`** (Q2 Advanced Booking Transaction)
   - A PL/pgSQL transaction-safe booking procedure.
   - Utilizes `FOR UPDATE NOWAIT` to lock a seat immediately or abort if unavailable.
   - Safely commits or rolls back using transaction blocks.

3. **`03_parallel_booking.sql`** (Q3 Parallel Booking Handling)
   - Demonstrates the highly efficient `FOR UPDATE SKIP LOCKED` query.
   - Allows multiple users to simultaneously query and lock different available seats without bottlenecking the system.

4. **`04_deadlock_simulation.sql`** (Q4 Deadlock Simulation)
   - Simulates a scenario where two transactions lock seats in reverse order, intentionally causing a deadlock.
   - Provides a concrete query strategy (ordering locks by Primary Key) to prevent deadlocks completely.

5. **`05_optimistic_locking.sql`** (Q5 Optimistic Locking)
   - Solves concurrency issues without row locking by using a `version` column.
   - Prevents the classic "Lost Update" problem.

6. **`06_failure_rollback.sql`** (Q6 Failure & Rollback)
   - Simulates a checkout failure (e.g., payment gateway failure).
   - Demonstrates how the `ROLLBACK` command perfectly undoes the pending booking and releases the seat lock automatically.

7. **`07_isolation_levels.sql`** (Q7 Isolation Level Analysis)
   - Tests and explains the impact of `READ COMMITTED` vs `SERIALIZABLE` isolation levels.
   - Outlines how these levels affect seat availability visibility and booking consistency.

8. **`08_bonus_challenge.sql`** (Q8 Bonus Challenge 🔥)
   - **Auto-release**: Uses timestamp logic (`locked_at`) to identify and release expired seat locks (e.g., after a 5-minute checkout timeout).
   - **Waitlist Queue**: Implements a `Waitlist` table. When a booking is cancelled, it safely assigns the seat to the next person in line using `FOR UPDATE SKIP LOCKED`.

## 🚀 How to Run

1. Ensure you have a **PostgreSQL** database running (e.g., via pgAdmin, Docker, or terminal).
2. Execute `01_schema_and_seed.sql` first to set up the foundation.
3. You can execute the subsequent scripts one by one to see how the system handles different transaction scenarios. For `04` and `07`, you will need to open two separate query windows/terminals to simulate concurrent users.

## ✅ Expected Outcomes Met
- **No Double Booking**: Guaranteed by strict Row-Level Locks and Optimistic Locking mechanisms.
- **Safe Concurrent Transactions**: Achieved via ACID-compliant transaction blocks.
- **Efficient Multi-User Handling**: Mastered through `SKIP LOCKED` queries, avoiding massive queues and application lag.
