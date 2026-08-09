# Flashcards
In no particular order.
### When would you choose streaming over batch processing?
Batch suits high-volume, latency-tolerant workloads such as daily reports. Streaming suits fraud detection and operational alerts.
### What is the difference between Spark local mode and cluster mode?
Local mode runs all SPark processes in one JVM on one machine for development and testing. Cluster Mode runs across multiple nodes with a cluster manager such as YARN, Kubernetes, or a Stand alone cluster manager.
### What is the difference between AWS S3 and HDFS for data storage?
AWS S3 decouples storage from compute with infinite scale and cloud-native benefits. HDFS is a distributed file system that is hierarchical (not flat). It is not the cloud, it's local. Files are co-located with compute. It's coupled and has better data locality.
### ________ provides a managed cluster for running big data frameworks like Spark, HIVE, and Hadoop on EC2 instances.
AWS Elastic Map Reduce
### IaaS vs PaaS vs SaaS
Infrastructure consists of virtual machines and storage, e.g. EC2, S3.
Platform consists of managed platforms for deploying apps, e.g. Elastic Beanstalk.
Software consists of ready to use apps, e.g. SalesForce, Gmail.
### What does OPTIMIZE do in a Delta Lake?
It compacts small files into fewer large files which improves read performance with z-ordering  to co-locate related data within files.
### What is the different between Delta Lake and Apache Iceberg conceptually?
They differ on catalog approach, partition evolution support, and engine compatability. Iceberg has hidden partitioning without rewrites and partition evolution and catalog decoupling.
### What problem does Delta Lake's MERGE (upsert) solve?
It efficiently handles insert-or-update logic atomically. In one ACID transaction, matching rows are updated and non-matching rows are inserted.
### What does df.explain(extended=True) do?
Prints the physical and/or logical query execution plan, essential for optimizing our queries. 
### How does a correlated subquery differ from a non-correlated one? 
A correlated subquery references the outer queries rows and re-executes for each row, making it potentially slow. Non-correlated subqueries run once independently. (Use when a nested query depends on values from the outer query to execute)
### What is encapsulation in OOP?
Bundling data + methods together & Restricting direct access to internal state. A leading underscore in Python is used as best practice to delimit a private detail but it is not enforced.
### How is data structured in a relational database?
Data in a RDB is structured in tables (formally known as relations) that are organized into a grid of rows + columns. This structure is designed to represent real-world entites and their connections while minimizing redundancy.
### Apache Spark vs. Hadoop MapReduce
Spark has in-memory processing and caches data in-memory while MapReduce re-reads it from the disk with each pass from the physical hard-drive.
### ORDER BY
It sorts results in ASC order by default or DESC order when specified.
Example: Getting a list of all students sorted alphabetically by last name
```
SELECT name, gpa
FROM Student
ORDER BY gpa DESC
```
### What are the 6 integrity constraints?
They enforce relationships in tables.
* Primary Key = unique, not null, uniquely identifies records
* Foreign Key = points to the primary key in a parent table and enforces REFERENTIAL INTEGRITY
* UNIQUE = no duplicate values. Null is not counted/ignored. Can have multiple nulls.
* NOT NULL = will ensure null values are not accepted in the column.
* DEFAULT = you choose a value to use if one is not specified.
* CHECK adds an extra condition on inserted data, e.g. x>5

### What is the Medallion Architecture?
A layered data organization pattern:
* Bronze - raw / as-is
* Silver - cleaned / validated
* Gold - biz-ready data, aggregated, facts + dimensions

### How does bucketing speed up joins?
When 2 large tables are bucketed on the same key with the same number of buckets, spark co-locates matching keys and skips the expensive shuffle during joins.
### What is a recursive CTE used for?
Recursive CTE's are used for querying hierarchical data (such as file systems) or graph data by iteratively building a result set. Used for traversing trees / graphs: 
* employee -> manager hierarchy
* category -> sbucategory

### What is Spark's default number of partitions and when should you change it?
200 by default (spark.sql.shuffle.partitions).
* too high for small data
* too low for large data
### What is a Spark Stage Boundary?
Stages are groups of tasks that can run without a shuffle. A wide transformation such as groupBy, JOIN creates a stage boundary and requires shuffling.
1. Stage 1 - writes shuffle data
2. Stage 2 reads it
Minimizing stage boundaries reduces Disk I/O
### What is speculative execution in Spark?
Launching duplicate copies of slow-running tasks on other executors, using whichever finishes first + killing the other. 
```
spark.speculation=true
```
### In a star schema, how are dimension tables related to the fact table?
Dimension Tables are directly related to the Fact Table via Foreign Keys pointing to each Dimension Table. The schema looks like a star with the table table at the center.
### What is the purpose of the Coalesce() function in SQL?
It returns the first non-null value in a list of arguments.
### What is the purpose of CASCADE DELETE?
It maintains Referential Integrity by deleting all the rows in a child table that references the parent's Primary key for a row. This prevents orphaned records. Deleting a student will delete their data in the enrollment table as well.
### What are the 5 V's of Big Data?
* Volume = scale
* Velocity = speed
* Variety = different forms (structured / unstructured)
* Veracity = quality of data / it's accuracy
* Value the result of the previous 4 V's to bring economic value to the company.

### What does GROUP BY do in SQL?
Combines rows with the same value in specified columns into summary rows. It collapses rows with matching values into groups. Aggregate functions then operates per group rather than on the full table.
### A ____________ ___________is a prepared collection of SQL statements that you save so you can reuse them over and over.
STORED PROCEDURE - it helps automate repetitive tasks and improves performance. Example: A procedure called EnrollStudent that takes a StudentID and CourseID as input, checks if the course is full, and then inserts a record.
### A ________ is a piece of code that automatically executes in response to a specific event on a table such as INSERT, UPDATE, or DELETE.
TRIGGER - it helps maintain security, audit changes, and enforce complex business rules automatically. Example = Automatically creates a log entry in an Audit Table whenever a student's grade is changed.
### What is the difference between INNER JOIN and LEFT JOIN?
* INNER JOIN returns only those rows which match in both tables
* LEFT JOIN returns all rows from the left table, and only the rows that match from the right table using NULL when there is no match
* ### What is Apache Spark?
* A distributed, open-source data processing engine for speedy big data analytics using in-memory processing with minimal Disk I/O writing. Does both batch processing and real-time streaming data processing.
* ### What is an Aggregate function?
* A function that performs a calculation on multiple values and returns a single value (SUM, AVG, COUNT).
* ### What is a Resilient Distributed Dataset in Spark?
* An RDD is the fundamental data structure of Spark. It's an immutable, distributed collection of objects that can be operated on in parallel.
* ### Dataframe (Spark)
* 2D table-like data structure with rows and columns, in-memory objects, you use an API to interact with them, used for cleaning, analyzing, and transforming data. You can filter, map, flatmap. It is a Data API and Data Type. It represents a table, schema is read on write.
* ### What is a schema in SQL?
A SCHEMA is the blueprint of the database that defines its tables, columns, data types, and the relationships between them. 
### Insert data into a users table with the columns UserID and name using SQL
```
INSERT INTO Users (UserID, Name) VALUES (1, 'Alice');
```
### UNION VS UNION ALL (SQL)
UNION returns only unique rows from combined queries. UNION ALL returns all rows, including duplicates, and is generally faster.
### UNION operator and it's syntax
The Union operator is used to combine the RESULT SET of two or more SELECT statements. BUT, they must have the same number of columns and must have similar data types. 
Will a column with numbers stored as strings combine with the matching column of another table if that column stores them as integers?
```
SELECT columns FROM table1
UNION
SELECT columns FROM table2;
```
### GET the name colum from the Users table where the UserID = 1
```
SELECT name FROM users
WHERE UserID = 1;
```
### How do you persist an RDD in memory?
* call cache() 
* call persist()
### What is a filter context in PowerBI?
a set of active filters applied to a data model before a formula calculates a result
### What is a List Comprehension in Python?
A concise way to create lists.
```
[x**2 for x in range(10)]
```
### What are "first-class functions" in Python?
In Python, all functions are objects that can be passed as arguments, returned by other functions, and even assigned to variables.
### How is NoSQL different from a traditional RDBMS?
NoSQL DB's are typically non-relational, distributed, and schema-less which allows them to handle unstructured data and scale horizontally.
### In PowerBI, why is GEOMEAN the right function for averaging growth rates and AVERAGE the right function for averaging dollar amounts?
GEOMEAN is used because growth rates compound over time while dollar amounts are simply additive.
### What is a pull request?
A request for teammates to review and approve changes before merging a branch.
### What is PowerBI Gateway used for?
The Gateway is software installed on-premises that acts as a bridge between PowerBI Service in the cloud and local data sources (SQL servers, files) that are not directly interacted with/accessible.




















