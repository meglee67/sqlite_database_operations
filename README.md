# sqlite_database_operations
* HHA 504 HW 3
* Due September 24th
* OG assignment instructions below

## Instructions on how to replicate this assignment
The datasets were from [Good Samaritan](https://goodsamsanjose.com/about/legal-information/pricing-transparency-cms-required-file-of-standard-charges.dot) and [NYU Langone](https://med.nyu.edu/standard-charges/). 

### Data Exploration and Analysis
For this section of the assignment I did the usual process from previous assignments of inspecting shape and size, previewing the data, checking for missing values and dealing with duplicate columns/rows. Additionally this time I did something new of changing the data types from objects to int64 for some columns, as I initially tried to go without this step, but then my attempt at histogram creation was thwarted. I then proceeded to do basic statistics with ``` df.describe() ```. To look at the data distirbution I created histograms and boxplots.

### SQLite Database Operations
1. First I imported the necessary packages ```from sqlalchemy import create_engine``` and ```import sqlite3```.
2. Then I created a local temporary database with the command
```
conn = sqlite3.connect('health.db')
c = conn.cursor()
```
3.  Once a database named health.db has been created, then I created a table named patient_data and defined the fields and datatypes I wanted in this table and committed it. 
```
c.execute('''
            CREATE TABLE patient_data (
              patient_ID INTEGER PRIMARY KEY AUTOINCREMENT,
              name TEXT,
              age INTEGER,
              gender TEXT,
              weight_lbs REAL,
              height_ft REAL
            );
        ''')
conn.commit()
```
4. Then to confirm that the new table had been created I ran the command
```
c.execute('''
  SELECT name
  FROM sqlite_master
  WHERE type='table';
  ''')

c.fetchall()
```
5. Then I proceeded to inser data. First re-stating the existing fields I had defined earlier, then in the same order writing my values.
 ```
sql_query = """

INSERT INTO patient_data (
  'name',
  'age',
  'gender',
  'weight_lbs',
  'height_ft'
  )
  values 
  ('Astarion Ancunin', 39, 'Male', 165, 5.75),
  ('Gale Dekarios', 42, 'Male', 170, 6.2),
  ('Wyll Ravengard', 28, 'Male', 200, 6);
"""
```
6. Then to commit the data insert I did
```
c.execute(sql_query)
conn.commit()
```
7. I then checked that the data had been inserted properly 
```
sql_query_2 = """

select *
from patient_data;

"""

c.execute(sql_query_2)
print(c.fetchall())
```
8. Then I used Pandas to read the inserted data ```pd.read_sql_query("select * from patient_data;", conn)```

#### Automatic Table Creation
1. First I connected to the SQLite database with ```conn = sqlite3.connect('health.db')```
2. Then you use the 'to_sql' function to create a table from the DataFrame and set 'index' to False to exclude the DataFrame index as a column
```
df_goodsam.to_sql('goodsam_data', conn, if_exists='replace', index=False)
conn.commit()
```
3. Then to query and see the table
```
query = "SELECT * FROM goodsam_data"  
df_read = pd.read_sql_query(query, conn)
df_read
```
4. Once you have successfully automatically created a table, close the database connection with ```conn.close()```




  ## **Week 3 Homework Assignment: Introduction to Databases with SQLite**

### **Objective**:
Introduction to the world of databases, starting with SQLite. Integrate data processing with Python, utilize Pandas for exploratory data analysis, and conduct database operations using SQLite.

### **Instructions**:

#### **1. Data Exploration and Analysis**:
1.1 Import pricing transparency datasets from at least two different hospital systems into a Pandas DataFrame. You can use one of the examples provided in the WK3/data folder, or try finding a hospital on your own. The google search approach I used to find these ones was: `{hospital name} price transparency machine readible` - replacing hospital name with the name of the hospital you want to find data for. There will typically be a page that contains download links for a .CSV, .JSON, or .XLS(X) file.  
  - Conduct a basic exploratory analysis using Python, focusing on aspects such as data distribution, missing values, and basic statistics.
  - Document any captivating insights or observations derived from your analysis.
  - My expection is to have at least basic descriptive statistics for numerical columns, and frequency counts for categorical columns

#### **2. SQLite Database Operations**:
2.1 Create a local SQLite database called `health.db`

2.2 *Manual table creation*: Manually create a table (schema) using `CREATE TABLE` SQL query. The table should contain at least 5 columns. Please have at least two columns be numerical columns, and two columns should be categorical (string) columns. Then write at least one `INSERT INTO` SQL query to populate 1 or 2 rows worth of fake data. 

2.3. *Automatic table creation*: Implement the `to_sql` function from Pandas to take the example data from Part 1 of this assignment into the SQLite database.

#### **(Optional) Dive Deeper with SQL**:
- Draft at least one custom `select` SQL query to perform a rudimentary analysis on the data within your SQLite database.
  - For instance, explore patterns in pricing for a specific code (CPT) across different insurances
- Document the SQL queries you've written, the subsequent results, and any conclusions or insights that emerge from them.

### **Submission**:
- Initiate a new GitHub repository titled `sqlite_database_operations` in your GitHub account.
- Leverage components of code from prior assignments where necessary.
- Configure your GitHub repository to include:
  - Python scripts (or colab notebook) illustrating your exploratory data analysis and database transactions.
  - Your custom SQL queries and analysis (if you embarked on this optional task).
  - A comprehensive README.md file that provides:
    - Details on the datasets you've selected.
    - An account of the exploratory data analysis process.
    - Instructions to replicate your SQLite database setup.
    - Any other pertinent documentation or guidelines.
- Present the URL of your repository as your assignment submission.
- Confirm that your repository is public to ensure it's available for evaluation.

**Tip**: The process of data preprocessing prior to its transfer to any database is crucial. Not only does it guarantee data accuracy, but it also fosters efficient storage and querying.
