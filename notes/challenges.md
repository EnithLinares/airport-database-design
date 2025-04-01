Adjust Airline Table Schema

The operating_airline_iata_code column has a unique constraint. To allow duplicates for this column while maintaining uniqueness for the combination of all columns (operating_airline_name, operating_airline_iata_code, published_airline_name, published_airline_iata_code), modify the table schema.

    ALTER TABLE sfo_airport.airline
    DROP CONSTRAINT airline_operating_airline_iata_code_key;

    ALTER TABLE sfo_airport.airline
    ADD CONSTRAINT airline_unique_combination UNIQUE (
    operating_airline_name,
    operating_airline_iata_code,
    published_airline_name,
    published_airline_iata_code);

The first command removes the unique constraint on operating_airline_iata_code.

The second command adds a composite unique constraint across all four columns. This ensures that duplicate rows with identical values across all columns are rejected, but allows duplicates in individual columns.

Due to the lack of aircraft_id, I dropped the not null constrain
