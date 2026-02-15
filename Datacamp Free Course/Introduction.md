# What is PySpark?

PySpark is the Python interface for the open source software Apache Spark, which is used for distributed data processing.  
Handles large data sets efficiently including CSV, Parquet and JSON.  
SQL integration allows querying of data using Python and SQL syntax.  
Optimised for speed at scale.

---

# When Would You Use PySpark?

- Big Data Analytics  
- Distributed Data Processing  
- Real-time data streaming  
- Machine learning on large datasets  
- ELT and ETL pipelines  

Working with diverse data sources:
- CSV  
- JSON  
- Parquet  
- Many more  

---

# Spark Cluster

A Spark Cluster is a group of devices working together.

## Master Node

- Manages the cluster  
- Coordinates tasks and schedules jobs  

## Worker Nodes

- Execute the tasks assigned by the master  
- Responsible for executing the actual computations and storing data in memory or disk  

---

# Spark Session

SparkSessions allow you to access your Spark cluster and are critical for using PySpark.

## How to Create a Spark Session in PySpark

```python
# Import SparkSession
from pyspark.sql import SparkSession

# Initialise a SparkSession
spark = SparkSession.builder.appName('MySparkApp').getOrCreate()
```

- `.builder` sets up a session  
- `getOrCreate()` creates or retrieves a session  
- `appName()` helps manage multiple sessions  

---

# PySpark DataFrames

- Similar to other DataFrames (like Pandas) but optimised for PySpark  

## Example of Loading Data into a DataFrame

```python
# Import and initialise a Spark session
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName('MySparkApp').getOrCreate()

# Create a DataFrame
census_df = spark.read.csv(
    'census.csv',
    header=True,
    inferSchema=True
)

# Show the DataFrame
census_df.show()
```
