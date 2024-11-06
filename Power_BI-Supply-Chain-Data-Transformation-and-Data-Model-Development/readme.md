# Power BI Supply Chain Data Transformation and Data Model Development

## Data Cleaning

The first step was to import and clean the *Sheet1* data source in the [SuperStore Data Set Excel file](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/Data-Source-Files/SuperStore%20Sales%20DataSet.xlsx) using **Power Query**. All columns were checked for errors, missing values, duplicates. I used the *Column quality*, *Column distribution* and *Column profile* to identify where there might be missing values, discrepancies, errors and possible duplicates. The *Returns* column had errors which were replaced with 0's and data type changed to *Whole Number*. The last 2 columns *ind1* and *ind2* had null values and were removed. The *Sheet1* data source is disabled from being loaded. 

## Data Mapping

The next approach was to identify how the data in *Sheet1* could be normalized to separate tables. Generally, we see that we have orders, customers, products, locations and sales. Customers, products and locations are essentially unique entities and can be extracted from the data as 3 separate Dimension tables. However the *Order ID* was not unique and *Order Date*, *Ship Date*, *Product ID* and *Sales* all had different variations which essentially dictated that orders could not be separated from sales. The *Row ID+O6G3A1:R6* column is also not unique and connot be used as a unique Order key or Sales key. This observation was made by using the *Group By* feature in Power Query to count > 1 for any combination of these columns. The following **Data Mapping document** was then used to build the initial data model for the valid columns:

[SuperStore Data Mapping file](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/Power_BI-Sales-Data-Transformation-and-Data-Model-Development/SuperStore%20Data%20Mapping.xlsx)

![SuperStore Data Mapping](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/images/SuperStore_Data_Mapping.jpg?raw=true)

## Data Transformation Steps

The *Region* table is extracted in Power Query by referencing *Sheet1* and keeping the *Country*, *City*, *State* and *Region* columns. Removed row duplicates and added a unique index column named *Region ID*.

![Region Table Transformation.jpg](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/images/Region_table_transformation.jpg?raw=true)

The *Product* table is extracted in Power Query by referencing *Sheet1* and keeping the *Product ID*, *Category*, *Sub-Category* and *Product Name* columns. Removed row duplicates and found *Product ID* duplicates by referencing the *Product* table, doing a group by *Product ID* and checking for count > 1. Found 28 duplicates of *Product ID* which were removed from the *Product* table.

![Product Table Transformation.jpg](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/images/Product_table_transformation.jpg?raw=true)

The *Customers* table is extracted in Power Query by referencing *Sheet1* and keeping the *Customer ID*, *Customer Name* and *Segment* columns. Removed row duplicates and checked that there are no duplicates by referencing the *Customers* table, doing a group by Customer ID and checking for count > 1. 

![Customers Table Transformation.jpg](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/images/Customers_table_transformation.jpg?raw=true)

The *Sales* table is extracted in Power Query by referencing *Sheet1* and removing the *Customer Name*, *Segment*, *Category*, *Sub-Category* and *Product Name* columns. Merged with the *Region* table by *Country*, *City*, *State* and *Region*. Added *Region ID* from *Region* table and removed the *Country*, *City*, *State* and *Region* columns. Added a unique index column named 'Sales ID'.

![Sales Table Transformation.jpg](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/images/Sales_table_transformation.jpg?raw=true)

# DAX Calculated Date Tables

For dates, I created the **calculated table** called *Calendar* using **DAX** using the min and max dates from *Order Date* in the *Sales* table. We define the columns *Date*, *Year*, *Quarter*, *Month No* and *Day* with the following DAX code:

    Calendar = 
      VAR StartDate = MIN(Sales[Order Date])
      VAR EndDate = MAX(Sales[Order Date])

    RETURN
        ADDCOLUMNS(
            CALENDAR(StartDate, EndDate),
            "Year", YEAR([Date]),
            "Quarter", QUARTER([Date]),
            "Month No", MONTH([Date]),
            "Day", DAY([Date])
        )

To provide Month names in visuals that are sorted by *Month No*, I created a *Data table* called *MonthCal* using the following DAX code:

    MonthCal = DATATABLE(
        "Month Long", STRING,
        "Month Short", STRING,
        "Month No", INTEGER,
        {
            {"January", "Jan", 1},
            {"February", "Feb", 2},
            {"March", "Mar", 3},
            {"April", "Apr", 4},
            {"May", "May", 5},
            {"June", "Jun", 6},
            {"July", "Jul", 7},
            {"August", "Aug", 8},
            {"September", "Sep", 9},
            {"October", "Oct", 10},
            {"November", "Nov", 11},
            {"December", "Dec", 12}
        }
    )

One of the **KPIs** in Dashboard looks at average delivery days. To formulate that, I create a calculated column using DAX called *DaystoDelivery* in the *Sales* table by subtracting the number of days between *Order Date* and *Ship Date*.

        DaystoDelivery = DATEDIFF(Sales[Order Date],Sales[Ship Date], DAY)

# Final Data Model 

Lastly, I created relationships a **one-to-many relationship** between *Product_ID* in the *Product* table to *Product_ID* in the *Sales* table, *Region_ID* in the *Region* table to *Region_ID* in the *Sales* table, *Customer_ID* in the *Customers* table to *Customer_ID* in the *Sales* table, and *Date* in the *Calendar* table to *Order Date* in the *Sales* table. A one-to-many relationship is also created between *Month No* in the *CalMonth* table to *Month No* in the *Calendar* table.

![Power_BI_Sales_Data_Model_Relationships.jpg](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/images/Power_BI_Sales_Data_Model_Relationships.jpg?raw=true)

And our final **Star Schema** Data Model now looks like this:

![Power_BI_Final_Sales_Data_Model.jpg](https://github.com/danvuk567/Predictive-Sales-Forecasting/blob/main/images/Power_BI_Final_Sales_Data_Model.jpg?raw=true)
