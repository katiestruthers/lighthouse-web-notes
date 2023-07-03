# Lecture 16: Intro to SQL

### So far, how have we stored data in our apps?
* In-memory via variables
* Files via NodeJS 'fs' module

### Pros of In-Memory Data
* Quick and easy to set-up
* Can be faster to read/write from memory
* Variables are easy to retrieve and worth with, as it is all JS
* No need for other software/packages/etc.

### Cons of In-Memory Data
* Not persistant, as it disappears when we shutdown the server
* Difficult to save or share this data
* Adds code file bloat &rarr; lack of seperation of concerns, mixing our code base with our data
* Limited to the RAM afforded by the hardware

### Benefits of Using Databases
* Higher performance
* More consistent, safer handling of data

### DBMS - Database Management System
* Software we use to store data and work with relational databases
* Examples:
  * PostgreSQL (what we will use in this course!)
  * Microsoft SQL Server (MSSQL)
  * MariaDB
  * MySQL
  * Oracle

### The Relational Database Model
The relational database model contains the following:
1. A database / collection, which contains many...
2. Tables containing columns / format for its records, with one or more...
3. Rows / Records / Entities, with data in each column

* **Primary Key** &rarr; the main unique identifier for information in a particular table
  * This is often indexed and is a simple value (integer / serial)
  * If item is deleted, we do not re-use the primary key
* **Foreign Key** &rarr; represents a record in another table (references that other table's PK)

### SQL (Structured Query Language)
* Developed in the 1970s as a human-readable way to interact with data
* DDL &rarr; Data Definition Language
  * When we are setting up our database, tables, and columns
* DML &rarr; Data Manipulation Language
  * Requesting, organizing, storing, editing, deleting data

### Basic SQL Syntax
```SQL
-- This is an SQL comment!
-- They are only one line...
-- ..so if you want multiple lines, you'll need to add two dashes each time.

-- Select everything from the authors table
SELECT * FROM authors;

-- Select only the name column from the authors table
SELECT name FROM authors;
```

* While not strictly necessary, it is common practice to capitalize the SQL commands and lower-case the column names, table names, etc.
* As long as there is at least one space between words, you really have free-reign to add tabs, new lines, etc. as you like to make it more readable

### schema.sql
* When you see the word 'schema', it is likely referring to the DDL, where you are setting up your database

```SQL
CREATE DATABASE intro_to_sql;

USE intro_to_sql;

CREATE TABLE students(
  id SERIAL PRIMARY KEY, -- SERIAL is an auto-incrementing integer
  name TEXT, -- TEXT is a string of characters
);

CREATE TABLE pets(
  id SERIAL PRIMARY KEY,
  name TEXT,
  breed TEXT,
  type TEXT,
  age INTEGER,
  student_id INTEGER,
  FOREIGN KEY (student_id) REFERENCES students(id) -- Explicit way of declaring where FK is coming from
);
```

### seeds.sql
* When you see the word 'seeds', it is likely referring to the DML, where you are manipulating your database data

```SQL
INSERT INTO students(name) VALUES('Warren');

INSERT INTO pets(name, breed, type, age, student_id) -- column order matters!
VALUES('Quorra', 'Akita', 'Dog', 2, 1); -- VALUES must match the order and number of columns
```

Command Line:
* psql -U dbuser intro_to_sql
* \dt &rarr; see current tables in database
* \i schema.sql &rarr; create two tables from schema
* \i seeds.sql &rarr; populate two tables from seeds

### select_queries.sql
```SQL
SELECT * FROM pets;
SELECT * FROM students;

SELECT name, type FROM pets;
SELECT name from STUDENTS;

-- Select a specific row
SELECT name FROM pets
WHERE student_id = 2; -- Note the one equal sign, equivalent to === in JS

-- Page one of pets (3 per page)
SELECT name FROM pets
OFFSET 0 -- Not strictly necessary as will default pick the first rows
LIMIT 3;

-- Page two of pets
SELECT name FROM pets
OFFSET 3 -- Skip the first three rows shown on page 1
LIMIT 3;

-- Page three of pets
SELECT name FROM pets
OFFSET 6 -- Skip the first 6 rows shown on page 1 & 2
LIMIT 3;

-- Get all student and pet names at the same time
-- Identical column names, so have to be specific with which table: students.name vs. pets.name
SELECT students.name AS student_name, -- Create query-specific alias with AS to improve readability
       pets.name AS pet_name,
FROM students -- LEFT TABLE
JOIN pets -- RIGHT TABLE
ON students.id = pets.student_id; -- What to glue the tables together based on

-- Alphabetically order by pet name
SELECT * FROM pets
ORDER BY name; -- can add keyword DESC to reverse the order

-- How many pets does each student have?
SELECT
  students.name AS student_name,
  COUNT(pets.id) AS pet_count -- Aggregate function built into SQL
FROM students -- LEFT TABLE
JOIN pets -- RIGHT TABLE
ON students.id = pets.student_id
GROUP BY students.name;

-- Left join example - will show all students, even if the student has no pet associated with them
SELECT students.name AS student_name,
       pets.name AS pet_name,
FROM students
LEFT JOIN pets -- can also do RIGHT JOIN to prioritize the pets table
ON students.id = pets.student_id;
```

* Want to learn more? [Postresql Tutorial](https://www.postgresqltutorial.com/) is a great reference!