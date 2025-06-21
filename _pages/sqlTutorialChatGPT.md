---
layout: default
title: "SQL for Everyone: Learning Relational Databases with SQLite and DBeaver"
permalink: /sqltut-chatgpt/
---
SQL for Everyone: Learning Relational Databases with SQLite and DBeaver

Author’s Note: This comprehensive course is designed for technically literate readers with no prior database or SQL experience. We will start from the very basics of what databases are, then gradually introduce SQL querying and data management concepts, using SQLite as our database engine and DBeaver as a user-friendly graphical interface. Along the way, you’ll get hands-on practice with real-world scenarios and multiple sample databases (e-commerce, academic, and healthcare domains). Each level builds on the previous, and exercises with solutions are provided for practice. By the end, you will have designed and built a complete database for a fictional clinic as a capstone project, and you’ll be equipped to apply your skills to other database systems like PostgreSQL or Snowflake.

Getting Started: Installing SQLite and DBeaver

Before diving into SQL, we need to set up our working environment. We will use SQLite (a lightweight, file-based relational database) and DBeaver (an open-source database GUI tool) for all examples and exercises. Instructions are provided for Windows 10/11 and Ubuntu/Debian Linux.

Windows 10/11 Setup
	1.	Install SQLite (Command-Line): SQLite doesn’t have a complex installation – it’s just a single executable. Download the SQLite tools package for Windows from the official SQLite website (look for a file like sqlite-tools-win32-x86...zip). Unzip this file to a convenient directory (e.g., C:\sqlite\). This contains sqlite3.exe, the SQLite command-line shell. For ease of use, add the directory to your PATH environment variable (so you can invoke sqlite3 from any command prompt). To test, open Command Prompt and run sqlite3 --version; you should see SQLite’s version output.
	2.	Install DBeaver (GUI): Download the DBeaver Community Edition installer from the official DBeaver site. Run the installer and follow the prompts (the default options are fine). DBeaver requires Java but the installer includes a compatible Java Runtime Environment by default, so you typically don’t need to install Java separately. Once installed, launch DBeaver.
	3.	Verify Setup: In DBeaver, go to Help -> About DBeaver to ensure it opened correctly. You’re now ready to connect to databases.

Alternative: As an alternative to manual download, advanced users can install via package managers (e.g., winget or chocolatey for DBeaver on Windows). For example, in an elevated PowerShell: winget install DBeaver.DBeaverCE would install DBeaver. This is optional – the standard installer achieves the same result.

Ubuntu/Debian Linux Setup
	1.	Install SQLite (CLI): Open a terminal and update your package index: sudo apt update. Then install the SQLite package: sudo apt install sqlite3. This installs the SQLite command-line shell. You can verify by running sqlite3 --version in the terminal, which should output the SQLite version if installed properly.
	2.	Install DBeaver (GUI): There are multiple ways to install DBeaver on Linux. The simplest is to use Snap (available on Ubuntu by default):

sudo snap install dbeaver-ce

This will download and install DBeaver Community Edition. Alternatively, DBeaver provides a Debian package (.deb) and an APT repository. For instance, using DBeaver’s official APT repository:

wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/dbeaver.gpg
echo "deb [signed-by=/usr/share/keyrings/dbeaver.gpg] https://dbeaver.io/debs/dbeaver-ce /" | sudo tee /etc/apt/sources.list.d/dbeaver.list
sudo apt update && sudo apt install dbeaver-ce

The above adds DBeaver’s repository and installs it. If you prefer a quicker method and don’t mind Snap, use the snap install command as shown for an easy one-step install.

	3.	Verify Setup: Launch DBeaver from the application menu or by running dbeaver in a terminal. It should open the GUI. Ensure the SQLite CLI works by running sqlite3 in the terminal and then exiting (.exit).

Creating a Connection in DBeaver

Once DBeaver is installed, you need to create a connection to a SQLite database file. DBeaver will serve as our workspace for executing SQL queries:
	•	Open DBeaver and click the “New Database Connection” button (or go to File -> New -> Database Connection). In the list of databases, find and select SQLite and click Next.
	•	For SQLite connection settings, DBeaver will ask for the database file path. If you already have a SQLite database file, you can select it. Otherwise, specify a new path where you want the database to reside, e.g., C:\Users\<YourName>\ecommerce.db on Windows or ~/ecommerce.db on Linux. DBeaver will create a new SQLite file at that location when you connect. You can name it according to the database (we’ll create multiple, e.g., ecommerce.db, university.db, etc. for our examples).
	•	Click Finish. DBeaver will open the new connection. You should see it in the Database Navigator panel.

To execute SQL queries, right-click on the connection and choose SQL Editor -> New SQL Script. This opens a SQL worksheet where you can write and run SQL commands (use the execute button or press Ctrl+Enter to run a query).

Now that the environment is ready, let’s dive into the world of databases and SQL!

Level 0: Database Fundamentals

Before writing any SQL, we need to understand what databases are and how they organize data.

What is a Database? At its core, a database is an organized collection of data, stored electronically, designed to make data management efficient ￼. Instead of having information scattered across various files or spreadsheets, a database stores data in a structured way. Databases are managed by software called a Database Management System (DBMS), which handles how data is stored, retrieved, and updated, ensuring consistency and security of the data ￼.

Relational Databases: In a relational database (the most common type), data is organized into one or more tables. A table is a collection of rows and columns ￼. Each table represents a specific entity or category of data. For example, you might have one table for Customers, another for Orders, etc. Each row (also called a record) in a table is one instance of that entity (e.g., one customer), and each column (also called a field) is an attribute describing that instance (e.g., the customer’s name or email). It may help to think of a table as similar to an Excel spreadsheet: rows are horizontal entries, and columns are vertical categories of data.

Each table should have a way to uniquely identify each row. This is typically done with a primary key – a column (or combination of columns) that is unique for every row (like an ID number). Primary keys help relate tables to one another (more on this soon).

SQL – The Language of Data: SQL stands for Structured Query Language, and it is the standard language used to communicate with relational databases. With SQL, we can query (ask questions about) the data, and also define or modify the database structure and contents. In fact, SQL has several sub-languages:
	•	Data Query Language – primarily the SELECT statement, used to retrieve data (Level 1 will focus on this).
	•	Data Manipulation Language (DML) – commands like INSERT, UPDATE, DELETE to modify data (Level 4).
	•	Data Definition Language (DDL) – commands like CREATE TABLE or ALTER TABLE to define or change the structure of the database (Level 5).
	•	Data Control Language (DCL) – commands to control access or permissions (beyond our scope in this beginner course).

SQL is declarative, meaning you specify what you want (e.g., “get all customers from France”) and the database engine figures out how to compute the answer. A key point is that a SELECT query (read-only) does not alter data; it just returns results. Other statements (like INSERT or DELETE) will change data.

Why Use a Database? You might wonder why not just use spreadsheets or simple text files to store data. Databases have several advantages:
	•	Efficiency and Scale: Databases handle large volumes of data and concurrent access by many users or applications. They can index data for fast lookups and optimize queries internally.
	•	Consistency: A DBMS ensures that updates either fully succeed or fail (a concept called transactions and ACID properties – Atomicity, Consistency, Isolation, Durability). This prevents partial updates or corrupted data when something goes wrong (like power failure mid-update).
	•	Avoiding Redundancy: By structuring data into tables and linking them, databases prevent duplicate data storage. For example, you store a customer’s info once in a Customers table rather than repeating their details in every order record. This minimizes anomalies when updating or deleting data.
	•	Flexibility of Queries: With SQL, you can answer complex questions that would be hard to do with basic filters in a spreadsheet. For instance, “What was the total sales revenue for each product category last quarter?” or “List all patients who have appointments next week that have not yet been confirmed.”
	•	Security and Access Control: Databases can restrict access at various levels (which users can see or modify which data), something hard to manage with shared files.

Tables and Relationships: Each table usually models one entity type. For example, an Employees table might have one row per employee (columns: EmployeeID, Name, Department, etc.). If you have multiple tables, they often are related. A relationship between tables is usually established by storing the primary key of one table as a reference in another table – this is called a foreign key. For example, an Orders table might have a column CustomerID that references the unique ID of a customer in the Customers table. This way, order records are tied to customer records. Using foreign keys, we can join tables back together in queries to get complete information (Level 2 covers JOINs in depth).

To illustrate:
	•	Customers table: each row is a customer (with CustomerID as primary key).
	•	Orders table: each row is an order (with OrderID as primary key) and includes a CustomerID column indicating which customer placed the order. CustomerID in Orders is a foreign key linking to Customers.
	•	We say Customers and Orders have a one-to-many relationship: one customer can place many orders, but each order is for one specific customer.

Relational databases excel at these relationships, ensuring referential integrity (e.g., you can enforce that an order’s CustomerID must actually exist in the Customers table).

Data Model and Schema: The design of tables, columns, and their relationships is called the database schema or data model. A well-designed schema is critical for a robust database. It involves deciding how many tables to use, how to normalize data (breaking it into related tables to avoid redundancy), and defining constraints (like primary keys, foreign keys, allowed data types, etc.). We will touch on schema design principles (including normalization) in Level 5.

Example: Flat File vs. Database Table

To cement these ideas, consider a simplistic example. Suppose you have a list of employees and their department and manager, maintained in a CSV (comma-separated values) flat file:

EmployeeName, Department, ManagerName
John Doe, Sales, Jane Smith
Alice Jones, Sales, Jane Smith
Bob Lee, Engineering, Tom Roe
Charlie Kim, Engineering, Tom Roe

This file lists each employee’s department and manager. There is redundancy – the manager’s name “Jane Smith” is repeated for every Sales employee. If Jane’s name changes (say she got married and has a new last name), you’d have to update it in every row. In a relational database, you might have separate tables for Employees, Departments, and Managers, and use IDs to reference them – removing the redundancy. The flat file is simpler but prone to data inconsistency.

A relational design could be:
	•	Employees: (EmployeeID, Name, DepartmentID, ManagerID)
	•	Departments: (DepartmentID, DepartmentName)
	•	Managers (or just treat managers as a subset of Employees with a role)

This way, “Jane Smith” appears once in the Managers table (or Employees table) with an ID, and the Sales department appears once in Departments with an ID. Employees then refer to those IDs. This eliminates duplicated names and makes updates atomic. We’ll delve more into how to design such schemas later, but this is a peek into the motivation for relational structures.

Setting Up Sample Databases (E-commerce and University)

Throughout this course, we will use multiple real-world example databases to illustrate concepts: an e-commerce database, a university database, and later a clinic/healthcare database. Now that we understand the basics of tables, let’s set up the first two sample databases. Don’t worry if you don’t fully understand the SQL commands below – we will cover CREATE TABLE and other statements in detail in Level 5. For now, follow these steps to create the databases, so we have data to practice querying.

1. E-Commerce Database: This database models a simple online store with customers, products, orders, and order line items.
	•	Entities and Schema:
	•	Customers – basic info about customers.
	•	Products – items for sale (including price and category).
	•	Orders – an order header (who ordered and when).
	•	OrderItems – line items linking orders to products (what products and quantities in each order).
	•	Create and Seed Data: Connect to SQLite (in DBeaver or SQLite CLI) and run the following SQL script. This will create the tables and insert some sample records:

-- Switch to the e-commerce database (if using DBeaver, ensure you're connected to ecommerce.db)
-- Create Customers table
CREATE TABLE Customers (
    CustomerID   INTEGER PRIMARY KEY,
    Name         TEXT,
    Email        TEXT,
    Country      TEXT
);
-- Create Products table
CREATE TABLE Products (
    ProductID    INTEGER PRIMARY KEY,
    Name         TEXT,
    Category     TEXT,
    Price        REAL
);
-- Create Orders table (with a foreign key referencing Customers)
CREATE TABLE Orders (
    OrderID      INTEGER PRIMARY KEY,
    OrderDate    TEXT,
    CustomerID   INTEGER,
    FOREIGN KEY(CustomerID) REFERENCES Customers(CustomerID)
);
-- Create OrderItems table (linking Orders and Products, with composite foreign keys)
CREATE TABLE OrderItems (
    OrderID    INTEGER,
    ProductID  INTEGER,
    Quantity   INTEGER,
    PRIMARY KEY (OrderID, ProductID),
    FOREIGN KEY(OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY(ProductID) REFERENCES Products(ProductID)
);

-- Insert sample Customers
INSERT INTO Customers (Name, Email, Country) VALUES
('Alice Johnson', 'alice@example.com', 'USA'),
('Bob Smith', 'bob@example.com', 'USA'),
('Charlie Lee', 'charlie@example.com', 'Canada'),
('Diana Evans', 'diana@example.com', 'UK'),
('Evelyn Garcia', 'evelyn@example.com', 'USA');

-- Insert sample Products
INSERT INTO Products (Name, Category, Price) VALUES
('Laptop',      'Electronics',  899.99),
('Smartphone',  'Electronics',  699.50),
('Coffee Maker','Home Appliances', 129.99),
('Desk Chair',  'Furniture',    249.00),
('Headphones',  'Electronics',  199.99),
('Espresso Beans','Groceries',   15.00);

-- Insert sample Orders (with OrderDate and CustomerID)
INSERT INTO Orders (OrderDate, CustomerID) VALUES
('2025-06-01', 1),  -- Alice's order
('2025-06-02', 1),  -- Alice again
('2025-06-02', 2),  -- Bob's order
('2025-06-05', 3),  -- Charlie's order
('2025-06-05', 5);  -- Evelyn's order (no items perhaps, or maybe later)

-- Insert sample OrderItems (linking orders to products with quantities)
-- Alice's first order (OrderID 1)
INSERT INTO OrderItems (OrderID, ProductID, Quantity) VALUES
(1, 1, 1),   -- 1 Laptop
(1, 5, 2);   -- 2 Headphones (maybe she bought two for gifts)
-- Alice's second order (OrderID 2)
INSERT INTO OrderItems (OrderID, ProductID, Quantity) VALUES
(2, 2, 1),   -- 1 Smartphone
(2, 3, 1);   -- 1 Coffee Maker
-- Bob's order (OrderID 3)
INSERT INTO OrderItems (OrderID, ProductID, Quantity) VALUES
(3, 4, 1);   -- 1 Desk Chair
-- Charlie's order (OrderID 4)
INSERT INTO OrderItems (OrderID, ProductID, Quantity) VALUES
(4, 6, 3);   -- 3 bags of Espresso Beans
-- Evelyn's order (OrderID 5) - let's leave it with no items (maybe she canceled or an empty cart scenario)

This script creates the e-commerce schema. We set primary keys (CustomerID, ProductID, etc.) and foreign keys for referential integrity (so OrderItems must refer to valid OrderIDs and ProductIDs). We then inserted a handful of records:
	•	5 customers (Alice, Bob, Charlie, Diana, Evelyn)
	•	6 products (3 Electronics, 1 Appliance, 1 Furniture, 1 Grocery)
	•	5 orders (two by Alice, one by Bob, one by Charlie, one by Evelyn)
	•	Order items: Alice’s orders have 2 items each, Bob’s has 1, Charlie’s has 1, Evelyn’s has none (demonstrating an order with no items).

Having some customers with no orders (e.g., Diana has no orders) and an order with no items will allow us to demonstrate outer joins and other concepts later.

2. University Database: This database models a simple university registration system with students, courses, and enrollments.
	•	Entities and Schema:
	•	Students – each row is a student.
	•	Courses – each row is a course offered.
	•	Enrollments – a join-table indicating which student is enrolled in which course (and possibly the grade they received).

This reflects a many-to-many relationship: students take many courses, and each course has many students. The Enrollments table resolves this by pairing student IDs with course IDs (plus an extra attribute for grade).
	•	Create and Seed Data: Connect or create a separate SQLite database (e.g., university.db). Run:

-- Create Students table
CREATE TABLE Students (
    StudentID   INTEGER PRIMARY KEY,
    Name        TEXT,
    Major       TEXT
);
-- Create Courses table
CREATE TABLE Courses (
    CourseID    INTEGER PRIMARY KEY,
    Title       TEXT,
    Department  TEXT,
    Credits     INTEGER
);
-- Create Enrollments table (associating Students and Courses, with a grade)
CREATE TABLE Enrollments (
    StudentID   INTEGER,
    CourseID    INTEGER,
    Grade       INTEGER,  -- grade as a percentage for simplicity
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY(StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY(CourseID)  REFERENCES Courses(CourseID)
);

-- Insert Students
INSERT INTO Students (Name, Major) VALUES
('Aaron Adams',    'Computer Science'),
('Beth Brown',     'Computer Science'),
('Cathy Clark',    'Literature'),
('David Doe',      'Physics'),
('Emily Evans',    'Computer Science');

-- Insert Courses
INSERT INTO Courses (Title, Department, Credits) VALUES
('Database Systems',    'Computer Science', 3),
('Algorithms',          'Computer Science', 3),
('Shakespearean Literature', 'Literature', 4),
('Quantum Mechanics',   'Physics', 4),
('Art History',         'Arts', 2);

-- Insert Enrollments with grades
-- Aaron (Student 1) enrolls in 3 courses:
INSERT INTO Enrollments (StudentID, CourseID, Grade) VALUES
(1, 1, 85),  -- Aaron -> Database Systems
(1, 2, 90),  -- Aaron -> Algorithms
(1, 4, 70);  -- Aaron -> Quantum Mechanics
-- Beth (Student 2) enrolls in 2 courses:
INSERT INTO Enrollments (StudentID, CourseID, Grade) VALUES
(2, 1, 88),  -- Beth -> Database Systems
(2, 2, 92);  -- Beth -> Algorithms
-- Cathy (Student 3) enrolls in 2 courses:
INSERT INTO Enrollments (StudentID, CourseID, Grade) VALUES
(3, 1, 75),  -- Cathy -> Database Systems
(3, 3, 95);  -- Cathy -> Shakespearean Literature
-- David (Student 4) enrolls in 1 course:
INSERT INTO Enrollments (StudentID, CourseID, Grade) VALUES
(4, 4, 80);  -- David -> Quantum Mechanics
-- Emily (Student 5) does not enroll in any courses (to illustrate a student with no enrollments)

This sets up 5 students and 5 courses, and a number of enrollment records:
	•	Aaron (ID 1) is in 3 courses, Beth in 2, Cathy in 2, David in 1, Emily in 0.
	•	Course-wise: Database Systems has 3 students (Aaron, Beth, Cathy), Algorithms 2 (Aaron, Beth), Shakespearean Lit 1 (Cathy), Quantum Mechanics 2 (Aaron, David), Art History 0 (no enrollments yet).

Now we have two databases ready to use:
	•	ecommerce.db – for Levels 1-4 exercises (querying, joining, grouping, modifying data).
	•	university.db – also for practice with joining and grouping (and demonstrating concepts like many-to-many relationships).

In DBeaver, you can manage multiple connections; just be careful to execute queries in the correct connection. It can be helpful to open two SQL Editor tabs side by side – one connected to e-commerce, one to university.

With the fundamentals and sample data in place, let’s learn how to retrieve information using SQL.

Level 1: Core SQL Queries – SELECT, WHERE, ORDER BY, LIMIT, DISTINCT

In this level, we focus on the core SQL query for reading data: the SELECT statement. We’ll learn how to filter results with WHERE clauses, sort them with ORDER BY, limit the number of results, and eliminate duplicates with DISTINCT. All examples will use the sample databases we created.

The SELECT Statement: Retrieving Data

The SELECT statement is used to query data from one or more tables. At minimum, a SELECT query specifies which columns to retrieve and from which table. The simplest form is:

SELECT <columns>
FROM <table>;

For example, to fetch all columns from a table, you can use SELECT *:

SELECT * 
FROM Customers;

This query on the e-commerce database’s Customers table would return every row and every column in the Customers table. Try this in DBeaver: you should see the five customers we inserted, with their details.

Let’s break down the basic clauses of a SELECT query:
	•	SELECT – which columns you want. You can list specific columns (e.g., SELECT Name, Country FROM Customers) or use * as a wildcard for all columns.
	•	FROM – which table(s) to query.
	•	(Optionally) WHERE – conditions to filter which rows to include.
	•	(Optionally) ORDER BY – how to sort the result set.
	•	(Optionally) LIMIT (and OFFSET) – to restrict how many rows are returned (especially useful in big data sets or pagination).
	•	(Optionally) DISTINCT – a modifier to select unique values only, eliminating duplicates.

We will cover each of these.

Selecting Specific Columns

Often you don’t need all columns. For instance, to list all product names and prices in the Products table:

SELECT Name, Price 
FROM Products;

This will produce a list of product names and their prices. In our e-commerce data, that might output:

Name	Price
Laptop	899.99
Smartphone	699.50
Coffee Maker	129.99
Desk Chair	249.00
Headphones	199.99
Espresso Beans	15.00

You can also use simple expressions in the SELECT list. For example:

SELECT ProductID, Price*Quantity AS TotalLineCost
FROM OrderItems
WHERE OrderID = 1;

This calculates a new column (aliased as TotalLineCost) for order 1’s items, which multiplies Price by Quantity. But wait – our OrderItems table doesn’t directly hold Price, only ProductID and Quantity. We’d need to join with Products to get the price (we’ll cover joins in Level 2). Alternatively, if we had stored the price in OrderItems (denormalized for history), we could multiply. For now, keep in mind you can perform calculations, combine fields, etc., in the SELECT clause.

Filtering Rows with WHERE

The WHERE clause specifies conditions that each row must meet to be included in the results. It allows using comparison operators (=, !=, <, >, <=, >=), logical operators (AND, OR, NOT), and more.

Examples:
	•	List customers from the USA:

SELECT Name, Email 
FROM Customers
WHERE Country = 'USA';

This would return Alice, Bob, and Evelyn from our Customers data (three out of five customers are in USA).

	•	Find products cheaper than $200:

SELECT Name, Price
FROM Products
WHERE Price < 200;

This should retrieve Coffee Maker ($129.99), Headphones ($199.99 – actually just under 200), and Espresso Beans ($15.00) – assuming we interpret “cheaper than $200” as strictly less than 200. (Headphones at 199.99 qualify, Desk Chair at 249.00 does not.)

	•	You can combine conditions. Suppose we want electronics under $800:

SELECT Name, Price
FROM Products
WHERE Category = 'Electronics'
  AND Price < 800;

Our Electronics are Laptop ($899.99), Smartphone ($699.50), Headphones ($199.99). This query would return Smartphone and Headphones (Electronics AND price < 800), but not the Laptop (which is Electronics but price is not < 800).

	•	Using OR: If we wanted products that are either in Electronics or cheaper than $100:

SELECT Name, Category, Price
FROM Products
WHERE Category = 'Electronics'
   OR Price < 100;

This would include all Electronics (Laptop, Smartphone, Headphones) plus any product under $100 (Espresso Beans at $15). It would include Laptop even though it’s >1000, because it’s Electronics, and Espresso Beans even though they’re Grocery, because Price < 100. Be careful with OR precedence when mixing with AND; it’s often wise to use parentheses to be explicit about order of evaluation.

	•	Other operators:
	•	BETWEEN: e.g., Price BETWEEN 100 AND 500 (inclusive range check).
	•	IN: e.g., Country IN ('USA','Canada') (membership in a list).
	•	LIKE: pattern matching with wildcards (useful for strings). e.g., Name LIKE 'A%' finds names starting with A. (% is wildcard for any sequence of characters).
	•	IS NULL / IS NOT NULL: check for null (missing) values. For example, WHERE Email IS NULL to find entries with no email.

Tip: In SQLite (and SQL generally), string comparisons are often case-insensitive by default for ASCII letters (depending on collation). 'USA' = 'usa' might return true. But this can vary; to be safe or to force case-sensitivity, consider using functions (not covered here) or specific collations. For our purposes, assume simple case-insensitive matching.

Sorting Results with ORDER BY

By default, SQL results have no guaranteed order (unless an index scan or something coincidentally yields sorted data, but you shouldn’t rely on that). Use ORDER BY to sort the output.

Example: List all customers by name alphabetically:

SELECT CustomerID, Name, Country
FROM Customers
ORDER BY Name;

This will output customers sorted A–Z by Name. To sort in descending order, use DESC:

SELECT ProductID, Name, Price
FROM Products
ORDER BY Price DESC;

This lists products from highest price to lowest. You can specify multiple keys:

SELECT Name, Country, CustomerID
FROM Customers
ORDER BY Country, Name;

This will sort first by Country, and within each Country, by Name. (So all Canada customers together, then UK, then USA, each group sorted by name).

SQLite (like most SQL) sorts text in dictionary order (likely case-insensitive by default). Numeric and date sorting follow natural order (provided dates are stored in a comparable format like YYYY-MM-DD text which sorts chronologically, which our OrderDate is).

Limiting Rows with LIMIT (and OFFSET)

Sometimes you only want to see a subset of the results, especially when there are many. LIMIT N will return only the first N rows of the result (after sorting, if ORDER BY is used). This is useful for things like “top 10” queries or paginating results.

Example: Get the first 3 products by price (cheapest 3):

SELECT Name, Price
FROM Products
ORDER BY Price ASC
LIMIT 3;

This will give the 3 lowest-priced products. Based on our data, sorted by Price ascending: Espresso Beans ($15), Coffee Maker ($129.99), Headphones ($199.99) would be the cheapest three.

We can use OFFSET to skip a certain number of rows. For example, to get the “next 3” products (4th through 6th cheapest):

SELECT Name, Price
FROM Products
ORDER BY Price ASC
LIMIT 3 OFFSET 3;

The combination of ORDER BY, LIMIT, and OFFSET is how you implement pagination (e.g., page 2 of search results, etc.).

Eliminating Duplicates with DISTINCT

If a query’s result has duplicate rows, you can ask SQL to eliminate duplicates by adding DISTINCT after SELECT. This is often used when selecting a single column and you want unique values, or combinations of columns.

Example: which countries do our customers come from?

SELECT Country
FROM Customers;

This might return: USA, USA, Canada, UK, USA for our five customers (since USA appears thrice). To get each country once, use:

SELECT DISTINCT Country
FROM Customers;

This would yield: Canada, UK, USA (likely in alphabetical order because that’s how SQLite will output distinct by default, but technically DISTINCT doesn’t guarantee sort order unless combined with an ORDER BY). If you want it sorted, you can add ORDER BY Country as well.

DISTINCT applies to the combination of all selected columns. If you do SELECT DISTINCT Country, Major FROM Students in the university DB, you’ll get unique pairs of (Country, Major) that exist (though in Students we didn’t include country, just illustrating point).

Be cautious: DISTINCT has to scan the results and remove dupes, which can be memory-intensive for large results. Use it only if needed (e.g., sometimes an aggregation query or a proper join can be a better solution to get unique categories, etc., but for simple use it’s fine).

Putting It Together – A Complex Query Example

Let’s use e-commerce data to answer a question: “List all orders placed by customer Alice Johnson, showing the order ID, order date, and total number of items in each order, sorted by date.”

To do this, we need to:
	•	Find Alice Johnson’s CustomerID.
	•	For those orders (Orders table entries where CustomerID matches Alice’s), count the items in OrderItems for each order.
	•	Possibly calculate total items or total quantity.
	•	Sort by OrderDate.

We haven’t covered joining or aggregation yet, which are needed for counting items and linking customer name to orders. This question actually foreshadows Level 2 (joins) and Level 3 (aggregates). We will come back to this after learning those topics properly.

For now, let’s try simpler queries combining what we have learned in Level 1:
	•	Query 1: “What products fall under the ‘Electronics’ category and cost more than $500?”

SELECT Name, Price 
FROM Products
WHERE Category = 'Electronics'
  AND Price > 500;

Based on our data, this returns the Laptop (899.99) and Smartphone (699.50).

	•	Query 2: “Give me the first 2 orders (by OrderID) from the Orders table and show which customer made them.”
We haven’t learned joins yet, so we can’t easily show the customer name instead of ID. But we can do:

SELECT OrderID, OrderDate, CustomerID
FROM Orders
ORDER BY OrderID
LIMIT 2;

This will likely show Order 1 and Order 2, with their dates and customer IDs (both were Alice in our data).

	•	Query 3: “Show all unique majors of students in the university database.”

SELECT DISTINCT Major
FROM Students;

For our inserted students, this yields: Computer Science, Literature, Physics. (Computer Science appears multiple times among students, but DISTINCT filters it to one).

These basic queries demonstrate selecting columns, filtering, sorting, limiting, and distinct.

Let’s practice what we’ve learned so far with some exercises.

Practice Exercises – Level 1 (Core SELECT Queries)

Using the sample databases you created, answer the following questions using SQL queries. Try to formulate the query yourself, then check the provided solution:
	1.	Products in Furniture or Appliances: List the names of products that belong to either the Furniture category or the Home Appliances category. (Use the e-commerce database.)
	2.	Canadian Customers: Retrieve the CustomerID and Name of all customers from Canada. (E-commerce database.)
	3.	Top 2 Expensive Products: Show the name and price of the 2 most expensive products. (E-commerce database.)
	4.	Students in CS Department: From the university database, get a list of student names who are majoring in ‘Computer Science’.
	5.	Course Catalog: Display all courses (CourseID, Title) sorted by department, then by title. (University database.)

Try writing the SQL for each before peeking at the solutions!

Solutions:
	1.	Products in Furniture or Appliances: We need rows from Products where Category is either “Furniture” OR “Home Appliances”. Use a WHERE with OR:

SELECT Name
FROM Products
WHERE Category = 'Furniture' 
   OR Category = 'Home Appliances';

Solution Explanation: The OR condition ensures we capture products in either category. In our data, this yields Desk Chair (Furniture) and Coffee Maker (Home Appliances).

	2.	Canadian Customers: Filter Customers by Country = 'Canada':

SELECT CustomerID, Name
FROM Customers
WHERE Country = 'Canada';

Result: Should return Charlie’s ID and name (Charlie Lee) as he is the only Canadian customer in the sample. (If no result, double-check the spelling and case of ‘Canada’.)

	3.	Top 2 Expensive Products: Sort products by price descending and limit:

SELECT Name, Price
FROM Products
ORDER BY Price DESC
LIMIT 2;

Explanation: Ordering by Price descending puts highest prices first. LIMIT 2 takes the top two. From our data, Laptop (899.99) and Smartphone (699.50) are the two most expensive products.

	4.	Students in CS Department: Assuming “majoring in Computer Science” corresponds to Major = 'Computer Science' in Students:

SELECT Name
FROM Students
WHERE Major = 'Computer Science';

Expected Output: Aaron Adams, Beth Brown, Emily Evans (the three students whose Major is Computer Science). The query will list those names.

	5.	Course Catalog sorted: We want CourseID and Title from Courses, sorted by Department then Title:

SELECT CourseID, Title
FROM Courses
ORDER BY Department, Title;

What happens: Courses will be grouped by Department alphabetically (Arts, Computer Science, Literature, Physics in our case) and within each, sorted by Title alphabetically. Our course list sorted this way would be:
	•	Art History (Arts)
	•	Algorithms (Computer Science)
	•	Database Systems (Computer Science)
	•	Shakespearean Literature (Literature)
	•	Quantum Mechanics (Physics)

Great! By completing these, you’ve practiced the fundamental querying skills. You can now retrieve and filter data from single tables. Next, we’ll learn how to pull data from multiple tables simultaneously, which is where relational databases truly shine.

Level 2: SQL Joins – Combining Tables

In relational databases, data is often split across tables for good design. Joins allow us to recombine that data by matching rows from different tables based on a related column. This is one of the most powerful aspects of SQL. In this level, we’ll cover the main types of joins:
	•	INNER JOIN
	•	LEFT (OUTER) JOIN
	•	RIGHT (OUTER) JOIN
	•	FULL (OUTER) JOIN
	•	CROSS JOIN

We’ll use examples primarily from the e-commerce and university databases to illustrate each join type.

The Concept of a Join

A join logically combines rows from two tables by comparing values in specified columns. Usually, we join a foreign key in one table to the primary key of another. For example, join Orders to Customers on CustomerID to find out which customer placed each order.

In SQL, join syntax can be:

SELECT ... 
FROM TableA
JOIN TableB ON TableA.common_field = TableB.common_field
...;

The ON clause specifies how to match rows between tables (the join condition).

Let’s define some relationships in our data:
	•	In the e-commerce DB: Orders.CustomerID is a foreign key to Customers.CustomerID. Also, OrderItems links to Orders by OrderID and to Products by ProductID.
	•	In the university DB: Enrollments.StudentID links to Students.StudentID, and Enrollments.CourseID links to Courses.CourseID.

These are the fields we’ll join on to reconstruct meaningful information.

INNER JOIN

An INNER JOIN returns only the rows that have matching values in both tables. In other words, the result will include data only for those cases where the join condition is true for both sides. Rows that don’t have a match in the other table are excluded.

Example 1: List all orders along with the customer’s name who placed each order (e-commerce database). This requires joining Orders with Customers on the CustomerID.

SELECT Orders.OrderID, Orders.OrderDate, Customers.Name AS CustomerName
FROM Orders
JOIN Customers 
    ON Orders.CustomerID = Customers.CustomerID;

	•	We use JOIN Customers ON Orders.CustomerID = Customers.CustomerID. This will pair each order with the customer that has the same ID.
	•	Because this is an inner join (the default JOIN without specifying left/right/full is an inner join), it will only include orders that have a matching customer.
	•	In our data, all orders have a valid CustomerID (1,1,2,3,5). We have customers 1,2,3,5 in Customers (we do have those). So all 5 orders will appear with customer names. If there were an order with CustomerID = 99 that doesn’t exist in Customers, that order would not appear in the result at all with an inner join.

The output might look like:

OrderID | OrderDate   | CustomerName
------- | ----------- | ------------
1       | 2025-06-01  | Alice Johnson
2       | 2025-06-02  | Alice Johnson
3       | 2025-06-02  | Bob Smith
4       | 2025-06-05  | Charlie Lee
5       | 2025-06-05  | Evelyn Garcia

Notice that Diana (customer 4) had no orders, and she appropriately does not appear since we are selecting from Orders (driving table) joined to Customers.

Example 2: Find the enrollment records with student names and course titles (university DB). That means join Enrollments to Students and Courses to replace IDs with actual names.

We can chain joins:

SELECT Students.Name AS StudentName,
       Courses.Title AS CourseTitle,
       Enrollments.Grade
FROM Enrollments
JOIN Students ON Enrollments.StudentID = Students.StudentID
JOIN Courses  ON Enrollments.CourseID = Courses.CourseID;

This takes each row in Enrollments, finds the matching student and course, and returns a combined row. It will only include enrollment records where both a student and a course match (which should be all, given our data integrity). If, for some reason, an enrollment referenced a non-existent student or course, that record would be dropped from an inner join result.

For our sample data, one output row would be:

StudentName | CourseTitle           | Grade
----------- | --------------------- | -----
Aaron Adams | Database Systems      | 85
...

and so on for each enrollment. Note that because Enrollments is the initial table in the FROM, only enrollments that exist are listed. Students like Emily (who has no enrollments) won’t appear because there’s no enrollment row for her to drive the join.

INNER JOIN Summary: Use inner join when you only want “matched” data – e.g., orders that have customers, students that are enrolled in courses, etc. If either side is missing a matching entry, that row is omitted.

LEFT JOIN (Left Outer Join)

A LEFT JOIN returns all rows from the left table, and the matched rows from the right table. If there’s no match on the right, the result will still include the left table’s row, with NULLs for the right table’s columns.

To put it simply: “show everything from the left, even if there’s no corresponding record on the right.”

Example 1: List all customers and any orders they have made. If a customer has no orders, still list the customer (with order info as NULL). Here, “left table” is Customers and “right table” is Orders, since we want all customers.

SELECT Customers.Name, Customers.Country, Orders.OrderID, Orders.OrderDate
FROM Customers
LEFT JOIN Orders 
    ON Customers.CustomerID = Orders.CustomerID
ORDER BY Customers.Name;

	•	We choose Customers as the left table because we want all customers.
	•	The join condition is the same (match CustomerID).
	•	The result will have one row per each match; if a customer has multiple orders, that customer will appear in multiple result rows (one per order). If a customer has no orders, they appear once with NULLs for OrderID/OrderDate.

From our data:
	•	Alice (Customer 1) has 2 orders, so Alice appears in two rows (with different OrderIDs 1 and 2).
	•	Bob (2) has 1 order.
	•	Charlie (3) has 1 order.
	•	Diana (4) has 0 orders, so Diana appears once with OrderID and OrderDate as NULL.
	•	Evelyn (5) has 1 order.

So we will see Diana’s name with NULL for order fields — that’s something an inner join would have filtered out entirely. The LEFT JOIN preserved it, fulfilling the “show all customers” requirement.

Example 2: List all courses, and the number of students enrolled (if any). This could be achieved with aggregation, but to illustrate a join, we can join Courses (left) with Enrollments (right).

SELECT Courses.Title, Enrollments.StudentID
FROM Courses
LEFT JOIN Enrollments 
    ON Courses.CourseID = Enrollments.CourseID
ORDER BY Courses.Title;

This will produce a result where each enrollment is listed under the course, but importantly, courses with no enrollments will still appear. In our data, “Art History” has no enrollment; it will show up with StudentID as NULL. Courses with enrollments (like Database Systems) will appear multiple times (once per student enrolled). This might not be the neatest output (because courses repeat), but it demonstrates that all courses are present. If our goal was to count students per course, we’d likely use a GROUP BY (level 3 topic), but a left join is still used often if you plan to aggregate after (e.g., SELECT Courses.Title, COUNT(Enrollments.StudentID) ... GROUP BY Courses.Title – which would count NULLs as 0 appropriately for courses with none).

LEFT JOIN Summary: Use left join when you want to keep all records of the left table regardless of matches, and still bring in data from the right table when available. Typical use-case: “list all X and (if applicable) their associated Y.” E.g., list all customers and their orders, all products and their sales, all students and their grades – including those with none.

RIGHT JOIN (Right Outer Join)

A RIGHT JOIN is the mirror image of a left join: it returns all rows from the right table, and matched rows from the left. Rows on the right with no left match still appear (with NULLs from the left side). Not all SQL engines support right join syntax (SQLite only added support recently), but conceptually it’s like swapping the tables in a left join.

Since right join is just the opposite perspective, one can always rewrite a right join as a left join by switching which table is first. SQLite historically lacked RIGHT JOIN until version 3.39, but assuming we have it, we can demonstrate:

Example: List all courses and the students enrolled (if any), but using a RIGHT JOIN starting from Enrollments as left and Courses as right (just for demonstration):

SELECT Courses.Title, Students.Name
FROM Students
RIGHT JOIN Enrollments ON Students.StudentID = Enrollments.StudentID
RIGHT JOIN Courses ON Enrollments.CourseID = Courses.CourseID;

This example is more complicated than needed; typically one would use left joins or restructure the query. In practice, you rarely need to use RIGHT JOIN if you can think in terms of left joins by swapping roles.

A clearer scenario: “List all courses and the name of the professor teaching them, if any” – If courses is right and professors is left, a right join would keep all courses. But easier is just left join courses to profs.

In summary: Right joins are less commonly used and can always be achieved by flipping the join order and using a left join. We mention them for completeness.

FULL OUTER JOIN

A FULL JOIN (full outer join) returns all rows when there is a match in either table. This means it includes rows that match (like inner), plus rows from left that had no match (like left join), plus rows from right that had no match (like right join) ￼. Where there is no match on one side, that side’s columns will be NULL.

SQLite (as of recent versions) supports FULL JOIN, but historically it didn’t without workarounds (like using UNION of left and right join results). We’ll assume availability for conceptual understanding:

Example: Suppose we had two lists: a table of current students and a table of alumni. A full join on an attribute like email could list everyone who is either a current student or alumnus, pairing those who appear in both lists. Any email present only in one side would still show up with NULLs for the other side’s fields.

For our existing data, an example could be contrived: Imagine a scenario where you have a table of Employees and a table of Contractors, and you want a list of all people who are either employees or contractors (some might be both if an employee also has a contractor role record). A FULL JOIN on name or ID would include everyone.

In our current sample databases, a full join use-case isn’t obvious because our schema is designed with clear relationships. But just for demonstration, consider: List all customers and all order records in one result, matching them where possible. If a customer never ordered, include them; if an order has a customer (should always; if not, include the order with customer info null).

SELECT Customers.Name, Orders.OrderID, Orders.OrderDate
FROM Customers
FULL JOIN Orders ON Customers.CustomerID = Orders.CustomerID;

This would output what the left join did (all customers, with orders if any) plus, in theory, any orders that had a CustomerID not in Customers (if such existed, which ideally shouldn’t if referential integrity holds). Since our data has no orphan orders, the FULL JOIN result here would actually be the same as the LEFT JOIN result, with the row for Diana as the only one with NULL order fields. If there was an order referencing a non-existent customer, full join would also show that order (with NULL name).

FULL JOIN Summary: It’s useful for merging datasets where you want to keep all records from both sides. In well-designed relational databases with enforced foreign keys, full joins are less common because one side’s unmatched scenario might not occur (or if it does, it indicates data inconsistency). But in data integration tasks (e.g., combining two lists), full join is handy.

For visualization, here is a diagram of how the various joins work (treat the two circles as two tables):

Venn diagram of SQL JOIN types. An inner join returns only the overlapping area (matching rows). A left join returns the overlap plus the left-only portion (with NULLs for right side), a right join returns overlap plus the right-only portion, and a full join returns all colored areas (union of both sets).

CROSS JOIN

A CROSS JOIN returns the Cartesian product of the two tables – every row of the left table paired with every row of the right table. This typically results in a number of rows equal to (rows in left) × (rows in right). Cross joins are rarely needed unless you have a specific reason to combine everything with everything (e.g., generating all combinations, or when preparing data for pivoting, etc.).

You can write FROM A CROSS JOIN B, or simply FROM A, B (listing multiple tables without a join condition is an implicit cross join in SQL).

Example: If we did:

SELECT Customers.Name, Products.Name
FROM Customers
CROSS JOIN Products
LIMIT 10;

This would pair every customer with every product, yielding 5 (customers) × 6 (products) = 30 combinations in total. We limited to 10 just to not overflow, but conceptually it’s 30. The first few results might be:
	•	Alice Johnson – Laptop
	•	Alice Johnson – Smartphone
	•	…
	•	Alice Johnson – Espresso Beans
	•	Bob Smith – Laptop
	•	… etc.

As you can see, without a meaningful condition, cross join results usually don’t make sense for direct interpretation. If you forget a WHERE clause in a normal join, you might accidentally get a cross join and huge output, so be careful!

One intentional use of CROSS JOIN: creating a “matrix” of combinations. For instance, maybe you have a small table of all months and you want to ensure a sales report shows every product for every month (even if zero sales in some). You could cross join products and months to generate all product-month pairs, then left join that with an actual sales table to fill in numbers or zeros where missing.

Joining Multiple Tables

We saw a glimpse of joining three tables in one query (the enrollments example). You can chain as many joins as needed:

FROM TableA
JOIN TableB ON ...
JOIN TableC ON ...
JOIN TableD ON ...

Each join links one more table into the mix. Just ensure each ON clause uses a table that has been introduced before. The order of writing joins can matter if using LEFT/RIGHT joins – it affects which set is considered “left” at each step. For multiple left joins, typically you start from a primary table and keep adding optional data.

Alias in Joins

For complex queries, using table aliases makes writing easier. Example:

SELECT c.Name, o.OrderID, o.OrderDate
FROM Customers AS c
LEFT JOIN Orders AS o
  ON c.CustomerID = o.CustomerID;

This way you can refer to Customers as c and Orders as o throughout. Aliases are also required if you join the same table to itself (self-join) to differentiate the two instances.

Self-Join (Briefly)

A self-join is when a table is joined to itself. This is useful for hierarchical or comparative data. For example, if you have an Employee table with a ManagerID that references another row in the same Employee table, you could self-join Employee to itself to get each employee’s manager’s name.

We won’t delve deep into self-joins, but keep in mind it’s just applying the same principles, except you must use aliases to treat one table as “alias1” and the other as “alias2” even though they are the same underlying table.

Practice Exercises – Level 2 (Joins)

Using our e-commerce and university databases, try to solve these joining problems:
	1.	Orders with Customer Info: Retrieve a list of orders, showing OrderID, OrderDate, and the Name of the customer who made each order. (Use an appropriate join.)
	2.	Order Items Details: List all order line items with the order ID, product name, and quantity. (This involves joining OrderItems to Products, and you could also bring in Orders to see OrderDate if you want to enrich it further.)
	3.	Students and Courses: Produce a list of student enrollments, with student name, course title, and grade. (University DB)
	4.	All Students with Courses (including those with none): List each student’s name and the titles of courses they are enrolled in. Include students who are not enrolled in any course (they should still appear, perhaps with NULL course). (This suggests a left join.)
	5.	Courses and Student Count: Using a join and perhaps grouping (or just a left join and manual counting), list each course title and how many students are enrolled in it. Include courses with 0 students. (We haven’t formally covered grouping yet in detail, so you can either attempt a COUNT with GROUP BY or come back to this in Level 3. If you use grouping, consider it a sneak peek.)

Solutions:
	1.	Orders with Customer Info:

SELECT o.OrderID, o.OrderDate, c.Name AS CustomerName
FROM Orders o
JOIN Customers c
  ON o.CustomerID = c.CustomerID;

This inner join yields only orders that have a matching customer. All our orders do, so it lists all 5 orders with the customer names (Alice for orders 1 & 2, Bob for 3, Charlie for 4, Evelyn for 5).

	2.	Order Items Details:

SELECT oi.OrderID, p.Name AS ProductName, oi.Quantity
FROM OrderItems oi
JOIN Products p
  ON oi.ProductID = p.ProductID;

This will list each order line with the product name and quantity. For example, one row might be “OrderID 1 – Laptop – Qty 1”, another “OrderID 1 – Headphones – Qty 2”, etc. If we wanted to include OrderDate or Customer, we could further join Orders or Customers to this query using oi.OrderID = Orders.OrderID, then Orders.CustomerID = Customers.CustomerID.

	3.	Student enrollments (name, course, grade):

SELECT s.Name AS Student, c.Title AS Course, e.Grade
FROM Enrollments e
JOIN Students s ON e.StudentID = s.StudentID
JOIN Courses c  ON e.CourseID = c.CourseID;

This inner join ensures we only list actual enrollments. Each result row is a student-course pair with grade. E.g., “Aaron Adams – Database Systems – 85”. Students without enrollments won’t appear (Emily won’t show up here).

	4.	All Students with Courses (including none):
We want all students, so start with Students left joined to Enrollments (and further joined to Courses for the title):

SELECT s.Name AS Student, c.Title AS Course
FROM Students s
LEFT JOIN Enrollments e ON s.StudentID = e.StudentID
LEFT JOIN Courses c ON e.CourseID = c.CourseID;

This will list each student multiple times if they have multiple courses, and at least once even if they have none. For a student with no enrollments, c.Title will be NULL. In our data, Emily Evans would appear with Course = NULL. Aaron would appear with each of his 3 courses. This result isn’t aggregated; it’s a raw listing of who takes what, plus lonely students with NULL for course.

	5.	Courses and Student Count:
We haven’t formally done aggregation yet, but here’s how you could do it (feel free to skip if it’s confusing until Level 3):

SELECT c.Title,
       COUNT(e.StudentID) AS StudentCount
FROM Courses c
LEFT JOIN Enrollments e ON c.CourseID = e.CourseID
GROUP BY c.CourseID;

By left joining enrollments, courses with no enrollment will have NULLs that won’t be counted by COUNT(e.StudentID) (since COUNT ignores nulls). The GROUP BY collapses multiple join-generated rows per course into one, counting how many students joined. The result might be:
	•	Database Systems – 3
	•	Algorithms – 2
	•	Shakespearean Literature – 1
	•	Quantum Mechanics – 2
	•	Art History – 0
(Art History is listed with 0 because of the left join and count.)
If you attempted a non-aggregated approach: You could manually eyeball a left join list as in exercise 4 and count, but using SQL’s aggregation is the way to go. We’ll cover COUNT and GROUP BY properly next.

With these join queries, you can now combine information from multiple tables, which is crucial for real-world questions like “which products are selling and to whom”, “which students are in which classes”, etc.

Next, we’ll dive into aggregation – summarizing data across multiple rows – which often goes hand-in-hand with joins for producing reports and insights.

Level 3: Aggregation – GROUP BY and HAVING with Aggregate Functions

In this level, we focus on aggregate functions and grouping, which let us compute summaries over sets of rows – for example, counting records, summing values, finding averages, etc. This is how you can answer questions like “How many orders did each customer place?” or “What’s the average grade in each course?”

We’ll cover:
	•	Common aggregate functions: COUNT, SUM, AVG, MIN, MAX.
	•	The GROUP BY clause to arrange rows into groups for aggregation.
	•	The HAVING clause to filter groups (analogous to WHERE but for aggregated groups).

Aggregate Functions Overview

Aggregate functions operate on multiple row values and produce a single result:
	•	COUNT – counts the number of rows (or non-NULL values in an expression).
	•	SUM – sums up numeric values.
	•	AVG – calculates average of numeric values.
	•	MIN / MAX – find the minimum or maximum value in a set.
	•	Others include statistical or special ones (SQLite has e.g., GROUP_CONCAT for concatenating strings, etc.), but we’ll focus on the basics.

By default, if you use an aggregate without grouping, it treats the entire table (or result set) as one group.

Example: To count how many products are in the Products table:

SELECT COUNT(*) 
FROM Products;

COUNT(*) counts rows. This would return 6 for our data (since we inserted 6 products).

To get the cheapest and most expensive product price:

SELECT MIN(Price), MAX(Price)
FROM Products;

This would yield two values: 15.00 (min) and 899.99 (max) in our case.

If you try to select regular columns along with aggregates without grouping, you’ll get an SQL error. For example, SELECT Name, COUNT(*) FROM Products; is invalid because when using an aggregate, any non-aggregated column in the select must be part of a GROUP BY. That leads us to the next topic.

GROUP BY Clause

The GROUP BY clause is used to collect rows into groups based on one or more columns having the same values. You then apply aggregate functions to each group separately.

Think of it as “collapse the table into one row per group, computing aggregates for each group.”

Example 1: Number of orders per customer. We want to group orders by customer. Using the e-commerce DB:

SELECT CustomerID, COUNT(*) AS OrderCount
FROM Orders
GROUP BY CustomerID;

What happens here:
	•	The table Orders is grouped by CustomerID. All orders for the same CustomerID form one group.
	•	We select CustomerID (it must either be in GROUP BY or an aggregate; here it’s in GROUP BY) and COUNT(*) for each group.
	•	The result might be:
	•	CustomerID 1 → Count 2 (Alice had 2 orders)
	•	CustomerID 2 → Count 1 (Bob had 1)
	•	CustomerID 3 → Count 1 (Charlie had 1)
	•	CustomerID 5 → Count 1 (Evelyn had 1)

Notice Customer 4 (Diana) is missing because she had no orders; there was no group for CustomerID 4 in the Orders table. If we wanted to include customers with zero orders, we’d need to left join with Customers as in earlier example and use COUNT on orders, or use a trick with COUNT inside a subquery. But pure grouping on Orders naturally excludes IDs not present.

We can improve that by joining first then grouping:

SELECT c.Name, COUNT(o.OrderID) AS OrderCount
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerID;

Now Customers is the primary set, left joined with Orders. Group by customer ensures one row per customer, and COUNT(o.OrderID) will count how many orders (it will count nulls as 0 effectively because COUNT of a column counts only non-nulls). This would list Diana as well with 0.

Example 2: Total quantity sold per product. Use OrderItems, group by ProductID:

SELECT ProductID, SUM(Quantity) AS TotalUnitsSold
FROM OrderItems
GROUP BY ProductID;

This would sum up quantities for each product across all orders. If a product never appears in OrderItems (meaning it was never ordered), it won’t show up. For our data:
	•	Product 1 (Laptop) appears in OrderID 1 (qty 1) -> sum = 1
	•	Product 2 (Smartphone) in OrderID 2 (qty 1) -> sum = 1
	•	Product 3 (Coffee Maker) in OrderID 2 (qty 1) -> sum = 1
	•	Product 4 (Desk Chair) in OrderID 3 (qty 1) -> sum = 1
	•	Product 5 (Headphones) in OrderID 1 (qty 2) -> sum = 2
	•	Product 6 (Espresso Beans) in OrderID 4 (qty 3) -> sum = 3

So each product 1-6 gets a sum (some just 1, some higher). If we had a product that never sold, it wouldn’t appear here.

We likely want to join to get product names for a nicer report:

SELECT p.Name, SUM(oi.Quantity) AS TotalUnitsSold
FROM Products p
LEFT JOIN OrderItems oi ON p.ProductID = oi.ProductID
GROUP BY p.ProductID;

This would give each product’s name and total units sold (with 0 for those with none). Left join ensures every product appears.

Example 3: University – average grade per course:

SELECT c.Title, AVG(e.Grade) AS AvgGrade
FROM Courses c
JOIN Enrollments e ON c.CourseID = e.CourseID
GROUP BY c.CourseID;

This groups by course, so we get one row per course that has enrollments, showing the average grade. Courses with no students wouldn’t appear (because inner join used; if we want them with NULL or some indicator, use left join). Our data would yield:
	•	Database Systems: (85+88+75)/3 = 82.67
	•	Algorithms: (90+92)/2 = 91
	•	Shakespearean Literature: 95 (only one student)
	•	Quantum Mechanics: (70+80)/2 = 75
	•	(Art History has no grades, so not listed with the inner join approach. With left join, we could list it with NULL or no average.)

Important: GROUP BY columns should be the ones that define the grouping. You can group by multiple fields if needed (like grouping by CustomerID and OrderDate to maybe count orders per customer per date, etc.). Every column in SELECT that is not inside an aggregate function must appear in the GROUP BY clause. Conversely, every column in GROUP BY can appear in SELECT. You can also group by an expression (like by year of a date, etc., using SQLite date functions).

HAVING Clause

The HAVING clause is like a WHERE for aggregated results. After grouping and aggregation, you may want to filter out some groups based on the aggregated values.

For example: “Which customers have placed more than 1 order?” We saw how to get order count per customer. We can add HAVING OrderCount > 1 to filter to only those groups with count > 1.

Using the earlier query:

SELECT CustomerID, COUNT(*) AS OrderCount
FROM Orders
GROUP BY CustomerID
HAVING COUNT(*) > 1;

This would return only CustomerID 1 with count 2 in our data (since Alice is the only one with more than one order). We could join to get the name as well:

SELECT c.Name, COUNT(o.OrderID) AS OrderCount
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerID
HAVING COUNT(o.OrderID) > 1;

This would list Alice Johnson – 2. (Left join ensures we considered all, but having filters to those >1).

Some points:
	•	We use HAVING COUNT(o.OrderID) > 1 because WHERE cannot be used with aggregates. WHERE filters before grouping (on individual rows). HAVING filters after grouping (on groups).
	•	You can also have conditions on non-aggregated grouped columns in HAVING, but those are often logically moved to WHERE. E.g., you could do HAVING c.Country = 'USA' in a grouped query by customer to only consider groups of US customers, but it’s cleaner to put WHERE c.Country='USA' before grouping, since that filters input rows.

Another example: “Find all courses where the average grade is below 80.”

SELECT c.Title, AVG(e.Grade) AS AvgGrade
FROM Courses c
JOIN Enrollments e ON c.CourseID = e.CourseID
GROUP BY c.CourseID
HAVING AVG(e.Grade) < 80;

This will calculate avg grades per course, then the HAVING keeps only those with average less than 80. Based on our data:
	•	Quantum Mechanics average was 75, so that qualifies.
	•	Database Systems avg ~82.67, not <80.
	•	Algorithms 91, no.
	•	Shakespearean Literature 95, no.
So result would list “Quantum Mechanics – 75”.

If a group has no rows (like a course with no students yields no group in an inner join scenario), it’s not considered at all. If we left join courses to enrollments, those courses have null grade values which AVG() would ignore (AVG ignores nulls like most aggregates except COUNT(*)). Those courses would appear if we remove the JOIN filter, but their AVG would be null and the condition AVG(…) < 80 in HAVING would be false (null fails the comparison). So they wouldn’t appear either. We could adjust by using IFNULL or something to treat null as 0 if we wanted to include them in such comparisons, but that’s beyond scope.

Combining JOINs, GROUP BY, and HAVING

Often you’ll use joins to bring together data, then group and aggregate, then maybe filter groups. We’ve been doing that in some examples already.

Another example in e-commerce: “Which product category has the highest total sales revenue?”
	•	We’d join OrderItems to Products to get price, multiply price*quantity to get revenue per line, sum that per category, then maybe sort or find max.

Steps:
	1.	Join OrderItems to Products to get Price.
	2.	Calculate Revenue = Price * Quantity for each line (in SELECT or in an inner subquery).
	3.	Group by category, sum the revenue.
	4.	Use MAX or ORDER BY to find highest.

We can do:

SELECT p.Category, SUM(p.Price * oi.Quantity) AS TotalRevenue
FROM OrderItems oi
JOIN Products p ON oi.ProductID = p.ProductID
GROUP BY p.Category
ORDER BY TotalRevenue DESC;

This groups sales by category. In our small data, categories:
	•	Electronics: Laptop(899.991) + Smartphone(699.501) + Headphones(199.99*2) = 899.99 + 699.50 + 399.98 = 1,999.47
	•	Home Appliances: Coffee Maker 129.99*1 = 129.99
	•	Furniture: Desk Chair 249*1 = 249
	•	Groceries: Espresso Beans 15*3 = 45

So Electronics is highest (~1999.47). The query sorts descending so Electronics would be first.

We could add LIMIT 1 to just get the top category if needed.

A Note on COUNT(*), COUNT(column), and COUNT(DISTINCT)
	•	COUNT(*) counts rows, including those with NULLs in any columns (it doesn’t care about specific column).
	•	COUNT(column) counts non-NULL values of that column. If you know the column is never null (e.g., an ID in a joined side where a join might produce null if no match), then COUNT(column) effectively counts how many matched. In left joins, COUNT(joinedTable.ID) can act like counting how many matches existed per group.
	•	COUNT(DISTINCT column) gives the number of unique non-null values of that column in the group or whole set.
	•	Example: How many different countries do our customers come from?

SELECT COUNT(DISTINCT Country)
FROM Customers;

If our customers are from USA, Canada, UK, USA, USA, that yields 3.

Practice Exercises – Level 3 (GROUP BY and Aggregates)

Use the databases to answer these:
	1.	Orders per Customer: List each customer’s name and the number of orders they have placed. Include customers with 0 orders. Sort by the number of orders descending.
	2.	Products Sold Count: List each product name and the total quantity sold across all orders. (Use OrderItems and maybe join with Products for name). Sort by most sold first.
	3.	Average Grade by Department: For the university, find the average grade of enrollments for each department. (Hint: join Enrollments -> Courses to get department, group by department).
	4.	Top Student per Course: For each course, find the highest grade received and the name of the student(s) who achieved it. (This one’s a bit challenging: you might need a subquery or a two-step process: first find max grade per course, then join back to enrollment/student to get who had that grade. It might be easier to break it down, but give it a shot.)
	5.	Order Stats: Compute the total number of orders and the average number of items per order overall (not grouped by anything; just two aggregate numbers). (Hint: average items per order = total items ordered / total orders, which can be done with aggregates or by subquery.)

Solutions:
	1.	Orders per Customer:

SELECT c.Name, COUNT(o.OrderID) AS NumOrders
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerID
ORDER BY NumOrders DESC;

Explanation: We left join to include those with zero orders. Group by customer, count orders. Result:
	•	Alice Johnson – 2
	•	Bob Smith – 1
	•	Charlie Lee – 1
	•	Evelyn Garcia – 1
	•	Diana Evans – 0  (Diana appears with 0 because of left join and no matching orders)
Sorted descending: Alice first (2), then Bob/Charlie/Evelyn (1, tie – any order among them or alphabetical if same count), then Diana (0).

	2.	Total quantity sold per product:

SELECT p.Name, SUM(oi.Quantity) AS TotalSold
FROM Products p
LEFT JOIN OrderItems oi ON p.ProductID = oi.ProductID
GROUP BY p.ProductID
ORDER BY TotalSold DESC;

This includes products not sold (they’ll have NULL join, sum will treat null as 0). Based on our data:
	•	Headphones – 2
	•	Espresso Beans – 3
	•	(Actually Espresso Beans 3 is higher than 2, so Espresso Beans would be listed first with 3, then Headphones 2, then others 1 or 0.)
Let’s recount properly:
Electronics: Laptop 1, Smartphone 1, Headphones 2.
Others: Coffee Maker 1, Desk Chair 1, Espresso 3.
So Espresso Beans has 3 (highest), Headphones 2, others 1, anything unsold 0.
So sorted: Espresso Beans (3), Headphones (2), then tie of 1s (Laptop, Smartphone, Coffee Maker, Desk Chair), then any 0 (none in our data, since every product got sold at least once).

	3.	Average grade by department:

SELECT c.Department, ROUND(AVG(e.Grade), 1) AS AvgGrade
FROM Enrollments e
JOIN Courses c ON e.CourseID = c.CourseID
GROUP BY c.Department;

We join to get department for each enrollment, then group by department.
Grades:
	•	Computer Science dept: courses (DB Systems avg 82.67, Algorithms avg 91) but we need overall dept avg (that’s average of all grades in CS courses combined, which effectively is a weighted average by course enrollment counts or we can just compute by all enrollments in those courses). By simply grouping by department, we lump all CS enrollments together: those grades are 85,88,90,92,75 (Aaron, Beth in DB Sys and Alg, Cathy in DB Sys). Actually listing: Aaron(85,90,70 for QM but QM is Physics), Beth(88,92), Cathy(75,95 for Lit), David(80 for QM), rest. So for Computer Science: [85,88,90,92] (grades in CS courses: DB sys & Algo by Aaron, Beth, Cathy). Those average (85+88+90+92)/4 = 88.75.
	•	Literature dept: just one course (Shakespeare) with grade 95 -> avg 95.
	•	Physics dept: one course (QM) with grades 70 (Aaron) and 80 (David) -> avg 75.
	•	Arts dept: no enrollments (Art History none) -> this group likely won’t show up with an inner join. If we wanted it, left join and handling null needed. Here we did join so Arts not present.
The query as written will output:
Computer Science – ~88.8, Literature – 95, Physics – 75.
(I included ROUND(...,1) to maybe format it one decimal, optional.)

	4.	Top student per course:
This one’s more complex. One approach:
	•	Find max grade per course:

SELECT CourseID, MAX(Grade) AS TopGrade
FROM Enrollments
GROUP BY CourseID;


	•	This yields each course’s top grade.
	•	Now join this result with Enrollments and Students to get the student(s) who got that grade.
Putting it together using a subquery:

SELECT c.Title, s.Name AS Student, e.Grade
FROM Enrollments e
JOIN Students s ON e.StudentID = s.StudentID
JOIN Courses c ON e.CourseID = c.CourseID
JOIN (
    SELECT CourseID, MAX(Grade) AS TopGrade
    FROM Enrollments
    GROUP BY CourseID
) tg ON e.CourseID = tg.CourseID AND e.Grade = tg.TopGrade;

Explanation: The subquery tg finds top grade per course. We join it to enrollments on matching CourseID and grade equals TopGrade. That filters e to only those rows that have the top grade for their course. Then join to get student name and course title.
Result would be:
	•	Database Systems – Beth Brown – 88 (Beth had 88 vs Aaron 85, Cathy 75; 88 is max)
	•	Algorithms – Beth Brown – 92 (Beth 92 vs Aaron 90; 92 max)
	•	Shakespearean Literature – Cathy Clark – 95 (only student)
	•	Quantum Mechanics – David Doe – 80 (David 80 vs Aaron 70; 80 max)
	•	Art History – (no enrollment, thus not listed; if we wanted we could list with no student, but skip).
If multiple students tied for top, this would list them both (e.g., if two students shared the max grade in a course, both rows appear, which is correct).

	5.	Order stats (total orders, avg items per order):
We can do this with aggregates without grouping (treat whole table as one group):

SELECT COUNT(*) AS TotalOrders,
       CAST(SUM(oi.Quantity) AS REAL) / COUNT(DISTINCT oi.OrderID) AS AvgItemsPerOrder
FROM OrderItems oi;

Here I use COUNT(DISTINCT oi.OrderID) to count how many distinct orders appear in OrderItems (effectively number of orders that had items; presumably all orders do except possibly order 5 which had none, but order 5 won’t appear in OrderItems at all, so it wouldn’t be counted here – slight issue if we want to count that as an order with 0 items).
Alternatively, we know Orders count = 5 from Orders table, and total items = sum of all quantities. We could:

SELECT 
    (SELECT COUNT(*) FROM Orders) AS TotalOrders,
    (SELECT SUM(Quantity) FROM OrderItems) * 1.0 / (SELECT COUNT(*) FROM Orders) AS AvgItemsPerOrder;

This way, we count all orders (including those with no items) as denominator. In our data: total orders = 5. Sum of quantities: 1+2 (order1) + 1+1 (order2) + 1 (order3) + 3 (order4) + 0 (order5 has none) = 1+2+1+1+1+3 = 9. Average = 9/5 = 1.8 items per order.
If using the single query approach with OrderItems only, you’d get TotalOrders = 4 (because order 5 is absent), and sum quantity = 9, avg = 9/4 = 2.25 which is not accurate since it excluded the empty order. So the combined approach must consider orders with zero items. The subquery method above does that by counting from Orders table.
The answer: TotalOrders = 5, AvgItemsPerOrder = 1.8.
(If you used a different method that excluded empty orders, you might have said 2.25; but 1.8 is correct including the empty order.)

These exercises show the power of aggregation for summarizing data. You can now answer questions about totals, averages, etc., often combining it with joins and filtering groups via HAVING.

Level 4: Modifying Data – INSERT, UPDATE, DELETE

So far, we’ve focused on querying (SELECT) and reading data. In this level, we learn how to modify the data in the database:
	•	INSERT – add new rows to tables.
	•	UPDATE – change existing rows.
	•	DELETE – remove rows.

These commands, collectively part of DML (Data Manipulation Language), actually change the contents of the database.

⚠️ Warning: When experimenting, be careful with UPDATE/DELETE queries without a proper WHERE clause – you might accidentally affect all rows. It’s often wise to do a SELECT with the same condition first to verify which rows will be affected.

We’ll illustrate each using our sample databases (feel free to use a safe environment or transaction if you want to roll back changes; in SQLite, you can wrap in a transaction and rollback, or just re-run the creation script if needed).

INSERT – Adding New Records

The syntax for a basic insert is:

INSERT INTO TableName (Column1, Column2, ...) 
VALUES (Value1, Value2, ...);

You list the target columns and then provide corresponding values. If you omit the column list, you must provide values for all columns in the table, in their defined order.

Example 1: Add a new customer to Customers:

INSERT INTO Customers (Name, Email, Country)
VALUES ('Frank Zhang', 'frank@example.com', 'USA');

This creates a new customer row. Since CustomerID is an INTEGER PRIMARY KEY (autoincrement in SQLite default behavior for that alias type), it will auto-assign the next ID (which would be 6 if we had 5 existing customers). After this insert, if you do SELECT * FROM Customers, you’d see Frank Zhang added.

Example 2: Insert a new order for that new customer:

INSERT INTO Orders (OrderDate, CustomerID)
VALUES ('2025-06-10', 6);

Assuming Frank got CustomerID 6, this creates an order referencing him. The new OrderID will be next in sequence (6 if previously we had 5 orders). Always ensure the CustomerID exists (this is where foreign key constraints help if enabled – it would prevent inserting with a non-existent CustomerID).

Example 3: Insert multiple rows at once:

INSERT INTO Products (Name, Category, Price) VALUES
('Wireless Mouse', 'Electronics', 29.99),
('Office Desk', 'Furniture', 399.00);

This adds two new products in one statement. Many SQL dialects (including SQLite) allow inserting multiple rows by separating values tuples with commas.

INSERT from SELECT: You can also insert results of a query into a table, using INSERT ... SELECT ... syntax. For example, if we had another table and wanted to copy data or do an aggregated insert. We won’t deep dive, but be aware you can do INSERT INTO Table2 (col1, col2) SELECT colA, colB FROM Table1 ....

UPDATE – Changing Existing Rows

UPDATE allows modifying column values for rows that meet a condition:

UPDATE TableName
SET Column1 = newValue1,
    Column2 = newValue2, ...
WHERE someCondition;

Without a WHERE, all rows will be updated (so be careful!).

Example 1: A customer’s email changed:

UPDATE Customers
SET Email = 'alice.newemail@example.com'
WHERE CustomerID = 1;

This finds the customer with ID 1 (Alice) and updates her email.

Example 2: Give a 10% raise in price to all Electronics products:

UPDATE Products
SET Price = Price * 1.10
WHERE Category = 'Electronics';

This will increase the Price by 10% for any product where Category is Electronics. The expression Price * 1.10 uses the current value and updates it. (If any Electronics had a NULL price, that would result in NULL – but price likely not null here.)

Example 3: We realize we recorded the wrong customer for Order 5 (say it was actually Diana’s order, not Evelyn’s). We can update that order’s CustomerID:

UPDATE Orders
SET CustomerID = 4
WHERE OrderID = 5;

This changes OrderID 5 to point to Customer 4 (Diana). Now Diana would have 1 order and Evelyn would have 0 orders after this correction. (Without foreign key enforcement, nothing stops you from putting an invalid ID; if FKs are enforced and 4 exists, it’s fine. If you put 999 and no such customer, SQLite by default might allow it if PRAGMA foreign_keys not enabled, but logically it’s bad data.)

Mass Update Warning: If you run an update without a WHERE, e.g. UPDATE Orders SET CustomerID = 4;, it would set every order’s CustomerID to 4 – basically reassigning all orders to Diana. That’s likely not what you want. Always double-check the WHERE clause.

DELETE – Removing Rows

Delete syntax:

DELETE FROM TableName
WHERE condition;

As with update, omit WHERE at your own peril (that will delete all rows in the table!).

Example 1: Delete a customer (maybe one who asked to remove their data):

DELETE FROM Customers
WHERE CustomerID = 5;

This would remove Evelyn Garcia from Customers. If foreign key constraints are enabled and there are Orders referencing this customer, the deletion might be prevented or cascade depending on settings. By default, if using SQLite with foreign keys on and no cascade, you’d get an error if that customer still has orders. You would either have to delete their orders first or use cascading deletes if defined. For now, assume no FK or we handle dependencies manually.

Example 2: Delete all products in the ‘Groceries’ category (perhaps we are discontinuing groceries):

DELETE FROM Products
WHERE Category = 'Groceries';

This would remove Espresso Beans from Products. Note: if there are OrderItems referencing that ProductID, those references become orphaned unless foreign keys were set to cascade or we manually delete those order items too. Always consider related data.

Example 3: Delete all orders from before 2025:

DELETE FROM Orders
WHERE OrderDate < '2025-01-01';

This would check dates and remove orders older than the start of 2025. (In our data, none qualify since all our sample orders are in June 2025.)

Transactions (Brief Mention)

In SQLite (and other DBs), changes via insert/update/delete are auto-committed by default if you don’t explicitly start a transaction. You can group operations in a transaction:

BEGIN;
-- multiple inserts/updates/deletes
COMMIT;

If something goes wrong or you change your mind, you can do ROLLBACK; instead of COMMIT to undo changes in that transaction. This is useful to ensure either all or none of a set of changes apply (atomicity).

In DBeaver, you might have auto-commit on or off depending on settings. If off, you need to commit manually. Check DBeaver’s transaction settings if experimenting (it usually indicates if auto-commit is on in the status bar).

For simple use, just be aware that if you mess up an update/delete, you may need to reinsert data or so, because unless you explicitly wrapped in a transaction and rolled back, it’s applied. For learning purposes with small data, it’s fine; just document what changed or re-run your creation script to reset.

Practice Exercises – Level 4 (INSERT/UPDATE/DELETE)

Now some hands-on modifications (you can execute these on a copy of the data or in memory if you want to preserve original):
	1.	Add a Student: Insert a new student into the Students table: Name “George Green”, Major “Mathematics”. Then verify by selecting that student.
	2.	Course Correction: The course “Art History” was misclassified. Update its Department from “Arts” to “History”. (Courses table).
	3.	Enrollment Fix: Oops, we realize Emily Evans (student ID 5) actually decided to enroll in “Art History” after all. Insert an enrollment for Emily in course ID 5 with grade 87.
	4.	Order Cancellation: Customer Alice (ID 1) decided to cancel her second order (OrderID 2). Delete that order from Orders (and ensure its OrderItems are deleted too).
	5.	Price Increase: Increase the price of all products in category ‘Electronics’ by 5%. Show the before and after prices in a SELECT query for verification.

Solutions:
	1.	Add a Student:

INSERT INTO Students (Name, Major)
VALUES ('George Green', 'Mathematics');

This creates a new student. Since our existing had 5 students, George will get StudentID 6 (assuming auto-increment). We can SELECT * FROM Students WHERE Name='George Green'; to verify, expecting something like (6, George Green, Mathematics).

	2.	Course Department correction:

UPDATE Courses
SET Department = 'History'
WHERE Title = 'Art History';

Now if we SELECT Department FROM Courses WHERE Title='Art History';, it should show “History” instead of “Arts”.

	3.	Insert Emily’s enrollment:
First find CourseID of Art History. It should be 5 (since we inserted 5 courses and likely Art History was last with ID 5).

INSERT INTO Enrollments (StudentID, CourseID, Grade)
VALUES (5, 5, 87);

This adds a row linking Emily (5) to Art History (5) with grade 87. Now Emily is no longer without courses. If we query Enrollments for StudentID 5, we should see one record.
(If foreign keys on, ensure student 5 and course 5 exist – they do. If on older data where maybe we didn’t have course 5, adjust accordingly. Here, course 5 exists.)

	4.	Delete Alice’s second order:
OrderID 2 belongs to Alice. We must delete from OrderItems first or ensure cascade:

DELETE FROM OrderItems
WHERE OrderID = 2;
DELETE FROM Orders
WHERE OrderID = 2;

After this, OrderID 2 is gone. Checking Orders, we see only 1,3,4,5 remain. Alice now has one order left (order 1).
If we didn’t delete OrderItems first and had foreign keys with restrict, the Orders delete might fail. Without FK, the OrderItems would remain orphaned. Good practice: delete child rows then parent. In small examples, we do both.

	5.	Increase Electronics prices by 5%:

SELECT Name, Price FROM Products WHERE Category='Electronics';
-- note the prices for reference
UPDATE Products
SET Price = Price * 1.05
WHERE Category = 'Electronics';
SELECT Name, Price FROM Products WHERE Category='Electronics';

This sequence shows before and after. Originally:
	•	Laptop 899.99, Smartphone 699.50, Headphones 199.99.
After 5% increase (~1.05x):
	•	Laptop ~944.99, Smartphone ~734.48, Headphones ~209.99.
You can verify these in the second SELECT output. (The exact decimals might have more precision if not rounded, but roughly those values.)

Through these DML operations, we’ve seen how to modify the data. Typically, in an application, inserts happen when new business events occur (new order, new user signs up), updates when something changes (user updates profile, order status changes), deletes maybe for cleanup or if a record should be removed. Always use proper WHERE clauses to target the intended rows.

Now that we can change data, our next step is to learn how to design the database schema itself – creating tables, setting keys, and applying normalization principles.

Level 5: Schema Design Basics – CREATE TABLE, Keys, and Normalization (1NF–3NF)

In this level, we shift focus from data and queries to the structure of the database itself. We’ll cover:
	•	Creating tables with CREATE TABLE and defining column data types.
	•	Setting Primary Keys and Foreign Keys (and briefly, other constraints).
	•	Understanding the principles of Normalization (1st, 2nd, 3rd normal forms) which guide how we design table schemas to minimize redundancy and ensure data integrity.
	•	We’ll also apply these concepts with a small redesign scenario.

Creating Tables (CREATE TABLE)

The CREATE TABLE statement defines a new table’s structure: its columns, data types, and constraints (like keys).

General syntax:

CREATE TABLE TableName (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
    [table_constraints]
);

In SQLite, the common data types are INTEGER, REAL, TEXT, BLOB, and variations like VARCHAR(n) which behave like TEXT (SQLite is dynamic typed with type affinities). You can also specify DATETIME or use TEXT for date strings. SQLite is forgiving with types, but we’ll keep it simple. Other RDBMS like PostgreSQL have stricter types (INT, VARCHAR, DATE, etc. which you’d use correspondingly).

Primary Key: Uniquely identifies each row. Define with PRIMARY KEY constraint. If you want auto-incrementing integer, INTEGER PRIMARY KEY in SQLite does that. (In other systems, you might add SERIAL or use IDENTITY or a separate auto-increment clause.)

Foreign Key: References a primary key in another table. Syntax: FOREIGN KEY(column) REFERENCES OtherTable(OtherTablePK) possibly with actions like ON DELETE CASCADE etc. In SQLite, foreign keys enforcement requires PRAGMA foreign_keys = ON; to actually work, but we define them for design clarity.

Other constraints: NOT NULL (ensure a column cannot be NULL), UNIQUE (no duplicate values in that column across rows), CHECK (custom condition), etc.

Example: Let’s design a simple Employees and Departments schema:

CREATE TABLE Departments (
    DeptID   INTEGER PRIMARY KEY,
    Name     TEXT NOT NULL
);
CREATE TABLE Employees (
    EmpID    INTEGER PRIMARY KEY,
    Name     TEXT,
    DeptID   INTEGER,
    FOREIGN KEY(DeptID) REFERENCES Departments(DeptID)
);

Here:
	•	Departments table: DeptID as primary key, Name is required (NOT NULL).
	•	Employees table: EmpID primary key, Name, and DeptID which references Departments(DeptID). That means each employee optionally belongs to a department; the FK ensures if a DeptID is set, it must exist in Departments.

We could also declare primary key inline (as above) or separately:

CREATE TABLE Enrollments (
    StudentID INTEGER,
    CourseID  INTEGER,
    Grade     INTEGER,
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY(StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY(CourseID)  REFERENCES Courses(CourseID)
);

This one shows a composite primary key (StudentID, CourseID together unique) and two foreign keys.

When designing a table:
	•	Choose a primary key. Often an auto-increment integer (surrogate key) or a natural key (like a combination of fields that are naturally unique, e.g., perhaps a combination of codes).
	•	Define columns and pick appropriate data types and constraints (e.g., numeric types for numeric data, text for names, etc.).
	•	Use NOT NULL for fields that must always have a value (e.g., a Name might be NOT NULL, whereas a MiddleName might be NULLable).
	•	If some values have to be unique (like emails in a user table), you can add a UNIQUE constraint on that column.
	•	Add foreign keys for relationships to enforce referential integrity.

Let’s look at the Normalization aspect to see how to properly split data into tables.

What is Normalization?

Normalization is the process of organizing a database in a way that reduces redundancy and improves integrity ￼. There are several “normal forms” – rules that a schema can satisfy. The main ones to know:
	•	1st Normal Form (1NF): Ensure each column has atomic (indivisible) values and each row is unique. No repeating groups or arrays in a single column.
	•	2nd Normal Form (2NF): 1NF plus every non-key attribute is fully dependent on the entire primary key. This mainly applies when a table has a composite key: no partial dependency on part of that key.
	•	3rd Normal Form (3NF): 2NF plus no transitive dependencies – non-key attributes should not depend on other non-key attributes. In essence, every non-key should depend only on the primary key, not on some other column.

There are higher forms (BCNF, 4NF, etc.), but 1NF-3NF are usually sufficient for many designs.

In practice:
	•	1NF: design tables such that each field holds one value of the appropriate type. If you find yourself putting multiple pieces of info in one field (e.g., a comma-separated list of tags in one column), that violates 1NF – instead you might need a separate table.
	•	2NF: avoid having part of a composite key determine something on its own. For instance, if you had a table with primary key (StudentID, CourseID) and also stored StudentName, that’s bad – StudentName depends on StudentID alone, not on the combination. It should be in the Student table, not in the enrollment.
	•	3NF: avoid storing indirectly related info. E.g., if you have Employee table with DeptID, and also store DeptName in the Employee table, that DeptName is functionally dependent on DeptID (which is not the primary key of Employee), so it’s a transitive dependency. The DeptName belongs in Department table, not in Employee.

Another way to phrase:
	•	Eliminate repeating groups (1NF).
	•	Eliminate partial dependencies (2NF).
	•	Eliminate transitive dependencies (3NF).

Let’s illustrate normalization with a simple scenario:

Suppose we initially have a single table for a store that tracks orders, like:

OrdersTemp(OrderID, CustomerName, CustomerEmail, Product1, Product2, Product3, TotalAmount)

This is a bad design:
	•	If a customer places multiple orders, their name/email repeats in each row (redundancy).
	•	Product1, Product2, Product3 implies you only allow up to 3 products per order and you have repeating columns (not atomic if multiple products).
	•	If an order has fewer than 3 products, some fields might be null or unused – wasted space, awkward queries.
	•	TotalAmount can be derived from products’ prices, so storing it could be redundant (and risk inconsistency if someone updates items but not total).

Normalization approach:
	•	Break into multiple tables: Customers, Orders, OrderItems, Products.
	•	Customers table with CustomerID (PK), Name, Email.
	•	Products table with ProductID (PK), Name, Price, etc.
	•	Orders table with OrderID (PK), OrderDate, CustomerID (FK referencing Customers).
	•	OrderItems table with OrderID (FK), ProductID (FK), Quantity, perhaps Price each (or you can infer from Products but often you store a copy for historical price).
	•	Now each piece of info lives in one place: customer info in Customers, product info in Products. Order is a link between customer and items, and OrderItems holds potentially many product lines per order without limit.
	•	This eliminates duplicate storage of customer details on every order, and eliminates the fixed product column problem by having a separate table for line items (allowing any number of items per order via multiple rows).

We essentially applied 3NF:
	1.	No multi-valued columns (product list moved to child table).
	2.	The dependency of CustomerName on Order was removed (because CustomerName depends on Customer, not on order; we now store CustomerName only in Customer table).
	3.	Transitive dependency of, say, ProductName on Order (through ProductID) is removed by isolating Products.

1NF Violation Example: The earlier phone numbers example. If a table has columns like Phone1, Phone2, Phone3 for multiple phone numbers, or one column PhoneNumbers storing “123-4567, 234-5678”, that’s not 1NF. Fix: have a separate PhoneNumbers table (ClientID, PhoneNumber) with one number per row. Then each value is atomic.

A table violating 1NF by having multiple phone number fields. To normalize, separate phone numbers into individual records.

2NF Violation Example: Consider a table StudentCourses(StudentID, CourseID, StudentName, CourseName, Instructor). PK is (StudentID, CourseID).
	•	StudentName depends only on StudentID, not on the pair.
	•	CourseName, Instructor depend only on CourseID.
This table violates 2NF (partial deps). Fix: split into Students(StudentID, StudentName, …) and Courses(CourseID, CourseName, Instructor,…). And maybe an Enrollment table (StudentID, CourseID) as the relationship. Now StudentName is stored once in Students, CourseName once in Courses.

3NF Violation Example: Suppose an Employees table: (EmpID PK, Name, DeptID, DeptName). DeptName is dependent on DeptID (which is not PK here, EmpID is PK). So DeptName is transitively dependent on EmpID via DeptID. If you change a department name, you’d have to update all employees in that dept. To normalize, put DeptID, DeptName in a Department table, and just store DeptID in Employees. Now DeptName can be obtained by joining, and is stored in one place.

Normalization ensures:
	•	Data Integrity: updates and deletions have fewer anomalies. (E.g., if a student changes name, one row in Students table vs updating possibly many Enrollment rows if name was duplicated).
	•	No duplicate data: saves space and avoids inconsistency.
	•	Simpler schema maintenance: each fact is stored in one place.

There is sometimes a trade-off: too much normalization can lead to many tables and complex queries (joins everywhere). For read-heavy scenarios (like reporting), sometimes a bit of denormalization is used for performance. But as a beginner, aim for 3NF schema. You can always derive or cache combined info in views or reports rather than store it redundantly.

Summary of Steps to Design a Normalized Schema:
	1.	Identify entities (things like Customer, Order, Product, etc.) – these often become tables.
	2.	Identify attributes of each entity – these become columns in those tables.
	3.	Identify relationships between entities (one-to-many, many-to-many):
	•	One-to-many: add a foreign key column in the “many” side table (e.g., Orders has CustomerID).
	•	Many-to-many: create a join table linking the two (like Enrollment linking Students and Courses, or OrderItems linking Orders and Products).
	4.	Assign primary keys for each table (e.g., synthetic IDs or meaningful compound keys as appropriate).
	5.	Ensure no repeating groups or multi-valued fields (1NF).
	6.	If a non-key attribute depends only on part of a composite key (2NF violation), separate into a different table.
	7.	If a non-key attribute depends on another non-key (transitive), separate that out (3NF).
	8.	Add foreign key constraints to represent relationships (and enforce referential integrity).
	9.	Consider other constraints (UNIQUE, NOT NULL, etc.) based on requirements (e.g., email unique, quantity not negative, etc., possibly via CHECK constraints or application logic).

Let’s do a small case study: Clinic/Hospital (this foreshadows Level 8 too, but we can outline it here in terms of schema design).

Entities in a clinic might include:
	•	Patients (with details like PatientID, Name, DOB, Contact info).
	•	Doctors (DoctorID, Name, Specialty, etc.).
	•	Appointments (AppointmentID, Date, Time, PatientID, DoctorID, maybe Reason, etc.).
	•	Possibly Departments or ClinicLocations or MedicalRecords but let’s keep it basic.

Relationships:
	•	A patient can have many appointments. A doctor can have many appointments. An appointment is with one patient and one doctor (assuming one doctor per appointment).
	•	That’s a many-to-many between patients and doctors resolved through appointments (because over time, a patient sees many doctors, a doctor sees many patients, but each appointment links one of each).
	•	If we needed to allow multiple doctors per appointment (like a surgery with a team), that changes the model to have an appointment table and a separate table linking doctors to appointments (but we’ll assume one doctor per appointment for simplicity).

Design tables:

CREATE TABLE Patients (
    PatientID   INTEGER PRIMARY KEY,
    Name        TEXT NOT NULL,
    DateOfBirth TEXT,         -- stored as text 'YYYY-MM-DD' or could use numeric
    Phone       TEXT,
    Address     TEXT
    -- etc.
);
CREATE TABLE Doctors (
    DoctorID    INTEGER PRIMARY KEY,
    Name        TEXT NOT NULL,
    Specialty   TEXT,
    Phone       TEXT
);
CREATE TABLE Appointments (
    ApptID      INTEGER PRIMARY KEY,
    ApptDate    TEXT NOT NULL,   -- date or datetime
    ApptTime    TEXT NOT NULL,   -- or incorporate into ApptDateTime
    PatientID   INTEGER NOT NULL,
    DoctorID    INTEGER NOT NULL,
    Reason      TEXT,
    FOREIGN KEY(PatientID) REFERENCES Patients(PatientID),
    FOREIGN KEY(DoctorID)  REFERENCES Doctors(DoctorID)
);

This is a normalized design:
	•	Patient info only in Patients.
	•	Doctor info only in Doctors.
	•	Appointment holds references to those, and any appointment-specific info.
	•	We don’t store patient name in Appointments (we can get it via join). We don’t store doctor specialty in Appointments either.
	•	If we want to know how many patients each doctor has seen, we can query appointments grouping by DoctorID etc.

Normalization check:
	•	1NF: Each column holds a single value (Name, not multiple names in one, etc.), and we’ve separated what could be multi-valued (e.g., if a patient can have multiple contact numbers, we might have a separate table for contact numbers).
	•	2NF: Appointments’ PK is ApptID (surrogate). No composite PK here, so 2NF trivially.
	•	3NF: Non-key fields in Appointments (Reason) depend only on ApptID, that’s fine. In Patients, fields like Name, DOB all depend on PatientID (the key). No transitive dependency inside one table (like we haven’t stored Doctor’s specialty in Appointments or something that depends on DoctorID).
	•	We avoid redundancy: Doctor’s phone or Patient’s address stored once.
	•	If a patient changes phone, update one table. If a doctor changes specialty, update one table.

Normalization often also helps avoid anomalies:
	•	Insert anomaly: In a bad design, you might not be able to insert a record without some unrelated info. E.g., if we had a combined table of doctors and patients maybe, you couldn’t add a new doctor until they see a patient. By separating, you can add a doctor with no appointments yet, or a patient with no appointments yet.
	•	Update anomaly: In a redundant design, updating one instance but forgetting another causes inconsistency. E.g., if customer email was stored in orders too, updating it in Customers but not in past Orders leaves inconsistency. Normalization solves that by one place storage.
	•	Delete anomaly: In a poor design, deleting one record might inadvertently remove valuable data. E.g., if you had a combined student+enrollment table, deleting the last course enrollment for a student might remove their name entirely from the database if that was the only place it was stored. With separate tables, you can delete enrollment and still have student record.

Normalization principle is: Each fact is stored in one place.

When to Denormalize (Briefly)

While not part of the asked content, just know that sometimes for performance or convenience, a design might intentionally break 3NF. For example, storing a totals column that can be derived (like storing TotalAmount in Orders rather than always computing via sum of items). That’s okay if done carefully, usually with triggers or app logic to update it, to avoid inconsistency. In general, you normalize first, then denormalize only if needed and understand the trade-off.

Practice Exercises – Level 5 (Schema Design & Normalization)
	1.	Identify Keys: Looking at the University schema (Students, Courses, Enrollments), identify the primary keys of each table and the foreign keys in the Enrollments table. Explain in plain words the relationships.
	2.	Create Table SQL: Write a CREATE TABLE statement for a Departments table that has DeptID (integer primary key), DeptName (text, not null), and perhaps a unique constraint on DeptName (assuming no two departments have the same name).
	3.	Normalization Check: The following table is part of a library system: BooksTemp(BookID, Title, AuthorName, AuthorEmail, Genre, AuthorPhone). The library allows multiple books per author. What normal form problems do you see, and how would you redesign the schema?
	4.	Normalization Scenario: You’re designing a database for a music collection. Initially you have a single table: SongsTemp(SongID, SongTitle, AlbumTitle, ArtistName, ArtistGenre, AlbumReleaseYear). What redundancies or anomalies can arise? Propose a 3NF design (what tables might you separate this into?).
	5.	Foreign Key Use: Consider the e-commerce schema (Customers, Orders, OrderItems, Products). Why is it important to use foreign keys between Orders and Customers, and between OrderItems and Orders/Products? What could go wrong if we didn’t have those constraints (e.g., orphan records, inconsistent data)?

Solutions:
	1.	Keys in University schema:
	•	Students table: Primary Key is StudentID (each student has a unique ID).
	•	Courses table: Primary Key is CourseID.
	•	Enrollments table: Primary Key is a composite of (StudentID, CourseID) – assuming a student can enroll in a course only once, that pair is unique.
	•	Foreign keys in Enrollments: StudentID in Enrollments is a foreign key referencing Students(StudentID); CourseID in Enrollments references Courses(CourseID).
	•	Relationships: Each enrollment links one student to one course. A student can have many enrollments (many courses), a course can have many enrollments (many students). So it’s a many-to-many between Students and Courses resolved by the Enrollments table. The keys enforce that any StudentID in Enrollments must exist in Students, and any CourseID in Enrollments must exist in Courses.
	2.	CREATE TABLE Departments:

CREATE TABLE Departments (
    DeptID    INTEGER PRIMARY KEY,
    DeptName  TEXT NOT NULL UNIQUE
);

	•	This creates DeptID as primary key. DeptName is text, required (NOT NULL) and must be unique across rows (UNIQUE) to avoid duplicate department names.

	3.	BooksTemp normalization:
	•	Issues: The BooksTemp table includes author information (Name, Email, Phone) alongside each book. If an author has written multiple books, their contact info is repeated in each row – violating normalization (data redundancy, transitive dependency).
	•	If the author changes email or phone, we’d have to update it in every book row by that author (update anomaly).
	•	If we remove the last book of an author, we lose the author’s contact info (delete anomaly).
	•	To normalize, separate Authors into its own table and link to Books:
	•	Authors(AuthorID, Name, Email, Phone, etc., PRIMARY KEY AuthorID).
	•	Books(BookID, Title, Genre, AuthorID, PRIMARY KEY BookID, FOREIGN KEY(AuthorID) references Authors).
	•	Now each author’s info is stored once in Authors. Books just reference AuthorID. This is at least 3NF: Book’s non-key fields (Title, Genre) depend only on BookID; Author info is moved out to its own table.
	•	If a book can have multiple authors (co-authors), that’s a many-to-many which would need an AuthorBooks linking table, but if we assume one author per book, the above suffices.
	4.	SongsTemp normalization:
	•	In SongsTemp, we have SongTitle, AlbumTitle, ArtistName, ArtistGenre, AlbumReleaseYear all in one table. Redundancies:
	•	If an artist has multiple songs, the ArtistName and ArtistGenre repeat for each song.
	•	If multiple songs are from the same album, AlbumTitle and ReleaseYear repeat for each song.
	•	This invites anomalies: changing an album’s title or year would require updating all its songs’ rows; adding a new song by a new artist requires duplicating artist info in that row; deleting the last song of an album would lose the knowledge of that album existing.
	•	3NF Design:
	•	Artists table (ArtistID, Name, Genre, etc., PK ArtistID).
	•	Albums table (AlbumID, Title, ReleaseYear, ArtistID (if each album has one artist, or if compilation, then maybe a separate relation table; typically an album has one main artist or band so we can put ArtistID here), PK AlbumID, FK ArtistID references Artists).
	•	Songs table (SongID, Title, AlbumID, maybe also ArtistID if some songs are by guest artists, but if an album defines the artist, we might just go through album; simplest: Song references Album, which references Artist).
	•	So an Artist has many Albums, an Album has many Songs. If some songs have a different featured artist, that complicates beyond normal scope; assume not for now.
	•	This eliminates duplication:
	•	Artist info stored once in Artists.
	•	Album info stored once in Albums (with a link to Artist).
	•	Songs store minimal info (title) plus link to album (and through album to artist).
	•	To get a song’s artist name or album year, you join with those tables.
	•	Data integrity: One album’s year is in one place; one artist’s genre in one place.
	5.	Importance of Foreign Keys in e-commerce:
	•	Foreign keys between Orders and Customers ensure that you cannot have an order with a CustomerID that doesn’t exist in Customers. This maintains referential integrity: every order belongs to a valid customer.
	•	Without this, one could insert an Order with a non-existent CustomerID (a typo, for instance), leading to an orphan order that can’t be linked to a real customer. That’s bad for data consistency (you have an order but no customer details).
	•	Foreign key between OrderItems and Orders ensures no order item exists without a corresponding Order (prevents orphan line items). Similarly, OrderItems to Products ensures every product referenced in an order must exist in the Products table. This prevents scenarios like an order item referencing a product that’s been deleted or was never in the catalog.
	•	Without these, you could end up with:
	•	OrderItems that refer to OrderID 999 that isn’t in Orders (or an order that was deleted), meaning you have items that don’t tie to any actual order – data inconsistency.
	•	Or OrderItems with a ProductID that’s not in Products, meaning you have a sale record for a product that’s not in your product list (how to know its name or price?).
	•	Also, cascading rules: If you delete a Customer, you might want automatically to delete their orders (ON DELETE CASCADE) or prevent deletion if orders exist (ON DELETE RESTRICT) depending on business rules. Foreign keys let you specify that. Without them, one could accidentally delete the customer row and leave their orders orphaned.
	•	In summary, foreign keys enforce the logical relationships at the database level, catching mistakes that violate those relationships and thus protecting data integrity.

With a solid understanding of schema design and normalization, we can ensure our databases are structured efficiently and consistently. Next, we’ll see how to import or export data and then cover indexing and optimization in SQLite.

Level 6: Data Import/Export – Using CSVs or JSON with SQLite (via DBeaver)

In real-world usage, you often need to get data into or out of your database. Common formats for data exchange are CSV (Comma-Separated Values) and JSON (JavaScript Object Notation). SQLite (and DBeaver) provide ways to import and export data in these formats.

We will discuss:
	•	Importing a CSV file into a SQLite table.
	•	Exporting query results or tables to CSV.
	•	A note on JSON handling in SQLite.
	•	How to do these tasks conveniently in DBeaver.

Importing Data from CSV into SQLite

CSV Import via DBeaver:
DBeaver has an intuitive import tool:
	1.	Prepare a CSV file. For example, suppose we have new_products.csv:

Name,Category,Price
"Gaming Keyboard","Electronics",59.99
"Standing Desk","Furniture",299.00
"Air Purifier","Home Appliances",199.50


	2.	To import this into the Products table:
	•	In DBeaver’s Database Navigator, right-click on the target connection or database and choose Import Data.
	•	Choose “CSV” as the import format and select the CSV file.
	•	DBeaver will prompt for mapping the CSV columns to table columns. Map “Name”->Name, “Category”->Category, “Price”->Price.
	•	You can preview the data and then execute the import.
	•	DBeaver will internally either run a series of INSERT statements or use its CSV driver to load the data.
	•	After completion, do a SELECT * FROM Products to confirm the new rows.
In summary, DBeaver’s import wizard guides you through selecting source (CSV file) and target (existing table or new table), mapping columns, and importing.

CSV Import via SQLite CLI:
If not using DBeaver, SQLite’s command-line has a .import command:

.mode csv
.import 'path/to/new_products.csv' Products

This assumes the CSV columns align with table columns. If the table has an autoincrement PK, you might need to provide a blank value or handle accordingly (or use INSERT in a loop).

Create table from CSV on the fly: In DBeaver, you can also import CSV to a new table if needed by creating a table with matching columns, or DBeaver can create it for you.

Things to watch:
	•	Ensure data types match (text vs numbers, etc.). If a CSV column is blank but the table column is NOT NULL, import will fail or that row will be skipped.
	•	Watch out for CSV formatting issues (commas in quoted strings, etc. – the import tool usually handles standard quoting).
	•	For large CSVs, it may take time; DBeaver provides progress.

Example: Importing a list of new students from a CSV:

Name,Major
"Helen Hunt","Physics"
"Ian Ivy","Literature"

Use the import tool to map Name->Name, Major->Major in Students table. The StudentID will auto-assign for each inserted row. After import, those new students appear with new IDs.

Exporting Data to CSV (or Excel)

DBeaver makes exporting query results easy:
	•	After running a query, in the results grid, you can right-click and choose Export Results -> as CSV (or XLSX, etc.).
	•	You select the format (CSV), file path, and options (like include headers, delimiter, etc.).
	•	Save and you have a CSV of the data.

Alternatively, right-click a table in the navigator, choose Export Data:
	•	Select the table (or multiple tables), choose format (CSV).
	•	Configure options and execute. DBeaver will dump the table data to file.

Example: You want to export all orders with customer names to share with someone as CSV.

SELECT o.OrderID, o.OrderDate, c.Name AS Customer, SUM(oi.Quantity * p.Price) AS OrderTotal
FROM Orders o
JOIN Customers c ON o.CustomerID = c.CustomerID
JOIN OrderItems oi ON o.OrderID = oi.OrderID
JOIN Products p ON oi.ProductID = p.ProductID
GROUP BY o.OrderID;

Run that in DBeaver, then export the results to “orders_report.csv”. The CSV will have columns: OrderID, OrderDate, Customer, OrderTotal.

In SQLite CLI, you could do:

.mode csv
.headers on
.once 'orders_report.csv'
SELECT ...;

This directs output of that query to a CSV file (with headers).

JSON Import/Export

JSON Export: If you need JSON output (say for a web app), SQLite doesn’t directly export JSON but you can use DBeaver or a little scripting:
	•	DBeaver can export as JSON by choosing JSON format in the export dialog. It will produce a JSON array of objects.
	•	Or in code (Python, etc.), you fetch results and serialize to JSON.

JSON Import: SQLite (with the JSON1 extension) can actually query JSON data and even store JSON in TEXT columns, but importing JSON directly to tables isn’t as straightforward as CSV. DBeaver’s UI doesn’t directly “import JSON to table” unless you use some custom script or if the JSON is structured like an array of objects corresponding to rows.

However, you can:
	•	Write a small script or use an intermediary tool to parse JSON and insert into DB.
	•	Or if using SQLite CLI and JSON1, you could create a virtual table or use the json_each function to iterate JSON.

For beginners, easiest is to convert JSON to CSV if possible, then import CSV. Or write inserts.

Example: If you had a JSON file of products:

[
  { "Name": "Pen", "Category": "Stationery", "Price": 1.2 },
  { "Name": "Notebook", "Category": "Stationery", "Price": 3.5 }
]

To import this, you might either:
	•	Use a scripting language (Python, JavaScript) to read JSON and insert into DB via SQL.
	•	Or in DBeaver, perhaps manually copy the values into an INSERT statement.

DBeaver does have a feature to handle some JSON via the MongoDB/NoSQL capabilities, but that’s beyond our scope.

Bottom line: CSV is the most straightforward for tabular data import/export with SQLite/DBeaver. JSON is often used via application code rather than direct DB import, unless the DB itself is a JSON store (SQLite is relational but can store JSON in a column if needed).

Using SQLite’s .mode csv and .import (Advanced CLI tip)

We already touched on .import. Another helpful tip:
	•	Before importing, you may want to set .separator , if not default comma, or specify .nullvalue for what text should be interpreted as NULL.
	•	Ensure the table exists with the correct schema for the CSV columns, or create accordingly.

Practice Exercises – Level 6 (Import/Export)

These are more conceptual since actual import/export requires environment setup:
	1.	Export to CSV: How would you export the contents of the Students table to a CSV file using DBeaver? Describe the steps.
	2.	Import from CSV: Suppose you have a CSV of new courses:

Title,Department,Credits
"Database Security","Computer Science",3
"Gothic Literature","Literature",4

Describe how to import this into the Courses table.

	3.	Verifying Import: After importing data, what SQL query could you run to verify that the new rows are added correctly (use the above course example as context)?
	4.	JSON Export Use Case: If asked to provide a JSON of all customers (with their details) to a web developer, how might you generate that from the database?
	5.	Bulk Data Load Consideration: You have a CSV with 50,000 product records to import. What are some tips to ensure the import goes smoothly (consider things like transactions, indexing off during import, etc.)? (This is more open-ended, thinking of performance.)

Solutions:
	1.	Export Students to CSV with DBeaver:
	•	In DBeaver, right-click on the Students table in the navigator and choose Export Data (or open the table, then use the export option).
	•	Select CSV as the output format.
	•	Choose the destination file path, e.g., students_export.csv.
	•	Ensure the option “Include column headers” is checked if you want headers.
	•	Click Next/Finish to execute. DBeaver will save the table’s rows into the CSV file.
	•	(Alternatively, run SELECT * FROM Students, then in results right-click -> Export results -> CSV.)
	•	Confirm by opening the CSV in a text editor or spreadsheet to see the student data.
	2.	Import new courses CSV into Courses table:
	•	In DBeaver, right-click on the connection/database and choose Import Data.
	•	For source, select CSV file and pick the new_courses.csv.
	•	For target, select the Courses table.
	•	Map columns: “Title” -> Title, “Department” -> Department, “Credits” -> Credits.
	•	DBeaver shows a preview; ensure data types align (Credits should map to INT if that’s the type).
	•	Click through to finish the import. DBeaver will insert the two new course rows into Courses.
	•	(If doing via SQLite CLI, you’d ensure Courses table exists then use .mode csv and .import 'new_courses.csv' Courses.)
	3.	Verifying import of new courses:
After import, run a query to see if the new courses are present. For example:

SELECT * FROM Courses 
WHERE Title IN ('Database Security','Gothic Literature');

This should return the rows for “Database Security” and “Gothic Literature” with their Department and Credits. Check that Department and Credits match the CSV (Computer Science,3 and Literature,4 respectively). If they appear, the import succeeded. Alternatively, do SELECT Title, Department, Credits FROM Courses ORDER BY CourseID DESC LIMIT 2; assuming these were the last two added.

	4.	Provide JSON of customers:
	•	Easiest is to export via DBeaver: Run SELECT * FROM Customers (or specific fields) -> in results, right-click Export -> choose JSON as format. Save to customers.json.
	•	That JSON file will contain an array of customer objects like:

[
  {"CustomerID":1,"Name":"Alice Johnson","Email":"alice@example.com","Country":"USA"},
  ...
]


	•	If using code: one might connect to SQLite, query all customers, then use a programming language (Python with json module, etc.) to serialize to JSON.
	•	SQLite doesn’t natively output JSON from the shell easily (you could manually format, but DBeaver or a script is simpler).
	•	So basically, I’d use DBeaver’s JSON export feature to produce the JSON needed.

	5.	Bulk import (50k products) considerations:
	•	Use Transactions: Wrapping the import in a single transaction can dramatically speed it up. If DBeaver’s import doesn’t do it automatically, you could manually begin a transaction, run import, and commit. SQLite without transactions commits each insert, which is slow for 50k rows. With one big commit, it’s much faster.
	•	Disable Indexes (if any) during import: If the Products table has indexes (say on ProductName or something), inserting 50k rows will update the index for each insert, slowing it down. If possible, drop or disable indexes, import the data, then recreate the indexes. This can make bulk load faster.
	•	Ensure CSV cleanliness: Make sure no format issues (commas in fields properly quoted, etc.) that could cause errors mid-import. Also ensure data types in CSV are consistent (e.g., numeric fields contain only numeric data or blank for NULL).
	•	Sufficient disk space: 50k rows isn’t huge for SQLite, but ensure disk isn’t full.
	•	Memory and DBeaver settings: For very large imports, sometimes using SQLite’s CLI or .import might be more efficient. But 50k is fine through DBeaver typically.
	•	Check for constraints: If there are foreign key constraints, ensure the referenced data exists. If products refer to categories via foreign key, import might fail if a row has a category that isn’t in the categories table. So import order might matter (import all categories first, then products).
	•	After import: Run ANALYZE (in SQLite) to update statistics if needed, especially if you added a lot of data and want query planner to be up-to-date.
	•	In summary, open a transaction, import, then commit, and consider index optimization for speed. DBeaver’s UI might handle some of this, but being aware helps if performance is an issue.

Having learned how to import/export data, next we’ll look at indexing and performance tuning, which becomes important as data volume grows.

Level 7: Indexes and Performance Tuning (SQLite focus)

As databases grow, query performance becomes critical. Indexes are a key tool for speeding up data retrieval. We’ll discuss:
	•	What an index is and how it works (conceptually).
	•	How to create and use indexes in SQLite.
	•	The trade-offs (indexes speed up reads, but can slow down writes and take space).
	•	Some SQLite-specific tips (like the query planner, EXPLAIN QUERY PLAN, etc., to see if indexes are used).

What is an Index?

An index is essentially a sorted data structure (usually a B-tree in SQLite) that allows fast lookup of rows by the values of one or more columns. Think of it like an index in a book: instead of flipping through every page to find a term, you go to the index which is sorted and quickly find pages.

In a table without an index (except the rowid or primary key index that exists by default for the primary key):
	•	A query like SELECT * FROM Customers WHERE Name = 'Alice'; has to scan through all customers and check each name (this is a full table scan, O(n) time).
	•	If we create an index on Name, the database can use a binary search on that index to find ‘Alice’ quickly (O(log n) time).

An index in SQLite maps the indexed column values to the rowid (or primary key) of the table. So it can find the rowid of matching records quickly, then retrieve the row.

Creating Indexes

Syntax:

CREATE INDEX index_name ON TableName(column1, [column2, ...]);

	•	You can index one column or a combination (composite index).
	•	Example: Index on Customers Name:

CREATE INDEX idx_customers_name ON Customers(Name);


	•	Example: If we often query orders by date:

CREATE INDEX idx_orders_date ON Orders(OrderDate);



When to create indexes:
	•	On columns used frequently in WHERE clauses or JOIN conditions.
	•	On columns used in ORDER BY or GROUP BY (indexes can help sort efficiently).
	•	Primary keys are automatically indexed (no need to create one for PK).
	•	Foreign keys often benefit from index (for join performance, e.g., Orders.CustomerID might get an index if we often look up orders by customer).

Using Indexes

Once created, SQLite’s query planner will automatically use an index if it deems it helpful. You don’t have to mention the index in the query; just do the same SELECT queries, and if the index can speed it up, SQLite will use it.

Example: After CREATE INDEX idx_customers_name ON Customers(Name), the query:

SELECT * FROM Customers WHERE Name = 'Evelyn Garcia';

will likely use the index on Name to directly locate Evelyn’s record, rather than scanning all.

Multiple column index: If we have CREATE INDEX idx_orders_customer_date ON Orders(CustomerID, OrderDate), SQLite can use this index for a query with WHERE CustomerID = ? AND OrderDate > ? for example, or just CustomerID = ?. (The index on multi-columns can be used for a prefix of the indexed columns in queries; e.g., it can serve a query filtering just by CustomerID, but not one filtering just by OrderDate unless CustomerID is also in the condition, due to how composite index ordering works).

Performance Gains

An index makes lookups much faster especially on large tables:
	•	Without index: finding a specific value is O(n) linear.
	•	With index: ~O(log n). For 1,000,000 rows, log2(1,000,000) ~ 20, so ~20 comparisons instead of 1,000,000.

Also helps for sorting: If you ORDER BY Name and have an index on Name, SQLite might use the index order instead of sorting data after retrieval.

Coverage Index: If an index covers all the columns a query needs (e.g., you have an index on (LastName, FirstName) and query SELECT FirstName FROM People WHERE LastName='Smith'), SQLite might not even need to hit the table – it can answer from the index alone. This is called a covering index.

Trade-offs and Considerations
	•	Space: Indexes take up disk space (essentially another copy of the indexed data plus pointers).
	•	Write penalty: When you INSERT, UPDATE, or DELETE, any affected indexes must be updated. This makes writes slightly slower and can add overhead on large bulk operations. If you have many indexes, insertions can slow down.
	•	So, don’t index everything by default; index what you query on often.
	•	Too many indexes: Wastes space and can confuse the planner or overhead maintenance.
	•	Index on small table: If a table is very small, a full scan is fine and an index might not yield noticeable benefit.
	•	Selectivity: Indexes are most beneficial when the indexed column has high cardinality (many distinct values). For columns with few distinct values (like a boolean flag), an index might not be useful because e.g., half the rows have value = true, the DB might just scan rather than bounce between many index entries.

SQLite Query Planner and EXPLAIN

You can ask SQLite how it plans to execute a query:

EXPLAIN QUERY PLAN SELECT * FROM Orders WHERE CustomerID = 3;

This outputs something like:

QUERY PLAN
--SEARCH TABLE Orders USING INDEX idx_orders_customer_id (CustomerID=?)

This indicates it will use an index search. If it said USING INDEX, good; if it says SCAN TABLE Orders, that means it’s doing full scan (maybe no index available or decided not to use).

In DBeaver SQL editor, you might need to run EXPLAIN QUERY PLAN ... and look at output (or use GUI explain plan if available).

Example: After creating idx_orders_date, do:

EXPLAIN QUERY PLAN SELECT OrderID FROM Orders WHERE OrderDate = '2025-06-02';

It should indicate using idx_orders_date. If we drop that index and try again, it would show a full table scan.

When indexes are not used:
	•	If the query optimizer thinks the index wouldn’t help (e.g., if you’re selecting a huge fraction of the table, a sequential scan can be just as good).
	•	If your WHERE clause doesn’t match the index in a left-anchored way (like having an index on (A,B) but you query WHERE B = x without A, it might not use that index).
	•	If functions are used on the column in WHERE (like WHERE LOWER(Name) = 'alice' won’t use index on Name unless an appropriate functional index is created).
	•	If the stats mislead the planner, but SQLite’s auto-analyze tries to keep stats updated.

Maintaining Indexes:
	•	SQLite automatically updates indexes on data changes.
	•	You might occasionally run ANALYZE; which updates statistics that help the planner decide on index use.
	•	If performance is puzzling, you can always check query plan or even do benchmarks (time with index vs without).
	•	Remove indexes that don’t justify themselves to avoid overhead.

Practical Tuning in SQLite:
	•	Ensure PRAGMA cache_size is reasonable (not usually an issue for moderate usage).
	•	Use EXPLAIN QUERY PLAN to verify indexes are used as expected.
	•	Avoid suboptimal patterns – e.g., LIKE '%term' cannot use an index (leading wildcard). But LIKE 'term%' can utilize an index with COLLATE NOCASE maybe.
	•	For large inserts, as discussed, you might drop indexes then recreate (if inserting millions of rows, that’s faster).
	•	Use transactions to batch writes.

Practice Exercises – Level 7 (Indexing and Performance)
	1.	Identify Candidates: Given the e-commerce database, name two queries that would benefit from an index and specify on which column(s) you’d create the index. (For example, “Looking up orders by OrderDate” -> index on OrderDate in Orders.)
	2.	Create Index SQL: Write the SQL to create an index on the Students table for the Major column (assuming we often query students by major).
	3.	Measure Impact: Suppose the Orders table has 100k rows. Explain in simple terms how a query WHERE CustomerID = 5 improves with an index versus without (in terms of how many rows are examined).
	4.	Check Plan: If you have an index on (CustomerID, OrderDate) in Orders and run SELECT * FROM Orders WHERE CustomerID=5 ORDER BY OrderDate;, how does the index help for this query?
	5.	Downsides: What are the potential downsides of having an index on every column in every table? Why not just index everything?

Solutions:
	1.	Queries that benefit from an index (e-commerce):
	•	Query: “Find all orders on a specific date.” This would benefit from an index on the Orders.OrderDate column. With an index, the database can directly jump to the entries for that date instead of scanning all orders.
	•	Query: “List all order items for a given OrderID (joining OrderItems with Orders).” An index on OrderItems.OrderID would speed up retrieving all items for a particular order, as it can quickly find entries matching that OrderID instead of scanning the entire OrderItems table.
	•	Another: “Find product by name.” If we allow searching products by exact name, an index on Products.Name would help. Or if queries filter products by category, an index on Category in Products would help.
	•	Essentially, columns used in WHERE frequently (CustomerID in Orders, ProductID in OrderItems for joins, etc.) are candidates.
	2.	Create index on Students(Major):

CREATE INDEX idx_students_major ON Students(Major);

This will index the Major field. So queries like SELECT * FROM Students WHERE Major = 'Computer Science'; will benefit, getting quickly all CS students via the index rather than scanning all students.

	3.	Orders 100k rows, CustomerID=5 with vs without index:
	•	Without index: The database would perform a full table scan of 100,000 rows, checking each row’s CustomerID to see if it’s 5. So it examines 100k rows.
	•	With an index on CustomerID: The database uses the index (which is likely a B-tree keyed by CustomerID) to directly locate entries for CustomerID=5. If, say, customer 5 has 500 orders, it might perform on the order of log2(100000) ≈ 17 comparisons to find the start, then traverse ~500 entries. So maybe on the order of a few hundred row accesses total, instead of 100k.
	•	In simple terms: With index, it “jumps” to customer 5’s orders; without, it “looks at every order one by one.” So index drastically reduces work if customer 5’s orders are a small subset.
	4.	Index (CustomerID, OrderDate) used for SELECT with ORDER BY:
	•	The index on (CustomerID, OrderDate) is sorted first by CustomerID, then by OrderDate for that customer.
	•	The query WHERE CustomerID=5 ORDER BY OrderDate can fully utilize this index:
	•	It will go to the index, find the first entry where CustomerID=5, then read through in index order which already orders by OrderDate.
	•	This means it can retrieve all order IDs (or rowids) for customer 5 in ascending date order directly from the index without an extra sort step.
	•	Essentially, the index serves both the filtering and the sorting. So the DB fetches rows in the correct order as it scans the index range for CustomerID=5.
	•	This is much faster than scanning all orders and sorting, obviously. Also faster than using separate index for CustomerID then sorting those results (because it’s doing both in one go).
	5.	Downsides of indexing everything:
	•	Slower writes: Every INSERT, DELETE, or relevant UPDATE requires updating all indexes. If every column had an index, an insert into a table with, say, 10 indexes means 10 index insertions. This can significantly slow down insert or update performance.
	•	Storage overhead: Indexes consume disk space and memory (in cache). Indexing every column could double or triple the storage usage of the data (depending on column sizes).
	•	Query planner complexity: Too many indexes can sometimes confuse the optimizer or increase planning time (though minor). Also, some indexes might never be used, just sitting there overhead.
	•	Maintenance overhead: For bulk operations or batch loads, having many indexes slows it down; you might have to drop and recreate indexes, which is extra work.
	•	Many columns might not be searched often or benefit from an index (e.g., a boolean flag – indexing it might not help if ~half rows are true/false because queries still return large portions).
	•	In short, indexes are not free; you index what is necessary for performance. Index everything and you degrade performance for inserts and get diminishing returns on reads where the index isn’t selective or the query doesn’t need it.

So it’s about balance: index selectively. The aim is the minimal set of indexes that covers your frequent and performance-critical queries.

With indexing covered, we can now move on to a comprehensive real-world modeling exercise in the next level, consolidating what we’ve learned.

Level 8: Real-World Data Modeling Case Study – Designing a Complete Clinic Database

Now for a capstone exercise: designing and building a full database for a clinic (healthcare scenario). We’ll walk through the steps of modeling requirements into a schema, applying normalization, and then writing the SQL to create tables and perhaps some example queries.

Scenario: We need to manage a clinic’s data. Requirements (assume a small outpatient clinic):
	•	We have Patients with personal details.
	•	We have Doctors (physicians) with their details and specialties.
	•	Patients make Appointments with doctors on certain dates/times.
	•	We want to record some details of appointments (like reason for visit, maybe diagnosis or notes).
	•	Optionally, doctors belong to departments (like Cardiology, Pediatrics), and maybe patients have insurance info (we can include if needed).

Essentially:
	•	Patients ↔ Appointments ↔ Doctors is the main relationship (appointments link patients and doctors).
	•	Possibly a doctor can have many patients and a patient can see many doctors (many-to-many through appointments as discussed in Level 5).

Let’s model it step by step (some done earlier in Level 5 but here we formalize and possibly expand):

Entities and Tables:
	1.	Patients – store each patient’s info.
	•	Attributes: PatientID, Name, DateOfBirth, Phone, Address, etc.
	•	PatientID will be primary key (INTEGER).
	•	We want to ensure maybe unique combination of Name+DOB? (Not necessarily, could have two John Smith born same day theoretically, but if needed, we could add something, but we’ll skip unique enforcement aside from ID.)
	2.	Doctors – store doctor info.
	•	Attributes: DoctorID, Name, Specialty, Phone, maybe DepartmentID if department is separate.
	•	DoctorID primary key.
	3.	Departments (if we include) – optional, but maybe to demonstrate a one-to-many:
	•	DeptID, DeptName.
	•	A Doctor can belong to one Department (e.g., Dr. Adams in Cardiology).
	•	If included, Doctors table would have DeptID as foreign key.
	4.	Appointments – the core of scheduling.
	•	Attributes: ApptID, Date, Time (or DateTime combined), PatientID, DoctorID, Reason, Notes.
	•	Primary key: ApptID (each appointment unique).
	•	Foreign keys: PatientID references Patients, DoctorID references Doctors.
	•	We may also ensure a doctor can’t double-book the same slot or patient can’t overlap, but such constraints might require application logic or at least a composite unique index on (DoctorID, Date, Time) to prevent double-booking a doctor at same time. We can mention that.
	5.	Possibly Medications/Prescriptions or Visits if we wanted to track treatments or prescriptions given at appointments, but that might be too deep; let’s focus on appointments.
	6.	MedicalRecords or Diagnoses could be attached to appointments as well, but let’s assume notes cover it.

So final tables: Patients, Doctors, Departments (optional), Appointments.

We ensure normalization:
	•	Patients just holds patient info.
	•	Doctors holds doctor info (with DeptID if any).
	•	Appointments references IDs only, doesn’t store any redundant patient or doctor info directly.
	•	Department table holds dept names (no repeating dept names in multiple doctor rows, etc.).

Creating the Schema:

CREATE TABLE Departments (
    DeptID      INTEGER PRIMARY KEY,
    DeptName    TEXT NOT NULL UNIQUE
);

CREATE TABLE Patients (
    PatientID   INTEGER PRIMARY KEY,
    Name        TEXT NOT NULL,
    DateOfBirth TEXT,          -- stored as 'YYYY-MM-DD'
    Phone       TEXT,
    Address     TEXT
    -- Other fields: Email, Insurance, etc. can be added
);

CREATE TABLE Doctors (
    DoctorID    INTEGER PRIMARY KEY,
    Name        TEXT NOT NULL,
    Specialty   TEXT,
    Phone       TEXT,
    DeptID      INTEGER,       -- optional, can be NULL if no dept
    FOREIGN KEY(DeptID) REFERENCES Departments(DeptID)
);

CREATE TABLE Appointments (
    ApptID      INTEGER PRIMARY KEY,
    ApptDate    TEXT NOT NULL,   -- 'YYYY-MM-DD'
    ApptTime    TEXT NOT NULL,   -- 'HH:MM' maybe, or include seconds
    PatientID   INTEGER NOT NULL,
    DoctorID    INTEGER NOT NULL,
    Reason      TEXT,
    Notes       TEXT,
    FOREIGN KEY(PatientID) REFERENCES Patients(PatientID),
    FOREIGN KEY(DoctorID)  REFERENCES Doctors(DoctorID)
    -- We might add: UNIQUE(DoctorID, ApptDate, ApptTime) to prevent a doctor having two appts at same time
);

We would execute these CREATE TABLE statements in SQLite (via DBeaver SQL editor all at once or sequentially). They set up the schema.

Normalization check:
	•	1NF: All columns atomic (no lists in one column, etc.). Good.
	•	2NF: No composite PK except perhaps in uniqueness constraint on (DoctorID, ApptDate, ApptTime) but that’s not a primary key, just a constraint. Our actual PKs are single columns, so 2NF is fine.
	•	3NF: In Appointments, non-key fields (Reason, Notes) depend only on ApptID. In Doctors, non-keys (Name, Specialty, Phone, DeptID) depend on DoctorID. DeptName is in Departments table not repeated in Doctors – so yes, 3NF. Patient info only in Patients.

No transitive dependencies (DeptName via DeptID in Doctors is resolved by putting DeptName in Dept table).

Example Data (to illustrate usage):
We can insert some sample data:

-- Departments
INSERT INTO Departments (DeptName) VALUES ('Cardiology'),('Pediatrics'),('General');

-- Doctors
INSERT INTO Doctors (Name, Specialty, Phone, DeptID) VALUES
('Dr. Alice Heart', 'Cardiologist', '555-1001', 1),  -- Cardiology dept
('Dr. Bob Childs', 'Pediatrician', '555-1002', 2),
('Dr. Charlie Care', 'General Practitioner', '555-1003', 3);

-- Patients
INSERT INTO Patients (Name, DateOfBirth, Phone, Address) VALUES
('John Doe', '1980-05-15', '555-2001', '123 Maple St'),
('Jane Smith', '1990-08-22', '555-2002', '456 Oak Ave');

-- Appointments
INSERT INTO Appointments (ApptDate, ApptTime, PatientID, DoctorID, Reason, Notes) VALUES
('2025-07-01', '09:00', 1, 1, 'Chest pain', 'Initial consult, ordered ECG'),
('2025-07-01', '10:00', 2, 1, 'Follow-up', 'Review test results'),
('2025-07-02', '14:00', 2, 2, 'Annual check-up', NULL);

This populates:
	•	Department 1: Cardiology, 2: Pediatrics, 3: General.
	•	Dr. Alice (Cardiologist, dept 1), Dr. Bob (Pediatrician, dept 2), Dr. Charlie (GP, dept 3).
	•	Two patients: John (ID 1), Jane (ID 2).
	•	Appointments: John with Dr. Alice on 2025-07-01 09:00, Jane with Dr. Alice at 10:00 same day, Jane with Dr. Bob on 2025-07-02 14:00.

We could imagine Dr. Charlie might get an appointment too later.

Sample Queries:
	•	List all upcoming appointments with patient and doctor names:

SELECT a.ApptDate, a.ApptTime, p.Name AS Patient, d.Name AS Doctor, a.Reason
FROM Appointments a
JOIN Patients p ON a.PatientID = p.PatientID
JOIN Doctors d ON a.DoctorID = d.DoctorID
WHERE a.ApptDate >= date('now')   -- assuming current date for "upcoming"
ORDER BY a.ApptDate, a.ApptTime;

This joins across and would show each appointment line with human-readable names.
	•	Find all appointments for a given patient:

SELECT a.ApptDate, a.ApptTime, d.Name AS Doctor, a.Notes
FROM Appointments a
JOIN Doctors d ON a.DoctorID = d.DoctorID
WHERE a.PatientID = 2;  -- for Jane Smith (PatientID 2)

Would list her appointments (with Dr. Alice and Dr. Bob as per our data).
	•	Count how many patients each doctor has seen (distinct patients perhaps):

SELECT d.Name, COUNT(DISTINCT a.PatientID) AS PatientCount
FROM Doctors d
LEFT JOIN Appointments a ON d.DoctorID = a.DoctorID
GROUP BY d.DoctorID;

This uses a LEFT JOIN so that if a doctor has no appointments, they still appear (with count 0). DISTINCT patient count to count unique patients per doctor.
	•	If we had an index need: likely index on Appointments (PatientID) and (DoctorID) since we filter by those frequently. Also maybe composite on (DoctorID, ApptDate, ApptTime) for enforcing uniqueness and aiding scheduling queries by doctor.

Ensuring no anomalies:
	•	If a doctor leaves (deleted from Doctors), what about their appointments? With the current foreign key, by default SQLite will prevent deleting a doctor if appointments reference them (unless we specified ON DELETE CASCADE). We might add ON DELETE SET NULL or CASCADE depending on desired behavior. Possibly, if a doctor leaves, we still keep appointments for record but mark doctor null or cascade deletion.
	•	Could do: FOREIGN KEY(DoctorID) REFERENCES Doctors(DoctorID) ON DELETE SET NULL to preserve appointment but without a doctor.
	•	If a patient is deleted, similarly ensure appointments handled. Perhaps ON DELETE CASCADE for patients (delete their appointments too, if not needed anymore). But likely we wouldn’t delete patients normally, rather mark inactive.

We won’t digress too far; these are design decisions.

The design as given satisfies our main requirement: capturing clinic visits scheduling info in normalized tables.

Final SQL and Recap

Combining what we wrote:
We have the SQL for table creation above. That and some example data and queries demonstrate the final model.

This Level 8 exercise shows how to apply all prior concepts: identifying entities (tables), relationships (FKs), designing for 3NF (no duplicate data between tables), sample inserts (DML), and queries (select/join/aggregate) to utilize the schema.

We successfully built a small but realistic database for a clinic, which could be expanded with further detail (prescriptions, billing, etc. as additional tables if needed) following the same approach.

Additional Section: Using AI (ChatGPT) as a SQL Learning and Debugging Companion

Modern tools like ChatGPT (large language models) can be very helpful while learning SQL or troubleshooting queries – essentially acting as an intelligent assistant. Here’s how you can leverage an LLM effectively:
	•	Explain Concepts: If you come across a concept you don’t understand (e.g., “What is a foreign key?” or “Explain GROUP BY in simple terms”), you can ask ChatGPT for an explanation. It can provide quick, digestible summaries or examples in a conversational way.
	•	Generate Example Queries: If you’re stuck on syntax, you might prompt: “How do I write an SQL query to [task]?” For instance, “Write a query to find the top 5 customers by number of orders.” ChatGPT can often produce a correct or nearly-correct query, which you can study and adapt.
	•	Interactive Practice: You can use ChatGPT as a practice partner. For example, try writing a query and then ask ChatGPT, “Is this query correct? Can it be optimized?” It might spot errors or suggest improvements (like adding an index or rewriting a JOIN).
	•	Debugging Assistance: If you have an error message or a query that’s not working as expected, you can describe the problem to ChatGPT. E.g., “I’m getting a syntax error near ‘GROUP’ in my SQL query: [include query]. What might be wrong?” ChatGPT can often pinpoint common syntax issues (like a missing comma or misordered clause) and suggest fixes.
	•	Learning by Examples: Ask for examples: “Show me an example of a LEFT JOIN and explain the result.” Seeing examples with explanation can reinforce understanding.
	•	Alternative Solutions: You can challenge ChatGPT: “Is there a different way to achieve this result?” It might show you a different approach (maybe using a subquery vs. a join, or using window functions if appropriate). This broadens your perspective.
	•	Use for Explanations of Query Plans: If you use EXPLAIN and see something you don’t understand (e.g., “Using temporary; Using filesort” in MySQL or similar in SQLite), you can ask what that means and how to possibly eliminate it (like “Using filesort” indicates a sort that might be optimized by adding an index). ChatGPT can clarify such terms and provide tuning tips.

Caution and Best Practices:
	•	Always double-check the answers, especially for syntax specifics. ChatGPT’s suggestions are usually correct in logic but might have minor syntax mistakes occasionally (like a missing quote or misuse of a specific SQL dialect’s quirk). Use it as a guide, not absolute truth.
	•	Use small, focused questions. If you ask a very broad question (“How do I design a database?”), you’ll get a general answer. Instead, ask specific things (“Which columns should I index for query X?”).
	•	If something seems off, you can follow up. E.g., “Are you sure that will return distinct results? Could there be duplicates?” The interactive nature can refine answers.
	•	Keep in mind that ChatGPT doesn’t run queries on your actual database or see your data. If debugging a result discrepancy, you need to describe the situation. For example: “My count of orders is off because duplicate order items are being counted. How do I count unique orders?” ChatGPT might then suggest COUNT(DISTINCT OrderID).

Example of using ChatGPT for assistance:
	•	Learning assistant: “Explain the difference between INNER JOIN and LEFT JOIN with a simple example.” – ChatGPT might produce a clear explanation with a mini table example, helping you solidify the concept.
	•	Prompt formulation: “I have two tables, Customers and Orders. How can I get a list of customers who have no orders?” ChatGPT might respond: “Use a LEFT JOIN or NOT EXISTS. For example:

SELECT c.Name 
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
WHERE o.OrderID IS NULL;

This finds customers with no matching orders.”
It explains the approach. You can try it and see if it works.

	•	Debugging: You write a query and get an error “ambiguous column name”. You tell ChatGPT: “I have a query joining Customers and Orders, selecting ‘CustomerID’, but I got ‘ambiguous column’ error.” ChatGPT might explain: “Both tables have a CustomerID column. You need to qualify it, e.g., Customers.CustomerID or alias the tables and use alias.column.” So it provides a fix.

Overall, ChatGPT can accelerate learning by providing on-demand explanations, examples, and corrections. It’s like having a tutor who is available 24/7 to answer questions or give hints. Of course, practicing by actually writing and running queries is irreplaceable, but using an AI assistant can clarify doubts quickly and reduce frustration when stuck on a problem.

Using AI for SQL - A Note of Encouragement: Don’t hesitate to use such tools to supplement your learning. The more you explain your problem clearly to the AI (providing relevant details), the better help you’ll get. It’s a skill in itself to ask good questions – which mirrors the skill of precisely formulating problems in programming/SQL.

By leveraging AI companions thoughtfully, you can deepen your understanding and solve problems faster, while still ensuring you learn the underlying concepts.

Additional Section: Mini-Projects and Challenges for Each Level (with Solutions)

Let’s reinforce each level’s concepts with some challenges:

Level 0 Challenge: Database Basics

Challenge 0.1: Describe in your own words the difference between a table and a database.
Solution 0.1: A database is a collection of organized data, which can contain multiple tables. A table is a structured set of data within the database, organized into rows and columns ￼. For example, a library database might have tables for Books, Members, and Loans. The database is the whole collection, and each table holds data about one kind of thing.

Challenge 0.2: If you have a spreadsheet of employees and their departments, what advantages do you get by using a relational database with separate Employee and Department tables, compared to one big table?
Solution 0.2: Using separate tables avoids duplication of department information (each department name is stored once, not repeated for every employee) ￼. It ensures data integrity – e.g., you can’t assign an employee to a non-existent department (with a foreign key). Updates are easier (change a department name in one place). And you can flexibly query, like listing all employees in a department via a join, rather than filtering a single big table. Overall, it reduces redundancy and potential anomalies (update/delete issues) by normalization.

Level 1 Challenge: Core SELECT Queries

Challenge 1.1: Write a query to find the names of all products that cost more than 500, from the Products table.
Solution:

SELECT Name 
FROM Products
WHERE Price > 500;

(This will list product names with price > 500. We select just Name as asked.)

Challenge 1.2: Get a unique list of all countries where customers are from (Customers table).
Solution:

SELECT DISTINCT Country 
FROM Customers;

This uses DISTINCT to eliminate duplicates, so each country appears once ￼.

Challenge 1.3: Show the first 3 students (by StudentID ascending) from the Students table.
Solution:

SELECT * 
FROM Students
ORDER BY StudentID
LIMIT 3;

This sorts by StudentID and takes the first 3 rows.

Challenge 1.4: Find all orders (OrderID) that were placed on ‘2025-06-05’.
Solution:

SELECT OrderID 
FROM Orders
WHERE OrderDate = '2025-06-05';

(We assume dates stored as text ‘YYYY-MM-DD’; adjust format if needed.)

Level 2 Challenge: Joins

Challenge 2.1: Write a query to list each order with the customer’s name and order date.
Solution:

SELECT o.OrderID, o.OrderDate, c.Name AS CustomerName
FROM Orders o
JOIN Customers c ON o.CustomerID = c.CustomerID;

This performs an inner join, showing OrderID, date, and matching customer name.

Challenge 2.2: Find all students who are not enrolled in any course (use the university schema).
Solution: There are a couple ways (LEFT JOIN or subquery). Using LEFT JOIN:

SELECT s.Name
FROM Students s
LEFT JOIN Enrollments e ON s.StudentID = e.StudentID
WHERE e.CourseID IS NULL;

After a left join, students with no enrollment have NULL in Enrollments fields. We filter those.

Challenge 2.3: Using the e-commerce schema, list every product that has ever been ordered (i.e., appears in OrderItems) along with the total quantity ordered.
Solution: We need Products joined to OrderItems, grouping by product:

SELECT p.Name, SUM(oi.Quantity) AS TotalOrdered
FROM Products p
JOIN OrderItems oi ON p.ProductID = oi.ProductID
GROUP BY p.ProductID;

This inner join lists only products that have matches in OrderItems (products with at least one order) and sums quantities.

(If we wanted to include products never ordered, we’d use LEFT JOIN and handle NULL, but the phrasing says that has ever been ordered, so inner join is fine.)

Challenge 2.4: For each department, list the department name and doctor names in that department (clinic schema from Level 8).
Solution:

SELECT d.DeptName, dr.Name AS DoctorName
FROM Departments d
LEFT JOIN Doctors dr ON d.DeptID = dr.DeptID
ORDER BY d.DeptName;

This will list each department, and each doctor in it (multiple rows per dept if multiple docs, and departments with no docs will have NULL DoctorName - though with left join we still list the dept). The question didn’t specify including depts with no doctors; with LEFT JOIN we include them (with DoctorName null). If we only wanted departments that have doctors, an INNER JOIN would suffice.

Level 3 Challenge: Aggregates and GROUP BY

Challenge 3.1: How many orders did each customer make? List customer name and order count.
Solution:

SELECT c.Name, COUNT(o.OrderID) AS OrdersCount
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerID;

We use LEFT JOIN to include customers with zero orders. Group by each customer. COUNT of OrderID counts how many orders (it will count nulls as 0 appropriately, because COUNT(column) only counts non-nulls, and for customers with no orders all joined OrderIDs are null).

Challenge 3.2: In the University DB, find the average grade in each course. Show course title and average grade.
Solution:

SELECT c.Title, AVG(e.Grade) AS AvgGrade
FROM Courses c
JOIN Enrollments e ON c.CourseID = e.CourseID
GROUP BY c.CourseID;

This groups by course and averages grades. (Courses with no enrollments will not appear due to inner join; that might be acceptable here. If we wanted them with null avg, we’d left join and handle, but not asked.)

Challenge 3.3: Using the clinic database, find the number of appointments each doctor has, and the number of distinct patients each doctor has seen.
Solution:

SELECT d.Name AS Doctor, COUNT(a.ApptID) AS NumAppointments,
       COUNT(DISTINCT a.PatientID) AS NumPatients
FROM Doctors d
LEFT JOIN Appointments a ON d.DoctorID = a.DoctorID
GROUP BY d.DoctorID;

We left join so that doctors with no appointments show 0 and 0. We use COUNT(DISTINCT patient) for distinct patients.

Challenge 3.4: Identify any product categories that have more than 5 products (e-commerce). (Show category and count.)
Solution:

SELECT Category, COUNT(*) AS NumProducts
FROM Products
GROUP BY Category
HAVING COUNT(*) > 5;

This groups products by category and filters groups where count > 5. Those categories have more than 5 products. (If none, result empty.)

Level 4 Challenge: Data Modification

Challenge 4.1: Add a new customer “Greg Gonzalez” from Canada (Customers table).
Solution:

INSERT INTO Customers (Name, Email, Country)
VALUES ('Greg Gonzalez', 'greg@example.com', 'Canada');

Now Greg is in the Customers table with a new CustomerID (auto-assigned).

Challenge 4.2: A product’s price was entered incorrectly. Update the product “Coffee Maker” to have price 149.99 instead of 129.99.
Solution:

UPDATE Products
SET Price = 149.99
WHERE Name = 'Coffee Maker';

This finds the Coffee Maker row and updates its price.

Challenge 4.3: Remove all enrollments of student ID 3 (maybe they dropped all courses).
Solution:

DELETE FROM Enrollments
WHERE StudentID = 3;

This deletes all rows in Enrollments with StudentID 3. Now student 3 has no courses (but still exists in Students table unless we deleted them too).

Challenge 4.4: Insert a new appointment in the clinic DB for patient 2 with doctor 3 on 2025-08-01 at 15:00, reason “Check-up”.
Solution:

INSERT INTO Appointments (ApptDate, ApptTime, PatientID, DoctorID, Reason)
VALUES ('2025-08-01', '15:00', 2, 3, 'Check-up');

We leave Notes NULL. This schedules that appointment (assuming no conflict as per our data).

Challenge 4.5: Due to a data entry error, all customers had their country set to ‘USA’. Write an update to set all customer countries to NULL (we’ll fix them manually later).
Solution:

UPDATE Customers
SET Country = NULL;

(Be cautious: this updates every customer row – but that was intended here.)

Level 5 Challenge: Schema Design

Challenge 5.1: Explain why having a single table Enrollments(StudentID, StudentName, CourseID, CourseName) is inferior to having separate Students and Courses tables with an Enrollment link table.
Solution: In the single-table design, the student name repeats for every course they’re enrolled in, and course name repeats for every student taking it, leading to redundancy. If a student’s name changes, you’d have to update multiple rows. It violates 2NF and 3NF because StudentName depends only on StudentID (partial dep) and CourseName on CourseID. The separate tables ensure each piece of data (student name, course name) is stored once (in its own table), and Enrollment just references them, eliminating duplicate storage and anomalies.

Challenge 5.2: You have a table OrdersTemp(OrderID, OrderDate, CustomerName, CustomerEmail). Suggest a normalized schema.
Solution:
	•	Create a Customers table: (CustomerID, Name, Email, …) with CustomerID PK.
	•	Create an Orders table: (OrderID, OrderDate, CustomerID) with OrderID PK and CustomerID FK referencing Customers.
	•	Now CustomerName and Email stored only in Customers, and Orders refer to customer by ID.
	•	This avoids repeating Name/Email for each order, and allows having a single record for a customer even if they have many orders.

Challenge 5.3: In the clinic database, could we merge Doctors and Departments into one table? Why or why not?
Solution: We could store DeptName in each Doctor row, but that would repeat the department name for every doctor in that department (redundancy). By keeping Departments separate, the department info is stored once and referenced. If a department name changes, one update in Departments versus updating multiple doctor rows (transitive dependency if merged, since DeptName depends on DeptID which is not PK in a doctor row). Also, having a separate Departments table ensures referential integrity (only valid department names can be used via FK). So merging would violate normalization (likely 3NF) and cause update anomalies.

Challenge 5.4: What is the primary key of the Appointments table in the clinic schema, and what foreign keys does it have?
Solution: Primary key is ApptID (each appointment unique). Foreign keys: PatientID referencing Patients(PatientID), and DoctorID referencing Doctors(DoctorID). These enforce that an appointment must link to a valid patient and doctor.

Level 6 Challenge: Import/Export

Challenge 6.1: If you have a CSV file of Patients (Name, DOB, Phone, Address), how can you import it into the Patients table using DBeaver?
Solution: In DBeaver, right-click the connection or Patients table and choose Import Data. Select the CSV file as source. Map CSV columns to Patients table columns (Name->Name, etc.). Execute the import wizard. DBeaver will insert each CSV row as a new record in Patients. After import, run a SELECT on Patients to verify the new entries. Alternatively, use SQLite CLI .import command if using the shell.

Challenge 6.2: You want to share all product data with someone who doesn’t have database access. How do you export the Products table to an Excel file?
Solution: In DBeaver, right-click Products table -> Export Data. Choose Excel format (XLSX). Follow prompts to specify file path and options. Finish to generate an Excel file of the products. Or export as CSV which Excel can open. DBeaver makes it straightforward by just selecting format and destination.

Challenge 6.3: After importing 100 new records, what SQL query could you run to ensure they imported (assuming you know something about them)?
Solution: If I know, say, all new records have Country=‘Canada’ or IDs above a certain range, I could query SELECT COUNT(*) FROM Patients WHERE Country='Canada'; before and after to see the increase. Or if new records have a pattern (like all new products category = ‘NewCategory’), do SELECT * FROM Products WHERE Category='NewCategory'; to see they are there. Generally, verify by selecting on distinctive attributes of imported data. Also SELECT COUNT(*) after import vs before to see the count difference equals expected number of rows.

Level 7 Challenge: Indexes

Challenge 7.1: The Orders table is frequently queried by CustomerID. Write the SQL to create an index to optimize that.
Solution:

CREATE INDEX idx_orders_customer ON Orders(CustomerID);

Now queries like WHERE CustomerID = ? will use this index.

Challenge 7.2: Suppose you have an index on Orders(CustomerID). If 90% of orders are from the same customer (ID 1), will the index be very helpful to find customer 1’s orders?
Solution: Not particularly – if one customer accounts for 90% of rows, a query for CustomerID=1 will have to retrieve 90% of the table. The index can find where those entries start, but it will then retrieve a huge number of rows. A full table scan might be about as efficient. Indexes are most useful when the filter is selective (small subset). So for the majority case (customer 1), the index doesn’t save much scanning, but for other customers with fewer orders it helps. The query planner might even decide not to use the index for customer 1 if it estimates that cost > full scan.

Challenge 7.3: We add an index on Appointments(ApptDate). How does that help a query SELECT * FROM Appointments WHERE ApptDate = '2025-07-01'?
Solution: The index on ApptDate allows SQLite to quickly locate all appointments on July 1, 2025 without scanning all appointments. It will do a range lookup in the index for that date and retrieve matching rowids, then fetch those rows. This is much faster than checking every appointment date in the table. It essentially binary searches (or navigates B-tree) by date.

Challenge 7.4: True or False: Creating an index will never slow down database operations. Explain.
Solution: False. Indexes can slow down write operations (inserts, deletes, updates) because the index must be updated whenever data changes. Also, having many indexes can slow bulk inserts and consume more storage. Indexes speed up reads (SELECT queries using those columns), but there’s a trade-off with maintenance overhead on writes.

Level 8 Challenge: Clinic Case Study

Challenge 8.1: In the clinic DB, enforce that a doctor cannot have two appointments at the exact same date and time. How would you implement that?
Solution: One way is to add a unique constraint on (DoctorID, ApptDate, ApptTime) in the Appointments table. For example:

CREATE UNIQUE INDEX idx_unique_doc_slot 
ON Appointments(DoctorID, ApptDate, ApptTime);

SQLite doesn’t allow multi-column UNIQUE via index? Actually in SQLite you can do multi-col unique index or specify as table constraint:

CREATE TABLE Appointments (
  ...,
  UNIQUE(DoctorID, ApptDate, ApptTime)
);

This ensures no two rows have the same doctor and same date-time. Another approach could be in application logic, but at DB level unique constraint is best.

Challenge 8.2: If a patient is deleted, what happens to their appointments as per our current foreign key setup? How might you change it if you wanted to automatically delete their appointments too?
Solution: Currently, we didn’t specify ON DELETE, so by default SQLite’s foreign key would prevent deleting a patient if related appointments exist (no action), or if foreign keys are off, it would allow but leave orphan appointments. To automatically delete appointments when a patient is deleted, we can set cascading delete on the foreign key:

FOREIGN KEY(PatientID) REFERENCES Patients(PatientID) ON DELETE CASCADE

This way, removing a patient will cascade and remove all their appointments as well (assuming PRAGMA foreign_keys=ON). Similarly, could do for DoctorID if wanted.

Challenge 8.3: Write a SQL query to find all doctors who have seen more than 10 distinct patients. (clinic DB)
Solution:

SELECT d.Name, COUNT(DISTINCT a.PatientID) AS patient_count
FROM Doctors d
JOIN Appointments a ON d.DoctorID = a.DoctorID
GROUP BY d.DoctorID
HAVING COUNT(DISTINCT a.PatientID) > 10;

This finds doctors with more than 10 unique patients. If we want to include doctors with no appointments, use LEFT JOIN but then they won’t meet >10 anyway.

Challenge 8.4: Assuming we added a Medications table and a link between Appointments and Medications (many-to-many: e.g., appointment prescriptions), outline how that would fit in the schema (just describe tables and keys briefly).
Solution: We could have:
	•	Medications table: MedID, Name, perhaps other details (PK MedID).
	•	A join table, say Prescriptions or AppointmentMedications: linking Appointments and Medications:
	•	Columns: ApptID, MedID, maybe Dosage, etc.
	•	Primary key could be composite (ApptID, MedID) if an appointment won’t prescribe same med twice.
	•	Foreign key ApptID -> Appointments, MedID -> Medications.
This way, an appointment can have multiple medications prescribed, and a medication can be prescribed in many appointments. This is a normalized many-to-many linking table.

That concludes the challenges and their solutions, reinforcing each level’s lessons. By diligently working through these, you solidify your understanding and are ready to tackle real database problems!