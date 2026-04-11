# Microsoft SQL Server: Table Update Cheatsheet

## 1. **COLUMN OPERATIONS**

### Add Column
```sql
ALTER TABLE table_name
ADD column_name datatype [NULL|NOT NULL] [CONSTRAINT constraint_name DEFAULT default_value];
```
**Example:**
```sql
ALTER TABLE Employees
ADD Email VARCHAR(100) NOT NULL DEFAULT 'noemail@company.com';
```

### Drop Column
```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```
**Example:**
```sql
ALTER TABLE Employees
DROP COLUMN MiddleName;
```

### Rename Column
```sql
EXEC sp_rename 'table_name.old_column', 'new_column', 'COLUMN';
```
**Example:**
```sql
EXEC sp_rename 'Employees.EmpID', 'EmployeeID', 'COLUMN';
```

### Modify Column Data Type
```sql
ALTER TABLE table_name
ALTER COLUMN column_name new_datatype [NULL|NOT NULL];
```
**Example:**
```sql
ALTER TABLE Employees
ALTER COLUMN Salary DECIMAL(12,2) NOT NULL;
```

### Add Identity Column
```sql
ALTER TABLE table_name
ADD column_name INT IDENTITY(1,1) PRIMARY KEY;
```

## 2. **CONSTRAINT OPERATIONS**

### Add PRIMARY KEY
```sql
ALTER TABLE table_name
ADD CONSTRAINT pk_name PRIMARY KEY (column_name);
```

### Drop PRIMARY KEY
```sql
ALTER TABLE table_name
DROP CONSTRAINT pk_name;
```

### Add FOREIGN KEY (Relationship)
```sql
ALTER TABLE child_table
ADD CONSTRAINT fk_name FOREIGN KEY (child_column)
REFERENCES parent_table (parent_column)
[ON DELETE CASCADE|SET NULL|NO ACTION]
[ON UPDATE CASCADE|SET NULL|NO ACTION];
```
**Example:**
```sql
ALTER TABLE Orders
ADD CONSTRAINT FK_Orders_Customers 
FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
ON DELETE CASCADE;
```

### Drop FOREIGN KEY
```sql
ALTER TABLE table_name
DROP CONSTRAINT fk_name;
```

### Add UNIQUE Constraint
```sql
ALTER TABLE table_name
ADD CONSTRAINT unique_name UNIQUE (column_name);
```

### Add CHECK Constraint
```sql
ALTER TABLE table_name
ADD CONSTRAINT check_name CHECK (column_name > 0);
```
**Example:**
```sql
ALTER TABLE Employees
ADD CONSTRAINT CHK_SalaryPositive CHECK (Salary > 0);
```

### Add DEFAULT Constraint
```sql
ALTER TABLE table_name
ADD CONSTRAINT default_name DEFAULT default_value FOR column_name;
```

## 3. **MANAGING RELATIONSHIPS**

### View Existing Foreign Keys
```sql
SELECT 
    fk.name AS FK_Name,
    OBJECT_NAME(fk.parent_object_id) AS Table_Name,
    COL_NAME(fkc.parent_object_id, fkc.parent_column_id) AS Column_Name,
    OBJECT_NAME(fk.referenced_object_id) AS Referenced_Table,
    COL_NAME(fkc.referenced_object_id, fkc.referenced_column_id) AS Referenced_Column
FROM sys.foreign_keys fk
JOIN sys.foreign_key_columns fkc ON fk.object_id = fkc.constraint_object_id
WHERE OBJECT_NAME(fk.parent_object_id) = 'YourTableName';
```

### Temporarily Disable Foreign Key
```sql
ALTER TABLE child_table NOCHECK CONSTRAINT fk_name;
```

### Re-enable Foreign Key
```sql
ALTER TABLE child_table CHECK CONSTRAINT fk_name;
```

### Disable All Foreign Keys on a Table
```sql
ALTER TABLE table_name NOCHECK CONSTRAINT ALL;
```

### Enable All Foreign Keys on a Table
```sql
ALTER TABLE table_name CHECK CONSTRAINT ALL;
```

## 4. **TABLE-LEVEL OPERATIONS**

### Rename Table
```sql
EXEC sp_rename 'old_table_name', 'new_table_name';
```

### Truncate Table (Remove all rows, reset identity)
```sql
TRUNCATE TABLE table_name;
```

### Add Multiple Columns in One Statement
```sql
ALTER TABLE table_name
ADD 
    col1 DATATYPE,
    col2 DATATYPE,
    col3 DATATYPE;
```

## 5. **PRACTICAL EXAMPLES**

### Add Column with Foreign Key Reference
```sql
-- Add column first
ALTER TABLE Orders
ADD DepartmentID INT;

-- Then add foreign key
ALTER TABLE Orders
ADD CONSTRAINT FK_Orders_Departments 
FOREIGN KEY (DepartmentID) REFERENCES Departments(DeptID);
```

### Modify Column and Add Constraint
```sql
-- Change column to NOT NULL
ALTER TABLE Products
ALTER COLUMN Price DECIMAL(10,2) NOT NULL;

-- Add check constraint
ALTER TABLE Products
ADD CONSTRAINT CHK_PricePositive CHECK (Price > 0);
```

### Drop Column with Dependencies
```sql
-- First drop foreign key if exists
ALTER TABLE Orders DROP CONSTRAINT FK_Orders_Products;

-- Then drop column
ALTER TABLE Products DROP COLUMN ProductID;
```

## 6. **USEFUL QUERIES**

### Check Table Schema
```sql
SELECT 
    COLUMN_NAME,
    DATA_TYPE,
    CHARACTER_MAXIMUM_LENGTH,
    IS_NULLABLE,
    COLUMN_DEFAULT
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'YourTableName';
```

### Find All Constraints on a Table
```sql
SELECT 
    CONSTRAINT_NAME,
    CONSTRAINT_TYPE
FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS
WHERE TABLE_NAME = 'YourTableName';
```

## ⚠️ **Important Notes**
- `ALTER TABLE` operations can lock tables
- Adding NOT NULL to existing column fails if NULLs exist
- Renaming breaks dependent objects (views, procs, functions)
- Use transactions for multiple changes:
  ```sql
  BEGIN TRANSACTION
  -- your ALTER statements
  COMMIT -- or ROLLBACK
  ```