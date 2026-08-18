### Object Oriented Programming
OOP is a paradigm that organizes code around objects. Objects are bundles of data. They consist of Attributes and methods. A class ais a blueprint to make an instance of an object. In Python, __init__ is the constructor/initializer. An object is a specific things created from that blueprint.
```
class Dog:
    def __init__(self, name, breed):
        self.name=name
        self.breed=breed
    def bark(self):
        return f"{self.name} says woof!"
```
self is a reference to the current instance. It's the first parameter of every instance method. Python passes it automatically. You don't include it when calling the method.
### Tuples
can be used as keys in dictionaries, but lists cannot.
### Decorator
A python decorator is a specific change that we make in Python syntax to alter functions easily. 
### Attributes
* Instance attributes - set on self and are unique to each object
* Class attributes are defined on the class itself and shared by all instances.
```
class Counter:
    def __init__(self):
        self.count = 0 # each instance gets its own count
    def increment(self):
        self.count += 1
```
### Methods
1. Instance methods are regular methods that operate on an instance. The first parameter is self.
```
class Rectangle:
    def __init__(self, width, height):
        self.width=width
        self.height=height
    def area(self):
        return self.width*self.height
```
2. Class methods operate on the class itself, not an instance. Decorated with @classmethod, and the first parameter is 'cls' which is commonly used as an alternative to constructor.
### Inheritance
A class can inherit from another class, gaining its attributes and methods. The child class can them add new behavior or override it. Animals all make sounds for example, but dogs bark and cats meow (different implementations).
### Can a class inherit from more than one parent?
Yes
```
class FlyingFish(Fish, Bird):
      pass
```
### Encapsulation
### Polymorphism
### Abstraction

### Structured Query Language - manipulate relational databases (MySQL, Postgres, Oracle)
Relational DBs -> Tables, Rows, Columns, PKs, FKs, relationships. Data is inserted as records or rows in a table. Many tables related to each other. Each table has many different columns each with data types and constraints.
### Schema
A database's schema is the formal, logical blueprint of the database. It defines how data is organized, structured, and related through tables, fields and constraints.
* Follow up: Why would you have more than one?
You might have a schema for different departments where HR can access certain data that other departments cannot access for example. You might have multi-tenancy where each client has their own data but no access to the other client's data. You might want to organize the database into Sales, Inventory, and Accounting for logical reasons.
### Entity Relationship Diagram
An ERD is used to model and show relationships. 
### Common Table Expression 
They optimizes complex data queries by un-nesting complex logic in the same way a function might in other programming languages. 
```
WITH common_table_name AS name_of_logic
(query is written here inside of parenthesis)
SELECT * FROM name_of_logic
```
### Triggers in SQL
My SQL Triggers are for automatic actions such as applying a discount but first requiring a manager to review it.
A Trigger in SQL is a set of actions available in the form of a stored program invoked when an event occurs. 
### Creating different relationships:
* To create a one-to-one relationship, use a Foreign Key to relate to the Primary Key on another table and put UNIQUE constraint on the Forieng Key column.
* To create a one-to-many relationship, use a FK on the many side of a relationship.
* To create a many-to-many relationship, use a 3rd table called a junction table to connect them. The junction table will have 2 FKs that come from the many side of both relations.
Follow-up: If I want to model a university, what would be the relationship between student & professor.
A many-to-many relationship. A student has many professors, and a professor has many students. Use a junction table to hold both FKs.
### Normalization
The process of reducing redundency in a Database
1. First Normal form = The key (PK must exist, columns must be atomic and granular)
2. Second Normal form = The Whole Key (1NF+ every column relates to the primary key)
3. Third Normal Form = Nothing but the key (2NF + no transitive dependencies)
### What are the 5 sub-languages of SQL?
1. DDL = CREATE, ALTER, MODIFY, DROP, TRUNCATE
2. DML = SELECT, INSERT, UPDATE, DELETE
3. DCL = GRANT, REVOKE
4. DQL = SELECT
5. TCL = SAVEPOINT, ROLLBACK, COMMIT
   
### Scalar vs Aggregate
* Scalar = operates on one value at a time (UPPER, LOWER, TRIM, CONCAT, DATE)
* Aggregate = operates on many values (MIN, MAX, AVG, SUM)

### WHERE vs HAVING
They both filter. HAVING is used to aggregate functions to filter groups of records after aggregation. WHERE happens before.
### List vs. Tuple
The difference between a list and a tuple is that a list is mutable and a tuple is not. Tuples can be hashed. But what does that mean? It means that Tuples can be used as keys in dictionaries.
### What is a branch? What are some common branching strategies?
* A branch is a separate path of changes made to the code base.
* Typically you'll have a main or master branch from which production deployments are made, a dev branch for changes in progress, a features branch that branches from dev for features
```
git checkout -b myNewBranch
# or
git branch myNewBranch && git checkout myNewBranch
 ```
### What are the steps in the software development lifecycle?
1. Planning and Requirements gathering
2. Analysis
3. Design
4. Development
5. Testing
6. Deployment
7. Maintenance
### Waterfall vs Agile
* waterfall is linear, and you don't make changes. It is best for projects with fixed, rigid requirements.
* Agile follows an iterative approach that is highly collaborative with the customer and responsive to change.
### Scrum
* smaller, manageable sprints
* the focus is on collaboration and communication
* delivering value quickly with continual development
### users story
The format is a list of statements:
```
As a user I should be able to do [insert action here] so that I can [insert thing you want to do].
As a user I should be able to do [insert action here] so that I can [insert thing you want to do].
As a user I should be able to do [insert action here] so that I can [insert thing you want to do].
As a user I should be able to do [insert action here] so that I can [insert thing you want to do].
...and so on!
```
### Python
It uses an interpreter. Garbage collected. It's dynamically typed because of the variables. The scopes are local (function), global (module), enclosing (nested functions), and built-in.
### When would I use a tuple and when would I use a list?
* use a List when you need a collection that may change over time and you need to add, remove, and modify it.
* use a tuple when you need it to be constant, need it to be hashable so that is may be used as a key in a dictionary.
### What happens when you run a Python code?
When you write Python code and run it, the source code (.py files) are first compiled into byte code (.pyc files) which are for the Python Virtual Machine which reads the bytecode and executes it line by line at runtime.

### How to concatenate? 
+, or .extent()
```
a = [1, 2, 3]
b = [4, 5, 6]
res=a+b
a.extend(b)
```

### What is the difference between / and // ?
* print (5//2) = 2.5
* print (5/2) = 2

### Can you pass functions as arguments? (are they first class)?
functions, objects, variables of mixed types can all be passed as arguments because they're all objects functions that take other functions as arguments are called higher-order functions.
```
def add (x, y)
    return x + y
def apply_func (add, a, b):
    return func(a, b)
print(apply_func(add, 3, 5)
```

### what is pass ?
The 'pass' statement is a placeholder that does nothing. It's used when something is syntactically required but no code is needed to run. Why? define empty functions, classes, loops. 
### What is a lambda function?
An anonymous one-liner function

### What is List Comprehension?
List comprehension is a ways to create lists using concise syntax. It allows us to apply an expression to each item is an existing iterable
```
a = [2, 3, 4, 5]
res = [val**2 for val in a] #this is the comprehension
print(res) # [4, 9, 16, 25]
```
### What's the difference between *args and **kwargs?
*args is used in function definitions so we can pass a variable number of arguments to a function
```
def fun(*stuff):
    for arg in stuff:
        print(arg)
fun('I', 'am', 'passing', 'so', 'many', 'arguments', 'right', 'now')
```
**kwargs is used for passing key-value pairs
```
def fun(**kwargs):
  for k, val in kwargs.items():
      print("%5==%5"%(k,val))
#Driver code
fun(s1 = 'Oh', s2 = 'wow', s3 = 'how interestion')
```









