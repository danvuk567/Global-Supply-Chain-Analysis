# Power BI Supply Chain Dashboard Page Visuals

## Global Supply Slicers

The *Market* Slicer uses the *Market* field in the *Orders* table and the *Dates* Slicer uses the *Year*, *Quarter*, *Month No*, and *Date* in the *Calendar* table. All visuals on the page can be sliced by Markets and dates.

![Power_BI_Supply_Chain_Dashboard_Page_Slicers.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Slicers.jpg?raw=true)

## Global Supply Chain KPI Card Visuals

*Total Sales* is the sum of *Sales* in *OrderItems*, *Total Profit* is the sum of *Order Profit Per Order* in *OrderItems*, *Total Items Sold* is the sum of *Order Item Quantity* in *OrderItems*, *Total Transactions* is the count of *Order Id*, *Avg No. of Transactions per Day* uses the *AvgTransactions* Measure, *% of Late Deliveries* uses the *PercLateTransactions* Measure. 

![Power_BI_Supply_Chain_Dashboard_Page_KPI_Cards.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_KPI_Cards.jpg?raw=true)

When slicing data by Year, Total Sales, Total Profit and Total Items Sold has been on a slight decline from 2015 to 2017. 2.	Avg. Transactions per day went up in 2017. 3.	More than half of deliveries are late in all 3 years.

**2015**
![Power_BI_Supply_Chain_Dashboard_Page_KPI_Cards_2015.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_KPI_Cards_2015.jpg?raw=true)

**2017**
![Power_BI_Supply_Chain_Dashboard_Page_KPI_Cards_2017.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_KPI_Cards_2017.jpg?raw=true)

## Global Supply Chain Market Sales Donut Chart

The Donut Chart represents 5 global markets and uses *Sales* from *OrderItems* table and *Market* from *Orders* table.

![Power_BI_Supply_Chain_Dashboard_Page_Donut_Chart.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Donut_Chart.jpg?raw=true)

## Global Supply Chain Drill-Down Sales Clustered Bar Chart

This Clustered Bar Chart has the ability to drill down in sales by deparment, category, and product by using *Department Name* from the *Department* table, *Category Name* from the *Category* table, *Product Name* from the *Product* table, and *Sales* from the *OrderItems* table. Top Selling **Department** in **2015** to **2017** is **Fan Shop**, 2nd is **Apparel**. Drilling down to **Category**, we have **Fishing** 1st and **Cleats** 2nd. And drilling down to Product, we have other specific products that are quite descriptive but unclear and product image URLs were not displaying anything.

![Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Dept.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Dept.jpg?raw=true)

Drilling down to **Category**, we have **Fishing** 1st and **Cleats** 2nd.

![Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Category.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Category.jpg?raw=true)

And drilling down to **Product**, we have other specific products that are quite descriptive but unclear and product image URLs were not displaying anything.

![Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Product.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Product.jpg?raw=true)

## Global Supply Chain Sales by Country Map Visual

This map visual uses *Order Country* from the *Orders* table and *Sales* from the *OrderItems* table.

![Power_BI_Supply_Chain_Dashboard_Page_Map_Visual.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Map_Visual.jpg?raw=true)

The largest sales in **2015** and **2017** were in France**. Top **Department** sales were in **Fan Shop**. Drilling down in Fan Shop, top **Category** is **Fishing**, 2nd is **Camping & Hiking** in both years. 

![Power_BI_Supply_Chain_Dashboard_Page_2017.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_2017.jpg?raw=true)

## Global Supply Chain Stacked Bar Charts

The 1st Stacked Bar Chart aggregates *Sales*  from *OrderItems* table by *Customer Segment* in the *Customers* table. We clearly see that most customers were consumers. The 2nd Stacked Bar Chart shows the transactions processed by status using count of *Order Id* and *Transaction Status* from the *Orders* table. More transactions are in progress and not completed.

![Power_BI_Supply_Chain_Dashboard_Page_Stacked_Bar_Charts.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Stacked_Bar_Charts.jpg?raw=true)




