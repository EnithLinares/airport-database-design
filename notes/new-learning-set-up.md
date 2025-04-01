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

The implemented indexes are as follow:

-   Index on flight activity_period for faster lookups

          CREATE INDEX idx_flight_activity_period ON sfo_airport.flight(activity_period);

    This index is designed to speed up queries that filter or search based on the _activity_period_ column in the _flight table_. For example, queries like _SELECT _ FROM flight WHERE activity_period = '2025-04'\*; will benefit from this index

-   Index on airline operating IATA code for faster searches

          CREATE INDEX idx_operating_iata_code ON sfo_airport.airline(operating_airline_iata_code);

    This index facilitates faster lookups on the _operating_airline_iata_code_ column in the _airline table_. Queries like _SELECT _ FROM airline WHERE operating_airline_iata_code = 'UA'\*; will be optimized.a

-   Index on passenger passport number for unique lookups

          CREATE INDEX idx_passenger_passport ON sfo_airport.passenger(passport_number);

    This index is intended for unique lookups of passengers based on their passport numbers. Queries like _SELECT _ FROM passenger WHERE passport_number = 'A12345678'\*; will benefit significantly.
