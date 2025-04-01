Key Tasks:

Activity Period:

-   Ensure values are in YYYYMM format (e.g., 202501 for January 2025).

-   Split into year and month columns if needed:

          ALTER TABLE flight ADD COLUMN year INT, ADD COLUMN month INT;
          UPDATE flight
          SET year = LEFT(activity_period::TEXT, 4)::INT,
          month = RIGHT(activity_period::TEXT, 2)::INT;

Airline Codes:

-   Remove duplicates in operating_airline_iata_code and published_airline_iata_code (Excel: Data → Remove Duplicates).

-   Validate IATA codes are 2 characters (Excel formula: =LEN(A2)=2).

GEO Regions/Activity Types:

-   Use Excel’s Data Validation to enforce allowed values (e.g., GEO Summary = "Domestic" or "International").

-   Create lookup tables in PostgreSQL for consistency:

        CREATE TABLE geo_region (region_id SERIAL PRIMARY KEY, region_name VARCHAR(50) UNIQUE);
        INSERT INTO geo_region (region_name) VALUES ('US'), ('Asia'), ('Europe');

Cargo Weight:

-   Convert Cargo Weight LBS to kilograms if needed (1 lb = 0.453592 kg).

-   Ensure non-negative values with PostgreSQL constraints:

        ALTER TABLE cargo ADD CHECK (freight_weight_kg >= 0);
