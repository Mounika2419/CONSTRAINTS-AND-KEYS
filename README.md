# SQL: Department and Courses Database

## 1. Introduction

This SQL program creates two related tables:

* `dept` – stores department information.
* `courses` – stores course information and connects each course to a department.

The program demonstrates important SQL concepts such as:

* Primary Key
* Foreign Key
* Unique Constraint
* Check Constraint
* Table relationships
* `DESC`
* `INSERT`
* `SELECT`

---

## 2. SQL Program

```sql
USE aids;

CREATE TABLE dept(
    id INT PRIMARY KEY,
    dname VARCHAR(20)
);

DESC dept;

INSERT INTO dept VALUES(1,'cse');
INSERT INTO dept VALUES(2,'aids');
INSERT INTO dept VALUES(3,'aiml');

SELECT * FROM dept;

CREATE TABLE courses(
    cid INT PRIMARY KEY,
    cname VARCHAR(20),
    credits INT CHECK(credits > 0),
    UNIQUE(cname),
    id INT,
    FOREIGN KEY(id) REFERENCES dept(id),
    status VARCHAR(20)
);

DESC courses;

INSERT INTO courses VALUES(101,'python',3,1,'active');
INSERT INTO courses VALUES(102,'mysql',5,2,'active');
INSERT INTO courses VALUES(103,'java',4,2,'deactive');
INSERT INTO courses VALUES(104,'cpp',2,3,'deactive');
INSERT INTO courses VALUES(105,'ai',2,2,'active');

SELECT * FROM courses;
```

---

# 3. Database Selection

```sql
USE aids;
```

This selects the `aids` database.

All tables created after this command will be created inside the `aids` database.

---

# 4. Department Table

```sql
CREATE TABLE dept(
    id INT PRIMARY KEY,
    dname VARCHAR(20)
);
```

This creates a table named `dept`.

| Column  | Data Type   | Constraint  | Purpose              |
| ------- | ----------- | ----------- | -------------------- |
| `id`    | INT         | PRIMARY KEY | Unique department ID |
| `dname` | VARCHAR(20) | —           | Department name      |

Example data:

| id | dname |
| -: | ----- |
|  1 | cse   |
|  2 | aids  |
|  3 | aiml  |

---

# 5. Primary Key

```sql
id INT PRIMARY KEY
```

A **Primary Key** uniquely identifies every record in a table.

### Properties:

* Values must be unique.
* A primary-key column cannot contain `NULL`.
* A table can have one primary key constraint.

For the `dept` table, `id` is the primary key.

For example:

```text
1 → CSE
2 → AIDS
3 → AIML
```

Each department has a unique `id`.

---

# 6. DESC Command

```sql
DESC dept;
```

`DESC` means **DESCRIBE**.

It displays the structure of the table.

It shows information such as:

* Column name
* Data type
* Whether NULL is allowed
* Key information
* Default value

Similarly:

```sql
DESC courses;
```

shows the structure of the `courses` table.

---

# 7. INSERT Command

```sql
INSERT INTO dept VALUES(1,'cse');
```

The `INSERT` command is used to add records to a table.

Three department records are inserted:

```sql
INSERT INTO dept VALUES(1,'cse');
INSERT INTO dept VALUES(2,'aids');
INSERT INTO dept VALUES(3,'aiml');
```

---

# 8. SELECT Command

```sql
SELECT * FROM dept;
```

`SELECT` is used to retrieve data from a table.

The `*` means **all columns**.

Therefore:

```sql
SELECT * FROM dept;
```

displays all records and columns from the `dept` table.

---

# 9. Courses Table

```sql
CREATE TABLE courses(
    cid INT PRIMARY KEY,
    cname VARCHAR(20),
    credits INT CHECK(credits > 0),
    UNIQUE(cname),
    id INT,
    FOREIGN KEY(id) REFERENCES dept(id),
    status VARCHAR(20)
);
```

The `courses` table contains course information.

| Column    | Purpose           |
| --------- | ----------------- |
| `cid`     | Course ID         |
| `cname`   | Course name       |
| `credits` | Number of credits |
| `id`      | Department ID     |
| `status`  | Course status     |

---

# 10. Foreign Key

```sql
FOREIGN KEY(id) REFERENCES dept(id)
```

A **Foreign Key** is used to create a relationship between two tables.

Here:

```text
courses.id → dept.id
```

The `id` column in `courses` refers to the `id` column in `dept`.

For example:

```text
courses
id = 2
   ↓
dept
id = 2 → aids
```

Therefore, courses can be associated with their departments.

### Important Rule

A value inserted into `courses.id` must already exist in `dept.id`.

For example:

```sql
INSERT INTO courses
VALUES(106,'cloud',3,5,'active');
```

will fail because department `5` does not exist in `dept`.

---

# 11. CHECK Constraint

```sql
credits INT CHECK(credits > 0)
```

The `CHECK` constraint restricts the values that can be inserted.

Here, credits must be greater than `0`.

Valid:

```text
credits = 2
credits = 3
credits = 5
```

Invalid:

```text
credits = 0
credits = -2
```

For example:

```sql
INSERT INTO courses
VALUES(106,'cloud',0,1,'active');
```

will violate the `CHECK` condition.

---

# 12. UNIQUE Constraint

```sql
UNIQUE(cname)
```

The `UNIQUE` constraint prevents duplicate course names.

For example, this is valid:

```text
python
mysql
java
cpp
ai
```

But inserting another course with:

```text
python
```

would violate the unique constraint.

### Primary Key vs UNIQUE

| Primary Key                          | UNIQUE                                |
| ------------------------------------ | ------------------------------------- |
| Uniquely identifies a row            | Prevents duplicate values             |
| Cannot contain NULL                  | NULL handling depends on DBMS         |
| One primary key constraint per table | Multiple UNIQUE constraints can exist |
| Usually used as the main identifier  | Used for additional uniqueness        |

---

# 13. Relationship Between the Tables

The two tables have a **one-to-many relationship**.

```text
             DEPT
       ┌───────────────┐
       │ id  dname     │
       ├───────────────┤
       │ 1   cse       │
       │ 2   aids      │
       │ 3   aiml      │
       └───────┬───────┘
               │
               │ Foreign Key
               ↓
          COURSES
 ┌─────────────────────────┐
 │ cid cname credits id    │
 ├─────────────────────────┤
 │ 101 python 3      1     │
 │ 102 mysql  5      2     │
 │ 103 java   4      2     │
 │ 104 cpp    2      3     │
 │ 105 ai     2      2     │
 └─────────────────────────┘
```

One department can have multiple courses.

For example:

```text
AIDS (id = 2)
   ├── mysql
   ├── java
   └── ai
```

---

# 14. Keys Used in This Program

## Primary Key

```sql
id INT PRIMARY KEY
```

in `dept`.

```sql
cid INT PRIMARY KEY
```

in `courses`.

These uniquely identify records.

## Foreign Key

```sql
FOREIGN KEY(id) REFERENCES dept(id)
```

It connects `courses` with `dept`.

## Candidate Key

A **candidate key** is a column or combination of columns that can uniquely identify a record.

In the `courses` table:

```text
cid
cname
```

can provide uniqueness because `cid` is the primary key and `cname` has a UNIQUE constraint.

## Alternate Key

A candidate key that is not selected as the primary key is called an **alternate key**.

Here, `cname` can be considered an alternate unique identifier because `cid` is selected as the primary key.

---

# 15. Constraints Used

This program demonstrates the following constraints:

| Constraint  | Used On           | Purpose                         |
| ----------- | ----------------- | ------------------------------- |
| PRIMARY KEY | `dept.id`         | Unique identification           |
| PRIMARY KEY | `courses.cid`     | Unique course identification    |
| FOREIGN KEY | `courses.id`      | Connects two tables             |
| UNIQUE      | `courses.cname`   | Prevents duplicate course names |
| CHECK       | `courses.credits` | Ensures credits > 0             |

---

# 16. Final Course Data

The following records are inserted:

| CID | Course | Credits | Dept ID | Status   |
| --: | ------ | ------: | ------: | -------- |
| 101 | Python |       3 |       1 | active   |
| 102 | MySQL  |       5 |       2 | active   |
| 103 | Java   |       4 |       2 | deactive |
| 104 | C++    |       2 |       3 | deactive |
| 105 | AI     |       2 |       2 | active   |

The department mapping is:

```text
1 → CSE
2 → AIDS
3 → AIML
```

Therefore:

```text
Python → CSE
MySQL  → AIDS
Java   → AIDS
C++    → AIML
AI     → AIDS
```

---

# 17. Key Concepts – Short Notes

### Primary Key

Uniquely identifies each row in a table.

### Foreign Key

Connects one table to another table.

### UNIQUE

Prevents duplicate values.

### CHECK

Restricts values according to a condition.

### NOT NULL

Prevents a column from containing NULL values.

### DEFAULT

Automatically provides a value when no value is specified.

---

# 18. Conclusion

This SQL program demonstrates how to create and manage two related tables using **keys and constraints**.

The `dept` table stores department information, while the `courses` table stores course information. The **primary key** uniquely identifies records, the **foreign key** establishes a relationship between the tables, the **UNIQUE constraint** prevents duplicate course names, and the **CHECK constraint** ensures that course credits are positive.

This is a basic example of **relational database design using SQL**.
