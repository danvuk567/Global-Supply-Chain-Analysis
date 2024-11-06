# Power BI Supply Chain Data Transformation and Data Model Development

## Data Cleaning

The first step was to import and clean the *DataCoSupplyChainDataset* data source: [Global Supply Chain csv file](https://data.mendeley.com/datasets/8gx2fvg2k6/5/files/72784be5-36d3-44fe-b75d-0edbf1999f65/DataCoSupplyChainDataset.csv) using **Power Query**. All columns were checked for errors, missing values, duplicates. I used the *Column quality*, *Column distribution* and *Column profile* to identify where there might be missing values, discrepancies, errors and possible duplicates. Most importantly, I used the **data exploration** and **data cleaning** steps I executed in Python as a reference. 

1. All columns that were identified in Python that were not needed were removed except for the following columns:

   *Type*, *Days for shipping (real)*, *Days for shipment (scheduled)*, *Delivery Status*, *Late_delivery_risk*, *Category Id*, *Category Name*, *Customer City*, *Customer Country*, *Customer Fname*, *Customer Id*, 
   *Customer Lname*, *Customer Segment*, *Customer State*, *Customer Street*, *Customer Zipcode*, *Department Id*, *Department Name*, *Market*, *Order City*, *Order Country*, *order date (DateOrders)*, *Order Id*,
   *Order Item Discount*, *Order Item Discount Rate*, *Order Item Id*, *Order Item Profit Ratio*, *Order Item Quantity*, *Sales*, *Order Item Total*, *Order Profit Per Order*, *Order Region*, *Order State*,
   *Order Status*, *Product Card Id*, *Product Image*, *Product Name*, *Product Price*, *shipping date (DateOrders)* and *Shipping Mode*.

3. Changed data types were needed.

4. Nulls in *Customer Lname* column were replaced with blanks.

5. Created a custom column *Customer Name* as follows:

   ![Power_Query_Customer_Name.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Customer_Name.jpg?raw=true)

   Changed data type to Text, and removed *Customer Fname* and *Customer Lname*.

6. Create a custom conditional column *Zipcode* as follows:

   ![Power_Query_Zipcode.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Zipcode.jpg?raw=true)

   Changed data type to Whole Number, Removed *Customer Zipcode*, and renamed *Zipcode* to *Customer Zipcode*.

7. Create a custom conditional column *State* as follows:

   ![Power_Query_State.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_State.jpg?raw=true)

   Changed data type to Text, removed *Customer State*, and renamed *State* to *Customer State*.

8. Create a custom conditional column *City* as follows:

   ![Power_Query_City.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_City.jpg?raw=true)

   Changed data type to Text, removed *Customer City*, and renamed *City* to *Customer City*.

9. Create a custom conditional column *Street* as follows:

   ![Power_Query_Street.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Street.jpg?raw=true)

   Changed data type to Text, removed *Customer Street*, and renamed *Street* to *Customer Street*.

10. Split Column *order date (DateOrders)* by Delimeter using space into 2 columns with data types as Date and Time. Renamed columns to *Order Date* and *Order Time*.
   
11. Split Column *shipping date (DateOrders)* by Delimeter using space into 2 columns with data types as Date and Time. Renamed columns to *Shipping Date* and *Shipping Time*.


## Data Mapping

The next approach was to identify how the data in *DataCoSupplyChainDataset* could be normalized to separate tables. We used date exploration and transformation steps in Python to develop a **Data Mapping document**. The 2nd tab of the following excel sheet was then used to build the initial data model for the valid columns:

[Global Supply Chain Data Mapping file](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/Files/Global%20Supply%20Chain%20Data%20Mapping.xlsx)

![Global_Supply_Chain_Data_Mapping.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Global_Supply_Chain_Data_Mapping.jpg?raw=true)

## Data Transformation Steps

The *Orders* table is extracted in Power Query by referencing *DataCoSupplyChainDataset* and keeping the *Type*, *Days for shipping (real)*, *Days for shipment (scheduled)*, *Delivery Status*, *Late_delivery_risk*, *Market*, *Order City*, *Order Country*, *Order Date*, *Order Time*, *Order Id*, *Order Region*, *Order State*, *Order Status*, *Shipping Date*, *Shipping Time*, and *Shipping Mode*. Renamed *Days for shipping (real)* to *Actual Shipment Days*, *Days for shipment (scheduled)* to *Scheduled Shipment Days*, and *Late_delivery_risk* to Late Delivery Risk*. For simplicity sake, another custom column that groups *Order Status* into 3 states is created as follows:

  ![Power_Query_Transaction_Status.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Transaction_Status.jpg?raw=true)

After removing row duplicates, the *Orders* table has 19 columns:

  ![Power_Query_Orders.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Orders.jpg?raw=true)

The *Order Items* table is extracted in Power Query by referencing *DataCoSupplyChainDataset* and keeping the *Order Id*, *Order Item Discount*, *Order Item Discount Rate*, *Order Item Id*, *Order Item Profit Ratio*, *Order Item Quantity*, *Sales*, *Order Item Total*, *Order Profit Per Order*, and *Product Card Id*.

After removing row duplicates, the *Order Items* table has 10 columns:

  ![Power_Query_Order_Items.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Order_Items.jpg?raw=true)

The *Customers* table is extracted in Power Query by referencing *DataCoSupplyChainDataset* and keeping the *Customer City*, *Customer Country*, *Customer Name*, *Customer Id*, *Customer Segment*, *Customer State*, *Customer Street*, and *Customer Zipcode*.

After removing row duplicates, the *Customers* table has 8 columns:

  ![Power_Query_Customers.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Customers.jpg?raw=true)

The *Department* table is extracted in Power Query by referencing *DataCoSupplyChainDataset* and keeping the *Department Id*, and *Department Name*.

After removing row duplicates, the *Department* table has 2 columns:

  ![Power_Query_Department.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Department.jpg?raw=true)

The *Category* table is extracted in Power Query by referencing *DataCoSupplyChainDataset* and keeping the *Category Id*, *Category Name*, and *Department Id*.

After removing row duplicates, the *Category* table has 3 columns:

  ![Power_Query_Category.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Category.jpg?raw=true)

The *Product* table is extracted in Power Query by referencing *DataCoSupplyChainDataset* and keeping the *Category Id*, *Product Card Id*, *Product Image*, *Product Name*, and *Product Price*.

After removing row duplicates, the *Product* table has 5 columns:

  ![Power_Query_Product.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_Query_Product.jpg?raw=true)

  

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
