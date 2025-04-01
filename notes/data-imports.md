To start the process of populating the tables I first made use of my lookout geo_region table in Google Sheets and import the unique values to my regions table to assigned them ids and region names.

This is achieved throught pgAdmin4 with this query

    COPY sfo_airport.geo_region(region_name)
    FROM '/path/to/geo_region_cleaned.csv'
    DELIMITER ','
    CSV HEADER;

This process is repeated for activity_type,

To ensure the cleaned passenger data can be inserted into the flight table without errors, I must validate that all text values in your staging table (flight_staging) match existing entries in the reference tables (geo_region, activity_type, airline, terminal, boarding_area).

Here are my vvalidation queries:

    SELECT DISTINCT geo_region
    FROM flight_staging
    WHERE geo_region NOT IN (SELECT region_name FROM sfo_airport.geo_region);

To make sure all regions already exist in my geo_region table

Validating activity types

    SELECT DISTINCT activity_type
    FROM flight_staging
    WHERE activity_type NOT IN (
        SELECT type_name
        FROM sfo_airport.activity_type
        );

Validate airline: Check if all airlines (operating and published) in the staging table match entries in the airline table:

    SELECT DISTINCT operating_airline, operating_airline_iata
    FROM flight_staging
    WHERE (operating_airline, operating_airline_iata) NOT IN (
        SELECT operating_airline_name, operating_airline_iata_code
        FROM sfo_airport.airline
    );

    SELECT DISTINCT published_airline, published_airline_iata
    FROM flight_staging
    WHERE (published_airline, published_airline_iata) NOT IN (
        SELECT published_airline_name, published_airline_iata_code
        FROM sfo_airport.airline
    );

Validating terminals

    SELECT DISTINCT terminal
    FROM flight_staging
    WHERE terminal NOT IN (
        SELECT terminal_name
        FROM sfo_airport.terminal
    );

Validating boarding_area

    SELECT DISTINCT boarding_area, terminal
    FROM flight_staging
    WHERE (boarding_area, terminal) NOT IN (
        SELECT ba.boarding_area_name, t.terminal_name
        FROM sfo_airport.boarding_area ba
        JOIN sfo_airport.terminal t ON ba.terminal_id = t.terminal_id
    );

Since all validation queries returned no rows, this means the cleaned passenger data in thr staging table (flight_staging) is consistent with the IDs in the reference tables (geo_region, activity_type, airline, terminal, and boarding_area). I then proceed to insert the data into the flight table using SQL JOINs to transform text values into their corresponding foreign keys.

    INSERT INTO sfo_airport.flight (
    activity_period,
    geo_summary,
    geo_region_id,
    activity_type_id,
    price_category,
    airline_id,
    terminal_id,
    boarding_area_id,
    aircraft_id,
    passenger_count

)

    SELECT
        fs.activity_period,
        fs.geo_summary,
        gr.region_id,          *Convert text "geo_region" to numeric ID*
        at.activity_type_id,    *Convert text "activity_type" to numeric ID*
        fs.price_category,
        a.airline_id,           *Convert airline name and IATA code to numeric ID*
        t.terminal_id,          *Convert terminal name to numeric ID*
        ba.boarding_area_id,    *Convert boarding area name and terminal ID to numeric ID*
        NULL,                   *Aircraft ID (set as NULL since it's nullable)*
        fs.passenger_count

    FROM
        flight_staging fs
    JOIN sfo_airport.geo_region gr ON gr.region_name = fs.geo_region
    JOIN sfo_airport.activity_type at ON at.type_name = fs.activity_type
    JOIN sfo_airport.airline a ON a.operating_airline_name = fs.operating_airline
                                AND a.operating_airline_iata_code = fs.operating_airline_iata
    JOIN sfo_airport.terminal t ON t.terminal_name = fs.terminal
    JOIN sfo_airport.boarding_area ba ON ba.boarding_area_name = fs.boarding_area
                                    AND ba.terminal_id = t.terminal_id;
