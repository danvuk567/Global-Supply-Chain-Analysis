# Python Data Exploration

## Exploratory Data Analysis, Data Cleaning and Relational Data Modelling: *[data_exploration_analysis.ipynb](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/Python-Data-Exploration/data_exploration_analysis.ipynb)*

The 1st step is to load the csv file and take a look at the columns. There are 53 columns so we can split it up into 4 dataframes in order to explore the data for all columns in seperate chunks.

    f_path = r'C:\Users\Daniel\Documents\Projects\Global Supply Chain Analysis\DataCoSupplyChainDataset.csv'
    df_sup = pd.read_csv(f_path, encoding='ISO-8859-1')

![Python_df_sup_columns.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup_columns.jpg?raw=true)

    split_ind1 = df_sup.columns.get_loc("Category Id")
    df_sup1 = df_sup.iloc[:, :split_ind1] 
    df_sup2 = df_sup.iloc[:, split_ind1:]

    split_ind2 = df_sup2.columns.get_loc("Market")
    df_sup3 = df_sup2.iloc[:, :split_ind2] 
    df_sup4 = df_sup2.iloc[:, split_ind2:]
    df_sup2 = df_sup3
    df_sup3 = df_sup4

    split_ind3 = df_sup3.columns.get_loc("Product Card Id")
    df_sup4 = df_sup3.iloc[:, :split_ind3]
    df_sup5 = df_sup3.iloc[:, split_ind3:]
    df_sup3 = df_sup4
    df_sup4 = df_sup5
    del df_sup5

![Python_df_sup1_info1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup1_info1.jpg?raw=true)
![Python_df_sup1_info2.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup1_info2.jpg?raw=true)

For *df_sup1* dataframe, there are no nulls and there are more than 1 unique values per column. 

![Python_df_sup2_info1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup2_info1.jpg?raw=true)
![Python_df_sup2_info2.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup2_info2.jpg?raw=true)
![Python_df_sup2_info3.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup2_info3.jpg?raw=true)
![Python_df_sup2_info4.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup2_info4.jpg?raw=true)

In *df_sup2*, *Customer Lname* and *Customer Zipcode* have some null values. We will fill *Customer Lname* with blanks and combine *Customer Fname* and *Customer Lname* into one column called *Customer Name*. *Customer Fname* and *Customer Lname* are then dropped from *df_sup*.

    df_sup['Customer Lname'] = df_sup['Customer Lname'].fillna('')
    df_sup['Customer Name'] = df_sup['Customer Fname'] + ' ' + df_sup['Customer Lname']
    df_sup['Customer Name'] = df_sup['Customer Name'].str.strip()
    df_sup.drop(columns=['Customer Fname','Customer Lname'], inplace=True)

In looking at the nulls for *Customer Zipcode*, it appears that the Zipcode is in *Customer State*, the State is in *Customer City* and the City is in *Customer Street*. The Zipcodes appear to be valid as there are 10 unique Customer Streets with the same Zipcode.

![Python_df_sup2_info5.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup2_info5.jpg?raw=true)

Let's check which Customer columns have values in other columns. 

    df_loc = df_sup[['Customer City', 'Customer Country', 'Customer State', 'Customer Street', 'Customer Zipcode']].copy()
    df_loc['Customer City'] = df_loc['Customer City'].str.strip()
    df_loc['Customer Country'] = df_loc['Customer Country'].str.strip()
    df_loc['Customer State'] = df_loc['Customer State'].str.strip()
    df_loc['Customer Street'] = df_loc['Customer Street'].str.strip()
    df_loc['Customer Zipcode'] = pd.to_numeric(df_loc['Customer Zipcode'], errors='coerce').fillna(0).astype(int).astype(str)

    matches_dict = {}

    # Get the columns of the DataFrame
    columns = df_loc.columns

    # Iterate over pairs of columns
    for i in range(len(columns)):
        for j in range(i + 1, len(columns)):
            col1 = columns[i]
            col2 = columns[j]

            # Find the unique values in each column
            values_col1 = df_loc[col1].unique()
            values_col2 = df_loc[col2].unique()

            # Check if any value in col1 exists in col2
            matching_values = set(values_col1) & set(values_col2)  # Intersection

            # If there are matching values, add them to the dictionary
            if matching_values:
                match_entry = (col1, col2)
                if match_entry not in matches_dict:
                    matches_dict[match_entry] = list(matching_values)

    # Print the matches found
    if matches_dict:
        print("Matches found:")
        for key, values in matches_dict.items():
            print(f"{key}: {values}")
    else:
        print("No matches found.")

![Python_df_sup2_info6.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup2_info6.jpg?raw=true)

Now we'll replace the correct values in the correct corresponding columns. We'll leave the Street Names as 'Undefined' since there does't appear to be any unique Street Names for those locations.

    df_sup.loc[df_sup['Customer State'].isin(['91732', '95758']), 'Customer Zipcode'] = df_sup.loc[df_sup['Customer State'].isin(['91732', '95758']), 'Customer State'].astype(float)
    df_sup.loc[df_sup['Customer State'].isin(['91732', '95758']), 'Customer State'] = df_sup.loc[df_sup['Customer State'].isin(['91732', '95758']), 'Customer City']
    df_sup.loc[df_sup['Customer Street'].isin(['Elk Grove', 'El Monte']), 'Customer City'] = df_sup.loc[df_sup['Customer Street'].isin(['Elk Grove', 'El Monte']), 'Customer Street']
    df_sup.loc[df_sup['Customer Street'].isin(['Elk Grove', 'El Monte']), 'Customer Street'] = 'Undefined'

![Python_df_sup2_info7.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup2_info7.jpg?raw=true)

*Customer email* and *Customer Password* has 1 unique value of *XXXXXXXXX*. This most likely the result of a security mask. We can drop these 2 columns from *df_sup*.

    df_sup.drop(columns=['Customer Email', 'Customer Password'], inplace=True)

![Python_df_sup3_info1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup3_info1.jpg?raw=true)
![Python_df_sup3_info2.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup3_info2.jpg?raw=true)
![Python_df_sup3_info3.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup3_info3.jpg?raw=true)
![Python_df_sup3_info4.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup3_info4.jpg?raw=true)

*Order Zipcode* has too many nulls. Let's drop this column from *df_sup*.

        df_sup.drop(columns=['Order Zipcode'], inplace=True)

![Python_df_sup4_info1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup4_info1.jpg?raw=true)
![Python_df_sup4_info2.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup4_info2.jpg?raw=true)
![Python_df_sup4_info3.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup4_info3.jpg?raw=true)

*Product Description* is null. *Product Status’ only has one value of 0. Drop both columns from *df_sup*.

        df_sup.drop(columns=['Product Description', 'Product Status'], inplace=True)

Let's do a check for columns that have all the same values.

        matches_dict = {}

        # Get the columns of the DataFrame
        columns = df_sup.columns

        # Iterate over pairs of columns
        for i in range(len(columns)):
            for j in range(i + 1, len(columns)):
                col1 = columns[i]
                col2 = columns[j]
        
                # Check if the columns match for all rows
                if (df_sup[col1] == df_sup[col2]).all():
                    # Create a tuple
                    match_tuple = (col1, col2)
            
                    # Ensure that reverse tuples are not added
                    if (col2, col1) not in matches_dict.keys():
                        matches_dict[match_tuple] = True  # Store the match as True or any value

        # Print the matches found
        if matches_dict:
            print("Matches found:", matches_dict)
        else:
            print("No matches found.")

![Python_df_sup_duplicates.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup_duplicates.jpg?raw=true)

        df_sup.drop(columns=['Benefit per order', 'Sales per customer', 'Product Category Id', 'Order Customer Id', 'Order Item Product Price', 'Order Item Cardprod Id'], inplace=True)

We can also split up the dates for orders and shipping into Date format and Time format in case we want to work with date and time format separately.

        df_sup['order date (DateOrders)'] = pd.to_datetime(df_sup['order date (DateOrders)'])
        df_sup['Order Date'] = df_sup['order date (DateOrders)'].dt.date
        df_sup['Order Time'] = df_sup['order date (DateOrders)'].dt.time
        
        df_sup['shipping date (DateOrders)'] = pd.to_datetime(df_sup['shipping date (DateOrders)'])
        df_sup['Shipping Date'] = df_sup['shipping date (DateOrders)'].dt.date
        df_sup['Shipping Time'] = df_sup['shipping date (DateOrders)'].dt.time

        df_sup.drop(columns=['order date (DateOrders)', 'shipping date (DateOrders)'], inplace=True)

Now, let's look at how we can build a relational data model based on min and max unique values as a type of normalization technique. For orders, let's look at the 1st set of columns to see which ones are unique to *Order Id* so they can split out into separate tables.

        df_ord1 = df_sup[['Type', 'Days for shipping (real)', 'Days for shipment (scheduled)',
               'Delivery Status', 'Late_delivery_risk', 'Category Id', 'Category Name', 'Customer Name','Customer City', 
               'Customer Country', 'Customer Id', 'Customer Segment', 'Customer State',
               'Customer Street', 'Customer Zipcode','Order Id']]

        ord1_unique_counts = df_ord1.groupby('Order Id').agg(lambda x: x.nunique()).reset_index()
        ord1_min_max_values = ord1_unique_counts.agg(['min', 'max']).reset_index()
        print(ord1_min_max_values)

![Python_df_ord_agg1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_ord_agg1.jpg?raw=true)

We see that all columns have unique values except the category columns. This indicates that *Category Id* and *Category Name* can taken out of the *Orders* Fact table.

Now let's look at the next set of columns.

        df_ord2 = df_sup[['Department Id','Department Name', 'Latitude', 'Longitude', 'Market', 'Order City',
               'Order Country', 'Order Item Discount','Order Item Discount Rate', 'Order Item Id', 
               'Order Item Profit Ratio','Order Item Quantity', 'Sales', 'Order Item Total',
               'Order Profit Per Order','Order Id']]

        ord2_unique_counts = df_ord2.groupby('Order Id').agg(lambda x: x.nunique()).reset_index()
        ord2_min_max_values = ord2_unique_counts.agg(['min', 'max']).reset_index()
        print(ord2_min_max_values)
    
![Python_df_ord_agg2.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_ord_agg2.jpg?raw=true)

We see that the department columns and order item columns don't have unique values. These can be taken out of the *Orders* table and the order items columns can go to the *Order Items* table.

        df_ord3 = df_sup[['Order Region', 'Order State', 'Order Status',
               'Product Card Id', 'Product Image', 'Product Name', 'Product Price',
               'Shipping Mode', 'Order Date', 'Order Time',
               'Shipping Date', 'Shipping Time', 'Order Id']]

        ord3_unique_counts = df_ord3.groupby('Order Id').agg(lambda x: x.nunique()).reset_index()
        ord3_min_max_values = ord3_unique_counts.agg(['min', 'max']).reset_index()
        print(ord3_min_max_values)

![Python_df_ord_agg3.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_ord_agg3.jpg?raw=true)

Here the product columns don't have unique values. These can be taken out of the *Orders* table.

There is a *Customer Id* column which implies a unique customer record. Let's see if we have multiple orders per customer so that we can create a seperate Dimension table for customers.

![Python_df_cust_agg1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_cust_agg1.jpg?raw=true)

All customer columns are unique to *Customer Id* except for *Order Id* so the customer columns can be extracted as a separate *Customers* table.

Now let's look at the *Order Items* table columns to see what is unique to *Order Item Id*.

        df_ord_item = df_sup[['Category Id','Category Name','Department Id','Department Name','Product Card Id','Product Image','Product Name','Product Price','Order Item Id']].copy()
        df_ord_item.drop_duplicates(inplace=True)
        ord_item_unique_counts = df_ord_item.groupby('Order Item Id').agg(lambda x: x.nunique()).reset_index()
        ord_item_min_max_values = ord_item_unique_counts.agg(['min', 'max']).reset_index()
        print(ord_item_min_max_values)

![Python_df_ord_item_agg1.jpg ](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_ord_item_agg1.jpg?raw=true)

As expected, all columns are unique to *Order Item Id* and this would be the lowest level Fact table.

Now let's look at the relationship between department, category and product columns. With respect to departments, how unique are categories and products?

![Python_df_dept_agg1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_dept_agg1.jpg?raw=true)

All columns are not unique except *Department Name*. Now, let's look at categories.

        df_cat = df_sup[['Category Id', 'Category Name', 'Department Id','Department Name', 'Product Card Id','Product Image','Product Name','Product Price']].copy()
        df_cat.drop_duplicates(inplace=True)
        cat_unique_counts = df_cat.groupby(['Category Id']).agg(lambda x: x.nunique()).reset_index()
        cat_min_max_values = cat_unique_counts.agg(['min', 'max']).reset_index()
        print(cat_min_max_values)
        
![Python_df_cat_agg1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_cat_agg1.jpg?raw=true)

Department columns is unique to *Category Id* so we can extract the *Department* Dimension table. Are categories unique to products?

        df_prod = df_sup[['Category Id', 'Category Name', 'Product Card Id','Product Image','Product Name','Product Price']].copy()
        df_prod.drop_duplicates(inplace=True)
        prod_unique_counts = df_prod.groupby(['Product Card Id']).agg(lambda x: x.nunique()).reset_index()
        prod_min_max_values = prod_unique_counts.agg(['min', 'max']).reset_index()
        print(prod_min_max_values)

![Python_df_prod_agg1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_prod_agg1.jpg?raw=true)

Categories are unique to products based on *Product Card Id* so the *Categories* can be extracted as a Dimension table one level below *Department*. The remaining product columns belong to the *Product* Dimension table one level below *Category*.

For the *Customers* table, we want to ensure that the *Latititude* and *Longitude* are valid geographical coordinates. Are they unique to country, city, state, street and zip code.

        df_loc = df_sup[['Customer City', 'Customer State', 'Customer Country', 'Customer Street', 'Customer Zipcode','Latitude', 'Longitude']].copy()
        df_loc.drop_duplicates(inplace=True)
        loc_unique_counts = df_loc.groupby(['Latitude', 'Longitude']).agg(lambda x: x.nunique()).reset_index()
        loc_min_max_values = loc_unique_counts.agg(['min', 'max']).reset_index()
        print(loc_min_max_values)

![Python_df_loc_agg1.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_loc_agg1.jpg?raw=true)

We see that for each *Latititude* and *Longitude* coordinates, there are multiple country, city, state, street and zipcode combinations. These don't appear to be valid coordinates so we will drop those 2 columnsfrom df_sup.

        df_sup.drop(columns=['Latitude', 'Longitude'], inplace=True)

![Python_df_sup_final_len.jpg](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/images/Python_df_sup_final_len.jpg?raw=true)

Our final columns count is 41. We have removed 12 invalid or duplicate columns.         









