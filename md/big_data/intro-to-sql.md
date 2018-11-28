# Intro to SQL and Relational Databases

-

### Overview

- Definitions
- Connecting to a database
- C.R.U.D.
- SELECTING Data
- Relationships
- Analytics/Processing

-
-

### What is a databse?

A database is an organized collection of data. A database will usually have some sort of api to query the data. Using a Structured Query Language (SQL) will allow you to interact with the data in a predictable way. A query in this case is a statement you ask a database. A properly made database will be able to answer a query with all relevent information.

What makes a database relational though?

-

### Indexing

"
A database index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure
"

Basically, you put a sepcial tag on a piece of data so that you can find it faster. This requires extra work and takes more space on disk, but will help speed up your databse.

-

### Indexing

Most sql data sets will have columns based on the fields of an item and rows for each of the individaul items. When written out it sort of looks like an excel spreadsheet.

| First Name | Last Name | Age | Gender |
|:-----------|:----------|:----|:-------|
| Leon       | Hunter    | 24  | Male   |
| Wilhem     | Alcivar   | 23  | NULL   |
| Nhu        | Nguyen    | NULL| Female |

-

### Indexing

Issue: Let us say we want to find out Leon's age. We might tell our databse to return all the info related to the row with the first name of Leon. For now that would work since we only have one Leon in the databse, but what if we hired another Leon? We could ask for only Leon Hunter, but we really cannot rely on this data always being unique. 

| First Name | Last Name | Age | Gender |
|:-----------|:----------|:----|:-------|
| Leon       | Hunter    | 24  | Male   |
| Leon       | Smith     | 24  | Male   |

-

### Indexing

Solution: Add a unique id to each row. 

| ID | First Name | Last Name | Age | Gender |
|:---|:-----------|:----------|:----|:-------|
| 1  | Leon       | Hunter    | 24  | Male   |
| 2  | Wilhem.    | Alcivar   | 23  | NULL   |
| 3  | Nhu.       | Nguyen    | NULL| Female |

We can denote this id as the `PRIMARY KEY` which will make this an index. Searching for a row by its id will not only help us get only the data we want, but it will also be faster by a mesurable degree

-

### Indexing

Lets say we keep a list of phone numbers now: 

| ID | Phone Number | Phone Owner |
|:---|:-------------|:------------|
| 1  | 555-321-4547 | Wilhem      |
| 2  | 555-221-4548 | Leon        |
| 3  | 555-782-4549 | Nhu         |

Same issue as before. That owner there refers to only one person now, but how can we make sure that we match it to the correct person?

-

### Indexing

Solution: Use the person's unique id to identify who this number belongs to

| ID | Phone Number | Phone Owner ID |
|:---|:-------------|:---------------|
| 1  | 555-321-4547 | 2              |
| 2  | 555-221-4548 | 1              |
| 3  | 555-782-4549 | 3              |

To find out who owns the phone number we would take that id and search for it in the Person list that we made above. This is a relationship. Relational data uses these kinds of relationships

-

### Relationships

Say we have tables A and B.

One to One - One item in table A has one item in table B. One item in table B has one item in table A

One to Many - One item in table A has many items in table B. One item in table B has one item in table A

Many to Many - One item in table A has many items in table B. One item in table B has many items in table A

-
-

# Connecting to a database

-

### MySQL

Mysql is a popular Relational Database Management System (RDBMS). It runs on most systems so it can be run directly on your own machine, but its best to run it on a docker instance running linux. 

-

### MySQL

In order to connect to a MySQL instance, you need to know a few things

* Host - this is usually a url or ip address for a server
* Port - Port listening for a connection. Defaults to 3306
* Username - The MySQL user you are connecting with
* Password - The password for the user (This could be blank)

-

### MySQL

The easiest way to install a local MySQL instance on any machine is with a docker container. The following line will start up an instance

```bash
$ docker run --name local-mysql -e MYSQL_ROOT_PASSWORD=password\
-p 3306:3306 -d mysql:tag
```

This will allow you to connect using the following 

```
Host: localhost
Port: 3306
User: root
Password: password
```

-
-

# Creating a schema with tables

-

### Schemas and Tables

A schema is a collection of tables 

```SQL
CREATE SCHEMA zipcode
```

-
### Schemas and Tables

What have we just done? _We created a SCHEMA_

- Created a home for all data related to an application
- Allowed ourselves to manage multiple applications in the same databse without worrying about collisions

-

### Schemas and Tables

We can create tables within a schema as well to store and organize our data

```SQL
CREATE TABLE zipcode.teachers
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    specialty ENUM('FRONT END', 'MIDDLE TIER', 'DATA TIER')
);
CREATE UNIQUE INDEX teachers_id_uindex ON zipcode.teachers (id);
```

-

### Data Types - Numbers

* INT - -2147483648 to 2147483647 or 0 to 4294967295
* TINYINT - -128 to 127 or 0 to 255
* SMALLINT - -32768 to 32767 or 0 to 65535
* MEDIUMINT - -8388608 to 8388607 or 0 to 16777215
* BIGINT - -9223372036854775808 to 9223372036854775807 or 0 to 18446744073709551615

-

### Data Types - Floats
Floating point numbers store data by keeping track of decimal precision D and digit length M

* FLOAT(M, D) - Length of 10, Precision 2
* DOUBLE(M, D) - Length of 16, Precision 4
* DECIMAL(M, D) - No default precision or length. Each digit unpacked corresponds to one byte of storage. Also written as NUMERIC

-

### Data Types - Date and Times

* DATE - YYYY-MM-DD
* DATETIME - YYYY-MM-DD HH:mm:SS
* TIMESTAMP - YYYYMMDDHHmmSS
* TIME - HH:mm:SS
* YEAR - stores either a 2 digit or 4 digit year.
  - YEAR(2) goes from 1970 to 2069 (stored as 70 to 69)
  - YEAR(4) goes from 1901 to 2155
-

### Data Types - Strings

* CHAR(M) - A string of fixed length `M`
* VARCHAR(M) - A string with varrying length between 0 and M. Defaults to 255
* BLOB or TEXT - a string with max width 65535. Blobs are case sensitive and Texts are not
* ENUM - A String that is one of a predefined list of strings
  - ENUM("a", "b", "c") can only ever be "a" or "b" or "c"

-
-

### Altering Tables

A Table can be altered after it has been created using the `ALTER` keyword

```SQL
# Here we can add some columns
ALTER TABLE zipcode.teachers ADD first_name VARCHAR(25) NOT NULL;
ALTER TABLE zipcode.teachers ADD last_name VARCHAR(25) NOT NULL;
# Here we can drop columns
ALTER TABLE zipcode.teachers DROP name;
```

-

### Altering Tables

When altering a table, columns will be placed at the end of a table by default. This order only matters for storage purposes, but if desired you can place the column somewhere using the `AFTER` keyword.

```SQL
# Add the columns where the name column used to be
# this preserves the order of the colums originally set
ALTER TABLE zipcode.teachers ADD first_name VARCHAR(25) NOT NULL 
  AFTER name;
ALTER TABLE zipcode.teachers ADD last_name VARCHAR(25) NOT NULL 
  AFTER first_name;
ALTER TABLE zipcode.teachers DROP name;
```

-
-

### Inserts

Inserts will generally have 3 parts

- Reference to the table you want to insert into
- The columns you want to fill
- The values to fill those columns with

```SQL
INSERT INTO zipcode.teachers (first_name, last_name, specialty)
    VALUES ('John', 'Smith', 'FRONT END');
```

-

### Updates

Updating data is very similar to inserting, but we will use an `UPDATE` clause instead. For this we will need to specify a table we want to update, then a `SET` clause to specify how to change a field or fields.

```SQL
UPDATE zipcode.teachers
SET first_name = 'Johnathan';
```
-
-

# Viewing Data

-

### Selects

In order to view data we use a `SELECT` statement. This will generally have at least two parts.

- A Select clause where we say the columns we want to see 
- A FROM clause where we specify which table to pull that data from.

-

```
SELECT id, first_name, last_name, specialty 
FROM zipcode.teachers;
```

| ID | First Name | Last Name |  Specialty  |
|:---|:-----------|:----------|:------------|
| 1  | John       | Smith     | FRONT END   |
| 2  | Tabitha    | Schultz   | MIDDLE TIER |
| 3  | Jane       | Herman    | DATA TIER   |


-

### Where Clause

Data may be filtered using a `WHERE` clause

```
SELECT id, first_name, last_name, specialty
FROM zipcode.teachers
WHERE specialty='FRONT END';
```

| ID | First Name | Last Name |  Specialty  |
|:---|:-----------|:----------|:------------|
| 1  | John       | Smith     | FRONT END   |

-

### Limit and Order Clauses

Often times when SELECTING data, we want it to be ordered in some way. We can do so with the `ORDER BY` clause.

```SQL
SELECT * 
FROM blog_posts 
ORDER BY published_at DESC
```

A query like this might be used for a blog

-

### Limit and Order Clauses

While the query from before may work for a small blog, it might return too much data if there are hundreds or even thousands of posts. To limit the amount of data we return, we can use a `LIMIT` clause. 

```SQL
SELECT *
FROM blog_posts
ORDER BY published_at DESC
LIMIT 25;
```

-
-

# Relationships

-

### Joins

Viewing the data in one table can be useful, but often we'll want to see mutiple tables' data together. To do this we use a `JOIN`.

The Join clause will have two parts

- The table to join to
- The fields to compare

-

### Joins - One to One

Let's do our first join. Here we are going to write a query that connects a table of persons and a table of phone numbers. When each row of data in one table corresponds to one row in the other, it is called a One-to-One join.

-


| ID |      name       |  age  |
|:---|:----------------|------:|
| 1  | John Smith      | 56    |
| 2  | Tabitha Schultz | 43    |
| 3  | Jane Herman     | 21    |

| ID | person_id |  number  |
|:---|:----------|---------:|
| 1  | 2         | 555-4949 |
| 2  | 3         | 555-6767 |
| 3  | 1         | 555-1234 |

-

```SQL
SELECT p.name, n.number 
FROM person p
JOIN numbers n
  ON p.id = n.person_id
```

|      name       |  number  |
|:----------------|---------:|
| John Smith      | 555-1234 |
| Tabitha Schultz | 555-4949 |
| Jane Herman     | 555-6767 |

-

### Joins - One to Many

Next let us do a One-to-Many join. This join will connect two tables where 1 row in the first table will correspond to many rows in the second

Consider the person table from the previous example along with a table of vehicles

| ID |     model    | year | person_id |
|:---|:-------------|-----:|:----------|
| 1  | Kia Rio      | 2001 | 2         |
| 2  | Nissan Rouge | 2016 | 2         |
| 3  | Ford Taurus  | 2003 | 1         |

-

```SQL
SELECT p.name, v.model, v.year
FROM person p
JOIN vehicles v
  ON p.id = v.person_id
```

|      name       |     model    | year |
|:----------------|:-------------|-----:|
| Tabitha Schultz | Kia Rio      | 2001 |
| Tabitha Schultz | Nissan Rouge | 2016 |
| John Smith      | Ford Taurus  | 2003 |

Notice how Tabitha appears here twice and Jane doesn't appear here at all

-

### Joins - Many to Many

Lastly lets see a Many-to-Many join.

For this relationship, we can't logically keep the id's of one table in the other without getting a One-to-One or One-to-Many relationship. To create a many to many relationship we use a pivot table

-

| ID | First Name | Last Name | Age | Gender |
|:---|:-----------|:----------|:----|:-------|
| 1  | Leon       | Hunter    | 24  | Male   |
| 2  | Wilhem     | Alcivar   | 23  | NULL   |
| 3  | Nhu        | Nguyen    | NULL| Female |
| 4  | Dominique  | Clark     | 26  | Female |

| ID |      name       |  age  |
|:---|:----------------|------:|
| 1  | John Smith      | 56    |
| 2  | Tabitha Schultz | 43    |
| 3  | Jane Herman     | 21    |

-

This pivot table has teacher ids and student ids to connect the two tables

| ID | teacher_id | student_id |
|:---|:-----------|:-----------|
| 1  | 1          | 1          |
| 2  | 1          | 2          |
| 3  | 1          | 3          |
| 4  | 2          | 1          |
| 5  | 2          | 2          |
| 6  | 2          | 3          |
| 7  | 3          | 3          |

-

### Joins - Many to Many

First we select from our source table. From there we join our pivot table. Lastly we join our destination table.

```SQL
SELECT t.first_name, s.name
FROM teachers t 
JOIN student_teacher st
  ON st.teacher_id = t.id
JOIN students s
  ON s.id = st.student_id
```

-


| First Name |      name       |
|:-----------|:----------------|
| Leon       | John Smith      |
| Leon       | Tabitha Schultz |
| Leon       | Jane Herman     |
| Wilhem     | John Smith      |
| Wilhem     | Tabitha Schultz |
| Wilhem     | Jane Herman     |
| Nhu        | Jane Herman     |

-
-

# Analytics

-

### Aggregates

Aggregates are functions that allow you to run a function down a column rather than on each particular row. Some of these include

- COUNT
- MAX
- MIN
- SUM

-

### Aggregates - Count

Count will give you the total number of rows in a table based on a column. 

```
SELECT COUNT(*) as "count"
FROM teachers;
```
| count |
|:-----:|
| 3     |

Count does not consider the values in any column. It will simply add one for each row.

-

### Aggregates - Max/Min

Max and min will give you the largest or smallest value of a column in any row.

```
SELECT MAX(years) as "max", MIN(years) as "min"
FROM zipcode.teacher_meta
```

| max | min |
|:----|:----|
| 11  | 4   |

Max and Min will consider data type when doing this sort. Text will sort lexographically and numbers will sort numerically.

-

### Aggregates - Sum

Sum will take the value of a numeric column and add together all of the values.

```
SELECT SUM(years) as "sum"
FROM zipcode.teacher_meta
```

| sum |
|:---:|
| 19  |

-

### Aggregates - Group Concat

Group Concat will take a column and concatenate all the rows. 

```
SELECT GROUP_CONCAT(' ', first_name, ' ', last_name) as "group_concat"
FROM zipcode.teachers;
```

| group_concat                              |
|:------------------------------------------|
|  John Smith, Tabitha Schultz, Jane Herman |

-

### Group By 

Sometimes you want to take an aggregate of rows but you don't want to indiscriminately aggregate all rows. You want to group some rows together and aggregate those.

Right now in order to do that you could try using a `WHERE` clause, but you'd have to know some information about the rows beforehand. Instead we may group a column with a Group By.

Lets write a query to list teachers who have equivilent experience together.

-

### Group By

```
SELECT GROUP_CONCAT(' ', first_name, ' ', last_name) as "teachers", years
FROM zipcode.teachers t
JOIN zipcode.teacher_meta tm
  ON t.id = tm.teacher_id
GROUP BY tm.years;
```
| teachers                    | years |
|:----------------------------|:-----:|
| John Smith, Tabitha Schultz | 4     |
| Jane Herman                 | 11    |


-

### Having

A Having clause can be used to filter the results of a query. This is similar to the Where clause, but it works only with fields that are Aggregates.

Lets Write a query to find all students who have been assigned more than six assignemnts.

-

```SQL
SELECT s.name, COUNT(a_s.assignment_id) as "assignments given"
FROM zipcode.students s
JOIN zipcode.assignment_student a_s
  ON s.id = a_s.student_id
GROUP BY s.name
```

| name               | assignemnts given |
|:-------------------|:-----------------:|
| Linnell McLanachan | 8                 |
| Rodger McGuyandAll | 5                 |
| James Izrealperson | 3                 |

-

### Having

```SQL
SELECT s.name, COUNT(a_s.assignment_id) as "assignments given"
FROM zipcode.students s
JOIN zipcode.assignment_student a_s
  ON s.id = a_s.student_id
GROUP BY s.name
HAVING COUNT(a_s.assignment_id) > 6;
```

| name               | assignemnts given |
|:-------------------|:-----------------:|
| Linnell McLanachan | 8                 |

-

<img src="http://img.petshow.cc/ueditor/2016_01/2417092214536265621569945175386650530.jpg">

DBA corgis hope you're `HAVING` a blast!
