# SQL Week 2 – Movie Rental Database

This repository contains my solutions for the **SQL Week 2** assignment using a movie rental database.

## Database Schema

The database uses the following tables:

- `actors(actor_id, name, year_of_birth, nationality, gender)`
- `actsin(actsin_id, movie_id, actor_id)`
- `customers(customer_id, name, country, gender, date_of_birth, date_account_start)`
- `movies(movie_id, title, genre, runtime, year_of_release, renting_price)`
- `rentings(renting_id, customer_id, movie_id, rating, date_renting)`

## File Structure

- `sql/sql_week2_solution.sql`  
  Contains all SQL code for:
  - Section 1: Relationships & Constraints
  - Section 2: Triggers
  - Section 3: Views
  - Section 4: Functions
  - Section 5: Analytical Queries
  - Section 6: Summary Reports

## Section 1 – Relationships & Constraints

In Section 1, I added:

- `CHECK` constraints for rating (0–10), non-empty actor names, and minimum year_of_release (>= 1900).
- A `UNIQUE` constraint on `(title, year_of_release)` in `movies`.
- Foreign key constraints:
  - `actsin.movie_id` → `movies.movie_id` (ON DELETE CASCADE)
  - `actsin.actor_id` → `actors.actor_id` (ON DELETE CASCADE)
  - `rentings.customer_id` → `customers.customer_id` (ON DELETE CASCADE)
  - `rentings.movie_id` → `movies.movie_id` (ON DELETE CASCADE)
- Set `movies.genre` as NOT NULL, so every movie has a valid genre.

All SQL for this part is implemented in `sql/sql_week2_solution.sql`.

## How I Run the SQL

I use **Supabase** as my PostgreSQL database:
1. I connect to my project in Supabase.
2. I open the SQL editor.
3. I copy and paste the corresponding section from `sql/sql_week2_solution.sql`.
4. I run the script and verify the result.

This repository is mainly for documentation and version control of my SQL learning process.
