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


## How I Run the SQL

I use **Supabase** as my PostgreSQL database:
1. I connect to my project in Supabase.
2. I open the SQL editor.
3. I copy and paste the corresponding section from `sql/sql_week2_solution.sql`.
4. I run the script and verify the result.

This repository is mainly for documentation and version control of my SQL learning process.

## Section 2 – Triggers

In this section I implemented 4 triggers:

1. **Average Rating Trigger**
   - Function: `update_movie_avg_rating()`
   - Trigger: `trg_update_movie_avg_rating`
   - Purpose: Whenever a renting rating is inserted or updated, the movie's `avg_rating` column is recalculated based on all non-null ratings in `rentings`.

2. **Rating Validation Trigger**
   - Function: `validate_rentings_rating()`
   - Trigger: `trg_validate_rentings_rating`
   - Purpose: Prevents inserting or updating invalid ratings. Only `NULL` or values between 0 and 10 are allowed.

3. **Delete Protection Trigger**
   - Function: `prevent_movie_delete_if_related()`
   - Trigger: `trg_prevent_movie_delete_if_related`
   - Purpose: Blocks deletion of a movie if it still has related rows in `rentings` or `actsin`.

4. **Logging Trigger (Optional)**
   - Table: `log_activity`
   - Function: `log_movie_activity()`
   - Trigger: `trg_log_movie_activity`
   - Purpose: Logs every INSERT and DELETE operation on the `movies` table, storing table name, operation type, record ID, and timestamp.

## Section 3 – Views

In this section, I created several views to summarize key information from the movie rental database.

1. **`view_movie_summary`**
   - Columns:
     - `movie_id`
     - `title`
     - `genre`
     - `year_of_release`
     - `renting_price`
     - `avg_rating` (average of all non-null `rating` values from `rentings`)
     - `review_count` (number of ratings recorded for that movie)
   - Purpose: Quickly see each movie along with its basic information and rating statistics.
   - Note: The original assignment mentions including a director field, but this schema does not have a `directors` table, so the view was adapted accordingly.

2. **`view_actor_summary`**
   - Columns:
     - `actor_id`
     - `actor_name`
     - `number_of_movies` (count of distinct movies in `actsin` for that actor)
     - `avg_movie_rating` (average of all ratings for movies the actor played in)
   - Purpose: Summarize an actor's activity in the database and how their movies perform in terms of ratings.

3. **`view_genre_stats`**
   - Columns:
     - `genre_name`
     - `total_movies` (number of distinct movies in that genre)
     - `avg_genre_rating` (average rating across all movies in that genre)
   - Purpose: Provide a quick overview of how each genre performs and how many movies belong to it.

I tested these views in Supabase using simple queries like:

```sql
SELECT * FROM view_movie_summary LIMIT 10;
SELECT * FROM view_actor_summary ORDER BY number_of_movies DESC;
SELECT * FROM view_genre_stats ORDER BY avg_genre_rating DESC NULLS LAST;


## Section 4 – Stored Functions

This section includes the two functions required by the assignment:

### 1. `get_actor_avg_rating(actor_id)`
- Returns the average rating of all movies an actor has played in.
- Uses the tables `actsin` (actor–movie relationship) and `rentings` (ratings).
- Returns `NULL` if the actor's movies have no ratings.

Example:
```sql
SELECT actor_id, name,
       get_actor_avg_rating(actor_id) AS actor_average_rating
FROM actors;

## Section 5 – Analytical Queries

In this section I wrote 12 analytical SQL queries to explore complex logic and relationships in the movie rental database, for example:

- Top 5 movies by average rating.
- Number of movies per actor and actors who appeared in more than 3 movies.
- Actors whose average movie rating is greater than 7.
- Movies that have no reviews.
- Youngest actor in each genre.
- Genres with an average rating higher than the overall average.
- Actors who never appeared in a movie rated below 5.
- Yearly statistics: total movies, number of reviews, and average rating per release year.
- The most active reviewer (customer with most ratings).
- Average runtime per genre.
- Average number of movies rented per active customer.






