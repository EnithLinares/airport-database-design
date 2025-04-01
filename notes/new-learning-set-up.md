First, we make sure the database and connected, certifing the user by running the command:

        SELECT current_user, current_database();

current_database is a function of postgreSQL, so it needs to be invoque as such.

Pending on what type of work, roles might need to assignes.

Create Schema
CREATE SCHEMA sfo_airport;

Create Tables

The syntax for tables is CREATE TABLE sfo_airport.name_of_table

Instead of Ids we are going to be using SERIAL and PRIMARY KEY designations

Relations are set up with REFERENCES with sfo_airpoty.name_of_table(column_being_referenced)

UNIQUE constrains are being set up to create a unique index on the specified columns to enforce the constraint efficiently.

NOT NULL prevents empty values

FLOAT CHECK (eg.freight_weight_kg >= 0), checks for decimals and makes sure there's no negative values

PostgreSQL checks for duplicate values in the constrained column(s). If duplicates are found, the operation fails with an error
