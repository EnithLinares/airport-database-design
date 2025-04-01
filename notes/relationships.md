# Relationships

1. ## Airport

    ### Attributes:

    - airport_id: Primary key, unique identifier for each airport.
    - airport_name: Name of the airport.
    - icao_code: International Civil Aviation Organization (ICAO) code for the airport.
    - iata_code: International Air Transport Association (IATA) code for the airport.

    ### Relationships:

    - Linked to Terminals (One-to-Many).
    - Linked indirectly to Boarding Areas through Terminals.

2. ## Terminal

    ### Attributes:

    - terminal_id: Primary key, unique identifier for each terminal.
    - terminal_name: Name of the terminal.
    - airport_id: Foreign key referencing airport.airport_id.

    ### Relationships:

    - Linked to Airports (Many-to-One).
    - Linked to Boarding Areas (One-to-Many).
    - Linked to Flights (One-to-Many).

3. ## Boarding Area

    ### Attributes:

    - boarding_area_id: Primary key, unique identifier for each boarding area.
    - boarding_area_name: Name of the boarding area.
    - terminal_id: Foreign key referencing terminal.terminal_id.

    ### Relationships:

    - Linked to Terminals (Many-to-One).
    - Linked to Flights (One-to-Many).

4. ## Airline

    ### Attributes:

    - airline_id: Primary key, unique identifier for each airline.
    - operating_airline_name: Name of the operating airline.
    - operating_airline_iata_code: IATA code for the operating airline, must be unique.
    - published_airline_name: Name of the published airline (if different from operating airline).
    - published_airline_iata_code: IATA code for the published airline.

    ### Relationships:

    - Linked to Flights (One-to-Many).

5. ## Geo Region

    ### Attributes:

    - region_id: Primary key, unique identifier for each geographic region.
    - region_name: Name of the geographic region (e.g., Asia, Europe).

    ### Relationships:

    - Linked to Flights (One-to-Many).

6. ## Activity Type

    ### Attributes:

    - activity_type_id: Primary key, unique identifier for each activity type.
    - type_name: Name of the activity type (e.g., Enplaned, Deplaned, Intransit).

    Relationships:

    - Linked to Flights (One-to-Many).

7. ## Flight

    ### Attributes:

    - flight_id: Primary key, unique identifier for each flight.
    - activity_period: Integer representing year and month in YYYYMM format (e.g., "202307" for July 2023).
    - geo_summary: Indicates whether the flight is Domestic or International.
    - geo_region_id: Foreign key referencing geo_region.region_id.
    - activity_type_id: Foreign key referencing activity_type.activity_type_id.
    - price_category: Indicates fare type, either Low Fare or Other.
    - airline_id: Foreign key referencing airline.airline_id.
    - terminal_id: Foreign key referencing terminal.terminal_id.
    - boarding_area_id: Foreign key referencing boarding_area.boarding_area_id.
    - aircraft_id: Foreign key referencing aircraft.aircraft_id.

    ### Relationships:

    - Linked to Airlines, Terminals, and Boarding Areas (Many-to-One).
    - Linked to Cargo, Passengers, and their respective statistics.

8. ## Cargo

    ### Attributes:

    - cargo_id: Primary key, unique identifier for each cargo record.
    - flight_id: Foreign key referencing flight.flight_id.
    - freight_weight_kg: Weight of freight in kilograms, must be non-negative.
    - mail_weight_kg: Weight of mail in kilograms, must be non-negative.

    ### Relationships:

    - Linked to a specific flight (One-to-One).

9. ## Passenger

    ### Attributes:

    - passenger_id: Primary key, unique identifier for each passenger.
    - first_name, last_name: Passenger's name details.
    - email: Email address, must be unique per passenger.
    - phone_number: Phone number of the passenger.
    - passport_number: Unique passport number per passenger.

    ### Relationships:

    Linked to flights via a many-to-many relationship through the Passenger_Flight table.

10. ## Passenger_Flight

    ### Attributes:

    - Passenger_Flight_ID: Unique primary ID per record.
    - Passenger_ID references Passenger
