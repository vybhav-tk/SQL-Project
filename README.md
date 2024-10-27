# Real Estate Market Analysis using Snowflake and SQL

The goal of the Project is to provide a detailed analysis of the Polish real estate market by collecting, transforming, and analysing data from the otodom.pl website. The key objectives are:

1. **Scrape Property Listings Data**: Extract real estate data from the otodom.pl website, including property prices, number of rooms, property types, locations, descriptions, and more.
2. **Transform Data for Analysis**: Clean the data by removing unnecessary elements such as currency symbols from prices and unit labels from surface area values.
3. **Store and Manage the Data in Snowflake**: Load the scraped and transformed data into Snowflake (a cloud-based data warehouse) to ensure it is structured and ready for analysis.
4. **Analyse the Real Estate Market**:
   - Generate insights about the Polish property market by answering key questions related to rent, property sizes, pricing trends, and other metrics in major cities.
   - Identify patterns and trends, such as average rental prices, most expensive areas, and differences in property prices across various suburbs and cities.
5. **Build Reports and Visualizations**:
   - Use SQL queries and Snowflake’s built-in dashboard capabilities to create meaningful reports and visualizations, providing insights into the real estate market.
   - Produce clear and actionable information for prospective renters, buyers, or real estate investors who are interested in the Polish property market.
	


## Data Scraping and Loading into Snowflake

1. **Using Bright Data for Scraping**:
   - We used Bright Data, a powerful web scraping tool, to extract real estate data from otodom.pl, a Polish property listing site.
   - The scraped data was obtained in JSON format as a semi-structured format for easy data handling and export.
   - The extracted data contained various fields such as price, number of rooms, surface area, coordinates (latitude and longitude), description (in Polish), property type, and more.
2. **Loading Data into Snowflake**:
   - After scraping, the data was loaded into Snowflake—a cloud-based data warehouse using SnowSQL. SnowSQL is a command line interface for Snowflake
   - In its raw form, the JSON data was stored in a staging table.
3. **Flattening the JSON Data**:
   - The JSON structure was nested and had multiple levels of data. To make it queryable, we flattened the JSON using Snowflake’s built-in JSON functions such as FLATTEN(), which allows the extraction of key-value pairs from JSON arrays.
   - The flattened data was stored in a new table called otodom_data_flattened. This table contained fields such as:
     * advertiser_type
     * balcony_garden_terrace
     * description
     * form_of_property
     * heating
     * is_for_sale
     * lighting
     * location
     * no_of_rooms
     * parking_space
     * price
     * remote_support
     * rent_sale
     * surface
     * timestamp
     * title
     * url


## Data Analysis and Reporting

1. **Data pre processing**:
   We performed the following transformations on the data:
   - Price transformation: Removed the currency symbols (e.g., PLN, EUR) from the price and converted the price to a numeric field.
   - Surface area transformation: Removed the "m²" unit from the surface field to store it as a numeric value.
   - Address extraction: Parsed the JSON address data to extract the city, suburb, and country into separate columns.

2. **Creating an Apartment Flag**:
   - Not all the listings were apartments. Some listings were for other types of properties, such as commercial premises, garages, etc.
   - We used keywords like "commercial," "office," "shop," and so on to flag non-apartment listings. Additionally, we created logic based on price and surface area to further differentiate apartments from non-apartments.
   - This generated a new column called apartment_flag to distinguish between apartments and non-apartments.

3. **Final Transformed Table**:
   The fully transformed data was loaded into a new table called otodom_data_transformed. This table contained:
   - Cleaned price and surface fields.
   - Full addresses (suburb, city, country).
   - Translated English titles.
   - Apartment flag.

This table served as the foundation for building all further reports and answering key property market questions.

## Report Generation:

We generated several reports to answer real estate-related questions about the Polish property market. Some of the key questions addressed include:

1. Average Rent for Different Room Types Across Major Cities:
   - We wanted to find the average rental price for 1-room, 2-room, 3-room, and 4-room apartments in major cities like Warsaw, Krakow, Gdansk, and others.
   - We used SQL to:
     * Filter data for the major cities.
     * Filter for rental ads (is_for_sale = false).
     * Filter for apartments (apartment_flag = 'apartment').
     * Calculate the average rental price for each room type (1, 2, 3, 4 rooms).
   - We used Snowflake's pivot function to transform the result set into a more readable format with room types as separate columns.
   - Finally, we visualized the data using Snowflake’s built-in charting tool, creating a bar chart to display the average rent by city and room type.

2. Affordability Analysis:
   - We analyzed which suburbs in Warsaw had properties within a specific price range (e.g., 800,000 PLN to 1,000,000 PLN) and a specific apartment size (e.g., 90 to 100 m²).
   - This query helped identify affordable areas based on predefined criteria.
   
3. Size of Apartments for Given Rent in Major Cities:
   - For renters with a budget of 3,000 PLN to 4,000 PLN, we examined what size of an apartment they could expect in various major cities.
   -  This analysis highlighted the differences in rent-to-size ratios across different Polish cities, revealing that Warsaw was the most expensive city in terms of rental cost per square meter.
   
4. Identifying the Most Expensive Apartments:
   - We queried the data to find the most expensive apartment listings in Poland, showing that Warsaw and Krakow had some of the priciest listings in the country.
   - The listings were sorted by price, and we even cross-referenced the URLs of these listings to check if they were still active.
   
5. Agency vs. Private Listings:
   - We also examined the percentage of property ads listed by real estate agencies versus private owners. This analysis revealed that around 91% of ads were from agencies, meaning that most rental transactions involve paying agency fees.


## Final Output:
The final part of the project involved answering 11 key real estate questions using SQL queries, which were stored and shared via Snowflake. The outputs were used to generate insights about the Polish property market, such as average rents, size of apartments available, most expensive listings, and more.

This overview brings together all the key steps and insights from the otodom Data Analysis Project. By following these steps, anyone can replicate this project, adapt it to other regions or property markets, and gain deep insights using the power of modern data analysis tools.

