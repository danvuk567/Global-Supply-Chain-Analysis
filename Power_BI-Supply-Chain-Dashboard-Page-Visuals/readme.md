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

The Donut Chart represents 5 global markets with total *Sales* from *OrderItems* table by *Market* from *Orders* table.

![Power_BI_Supply_Chain_Dashboard_Page_Donut_Chart.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Donut_Chart.jpg?raw=true)

## Global Supply Chain Drill-Down Sales Clustered Bar Chart

This Clustered Bar Chart has the ability to drill down in total sales by deparment, category, and product by using *Department Name* from the *Department* table, *Category Name* from the *Category* table, *Product Name* from the *Product* table, and *Sales* from the *OrderItems* table. Top Selling **Department** in **2015** to **2017** is **Fan Shop**, 2nd is **Apparel**. Drilling down to **Category**, we have **Fishing** 1st and **Cleats** 2nd. And drilling down to Product, we have other specific products that are quite descriptive but unclear and product image URLs were not displaying anything.

![Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Dept.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Dept.jpg?raw=true)

Drilling down to **Category**, we have **Fishing** 1st and **Cleats** 2nd.

![Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Category.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Category.jpg?raw=true)

And drilling down to **Product**, we have other specific products that are quite descriptive but unclear and product image URLs were not displaying anything.

![Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Product.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Clustered_Bar_Chart_Product.jpg?raw=true)

## Global Supply Chain Sales by Country Map Visual

This map visual aggregates *Sales* from the *OrderItems* table by *Order Country* from the *Orders* table.

![Power_BI_Supply_Chain_Dashboard_Page_Map_Visual.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Map_Visual.jpg?raw=true)

The largest sales in **2015** and **2017** were in France**. Top **Department** sales were in **Fan Shop**. Drilling down in Fan Shop, top **Category** is **Fishing**, 2nd is **Camping & Hiking** in both years. 

![Power_BI_Supply_Chain_Dashboard_Page_2017.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_2017.jpg?raw=true)

## Global Supply Chain Stacked Bar Charts

The 1st Stacked Bar Chart aggregates *Sales* from *OrderItems* table by *Customer Segment* in the *Customers* table. We clearly see that most customers were consumers. The 2nd Stacked Bar Chart shows the transactions processed by status by applying the count of *Order Id* by *Transaction Status* from the *Orders* table. More transactions are in progress and not completed.

![Power_BI_Supply_Chain_Dashboard_Page_Stacked_Bar_Charts.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Stacked_Bar_Charts.jpg?raw=true)

## Global Supply Chain Clustered Column Chart

This Clustered Column Chart applies the count of *Order Id* by *Delivery Status* for each *Transaction Status* in the *Orders* table. The chart compares the total sales transactions processed by shipping status that were either cancelled, completed or in progress. The count of *Order Id*, *Delivery Status*, and *Transaction The goal was to see if the type of shipment status had any influence on getting transactions completed faster, slower or being cancelled. In observing the chart, late delivery did not cause cancellations and there were more transaction that were in progress than completed. Relatively speaking, the ratio of in progress to completed transactions for late deliveries as opposed to advance shipping or shipping on time were about the same.

![Power_BI_Supply_Chain_Dashboard_Page_Clustered_Column_Chart.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Power_BI_Supply_Chain_Dashboard_Page_Clustered_Column_Chart.jpg?raw=true)



