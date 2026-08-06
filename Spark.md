# PySpark Questions
Interviewers will evaluate your knowledge of PySpark syntax. The expectation is for the candidate to write PySpark code in a notepad to perform basic ETL jobs such as reading from files, transforming data, and loading.

### I have a column for first name and want to remove hyphens and commas.

### This code contains errors:
```
from pyspark.sql.functions import col

df1 = df.withcolumn("firstname", regex_epr(["-", ","],))

df2 = df.withcolumn("phone_number_1", explode (col("phone_number")))

df.write.mode('append').partitionBy("country").saveAsTable("table_name").orderBy("state")
```
### Do a broadcast join
```
df = df1.join(broadcast(df2), "left")
```

### Misc
```
regex_replace(col_name, ~['#', ''/'', '*'], "")
df.join(broadcast(df1), 'id', "leftouter")
df=spark.read.format("parquet").option("mergeSchema", "True").load("/user/spark/")
df.filter(df.lastname != null)
df2=df.filter(df.lastname==null).na.fill("NA")
df2.write.partitionBy("year").mode("append").parquet("abc");



with open(path, mode = 'append') as file:
  try: 
      if file = True:
              file.writer("Akanksh")
      except:
        Raise()
```

















### The interviewer will ask you to copy this from the Teams meeting chat into a Notepad:
```
[
{"id": 1, "name":"Anita", "age": null, "salary", "100000", "dept": "IT"}, 
{"id": 2, "name":"Bono", "age": "thirty", "salary", null, "dept": "Finance"},
{"id": 3, "name":"Catherine", "age": "35", "salary", "35000", "dept": "HR"},
{"id": 4, "name":"Dilbert", "age": "28", "salary", "50000", "dept": "Sales"}
]

```
Load into employee table : Transform & cleanse data, Fix datatypes, adding col salary grade as [ >= 60000 High, >= 45000 Medium, or Low], adding Avg has context menu.


### Sample Code
```
from pyspark.sql import SparkSession
from pyspark.sql.functions import when, avg, col
from pyspark.sql.types import IntegerType
#Start Spark Session
spark = SparkSession.builder.appName("SalaryProcessing").getOrCreate()
#Read JSON with schema merging
df = (
  spark.read.format("json").option("mergeSchema", "true").load("path/to/json/files")
)
### Add salary_Grade column
df = df.withColumn(
  "salary_grade",
  when(col("salary) >= 100000, "High").when(col("salary") >= 45000, "Medium").otherwise("Low")
)

### Filter rows where age is integer type
df = df.filter(col("age").cast(IntegerType()).isNotNull())

# Show results
avg_salary_df.show()

```

























