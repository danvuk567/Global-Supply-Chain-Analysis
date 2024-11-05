# Python Data Exploration

## Exploratory Data Analysis and Data Cleaning: *[data_exploration_analysis.ipynb](https://github.com/danvuk567/Global-Supply-Chain-Analysis/blob/main/Python-Data-Exploration/data_exploration_analysis.ipynb)*

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

image

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

image

Now we'll replace the correct values in the correct corresponding columns. We'll leave the Street Names as 'Undefined' since there does't appear to be any unique Street Names for those locations.

    df_sup.loc[df_sup['Customer State'].isin(['91732', '95758']), 'Customer Zipcode'] = df_sup.loc[df_sup['Customer State'].isin(['91732', '95758']), 'Customer State'].astype(float)
    df_sup.loc[df_sup['Customer State'].isin(['91732', '95758']), 'Customer State'] = df_sup.loc[df_sup['Customer State'].isin(['91732', '95758']), 'Customer City']
    df_sup.loc[df_sup['Customer Street'].isin(['Elk Grove', 'El Monte']), 'Customer City'] = df_sup.loc[df_sup['Customer Street'].isin(['Elk Grove', 'El Monte']), 'Customer Street']
    df_sup.loc[df_sup['Customer Street'].isin(['Elk Grove', 'El Monte']), 'Customer Street'] = 'Undefined'

*Customer email* and *Customer Password* has 1 unique value of *XXXXXXXXX*. This most likely the result of a security mask. We can drop these 2 columns from *df_sup*.

image

    df_sup.drop(columns=['Customer Email', 'Customer Password'], inplace=True)









*Order Zipcode* has too many nulls. Let's drop this column from *df_sup*.



