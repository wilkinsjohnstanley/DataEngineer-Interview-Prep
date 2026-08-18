# Flashcards
In no particular order.
### 3 Types of Data
1. Structured data (customer records, financial transactions, Excel sheets)
2. Semi-structured data (JSON, HTML, XML)
3. Unstructured (no fixed format such as social media posts, videos, and much more!!!)

### What are the 5 sublanguages of SQL?
1.  Data Definition Language
- Used to define and manage database structures like tables, indexes, and schemas.
- Key Operations: CREATE, ALTER, DROP, TRUNCATE, RENAME, COMMENT
2.  Data Manipulation Language
- Used to insert, update, and delete data within tables.
- Key Operations: INSERT, UPDATE, DELETE
3.  Data Control Language
- Manages user permissions and access control
- Key Operations: GRANT, REVOKE
4.  Data Query Language
- Focuses on retrieving data from the database.
- Key Operations: SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY, DISTINCT, LIMIT
5.  Transaction Control Language
- Handles transactions to ensure data integrity
- Key Operations: BEGIN TRANSACTION, COMMIT, ROLLBACK, SAVEPOINT

### When would you choose streaming over batch processing?
Batch suits high-volume, latency-tolerant workloads such as daily reports. Streaming suits fraud detection and operational alerts.
### What is the difference between Spark local mode and cluster mode?
Local mode runs all SPark processes in one JVM on one machine for development and testing. Cluster Mode runs across multiple nodes with a cluster manager such as YARN, Kubernetes, or a Stand alone cluster manager.

### Write a SQL query to find the 2nd highest salary in a table
Using a subquery:
```
SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary < FROM employees)
```
Using LIMIT 1 OFFSET 1
```
SELECT DISTINCT salary FROM employees
ORDER BY salary DESC LIMIT 1 OFFSET 1
```
Using a Window Function
```
SELECT salary
FROM
(
SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
FROM Employee
) AS ranked
WHERE rnk = 2;
```


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
### What is the difference between Delta Lake and Apache Iceberg conceptually?
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
Dimension Tables are directly related to the Fact Table via Foreign Keys pointing to each Dimension Table. The schema looks like a star with the fact table at the center. The fact table is in the middle and the dimension tables are the points of the star.
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
* Indexing improves data retrieval allowing the database engine to jump directly to the specified record. 
* In write-heavy workloads, every INSERT, UPDATE, or DELETE must update all relevant indexes.


### The Normal Forms (SQL)
* 1NF = have a PK
* 2NF = All columns relates to the PK
* 3NF = No transitive dependencies

### SQL Operators
* Comparison ( = , != , <> )
* Logic ( AND , OR , NOT )
* Range / List ( BETWEEN , IN )
* Pattern ( LIKE , % )
* NULL ( IS NULL )

### WHERE vs. HAVING (SQL)
The WHERE clause appears before the GROUP BY clause and filters individual rows before aggregation like SUM or COUNT. The HAVING clause appears after the GROUP BY clause and filters groups after aggregation is complete. 
### What does the Query Execution Plan show?
It shows the steps the database engine will take to execute a query, including join methods + index usage. EXPLAIN shows the optimizer's chosen path which indexes are used, the join order, estimated row counts, and cost.
It's essential for diagnosing slow queries. 

### What is a covering index?
An index that contains all columns needed by a query, so the database never needs to access the actual table rows (including the index itself). This avoids expensive look-ups back to the heap/table which dramatically speeds up the query. 

### Which aggregate function counts non-NULL values in a column?
* COUNT(Column_name) counts non-NULL values.
* COUNT(*) includes NULL values
* SUM + AVG ignores NULL values automatically.
### What is a foreign key?
* A column that references the primary key of another table... thereby enforcing REFERENTIAL INTEGRITY... to create a link between the tables.
### What is a PRIMARY KEY constraint?
It ensures values in a column are unique and not NULL. It uniquely identifies each row, it implicitly adds a UNIQUE + NOT NULL constraint. A table can have only one primary key. 

### How does Parquet achieve better compression than CSV?
* Parquet achieves optimized compression through it's Columnar storage which groups similar values together. (Integers with integers, strings with strings). This makes compression far more effective. Run-length encoding (a lossless data compression technique) works great on repeated values, dictionary encoding on low-cardinality strings. 

### What is the main difference between a star schema and a snowflake schema?
Snowflake schemas are normalized + Star Schemas are denormalized. 
* In a snowflake schema, dimension tables are normalized into multiple related tables (product_dim, category_dim, etc). Snowflake schemas save storage but require more joins. Wuh-oh!
* Star schemas: product_dim has a category_name (data is stored repeatedly on one table in a star schema)

### What is a database index and when can it hurt performance?
Indexes speed up reads by avoiding full table scans but each write must update all relevant indexes (INSERT, UPDATE, DELETE) + wastes space on low-cardinality columns. 

### How does Delta Lake time travel work?
You query a previous version using VERSION AS OF or TIMESTAMP AS OF. Delta replays the transaction log to reconstruct the table state at that point. 
```
spark.read.format('delta').option('versionAsOf', 5).load('path/to/your/file')
```
OR
```
TIMESTAMP AS OF '2026-08-14'
```
### What is the Delta Lake transaction log?
A JSON-based log (_delta_log/) that records every transaction (add/remove file, schema changes), enabling ACID guarantees + time travel on top of parquet files. 
### What is the difference between a data lake and a data warehouse?
* Lakes are schema-on-read and hold raw, unprocessed data of any of the 3 formats.
* Data warehouses are schema-on-write and hold cleaned, structured data and are ACID compliant with access control (governance).

### What is normalization? What the levels? (SQL)
It's the process of reducing redundancy in a relational database.
1. normal form = the key (PK + columns are atomic + granular)
2. normal form = the whole key (every column relates to the PK)
3. normal form = nothing but the key (2NF + no transitive dependencies)

### How would you create a 1 to 1 relationship? (A student and their parking space)
Use a foreign key with a unique constraint on one side of the relationship.

### Parquet (Spark)
* columnar storage is good for read heavy operations and offers optimized compression. You only query the columns you need. Parquet enables schema evolution.

### What is "grain" in data warehouse modeling?
The lowest level of detail a fact table represents. The grain defines what one row in the fact table means, e.g. 'one row per line item for each transaction.'
Establishing grain first is the most critical step in dimensional modeling. 

### Why are surrogate keys preferred over natural keys in a data warehouse?
Natural keys can change (emails get changed) or contain special characters or collide across source systems. Surrogate keys are stable integers that avoid these problems.

### Which window function assigns the same rank to ties but leaves no gaps in the ranking sequence?
DENSE_RANK() gives tied rows the same rank and then continues sequentially from the next number after the tie. With RANK(), if two people are tied for second place, then they are both assigned second place, but then there is no 'third place,' it continues counting from 'fourth place.'

### What is the difference between WHERE and HAVING?
* WHERE filters rows before aggregation
* HAVING filters after aggregation

### What happens during a Spark Shuffle?
Data is distributed across partitions over the network, rows with the same key land on the same partition. Shuffles are triggered by wide transformations like GroupBy, JOIN, and DISTINCT. Spark writes intermediate data to disk, transferring it over the network, and then reads it, which makes shuffling the most expensive part of Spark Jobs.

### When designing a PySpark SCD Type 2 pipeline, what technique is used to identify changed records?
join incoming data to the current dimension on a natural key + compare attribute hashes or columns to detect changes.

### What is the main difference between DROPPING TABLES that are managed vs. external tables? (HIVE)
HIVE owns managed tables and their metadata, so dropping them deletes everything. Dropping an external table leaves the metadata in place. 

### How would you create a many-to-many relationship? (A student can have many courses and a course has many students).
I would use a third table known as a junction table to connect them. It will have 2 foreign keys and will be the many side of both relations. 

### What are the 3 Phases of Map Reduce?
1. Map data into key-value pairs
2. Group by key using shuffling and sorting
3. Reduce (aggregate the results)


### What does a Window Function's PARTITION BY clause do?
PARTITION BY in a window function is like a GROUP BY but without collapsing rows. The function resets and runs independently for each partition (ROW_NUMBER, SUM, etc).

### Which PySPark method reads a CSV file into a DataFrame?
inferSchema scans the file to determine column types automatically
```
spark.read.csv('path', header=True, inferSchema=True)
```
### What is the difference between 'is' and '==' in Python?
* '==' checks equality of value
* 'is' checks identity, i.e. is it the same object in memory?
### Which exception is raised when accessing a dictionary key that doesn't exist?
KeyError is raised on missing dictionary key access. Use .get() to return None (or a default) instead of raising it. IndexError is for sequences.

### What is the time complexity of looking up an element in a Python set?
O(1) average time complexity. Sets use a hash table, giving O(1) average membership testing. Worst case is O(n).

### What does polymorphism allow in OOP?
Different classes can be used through the same interface. Polymorphism lets different classes implement the same interface or method name. 

### What is "whole stage code generation" in Spark?
It's why dataframes are faster than RDDs. Tungsten's whole-stage codegeneration fuses multiple operators (filter, project, aggregate) into one tight, taut JVM loop, eliminating virtual dispath + improving CPU's each utilization. 

### Which data structure would you use to count occurrences of words in a document?
A dict maps words to counts
```
occ = {}
for item in data:
    occ(item] = occ.get(item, 0) + 1
```
### What is the purpose of '__init__' in a Python class?
It is called when an instance is created to initialize its attributes. It is not a constructor, __new__ creates the object. It sets instance attributes when you call MyClass()
### What is a subquery?
It's a SELECT statement nested inside another SQL statement. It can appear in SELECT, FROM, WHERE, or HAVING clauses and returns a result used by the outer query. 

### What does a CROSS JOIN produce?
it returns every possible combination of the rows from both tables.
### Inheritance
child class inherits parent class methods and attributes
```
class Animal:
    def speak(self):
        pass
class Dog(Animal):
    def speak(self:
        return "Bark"
```
### Classes
The blueprint for creating objects
```
class Dog:
    def __init__(self,name):
        self.name = name
```
a template that defines properties (attributes) and behaviors (methods).
### In Agile Scrum, what is a sprint?
A time-boxed iteration in which a team completes a set of planned work.

### What does 'git merge' do compared to 'git rebase'?
'git merge' creates a merge commit preserving branch history, whereas 'git rebase' replays commits on top of another branch for a linear history.

### What is the difference between a list and a tuple?
* lists are mutable + use square brackets + duplicates are ok
* tuples are immutable + use parenthesis () + don't allow duplicates

### CRUD (SQL)
CREATE, READ, UPDATE, and DELETE are the four fundamental operations used to manage data in a relational database.

### Referential Integrity (SQL)
Data must be consistent, accurate, reliable and properly connected. A Foreign Key must match a Primary Key in another without it, databases become corrupted with orphaned records (which is prevented using CASCADE DELETE).


### Which keyword is used to handle exceptions in Python?
The except block catches exceptions as part of the try / except / else / finally structure.

### What does **kwargs capture in a function signature?
Keyword arguments as a dictionary. **kwargs collects any extra keyword arguments into a dictionary. *args collects extra positional arguments into a tuple.
### What is the purpose of the 'WITH' statement in file handling?
It ensures the file is closed automatically via the context manager protocol (closes the file event if an exception is raised)
### What command inserts a single document in MongoDB?
* insertOne({...}) inserts a single document
* insertMany({...}) inserts multiple documents
Both return an acknowledgement including the generated __id
### MongoDB equivalent of a SQL table?
A Collection
### Why might you denormalize a table in an analytics data warehouse?
To avoid expensive JOINs at query time. Pre-joining data improves read performance for analytical workloads. 
### Polymorphism
a child class can vary the implementation of parent methods and attributes. 
### If you need to change data frequently in Python, which data structure should you use?
a List
### If you need to ensure the data never changes, use _____.
a Tuple
### If you need to remove duplicates or wish to only include unique values, use a ______. 
a Set
### If you need to look up data by a specific name or ID, use a _______.
a Dict
### What does a FULL OUTER JOIN return?
All rows from both tables, with NULLS where No Match exists. 
### What is the difference between cache() and persist() in Spark?
* cache() always uses MEMORY_ONLY
* persist() lets you specify a storage level
cache() is a shortcut for persist(StorageLeve.MEMORY_ONLY)
persist() lets you specify other levels such as MEMORY_AND_DISK or DISK_ONLY
### What is the difference between a clustered and a non-clustered index?
A clustered index acts like a phone book. Skip to the 'H' section. (only one per table)
### Encapsulation
This refers to bundling data and methods into a single unit, the class, and restricting direct access.
### In Power BI, what is SUM vs SUMX ?
SUM is basic aggregation to add up values of a single column.
SUMX is an iterator and can add up multiple columns or do row by row math.
### In SQL, what is a LEFT JOIN?
all from left, any that match from the right
### In Python, What does the __init__ method do in a Python class?
It is a constructor method that is automatically called to initialize a new object's attributes when the class is instantiated. 

### What does the __init__ method do in a Python class?
It is a constructor method that is automatically called to initialize a new object's attributes when the class is instantiated.
### What is an index and why is it used? (SQL)
A data structure used to speed up data retrieval

### In SQL, what is an ERD? How is it useful?
An Entity-Relationship diagram is used to model and show relationships between different tables in a database.
### What is 'git stash' ?
A way to temporarily "shelve" uncommited changes so you can work on something else and apply them later.
### In SQL, What is a subquery?
A query nested within another query?
### _____ is for working with the actual data stored inside the tables.
The Data Manipulation language
* INSERT : Adds new rows of data to a table
* UPDATE : Modifies existing records within a table
* DELETE : Removes specific rows from a table based on a condition.

### In Python, What is type hinting?
A way to explicitly declare the expected data types of variables + function returns.
```
def add(a:int, b:int) -> int:)
```
It helps with debugging and IDE autocompletion.

### In Python, What does "if __name__ "__main__":" do?
It ensures that the code block inside only runs if the script is executed directly, not when it is important as a module by another script.
### RIGHT JOIN (SQL)
all from right, any that match from the left
### In SQL, ________ is used to define and modify the physical structure or schema of the database.
Data Definition Language
* CREATE: creates new objects like tables, views, databases
* ALTER: Modifies the structure of an existing object such as adding a column
* DROP: Permanently deletes an object and it's data
* TRUNCATE: removes all records from a table while keeping its structure intact.
### GROUP BY (SQL)
collapses multiple rows into a single summary row. It looks for identical values in a specific column and groups them together to perform a calculation on that group. 
* COUNT()
* SUM()
* AVG()
* MIN()
* MAX()
```
SELECT major, COUNT(student_id)
FROM Student
GROUP BY major;
```
### In Python OOP, what is the purpose of super()?
It refers to the parent class, allowing access to its methods. 
### Method Overriding (Python)
It's essentially Runtime Polymorphism. When a subclass provides a specific implementation of a method that is already defined in its parent class.
### In Power BI, what is DAX?
Data Analysis Expressions are a library of functions and operators used to build formulas and expressions for data modeling. 
### How would you handle reading large files in Python?
For text files, I'd iterate over the file object directly. Python's file iterator is lazy, so it yields one line at a time.
* Always use a 'with' statement so the file gets closed automatically even if there are exceptions.
* When reading large files, the key principle is to avoid loading the entire file into memory.
###  What is the key advantage of the document model in MongoDB?
Normalization is traded for read performance + schema flexibility. Embedding related data means a single read fetches all needed data (like order items inside an order document).
### In MongoDB, what does find({age:{$gt:30}}) do ?
$gt is MongoDB's 'greater than' query operator. find() returns matching documents. 
* $lt - less than
* $gte - greater than or equal to
* $lte less than or equal to
* $ne - not equal to
* $in - is this object "in" inside?
### What columns are typically added to support SCD Type 2?
* effective_date
* expiry_date
* is_current "flag"
Natural key stays the same across versions.

### What is a surrogate key?
A system generated identifier with no business meaning. Unlike natural keys such as order number or SSN.
### Explain the Database concepts of Atomicity, Consistency, Isolation, Durability
* All or nothin
* valid state before and after
* isolation such that concurrent transactions don't interfere with one another
* committed data survives crashes

### How can you create a set in Python?
Set a variable equal to unique elements enclosed in {curley} braces
### What is the output of print(type(lambda x:x)) ? 
<class 'function'>
Lambdas are anonymous functions. There is no 'lambda' type.
### What does a generator function return when called?
A generator object. It doesn't execute the body yet, values are produced lazily on each call of "next"
### Different filters in Power BI?
* Report filter - the entire report
* Page filter - all graphs on the page
* Visual filter - the graph
### In NoSQL, What is a "Shared Cluster" in MongoDB?
A method of distributing data across multiple machines to support very large datasets and high-throughput operations.
### In DAX, The ___________ function allows you to evaluate an expression while applying specific filters that you can override or add to the current filter context of your report. 
Calculate
```
West Sales = Calculate (Sum(Sales[Amount]), Sales[Region]="West")
```
### What does the 'yield' keyword do in a Python function?
It pauses execution and returns a value resuming on the next call "next".
Yield turns a function into a generator, each call to next() resumes from after the yield, making it memory efficient for large sequences.

### In Python, *args vs **kwargs?
* *args = tuples (any number of positional arguments may be accepted.
* **kwargs = dictionary (any number of keyword arguments may be accepted)
### In Python, What are generators?
They save memory by not saving an entire list in memory at once. Functions that use the yield keyword. They return an iterator that produces items incrementally (lazy evaluation) rather than storing the entire list in memory at once.
### In Python, how does garbage collection work in Python?
Python primarily uses Reference Counting. When an object's reference count drops to zero, it is removed. It also has a cyclical garbage collector to handle reference cycles.
### What is a namespace?
A system that ensures names are unique and can be used without conflict. Local, Global, or Built-in namespaces.
### Parquet format
Parquet files on S3 have no transaction log - partial writes or concurrent writes can corrupt data. Delta Lake adds a atransaction log giving ACID guarantees, time travel, and schema enforcement.
### Which built-in function returns an iterator of (index, value) pairs?
enumerate() wraps an iterable and yields (index, value) tuples, avoiding manual counter variables in loops. 
### What is the difference between a list and a tuple in Python?
They key difference is mutability. Tuples are immutable (cannot be changed after creation), making them hashable as a key in a dict. Lists, of course, are mutable. 
### When would you choose NoSQL over a relational database?
When:
* data is highly unstructured
* schema evolves quickly
* you need horizontal scaling
NoSQL = flexible schemas like DocumentStores, GraphDB, key:value lookup. Relational databases excelt with structured, complex data with relationships and consistency.
### In SQL, What does DISTINCT do?
It removes duplicate rows from the result set, showing only unique values. 
### What is the correct way to read a large file line-by-line without loading it all into memory?
Iterating over the file object after using the with open(f) as f syntax. readlines() loads everything into a list first - so be careful!!!
### What is a natural key?
A Natural key has business meaning such as SSN, email, product codes that ALREADY EXIST in the source data. On the other hand, Surrogate keys are generated by the DW system and have no business meaning. 

### What is the difference between a calculated column and a measure in PowerBI?
Calculated columns are stored in the Model. (row context, computed on refresh). Measures use DAX are are evaluated in the filter context at query time .... more memory-efficient for aggregations. 
### What are slicers in PowerBI?
The slicer is used to apply different filters to visuals. It filters data across visuals in a page. 
### What are the components of Power BI?
1. PowerQuery - cleans data
2. Modeling - connect the tables (PKs/FKs)
3. DAX - calc + math to build business logic
4. Visuals
5. Share
### How do you load an RDD from a JSON file?
1. readAsTextFile
2. ParseJSON json.load pass as lambda function
3. call map to generate a key-value RDD
### in SQL, _______ is specifically for retrieving data from the database.
Data Query Language
* SELECT: Fetches data from one or more tables
### Which of these is a valid custom exception definition?
```
a) class MyError(Base Exception):
        pass
b) def MyError(Exception):
        pass
c) exception MyError:
        pass
d) raise class MyError:
```
Custom exceptions are classes inheriting from Exception or BaseException. The answer is A. Which is the standard pattern.
### How do you read a JSON file into a Python dictionary?
```
import json
with open('data.json', 'r') as file:
      data = json.load(file)
print(data)
print(type(data)) # <class 'dict'>
```
### In Power Business Intelligence, What do we do to commonly "Clean Data"?
- handle null values (choose to give it a value or drop it entirely)
- split and merge columns
- correct data types (numbers stored as strings into proper integer data types)
- filter data
- handle duplicates
- filter out bad / unnecessary columns
- trimming data
### CROSS JOIN (SQL)
combines every single row from the first with every single row from the second row. For example, Finding every possible combination of Students + Clubs to create a master list of recruiting opportunities. 
### What is an alias?
Aliases are used to give a table or column a temporary name. It only exists for the duration of that query.
### In Spark, How do you rename a column in a Dataframe?
* withColumnRenamed()
* withColumn() - adds a new column
* Dataframe.join() - an expression
### In SQL, What is the difference between DELETE, DROP, and TRUNCATE?
* DROP removes the table + it's data
* DELETE removes specific rows or all if no WHERE clause is used
* TRUNCATE will empty a table of all it's data but leave the schema intact.
DROP + DELETE can be rolled back but TRUNCATE cannot.
### What is a dimension table in a data warehouse?
Dimension tables hold descriptive context: products, customers, dates, locations. They are joined to fact tables to add meaning to measurements. Used to filter or group facts.
### What is SparkSQL and how do you use it in PySpark?
Sparks module for structured data processing using SQL queries. 
* use df.createOrReplaceTempView('my_table') to register as a temp view the DataFrame.
* spark.sql('SELECT*FROM my_table') to return a new DataFrame mixing SQL with the DataFrame API.
### How do you handle exceptions in Python?
Using the try, except, else, and finally blocks. 
* try: wraps the code that might cause an error
* except: contains code to run if an error occurs in the try block
* else: runs if the try block was successful (no exception)
* finally: runs regardless - used for clean up tasks like closing files or db connections.
### In Python, what is a decorator?
A function that takes another function function and extents its behavior without explicitly modifying it, denoted with the @ symbol.












































































































































 


















