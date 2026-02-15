# PySpark DataFrames Guide

## Table of Contents

-   [1. About DataFrames](#1-about-dataframes)
-   [2. Creating DataFrames](#2-creating-dataframes)
    -   [2.1 Creating from CSV Files](#21-creating-from-csv-files)
    -   [2.2 Creating from Other Data
        Sources](#22-creating-from-other-data-sources)
-   [3. Viewing Data](#3-viewing-data)
    -   [3.1 Showing Rows](#31-showing-rows)
    -   [3.2 Printing the Schema](#32-printing-the-schema)
-   [4. Schema and Data Types](#4-schema-and-data-types)
    -   [4.1 Schema Inference vs Manual
        Schema](#41-schema-inference-vs-manual-schema)
    -   [4.2 Common PySpark Data Types](#42-common-pyspark-data-types)
-   [5. Core DataFrame Operations](#5-core-dataframe-operations)
    -   [5.1 Selecting and Filtering](#51-selecting-and-filtering)
    -   [5.2 Sorting and Handling Missing
        Values](#52-sorting-and-handling-missing-values)
    -   [5.3 Aggregations and Grouping](#53-aggregations-and-grouping)
-   [6. Practical Examples](#6-practical-examples)

------------------------------------------------------------------------

# 1. About DataFrames

DataFrames in PySpark:

-   Store data in a tabular format using rows and columns
-   Support SQL-like operations
-   Are comparable to a pandas DataFrame or a SQL table
-   Are designed for structured data

------------------------------------------------------------------------

# 2. Creating DataFrames

## 2.1 Creating from CSV Files

One of the most common ways to create a DataFrame is by reading a CSV
file.

``` python
census_df = spark.read.csv(
    "path/to/census.csv",
    header=True,
    inferSchema=True
)
```

### Parameter Breakdown

-   `header=True` Uses the first row as column names

-   `inferSchema=True` Automatically detects data types such as
    IntegerType, StringType, and DoubleType

### Without These Options

-   All columns are read as strings
-   Column names default to `_c0`, `_c1`, `_c2`, etc.

------------------------------------------------------------------------

## 2.2 Creating from Other Data Sources

PySpark supports multiple file formats:

-   CSV files for structured, delimited data
-   JSON files for semi-structured, hierarchical data
-   Parquet files for optimised storage and querying

Examples:

-   CSV: `spark.read.csv("path/to/file.csv")`
-   JSON: `spark.read.json("path/to/file.json")`
-   Parquet: `spark.read.parquet("path/to/file.parquet")`

------------------------------------------------------------------------

# 3. Viewing Data

## 3.1 Showing Rows

``` python
census_df.show()
```

This displays the first 20 rows by default.

------------------------------------------------------------------------

## 3.2 Printing the Schema

``` python
census_df.printSchema()
```

Displays column names, data types, and nullability.

------------------------------------------------------------------------

# 4. Schema and Data Types

## 4.1 Schema Inference vs Manual Schema

-   Spark can infer schemas using `inferSchema=True`
-   Manual schemas provide greater control and are useful for fixed data
    structures

Example of defining a schema:

``` python
from pyspark.sql.types import StructType, StructField, IntegerType, StringType, ArrayType

schema = StructType([
    StructField("id", IntegerType(), True),
    StructField("name", StringType(), True),
    StructField("scores", ArrayType(IntegerType()), True)
])

df = spark.createDataFrame(data, schema=schema)
```

------------------------------------------------------------------------

## 4.2 Common PySpark Data Types

-   `IntegerType` for whole numbers
-   `LongType` for large whole numbers
-   `FloatType` and `DoubleType` for decimal numbers
-   `StringType` for text data

------------------------------------------------------------------------

# 5. Core DataFrame Operations

## 5.1 Selecting and Filtering

-   Use `.select()` to choose specific columns
-   Use `.filter()` or `.where()` to filter rows

``` python
df.select("name", "age").show()

df.filter(df["age"] > 30).show()

df.where(df["age"] == 30).show()
```

------------------------------------------------------------------------

## 5.2 Sorting and Handling Missing Values

-   Use `.sort()` or `.orderBy()` to order data
-   Use `na.drop()` to remove rows with null values

``` python
df.sort("age", ascending=False).show()

df.na.drop().show()
```

------------------------------------------------------------------------

## 5.3 Aggregations and Grouping

-   `.groupBy()` groups rows
-   `.agg()` applies aggregate functions
-   Common aggregate functions include `sum()`, `min()`, `max()`, and
    `avg()`

Example:

``` python
row_count = census_df.count()
print(f"Number of rows: {row_count}")

census_df.groupBy("gender").agg({"salary_usd": "avg"}).show()
```

------------------------------------------------------------------------

# 6. Practical Examples

## Filtering by Company Location

``` python
CA_jobs = ca_salaries_df.filter(
    ca_salaries_df["company_location"] == "CA"
).filter(
    ca_salaries_df["experience_level"] == "EN"
).groupBy().avg("salary_in_usd")

CA_jobs.show()
```

------------------------------------------------------------------------

## Reading a CSV and Performing Aggregations

``` python
salaries_df = spark.read.csv(
    "salaries.csv",
    header=True,
    inferSchema=True
)

row_count = salaries_df.count()
print(f"Total rows: {row_count}")

salaries_df.groupBy("company_size").agg(
    {"salary_in_usd": "avg"}
).show()
```
