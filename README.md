# ETL functions with Python
The project here to be done is called scripting. Scripting in Python refers to writing small programs, often called scripts, that automate tasks, manipulate data, or perform routine operations. In this project I will demonstrate the scripting process using data Python Language. The process will involve extracting or loading data from a (csv, tex, xlsx e.tc file) whichever file you have data then load it into the python adn then use Pandas library to do some trasformation and manipulation, once done then we can we define some funtion and create a script to automate every step of the process and save the new a cleaned file.


### Tool and Libraries
- Software: Python V3.10. 3
- IDE: Pycharm or Notebooks (I like code editors so I use Pycharm )
- Libraries: Pandas is a library written for the Python programming language for data manipulation and analysis. In particular, it offers data structures and operations for manipulating numerical tables and time series.
- Libraries: NumPy functions are used to create, manipulate, and analyze NumPy arrays.
- Task/Objective: Description: Scripting is ideal for automating routine tasks such as file manipulation, data processing, and system administration. In this project its automating data manipulation.
- NOTE: There two types of processing namely batch and stream. Batch processing and stream processing are two approaches to handling small to large volumes of data, each suited to different types of tasks and applications. Stream processing involves continuously processing data as it is generated or received. This approach is used for tasks that require real-time or near-real-time data processing and Batch processing involves processing large volumes of data all at once. This is Batch processing in this project
  
### Importing libraries
```python
import pandas as pd
import numpy as np
```
### Loading file
```python
df = pd.read_csv("C:/Users/Ackson/Desktop/ETL_files/Sales_July_2019.csv")
```
### Data cleaning and Transforming:
```python
#2 Handling Missing Values - By removing observation with missing values
df.dropna(inplace=True) # Dropping missing values
df.drop_duplicates(inplace=True) # Removing duplicates

#3 Changing data types
df['Order Date'] = pd.to_datetime(df['Order Date'], errors='coerce')
df['Quantity Ordered'] = pd.to_numeric(df['Quantity Ordered'], errors='coerce')
df['Order ID'] = pd.to_numeric(df['Order ID'], errors='coerce')
df['Price Each'] = pd.to_numeric(df['Price Each'], errors='coerce')


#4 Extract two coulums and creating new colums
df['Date'] = pd.to_datetime(df['Order Date'].dt.date)
df['Time'] = pd.to_datetime(df['Order Date'].dt.date)
df['Total Sales'] = df['Quantity Ordered'] * df['Price Each']

#5 Droping order date and new null
df.drop(columns=['Order Date'], inplace=True)
df.dropna(inplace=True)
```

### Creating a function and a Python Script
We then take all those steps of manipulation and define a function that will be used to call and and do those steps whenever we give it a new dataset. Funtion name here is clean data.
```python

def clean_data(file):
    #1 Read data from CSV file
    df = pd.read_csv("C:/Users/Ackson/Desktop/ETL_files/Sales_July_2019.csv")
    # Set pandas options to show all columns in a single row
    pd.set_option('display.max_columns', None)
    pd.set_option('display.expand_frame_repr', False)

    # 2 Handling Missing Values - By removing observation with missing values
    df.dropna(inplace=True)  # Dropping missing values
    df.drop_duplicates(inplace=True)  # Removing duplicates

    # 3 Changing data types
    df['Order Date'] = pd.to_datetime(df['Order Date'], errors='coerce')
    df['Quantity Ordered'] = pd.to_numeric(df['Quantity Ordered'], errors='coerce')
    df['Order ID'] = pd.to_numeric(df['Order ID'], errors='coerce')
    df['Price Each'] = pd.to_numeric(df['Price Each'], errors='coerce')

    # 4 Extract two coulums and creating new colums
    df['Date'] = pd.to_datetime(df['Order Date'].dt.date)
    df['Time'] = pd.to_datetime(df['Order Date'].dt.date)
    df['Total Sales'] = df['Quantity Ordered'] * df['Price Each']

    # 5 Droping order date and new null
    df.drop(columns=['Order Date'], inplace=True)
    df.dropna(inplace=True)

    # Export cleaned data to CSV file 
    cleaned_file = file.replace('.csv', '_cleaned.csv') #The new dataset will be exported appended with a last name "_cleaned.csv"
    df.to_csv(cleaned_file, index=False) # This line now exports the trasformed dataset to a new dataset csv
    print(f"Data cleaned and saved to {cleaned_file}")

#Example Use
Clean_data("Path of a file needed to be cleaned")
```

### Pipeline:
This script can be used further to create simple data pipeline that takes raw data, transforms it, and stores the transformed data in a PostgreSQL database.
This sounds interesting right: Lets look at steps of how we can make this work.
Main Library: SQLAlchemy is a library that facilitates the communication between Python programs and databases. 

- The clean_data() function loads the data from the CSV file, performs any necessary data cleaning or transformation, and returns the cleaned DataFrame as _cleaned.csv as demostrated in the above example.
- The export_to_postgresql() function connects to the PostgreSQL database using SQLAlchemy's
- Create_engine() method and exports the DataFrame to a specified table in the database using the to_sql() method.
- The main() function orchestrates the data cleaning and export process. It calls the clean_data() function to clean the data, then calls the export_to_postgresql() function to export the cleaned data to PostgreSQL.
  
Objective: When you run the script, it will clean the data and export the cleaned data to your PostgreSQL database as new table or merges it to a table.
This is my Next Project in this repository.
Soon Coming



