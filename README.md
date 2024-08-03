# Columbia_Engineering_Data_Analytics_Project_2
Python and Pandas methods and functions and to list comprehensions to extract, transform, and clean data

## **Extract Crowdfunding**
------------------------------
> To begin extracting the crowdfunding data, we utilize pandas as the primary method. 
We start by importing the relevant excerpt from the resource file, which forms the basis of our second line of code. In the third line, we display the data types of the imported data.

#### __Creating Category & Subcategory Dataframe__
-------------------------------------------------
> In the fifth line, we separate the category and subcategory into two distinct columns. The sixth line demonstrates the necessity of using the `pandas.unique` function for both subcategory and category columns to identify the unique entries in each list. 
The seventh line presents the count of unique values within the subcategories and categories. In the eighth line, we create NumPy arrays with values ranging from 1 to 9 for the categories and from 1 to 24 for the subcategories.
For the ninth line, to prepend the title 'cat' to each category ID, we utilize the following code: `cat_ids = ['cat' + str(id) for id in category_ids]`. We print the `cat_ids` array to verify its contents. 
Similarly, we generate a subcategory ID array using 'subcat' in the code and parentheses.
In the tenth line, we create separate DataFrames for categories and subcategories, distinct from the initial crowdfunding DataFrame. Specifically, we assign the `category_id` array as the `category_id` and the categories list as the `category_name`. 
We apply the same procedure to the subcategory DataFrame, using the `subcategory_id` array as the `subcategory_id` and the subcategories list as the `subcategory_name`.
Lines eleven and twelve are dedicated to finalizing the creation of the category DataFrame. In the thirteenth line, we export both the subcategory and category DataFrames to the resource file.

#### __Campaign Dataframe__
--------------------------
> In line fourteen, we create a copy of the `crowdfunding_info_df` DataFrame and name it `campaign_df`. In line fifteen, we rename the respective columns to `descriptions`, `launched_date`, and `end_date`. For line sixteen, we observe that the `goal` and `pledge` column is a string value. 
> To convert this column into a float data type, we utilize the `.astype(float)` method for both. After this adjustment, we print out the data type in line seventeen to confirm that the `goal` column now has a float data type.
> In line eighteen, we convert the time measurement in the `launched_date` column from seconds to a date and time format. To achieve this, we use the following code: `campaign_df['launched_date'] = pd.to_datetime(campaign_df['launched_date'], unit='s').dt.strftime('%Y-%m-%d')`. 
> This code converts the `launched_date` column to a datetime format, specifying the unit of time as seconds, and formats it to display the year, month, and day. We repeat the same method for the `end_date` column in line eighteen, ensuring both date columns are properly formatted.
> In line nineteen we merged both the Campaign data frame and Category & Subcategory data frame. With this new code the column Category & Subcategory is still shows so for line twenty we drop the column by using the code campaign_merged_df = pd.merge(campaign_df, category_df, on="category").merge(subcategory_df, on="subcategory")
> campaign_merged_df.tail(10).  
> We finished this off in line twentyone by exporting the data frame as a csv file. 

#### __Contacts Dataframe__
---------------------------
> In line twenty-three, we first import the `json` module and create a dictionary. 
> Then, we iterate over the `contact_info_df` DataFrame. We import the data using `pd.read_excel` from the resource contact file, specifying that the first row (index 0) contains the header, which is why we use `contacts_info_df.iloc[1:].iterrows()`. 
> This line of code instructs the iteration to start from the second row. The formula `converted_data = json.loads(data)` transforms the data into a JSON format, making it more readable and easier to understand.


#### __Creating contacts Dataframe w/ Pandas__
----------------------------------------------
> In line twenty-three, we first import the `json` module and create a dictionary. Then, we iterate over the `contact_info_df` DataFrame. We import the data using `pd.read_excel` from the resource contact file, specifying that the first row (index 0) contains the header, which is why we use `contacts_info_df.iloc[1:].iterrows()`. This line of code instructs the iteration to start from the second row.
> The formula `converted_data = json.loads(data)` transforms the data into a JSON format, making it more readable and easier to understand. We have all the values to align with the dict_ values that we created and then we printed out the result. This will show contact ID, name, and email in that chronological order. 
> In line twenty-four, the code pulls the DataFrame and separates the data into `contact_id`, `name`, and `email` columns by executing `contacts_info = pd.DataFrame(dict_values, columns=['contact_id','name','email'])`. The `contacts_info.head(5)` command then displays the first five rows of the DataFrame.
> In line twenty-five, we verify the structure of the DataFrame to ensure the data is correctly organized into columns by using `contacts_info.info()`.
> In line twenty-six, we split the `name` column into two separate columns, `first_name` and `last_name`, and subsequently drop the original `name` column. This is accomplished with the following code: `contacts_info[["first_name","last_name"]] = contacts_info["name"].str.split(" ", expand=True) contacts_df = contacts_info.drop('name', axis=1) contacts_df`.
> Since the new columns appear on the right-hand side, it is necessary to rearrange their order for better visual presentation. This is done by executing `contacts_df_clean = contacts_df[['contact_id', 'first_name', 'last_name', 'email']]`. We then display the cleaned DataFrame with `contacts_df_clean`.
> To verify the data types, we repeat the data type check from line twenty-five in line twenty-eight.
> Finally, in line twenty-nine, we export the DataFrame to a CSV file using the appropriate code.

#### __Creating Contacts Dataframe w/ Regex__
--------------------------------------------
> In line thirty, we use the same data as earlier. 
> In line thirty-one, the first line sets the column headers to the values in the first row of the DataFrame. By putting `1` in the bracket, it excludes the first row, followed by `.reset_index(drop=True)`, which resets the DataFrame index.
> In line thirty-two, we extract the `contact_id` numbers into their own column.
> In line thirty-three, we confirm the data is correct using `contacts_info_df_copy.info()`, similar to our earlier verification.
> For the next line, we need to convert the `contact_id` column to an integer data type. The code for this is `contacts_info_df_copy['contact_id'] = contacts_info_df_copy['contact_id'].astype('Int64')`. We then verify the data types again with `contacts_info_df_copy.info()`.
> In line thirty-five, we use a similar code to extract the `name`, but with a different pattern: `(r'"name":\s*"([^"]+)"')` instead of `(\d{4})`.
> In line thirty-six, we repeat the extraction process for the `email` column using the pattern `(r'"email":\s*"([^"]+)"')`.
> In line thirty-seven, we create a new DataFrame `contacts_df_clean_2` by selecting the columns `contact_id`, `name`, and `email` from `contacts_info_df_copy` with `contacts_df_clean_2 = contacts_info_df_copy[['contact_id', 'name', 'email']]`, and then display the first ten rows with `contacts_df_clean_2.head(10)`.
> For the following line, we separate the `name` column into `first_name` and `last_name`, and then drop the original `name` column.
> This is done using `contacts_df_clean_2[["first_name","last_name"]] = contacts_df_clean_2["name"].str.split(" ", expand=True)` followed by `contacts_df_clean_2 = contacts_df_clean_2.drop("name", axis=1)`, and display the first ten rows with `contacts_df_clean_2.head(10)`.
> In line thirty-nine, we reorder the columns for better readability by executing `contacts_df_clean_2 = contacts_df_clean_2[['contact_id', 'first_name', 'last_name', 'email']]`, and display the first ten rows again with `contacts_df_clean_2.head(10)`.
> Similar to line thirty-three, in line forty, we check the data structure and types using `contacts_df_clean_2.info()`.
> Finally, in line forty-one, we export the cleaned DataFrame to a CSV file using *`contacts_df_clean_2.to_csv("Resources/contacts.csv", encoding='utf8', index=False)`*.


## **Crowdfunding DBD**
-------------------------
> The first step involves creating four distinct tables: campaign, subcategory, category, and contacts. 
> Data is then extracted to construct a diagram to show the relationships between these tables.
> >  <img src="./SQL Output Screenshots/crowdfunding_QuickDBD.png" alt="QuickDBD" width="800"/>
> >  <img src="./SQL Output Screenshots/crowdfunding - ERD.png" alt="Postgres SQL ERD" width="800"/>
> We have identified all primary keys, foreign keys, variable characters, doubles, floats, and dates relevant to each table. 
> To ensure it is right, we executed a select statement for each table. 
> Subsequently, the CSV file was imported into the corresponding table. 
> Finally, we verified the accuracy by running a select statement for each table.
> ><img src="./SQL Output Screenshots/crowdfunding_  campaign table_altered_Postgres SQL.png" alt="Campaign Table" width="800"/>
> > <img src="./SQL Output Screenshots/category_table_screenshot.png" alt="Category Table" width="800"/>
> > <img src="./SQL Output Screenshots/subcategory_table_screenshot.png" alt="Subcategory Table" width="800"/>
> > <img src="./SQL Output Screenshots/contacts_table_screenshot.png" alt="Contacts Table" width="800"/>

.
