# Concepts Needed for Deliverables

## Indexing

Indexes help find data faster, like a book's index. Without them, the database scans every row (slow for large tables).

#### Common Index Types

-   B-tree (default):

    -   Best for exact matches, range queries
    -   Example: finding flights between two dates

-   Hash:

    -   Best for equiality checks (=)
    -   Example: filtering by exact email

-   GIN:

    -   Best for arrays, JSON, full-text search
    -   Example: searching for rags in a blog post

-   BRIN:
    -   Best for large time-series data
    -   Example: Analyzing houly cargo shipments

### Trade-offs:

Indexes speed up rreads but slow downs _writes_ (inserts/updates). Potential solution: only create indexes for frequently queried columns

## Constrains

Constrains inforces rules to keep your data accurate and reliable

1. NOT NULL

    - Ensures a column can't be empty
    - Example: `ALTER TABLE passangers ADD COLUMN passport_number TEXT NOT NULL`

2. UNIQUE

    - Prevents duplicate values in a column.
    - Example: `ALTER TABLE airlines ADD CONSTRAINT unique_icao_code UNIQUE (icao_code)`

3. FOREIGN KEY

    - Links data between tables (eg. a flight must belong to a valid airline)
    - Example:

        ALTER TABLE flights  
         ADD CONSTRAINT fk_airline_id  
         FOREIGN KEY (airline_id) REFERENCES airlines(airline_id)

4. CHECK

    - Validates data before insertion
    - Examples: Ensuring cargo weight is positive:

        ALTER TABLE cargo  
         ADD CONSTRAINT check_weight CHECK (weight_kg > 0)

## Query Optimization

Use `EXPLAIN ANALYZE` : See how PostgreSQL executes your query (e.g., `EXPLAIN ANALYZE SELECT * FROM flights WHERE departure_time > '2025-03-30'`). This reveals if indexes are used or if a full table scan occurs

Avoid `SELECT *` : Only fetch needed columns to reduce data transfer

Replace subqueries with `JOIN`:

        Slow subquery:
        SELECT * FROM passengers WHERE flight_id IN (SELECT flight_id FROM flights WHERE status = 'delayed');

        Faster JOIN:
        SELECT passengers.*
        FROM passengers
        JOIN flights ON passengers.flight_id = flights.flight_id
        WHERE flights.status = 'delayed';

Normalize first: Spliy data into logical tables (e.g., separate airlines and flights tables)

Denormalize selectively: Add redundant columns for frequently joined data (e.g., include airline_name in flights to avoid joins)

### Key Takeaways

1. Index wisely: Focus on columns used in WHERE, JOIN, and ORDER BY.

2. Constraints = Data Quality: Prevent invalid data upfront.

3. Test with EXPLAIN: Always analyze query plans for bottlenecks.
