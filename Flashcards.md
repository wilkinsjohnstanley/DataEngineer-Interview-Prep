# Flashcards
In no particular order.
### What are the 5 sublanguages of SQL?
* Data Definition Language
- Used to define and manage database structures like tables, indexes, and schemas.
- Key Operations: CREATE, ALTER, DROP, TRUNCATE, RENAME, COMMENT
* Data Manipulation Language
- Used to insert, update, and delete data within tables.
- Key Operations: INSERT, UPDATE, DELETE
* Data Control Language
- Manages user permissions and access control
- Key Operations: GRANT, REVOKE
* Data Query Language
- Focuses on retrieving data from the database.
- Key Operations: SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY, DISTINCT, LIMIT
* Transaction Control Language
- Handles transactions to ensure data integrity
- Key Operations: BEGIN TRANSACTION, COMMIT, ROLLBACK, SAVEPOINT

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

### What's the difference between a Module and a Package?
* A Module is a single file (.py)
* A Package is a directory containing multiple modules and an __init__.py file

### What is a merge conflict?
An event where Git cannot resolve differences in code between two commits (usually when the same line has been edited by two devs)
###  In MongoDB, what is a Document?
The basic unit of data roughly equivalent to a "row"in a RDBMS.
### What is an Agile Sprint?
A set period usually 1 to 2 weeks during which work must be completed and ready for review.
### What is the difference between 'is' and '=='?
* 'is' checks identity, e.g. is it the same object in memory?
* '==' checks equality
### What is a virtual environment?
An isolated directory that contains a specific version of Python and it's own set of installed packages. It's great for preventing dependency conflicts between projects. 
### What is Power BI?
A business analytics service by Microsoft that provides interactive visualizations and business intelligence capabilities with an interface simple enough for end users to create their own reports.
### What is a lambda function?
A small, anonymous one-liner function without a name defined using the lambda keyword.
### _______ manages transactions (in SQL) to ensure data integrity, especially when multiple DML operations occur at one time.
Transaction Control Language.
* COMMIT - saves all changes made during the current transaction permanently
* ROLLBACK - undoes changes since the last commit if an error occurs
* SAVEPOINT - sets a point within the transaction that you can roll back to later.

### In Spark, what is an "option", and what are its two possible values? Name two ways to handle one.
It represents an optional values such as Some or None.
```
opt.getOrElse("default")
```
also
```
opt match {
case Some(v)=>u
case None => "empty"
}
```

### What does it mean to "de-structure" a tuple or case class in a 'match'?
Break apart the structure
```
val t = (1, "a")

t match {
  case (num, str) => s"$num and $str"
}
```

### In Power BI, what are the differences between COUNT(), COUNTROWS(), and DISTINCTCOUNT()?
* COUNT() - (values in a column including duplicates but ignoring blanks).
* COUNTROWS() - (all the rows in a table including duplicates but ignoring blanks)
* DISTINCTCOUNT() - (all unique values in a column and including blanks.

### In Python, what is a DocString?
A string literal appearing as the first statement in a function, class, or module. It's used for documentation, and it's accessible via obj.__doc__ or help()

### CALCULATE (Power BI)
each measure has a filter context, but CALCULATE can modify it  or remove them before the expression runs.
### NATURAL JOIN (SQL) 
joins all records based on columns that have the same name in both tables)
### What is the difference between 'git fetch' and 'git pull' ?
* 'git fetch' downloads commits from a remote repository but doesn't merge them
* 'git pull' is a combination of 'git fetch' followed immediately by 'git merge'
### What is a foreign key? (SQL)
A column in one table that points to the Primary Key in another table.
For example, a CourseID in an Enrollments table is the foreign key linking back to the courses table.
### _____ handles security and access management by controlling user permissions. 
Data Control Language
* GRANT: Gives specific permissions to the user like SELECT or INSERT to the user.
* REVOKE: removes previously granted permissions from a user.
### A.C.I.D.
* Atomicity - transactions must be completed in their entirety or be rolled back. No partial changes. It's all or nothing.
* Consistency - data remains in a consistent state after a transaction.
* Isolation - transactions do not interfere with each other.
* Durability - data will persist even in the event of a failure.

### What is a primary key?
A column that uniquely identifies each row in a table. CANNOT BE NULL. MUST BE UNIQUE.
### In Power BI, what types of filters are there?
* Visual filters that apply to a single visual.
* Page filters that apply to all the visuals on a page.
* Report filters that apply to all pages in a report.
* Drill-through filters that are navigation features where you can click on a data point and navigate to a special page.
### In Power BI, what are slicers?
They are used to create interactive filters that allow users to select values and filter data across multiple visuals. You can add them from the Visualizer pane.
### How would you read from a file with SparkSQL?
Call read on sparksession
* read.csv (specify whether the first row is a column or not and the file location then to inferSchema or not)
* read.sql
* read.parquet
### In Spark, what RDD is like a Tuple? (Hint: It's created from a tuple or using a map function)
* key-value pair RDD
* useful for grouping, aggregating, join by key
### DAX vs PowerQuery
* DAX - the formula language used to create calculations + custom logic in your model
* Power Query - used to clean up, shape, and prepare data before loading it into the model.
### What is the purpose of a Common Table Expression?
To create a named temporary result set within a query using a WITH clause. It improves readability and can be referenced multiple times in the same query.
### In SQL, Create a table with a UserID and name
CREATE TABLE Users (UserID int PRIMARY KEY Name VARCHAR(255));
### In SQL, Update the users table for the UserID of 1 to the name of 'Bob' for the name column.
UPDATE Users SET Name = 'Bob' WHERE UserId = 1;
### Delete the user with a UserID of 1
DELETE FROM Users WHERE UserID = 1;
### What's the difference between 'map' and 'flatmap' (PySpark)
* map = one output per input
* flatMap = zero or more outputs, flattened.
### What does DISTINCT do in a SELECT statement?
It removes duplicate rows from the result set. Because it compares all selected columns, it can slow queries down on large datasets since it requires sorting and hashing
### What makes a subquery correlated? 
It references a column from the outer query and re-executes for each outer row. A correlated subquery references the outer query's current row, e.g. WHERE inner.dept_id = outer.dept_id). It runs once per outer row, making it potentially slow, often replaceable with a JOIN or window function.
### What is the "Catalyst Optimizer" in Spark?
Spark's query optimizer that transforms logical plans through analysis, logical optimization, physical planning, and code generation.
### Resilient Distributed Datasets allow us to control what?
1. Fine-grain low-level control allowing for custom partitioning and complex transformations in addition to direct memory management.
2. Unstructured and schema-less data for raw data ingestion and flexible processing.
### INNER JOIN (SQL)
only matches
### OUTER JOIN
all records from both or else null on either side
### What are some APIs available in Spark?
* Dataframe API
* SparkSQL API
* Scala API
* Java API
* R API
### How do you handle NULL values in a PySpark DataFrame by filling them with a default value?
```
df.na.fill(value)

df.na.fill({'col':value})

df.na.drop() 
```
### Which PySpark method adds a new column or replaces an existing one?
withColumn(name, expression) returns a new DataFrame with the Specified column added or replaced.

### What is a Spark Session?
The unified entry point for SPark functionality in Spark 2.0+. It includes SparkContext, and the APIs for DataFrame and SQL.

### Which JOIN returns all rows from the left table, with NULLs for unmatched rows from the right?
LEFT JOIN keeps every row from the table on the left, and places NULL values when there is no matching value from the right table.

### What does df.groupBy('dept').agg(avg('salary')) do?
Groups rows by dept and computes average salary per group.

### When would you use repartition() vs coalesce() in Spark?
* repartition() can increase or decrease partitionings with a FULL SHUFFLE
* coalesce() only decreases partitions efficiently without a full shuffle, thereby minimizing data movement by merging adjacent partitions.
### What does spark.read.parquet('path') do?
Reads a Parquet file or directory into a Spark dataframe. Parquet's schema is embedded in the files so Spark automatically applies the correct schema without inferring it.

### What is lazy evaluation in Spark?
Transformations are not executed immediately. They are triggered by an action, allowing Spark to build a Directed Acyclic Graph of the transformations and use the Catalyst Optimizer to efficiently plan the transformations before an action triggers them (count(), collect()).

### DROP vs. TRUNCATE vs. DELETE
* DELETE is a DML statement inside a transaction and can be rolled back.
* TRUNCATE and DROP are both DDL statements and auto-commit in most databases.

### UNION vs. UNION ALL, what is the key difference?
* UNION removes duplicates
* UNION ALL keeps all rows.
UNION de-duplicates by sorting and comparing rows. UNION ALL skips that step, making it faster when duplicates are acceptable.
### A _______ is a temporary result set that you define at the start of a single query. Think of it as a temporary variable for a specific task.
Common Table Expression
### Common Table Expressions
These make complex queries more readable by breaking them into logical steps. For example, you want to create a temporary list of the Top Students and then join that list with a scholarship table.
```
WITH TopStudents AS (
SELECT student_id, name, gpa
FROM students
WHERE gpa >= 3.8
)
SELECT ts.student_id, ts.name, ts.gpa, s.scholarship_id, s.amount
FROM TopStudents ts
JOIN scholarships s
ON ts.student_id = s.student_id;
```
### A _____ is a saved query that you can treat like a virtual table. It doesn't store the data itself, it just remembers the SQL code used to generate it.
VIEW

```
CREATE VIEW StudentContactInfo AS
SELECT
s.StudentID, s.FirstName, s.LastName,
a.Street, a.City, a.State, a.ZipCode
FROM Student s
JOIN Address a
ON s.AddressID = a.AddressID;
```

Now staff can query:
```
SELECT * FROM StudentContactInfo;
```
### What's the difference between CTE's and Views?
CTE's are temporary for one query, while a View is saved in a database and can be reused by staff.
### Why is Parquet better than csv for analytics?
CSVs are row-based and larger and therefore slower for analytics. 
Parquets use a columnar format allowing you to quickly access only the necessary columns.
### SELF JOIN (SQL)
You treat the same table as if it were two separate tables using aliases, it's where a table is joined with itself. It is used when a relationship exists between rows within the same table, such as a hierarchy. For example, a course table might have a column for prerequisites that points to CourseID of another course in the same table.

### What is the difference between scalar and aggregate functions? Can you give examples?
* A Scalar function operates on a single value, ex. UPPER, LOWER, TRUM, CONCAT.
* An Aggregate function operates on multiple values, ex. MIN, MAX, AVG, SUM
### What's an RDD? It's the fundamental data structure of Spark. It's immutable and distributed. Low-level operations are done with RDDs. Great for unstructured data. It is fault tolerant using a lineage graph.
### What is Schema Evolution? (Parquet / AVRO)
The ability to add, remove, or rename columns in new data files while still being able to read old and new files together. Parquet and Avro both support this to varying degrees. Delta Lakes make it explicit and safe. 
### In a daily S3 -> PySpark -> data warehouse pipeline, what does idempotency mean? Why might you want that?
Idempotent pipelines can be rerun safely and get reliable results. 
### What is a broadcast join and when does Spark use it automatically?
It's a join where the smaller table is sent to all executors, avoiding shuffling. Spark broadcasts the smaller table to every executor so the join runs locally without shuffling the large table. If below 10 MBs, it's triggered automatically. 

### What is data skew, and what is 'salting'?
Skew causes one executor to process much more data than others, creating a bottle neck. Salting appends a random number to the key, exploding the skewed key across multiple partitions, then removing the salt post-join.

### What are the four pylons of Object-Oriented Programming?
* Encapsulation - hiding internal state
* Abstraction - showing only necessary features
* Inheritance - reusing code from a parent class
* Polymorphism - allowing objects to take multiple forms.

### What is Schema Drift? How do you handle it in a pipeline?
It's where upstream data changes without notice. 
1. Validate incoming data against the existing schema. "Fail fast"
2. Use a schema registry for streaming (confluent)
3. Use Delta Lake's schema evolution to safely add new columns (awesome)
4. Use explicit column selection over Select * (exquisite)

### What does ROW_NUMBER() do?
Assigns a unique, sequential integer to each row within a partition, with no ties. Each rows gets a unique number.
### A __________ performs a calculation across a set of rows that are related to the current row. Unlike GROUP BY, it doesn't collapse rows into a single summary, it keeps the individual rows.
WINDOW FUNCTION
### Window Function
A Window Function calculates running totals, rankings, and moving averages.
### Encapsulation
Bundling data and the methods that operate on that data into a single unit (a class) while restricting direct access to some of the class's components. 
### How would you create a 1-to-many relationship? e.g. A Department has many professors, a professor has one department.
I would use a foreign key on the many side of the relationship. 

### In Scrum, what 3 questions are answered in a Daily Standup? (Agile)
1. What did I do yesterday?
2. What will I do today?
3. Is something blocking me?
### What is the difference between UNION and UNION ALL? (SQL)
* UNION removes duplicate rows
* UNION ALL keeps all rows from both queries (therefore faster)
### How would you JOIN Dataframes in Spark?
* INNER JOIN
* OUTER JOIN
* FULL OUTER JOIN
* RIGHT JOIN
* LEFT JOIN
* ANTI JOIN
* SEMI JOIN
* LEFT SEMI (like LEFT INNER)
* CROSS JOIN
### What's the difference between Partitions and Buckets in Spark?
* Partitions are sub-directories for each value (chosen for low cardinality such as year, state, country), thereby enabling data pruning as you can skip entire folders during a read to reduce disk i/o.
* Bucketing is for a fixed number of files within a directory based on the hash rather than a column of low cardinality. Shuffling is reduced. The hash is decided to hold a rank of high cardinality information based on the Primary Keys such as UserID and ProductID.
### How would you save an RDD as a textfile?
```
rdd.saveAsTExtFile("path/to/output_directory")
```
### Partition (Spark)
This is how we distribute data logically across nodes in a cluster.

### What are the data types in SQL?
* Numeric = INT, SMALLINT, BIGINT, DECIMAL, FLOAT, REAL
* String = CHAR(n), VARCHAR(n), TEXT
* Date = TIME, DATE, DATETIME, TIMESTAMP
### What are the four pillars of Agile?
1. Customer collaboration over contract negotiation
2. responding to change over creating a plan
3. individuals + interactions over process & tools
4. working software over comprehensive documentation.

### What is a VIEW in SQL?
It is a saved SELECT query. It is a named, stored query. Querying a VIEW executes the underlying SELECT statement, which is useful for abstraction and reusability.

### Why is Shuffling considered a bottleneck in Apache Spark?
It involves expensive disk I/O of writing and reading files, massive network transfers and serialization as data is redistributed among executors.

### How would you debug a slow Spark job?
I would use a systematic approach. First, I would check the Spark UI to examine the Jobs to locate the slow stage. In the Tasks tab I can check to see if the times are uniform or not to determine if there is a skew. If on is slower than the others, there is a data skew. I could also check the SQL tab for the plan. Look for the data skew, missing broadcast, excessive shuffle, or too many or even too few partitions. Garbage collection pressure could also be an issue.

### What is a table  in HIVE?
A table is where we divide table data into sub-directories based on column values, so queries can skip irrelevant partitions. 

### How would you handle late-arriving data in a batch pipeline?
I would use a "watermark" or even a "look-back window". Spark Structured Streaming has a "watermark" feature that discards events beyond a threshold. It reprocesses the last N days / hours of data on each run to catch late records

### For Parquet format.... what skips row groups that don't match filters?
Predicate Pushdown?

### Why is Parquet preferred over CSV for analytical queries?
1. Columnar layout means only relevant columns are read.
2. Predicate pushdown skips row groups that don't match filters.
   Both drastically cut Disk I/O compared to CSV form files.

### What is bucketing in HIVE?
Bucketing applies a hash function to a column and assigns rows to a number of bucket files. BUcketing enables efficient sampling and optimizes joins between bucketed tables on the same key. 

### What is HDFS?
The Hadoop Distributed File System is a fault tolerant, distributed file system that splits files into 128MB blocks that get replicated and stored across multiple nodes. 
This enables data to be processed locally in the servers memory. 

### Which transformation causes a shuffle? 
Shuffling is caused by wide transformations.
*groupBy() requires data with the same key to be on the same partition. It must shuffle data across the network.
Others include: reduceByKey(), join(), repartition(), distinct()

### Narrow transformations
These do not require shuffling because they operate on the individual partitions on each node.
For example: filter(), map(), select(), union() flatmap(), mapPartitions()

### In a star schema, what type of data holds measureable business events such as sales or clicks?
Fact Tables store quantitative/measureable events with foreign keys to dimension tables. Dimension tables hold descriptive attributes like product name or customer city.
### What does the LAG() window function do?
LAG(col, n) returns the value from 'n' rows before the current row in the window. 
It's counterpart is LEAD() which does the opposite, it looks ahead. 
Why might you use it? To compare this months sales with the previous 'n' months of sales, or compare this months sales with the projected sales for the next 'n' months.

### Which Spark abstraction should you prefer for structured data in production ETL?
DataFrames. They have a schema. They enable the Catalyst optimizer and the Tungsten engine to enable transformations to execute as efficiently as possible. RDDs are lower-level and by pass these optimizations. 

### What is the key difference between AWS Glue and EMR for Spark workloads?
* AWS Glue abstracts cluster management. It's a serverless ETL service.
* Elastic Map Reduce gives full control over a provisioned Spark cluster. You can choose instance types, Spark configs, spot instances, and more! EMR is better for complex or high-throughput jobs.
### What is AWS Glue's Dynamic Frame?
Dynamic Frames handle schema inconsistencies gracefully. Each record can have a slightly different schema.

### What is Adaptive Query Execution in Spark3?
A feature that re-optimizes query plans at runtime based on actual statistics from completed stages.

### A.C.I.D. properties
* Atomicity -> all or nothing, uninterruptable
* Consistency -> valid to valid state
* Isolation -> two transactions are isolated from each other
* Durability -> changes persist

### When do indexes increase performance in SQL? When do the hinder performance?
Indexing improves data retrieval allowing the database engine to jump directly to the specified record. 

In write-heavy workloads, every INSERT, UPDATE, or DELETE must update all relevant indexes.
























































































 


















