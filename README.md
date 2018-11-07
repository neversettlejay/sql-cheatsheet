# sql-cheatsheet
Personal cheat sheet for querying relational database in SQL SERVER

## The Basics

### Basic SELECT Statement
```sql
SELECT select_list 
[ FROM table_source ]
[ WHERE search_condition ] 
[ GROUP BY group_by_expression ] 
[ HAVING search_condition ] 
[ ORDER BY order_expression [ ASC | DESC ] ] 
```

### WHERE Conditions
```sql
-- AND 
SELECT column_name FROM table_name
WHERE condition1 AND condition2
```

```sql
-- OR
SELECT column_name FROM table_name
WHERE condition1 OR condition2
```

```sql
-- EXISTS
SELECT column_name FROM table_name
WHERE EXISTS (SELECT column_name FROM table_name)
```

```sql
-- ANY
SELECT column_name FROM table_name
WHERE column = ANY (SELECT column_name FROM table_name)
```

```sql
-- ALL
SELECT column_name FROM table_name
WHERE column = ALL (SELECT column_name FROM table_name)
```

```sql
-- WHERE NOT
SELECT column_name FROM table_name
WHERE NOT condition
```

### CASE Statement
```sql
CASE
    WHEN condition THEN 'true' 
    ELSE 'false'
END
```

### INSERT INTO Table
###### Specify columns
```sql
INSERT INTO table_name (column, column)
VALUES (value, value)
```
###### Insert to all columns
```sql
INSERT INTO table_name
VALUES (value, value)
```

### UPDATE Table
```sql
UPDATE table_name
    SET column = value,
        column = value,
        column = value
WHERE condition;
```

### DELETE Table
```sql
DELETE FROM table_name WHERE condition;
```

### TRUNCATE Table
```sql
TRUNCATE TABLE table_name;
```

### Basic Functions
###### SELECT TOP
```sql
SELECT TOP [ number | percent ] column_name
FROM table_name
WHERE condition;
```

###### MIN/MAX
*Returns the smallest/biggest value in selected column*
```sql
SELECT [ MIN | MAX ] (column_name)
FROM table_name
WHERE condition;
```

###### COUNT/AVG/SUM
```sql
SELECT [ COUNT | AVG | SUM] (column_name)
FROM table_name
WHERE condition;
```

### LIKE Syntax
```sql
SELECT column
FROM table_name
WHERE column LIKE pattern;
```

###### pattern operator
```sql
WHERE column LIKE 'a%'	--Finds any values that start with "a"
```

```sql 
WHERE column LIKE '%a' --Finds any values that end with "a"
```	

```sql 
WHERE column LIKE '%or%' --Finds any values that have "or" in any position
```	

```sql 
WHERE column LIKE '_r%' --Finds any values that have "r" in the second position
```	

```sql 
WHERE column LIKE 'a_%_%' --Finds any values that start with "a" and are at least 3 characters in length
```	

```sql 
WHERE column LIKE 'a%o' --Finds any values that start with "a" and ends with "o"
```	

### STUFF 
###### The STUFF() function deletes a part of a string and then inserts another part into the string, starting at a specified position
```sql
-- Syntax
STUFF (character_expression, start, length, new_string )

-- Example: deletes the second digit of the product ID in the Productstable and replaces it with the characters '000'
SELECT STUFF([Product_ID], 2,1, '000')
FROM Products
-- OUTPUT: 20 becomes 2000
```

### COALESCE
###### Returns the first non-null value in a list:
```sql
SELECT COALESCE(NULL, NULL, NULL, 'JigJun', NULL, 1);
-- OUTPUT: 'JigJun'
```


### JOINS
###### Different types of JOINS
- (INNER) JOIN: Returns records that have matching values in both tables
- LEFT (OUTER) JOIN: Return all records from the left table, and the matched records from the right table
- RIGHT (OUTER) JOIN: Return all records from the right table, and the matched records from the left table
- FULL (OUTER) JOIN: Return all records when there is a match in either left or right table

```sql
SELECT column_name(s)
FROM table A
JOIN table B 
    ON A.column_name = B.column_name;
```


### SQL Statements

###### CREATE Table
```sql
CREATE TABLE table_name (
    column int IDENTITY(1,1) PRIMARY KEY, -- primary key and "IDENTITY(1,1)" for auto increment
    column data_type NOT NULL,            -- normal column
	ListingTypeID int NOT NULL FOREIGN KEY REFERENCES table_name(column) -- foreign key
);
```

###### ALTER Table

```sql
-- ADD Column
ALTER TABLE table_name
ADD column_name datatype;
```

```sql
-- DROP COLUMN
ALTER TABLE table_name
DROP column_name datatype;
```

```sql
-- ALTER COLUMN
ALTER TABLE table_name
ALTER column_name datatype;
```


###### CHECK Constraint
```sql
-- The CHECK constraint is used to limit the value range that can be placed in a column
CREATE TABLE table_name (
    ID int NOT NULL,
    [Percentage] DECIMAL(5,4),
    CHECK ([Percentage] <= 1.0000)
);
``` 

###### DEFAULT Constraint
```sql
-- DEFAULT constraint is used to provide a default value for a column
CREATE TABLE table_name (
    ID int NOT NULL,
    VARCHAR_column varchar(255) DEFAULT 'Text',
    INT_column INT DEFAULT 1
);
```

###### IF ELSE Statement
```sql
IF (@variable = 1)
BEGIN
    --insert code here
END
ELSE
BEGIN
    --insert code here
END
```

### Declare Temporary Table
```sql
DECLARE @Temp TABLE
    (
        column INT,
        column2 VARCHAR(10)
    );
```

### Stored Procedure Template
```sql
SET QUOTED_IDENTIFIER ON
SET ANSI_NULLS ON
GO

/******************************************************************************   
** Change History  
**  
** CID    Date				Author			Description   
** -----  ---------- ---------- -----------------------------------------------  
** CH001  06/11/2018		N.Sun		    Initial Version 
*******************************************************************************/
-- exec [SP_Template] 1
CREATE PROCEDURE [dbo].[SP_Template]
	@Parameter INT
AS
BEGIN
    DECLARE @Result TABLE
    (
        column INT,
        column2 VARCHAR(10)
    );

	INSERT INTO @Result
	(
	    column,
		column2,
	)
	SELECT column,
		   @column2
	FROM dbo.table_name A 
	INNER JOIN dbo.table_name B 
        ON A.column = B.column
	WHERE A.column = @Parameter
  
	SELECT 
			column,
			column2
	FROM @Result;
END;

GO
```


### OFFSET FETCH Clause
```sql
-- Skip first 10 rows from the sorted result set and return the remaining rows.
SELECT column1, column2 FROM table_name ORDER BY column1 OFFSET 10 ROWS;
```

```sql
-- Skip first 10 rows from the sorted resultset and return next 5 rows.
SELECT column1, column2 FROM table_name ORDER BY column1 OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;
```