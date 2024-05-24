# SQL

## Structured Query Language

**Relational Database** - a database that organizes information into one or more tables  
**Table** - a collection of data organized into rows and columns  
**Column** - a set of data values of a particular type  
**Row** - a single record in a table

**Statement**
- text that the database recognizes as a valid command
- always end in a semi-colon

**Clauses**
- perform specific tasks in SQL
- by convention, clauses are written in capital letters
- can also be referred to as commands

**Parameter** - a list of columns, data types, or values that are passed to a clause as an argument.

**Constraints**
```sql
PRIMARY KEY
UNIQUE
NOT NULL
DEFAULT
```

**CRUD**
```sql
- create table
CREATE TABLE label (
    label datatype constraints,
    label datatype constraints,
    ...
);

- insert
INSERT INTO table_name (column_names) VALUES (values);

- update
UPDATE table_name
SET column_name = value
WHERE column_name operator value;

ALTER TABLE table_name ADD COLUMN column_name datatype;

- delete
DELETE FROM table_name
WHERE column_name operator value;
```

**Select**
```sql
- select
SELECT * FROM table_name;
SELECT column_name, column_name FROM table_name;
SELECT column_name AS 'alias' FROM table_name;

- distinct
SELECT DISTINCT column_name FROM table_name;

- where
SELECT column_name
FROM table_name
WHERE column_name operator value;

SELECT column_name FROM table_name WHERE column_name LIKE pattern;
SELECT column_name FROM table_name WHERE column_name BETWEEN value AND value;
SELECT column_name FROM table_name ORDER BY column_name ASC|DESC;
SELECT column_name FROM table_name LIMIT value;

- case
SELECT column_name
    CASE
        WHEN column_name operator value THEN value;
        ...
        ELSE value
    END AS value
FROM table_name;

- aggregates
SELECT COUNT(column_name) FROM table_name;
SELECT SUM(column_name) FROM table_name;
SELECT MIN(column_name) FROM table_name;
SELECT MAX(column_name) FROM table_name;
SELECT AVG(column_name) FROM table_name;
SELECT ROUND(column_name, decimal_places) FROM table_name;

- group
SELECT column_name
FROM table_name
GROUP BY <col-label or number>
HAVING column_name operator value;

- join
SELECT column_name
FROM table1 [LEFT]
JOIN table2
    ON table1.column_name operator table2.column_name;

- union
SELECT column_name
FROM table1
UNION
    SELECT column_name FROM table1;

- intermediate
WITH label AS (
    ...
)
SELECT column_name
FROM label
JOIN table2
    ON condition;
```

**Clause**
```sql
clause table_name (
    parameter
);
```

**Operators**
```sql
- comparison operators
=
!=
>
<
>=
<=

- logical operators
AND
OR

- pattern
*       everything
_       exactly one character
%       zero or more characters
```

`IS [NOT] NULL` is a condition in SQL that returns true when the value is NULL and false otherwise.



## LiquiBase

### Release changelock
```sql
update databasechangeloglock set locked=false, lockgranted=null, lockedby=null where id=1;
select * from databasechangeloglock;
```
