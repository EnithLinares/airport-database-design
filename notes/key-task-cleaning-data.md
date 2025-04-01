Key Tasks:

Activity Period:

Ensure values are in YYYYMM format (e.g., 202501 for January 2025).

In Google Sheets create a column called valid period and use the following formula

        =IF(AND(LEN(A2)=6, ISNUMBER(A2)), "Valid", "Invalid")

Copy the formula to all the cells in the valid period column, create a filter and check for any invalid entries. Delete empty rows and reformat if needed.

Passenger Count

We repeat the process with passanger count, but this time checking for numerical values and making sure there's no negative values with the following formula

          =IF(AND(ISNUMBER(L2), L2 >= 0), "Valid", "Invalid")

Normalizing Geographic Regions:

We create a lookout table for valid geo regions by selecting the column, go to data > data cleanup > remove duplicates - This process leaves us with the unique values of the columns.
In a seperate sheet, a new table with two columns, original value and standardized value
The only values that need to be find and replace are US with United States and Australia / Oceania with Oceania.

This get achieved with Edit > Find and Replace > Match Case

Geo Summary

The summaries are divided by Domestic and International - to validate this data we create a column called valid geo summary and applied the following formula to the cells in the new column

        =IF(OR(F2="Domestic", F2="International"), "Valid", "Invalid")

Then we create a filter to check for invalid data and fix the issues if needed.

Price category

This follows the same process as geo summary, but this time checking for Low Fare or Other with this formula

    =IF(OR(I2="Low Fare", I2="Other"), "Valid", "Invalid")

Airline related columns

Uniqueness had to be dropped on iata codes as some airlines operate under the same operating name with a different published name and code, instead a combination constrained had to be enacted.

The airport table was populated manually as followed

        INSERT INTO sfo_airport.airport (airport_name, icao_code, iata_code)
        VALUES ('San Francisco International Airport', 'KSFO', 'SFO');

The terminals were also done manually by removind the duplicates and adding their relation to the airpor

        INSERT INTO sfo_airport.terminal (terminal_name, airport_id)
        VALUES
        ('Terminal 1', 1),
        ('Terminal 2', 1),
        ('Terminal 3', 1),
        ('International', 1),
        ('Other', 1);
