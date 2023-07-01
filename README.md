# Fetch-DATakehomeTest-FT

**Description of Given Data**

**Users:** The users table describes the user demographics such as gender, state, date of birth, id and the platform used to shop such as ios,android.
The "sign up source" column in a table that includes values such as "Apple," "Google," "Email," and "Facebook" likely refers to the source from which a user signed up for a particular service or platform.
This information can be useful for analyzing user behavior and identifying trends in how users sign up for a particular service or platform.
It also has role column to indicate if the user is a customer or bot. Also it has "active" column which can be used as a flag to indicate if he is active or not.

**Brands:**  The brands table gives us the details of each brand, their category, its name and code and cpg id. Each brand will have its own unique cpgid.
It also has brandcode which is text and might be unique for a brand.It cantains topbrand columns to indicate if it should feature as a topbrand or not.
The cpg id column in brands table can also be used for product identification and inventory tracking,management since it is unique and more trustable

**Receipts:** The given receipts table has lifecycle of a receipt such as transaction dates, who scanned the receipt (userid). It also has the items
associated within the receipt and the total items purchased and money spent on them. Since it has receipt items, it also has brandcodes and related brands
associated with it. It also contains bonus points, points earned and their corresponding reasons.

**First: Review Existing Unstructured Data and Diagram a New Structured Relational Data Model**
I used MySQL Workbench to draw the ER Diagram. The Given ERD is in 2NF (Second normalised form) which is one of the best practises used in designing an ERD.
- Each user may have different receipts. So one user id in Users table will have multiple receipt ids in receipts table. So, I mapped **"One to many"**  relationship between Users and Receipts table.
- Since receipts table had a column which has multiple values and it shows that the receipts table is not in **1Nf (First normalisation)**,I divided the receipts table into Receipt_Items which has data related to Receipt Items. Since one receipt may have many items in it, I mapped **"One to many"** relationship between Receipts and Receipt_Items table.
- Each brand can be available in many receipts, so I mapped **"One to many"** relationship between brands and Receipt_Items table.
- We also have a lot of redundant data in receipts table, even after removing the receipt items. It has data related to Metabrite data in receipts table.So dividing it into Metabrite table to indicate if the receipt item is a part of metabrite campaign. So I have mapped Receipt_Items and Metabrite table **"n to m"** mapping.

**Refererence : ERD_Fetch.pdf**

**Second: Write a query that directly answers a predetermined question from a business stakeholder**

I first installed **Microsoft Sql Server Management Studio 2014** and set up my local server. 
I also installed **Microsoft ACE OLEDB 12.0 drivers** to load data from csv into sql table.

I then imported excelfiles generated from .ipynb file given into **master** database and created tables for each of the files by clicking on Tasks-> Import Data. 
Then the SQL Server Import Wizard opens up to create a new destination table and then edit the mappings to load data from excel files.

After successful loading of data into master database, I have written the atached SQL queries (in MS SQL) for business questions given.

**Reference : FETCH SQL QUERIES.pdf**

**Third: Evaluate Data Quality Issues in the Data Provided**
**Exploratory Data Analysis:**
After extracting the unstructured json data into normalised data frame in python, I have noticed qute few data quality issues as indicated in the referenced file. They are as follows

- Even after normalising receipts.json.gz, When we looked at the receipts table it has a column called "rewardreceiptsItemList" which contained multiple columns such as Points earned,finished date,brandcode etc to be null. Since brandcode is the only joining column for receipts and brands table, it is not accurate to have many null values in that column.
- When looked at the distinct brandcodes in receipts table (227) and distinct brandcodes in the brands table (898), we see that brands table has thrice as many brands as receipts table which is not that accurate for data analysis, as we see a lot of missing or null data in receipts table.
- We see that users table has more than half duplicate values in the table
- The brands table has brandcode which is varchar but while joining it with other tables there is a fair possibility that the joins might 
  go wrong. It would be better if cpg id can be used for joins
- The brands table also have lot of null values in category code,topbrand columns.
- I also found a lot of inconsistent date formats. I had to divide the dates by 1000 since they are in millisecond format and use date time 
  functions to convert it back to datetime format

  The code to find all these analysis is found from **Reference : FETCH_EDA.ipynb**

**Fourth: Communicate with Stakeholders**
My thoughts regarding the questions given above and communicating them to Stakeholders through email can be found in
**Reference : FETCH_StakeholderCommunication.pdf**
