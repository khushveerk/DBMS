# DBMS Practical File – Separate SQL Programs and Queries

Based on the provided DBMS practical list from the syllabus.

---

# Practical 1: Create Database

```sql
CREATE DATABASE CollegeDB;
USE CollegeDB;
```

---

# Practical 2: Create STUDENT Table

```sql
CREATE TABLE STUDENT (
    RollNo CHAR(6) PRIMARY KEY,
    StudentName VARCHAR(20),
    Course VARCHAR(20),
    DOB DATE
);
```

---

# Practical 3: Create SOCIETY Table

```sql
CREATE TABLE SOCIETY (
    SocID CHAR(6) PRIMARY KEY,
    SocName VARCHAR(20),
    MentorName VARCHAR(20),
    TotalSeats INT UNSIGNED
);
```

---

# Practical 4: Create ENROLLMENT Table

```sql
CREATE TABLE ENROLLMENT (
    RollNo CHAR(6),
    SID CHAR(6),
    DateOfEnrollment DATE,
    PRIMARY KEY (RollNo, SID),
    FOREIGN KEY (RollNo) REFERENCES STUDENT(RollNo),
    FOREIGN KEY (SID) REFERENCES SOCIETY(SocID)
);
```

---

# Practical 5: Insert Data into STUDENT Table

```sql
INSERT INTO STUDENT VALUES
('X10009','Aman','Computer Science','2001-05-12'),
('Z20009','Anjali','Chemistry','2002-07-15'),
('C30001','Rohit','Physics','2000-03-10'),
('D40002','Sneha','Computer Science','2001-11-25'),
('E50003','Aakash','Mathematics','2003-01-19'),
('F60004','Priya','Chemistry','2001-08-08');
```

---

# Practical 6: Insert Data into SOCIETY Table

```sql
INSERT INTO SOCIETY VALUES
('S1','NSS','Raj Gupta',100),
('S2','Debating','Anil Sharma',50),
('S3','Dancing','Riya Gupta',40),
('S4','Sashakt','Mohit Verma',30),
('S5','Sports','Karan Gupta',80);
```

---

# Practical 7: Insert Data into ENROLLMENT Table

```sql
INSERT INTO ENROLLMENT VALUES
('X10009','S1','2024-01-10'),
('X10009','S2','2024-01-11'),
('Z20009','S1','2024-01-12'),
('C30001','S3','2024-01-13'),
('D40002','S2','2024-01-14'),
('D40002','S4','2024-01-15'),
('E50003','S5','2024-01-16');
```

---

# Query 1: Retrieve names of students enrolled in any society

```sql
SELECT DISTINCT StudentName
FROM STUDENT S
JOIN ENROLLMENT E
ON S.RollNo = E.RollNo;
```

---

# Query 2: Retrieve all society names

```sql
SELECT SocName
FROM SOCIETY;
```

---

# Query 3: Retrieve students names starting with letter A

```sql
SELECT StudentName
FROM STUDENT
WHERE StudentName LIKE 'A%';
```

---

# Query 4: Retrieve students details studying in Computer Science or Chemistry

```sql
SELECT *
FROM STUDENT
WHERE Course IN ('Computer Science','Chemistry');
```

---

# Query 5: Retrieve students whose roll number starts with X or Z and ends with 9

```sql
SELECT StudentName
FROM STUDENT
WHERE (RollNo LIKE 'X%9' OR RollNo LIKE 'Z%9');
```

---

# Query 6: Find society details with more than N total seats

```sql
SELECT *
FROM SOCIETY
WHERE TotalSeats > 50;
```

---

# Query 7: Update mentor name of a specific society

```sql
UPDATE SOCIETY
SET MentorName = 'Suresh Gupta'
WHERE SocID = 'S1';
```

---

# Query 8: Find society names in which more than five students enrolled

```sql
SELECT SocName
FROM SOCIETY S
JOIN ENROLLMENT E
ON S.SocID = E.SID
GROUP BY SocName
HAVING COUNT(E.RollNo) > 5;
```

---

# Query 9: Find youngest student enrolled in NSS

```sql
SELECT StudentName
FROM STUDENT S
JOIN ENROLLMENT E
ON S.RollNo = E.RollNo
JOIN SOCIETY SO
ON E.SID = SO.SocID
WHERE SO.SocName = 'NSS'
ORDER BY DOB DESC
LIMIT 1;
```

---

# Query 10: Find most popular society

```sql
SELECT SocName
FROM SOCIETY S
JOIN ENROLLMENT E
ON S.SocID = E.SID
GROUP BY SocName
ORDER BY COUNT(*) DESC
LIMIT 1;
```

---

# Query 11: Find two least popular societies

```sql
SELECT SocName
FROM SOCIETY S
LEFT JOIN ENROLLMENT E
ON S.SocID = E.SID
GROUP BY SocName
ORDER BY COUNT(E.RollNo) ASC
LIMIT 2;
```

---

# Query 12: Find students not enrolled in any society

```sql
SELECT StudentName
FROM STUDENT
WHERE RollNo NOT IN (
    SELECT RollNo FROM ENROLLMENT
);
```

---

# Query 13: Find students enrolled in at least two societies

```sql
SELECT StudentName
FROM STUDENT S
JOIN ENROLLMENT E
ON S.RollNo = E.RollNo
GROUP BY S.RollNo, StudentName
HAVING COUNT(E.SID) >= 2;
```

---

# Query 14: Find society names in which maximum students enrolled

```sql
SELECT SocName
FROM SOCIETY S
JOIN ENROLLMENT E
ON S.SocID = E.SID
GROUP BY SocName
HAVING COUNT(*) = (
    SELECT MAX(Total)
    FROM (
        SELECT COUNT(*) AS Total
        FROM ENROLLMENT
        GROUP BY SID
    ) AS T
);
```

---

# Query 15: Find students enrolled in societies and society names

```sql
SELECT StudentName, SocName
FROM STUDENT S
JOIN ENROLLMENT E
ON S.RollNo = E.RollNo
JOIN SOCIETY SO
ON E.SID = SO.SocID;
```

---

# Query 16: Students enrolled in Debating, Dancing or Sashakt

```sql
SELECT DISTINCT StudentName
FROM STUDENT S
JOIN ENROLLMENT E
ON S.RollNo = E.RollNo
JOIN SOCIETY SO
ON E.SID = SO.SocID
WHERE SO.SocName IN ('Debating','Dancing','Sashakt');
```

---

# Query 17: Find societies whose mentor name contains Gupta

```sql
SELECT SocName
FROM SOCIETY
WHERE MentorName LIKE '%Gupta%';
```

---

# Query 18: Find societies where enrolled students are only 10% of capacity

```sql
SELECT SocName
FROM SOCIETY S
JOIN ENROLLMENT E
ON S.SocID = E.SID
GROUP BY SocName, TotalSeats
HAVING COUNT(E.RollNo) = TotalSeats * 0.10;
```

---

# Query 19: Display vacant seats for each society

```sql
SELECT SocName,
       TotalSeats - COUNT(E.RollNo) AS VacantSeats
FROM SOCIETY S
LEFT JOIN ENROLLMENT E
ON S.SocID = E.SID
GROUP BY SocName, TotalSeats;
```

---

# Query 20: Increment total seats of each society by 10%

```sql
UPDATE SOCIETY
SET TotalSeats = TotalSeats + (TotalSeats * 0.10);
```

---

# Query 21: Add enrollment fees paid field

```sql
ALTER TABLE ENROLLMENT
ADD FeesPaid VARCHAR(3);
```

---

# Query 22: Update date of enrollment

```sql
UPDATE ENROLLMENT
SET DateOfEnrollment = '2018-01-15'
WHERE SID = 'S1';

UPDATE ENROLLMENT
SET DateOfEnrollment = CURDATE()
WHERE SID = 'S2';

UPDATE ENROLLMENT
SET DateOfEnrollment = '2018-01-02'
WHERE SID = 'S3';
```

---

# Query 23: Create view for society enrollment count

```sql
CREATE VIEW SocietyEnrollmentView AS
SELECT SocName,
       COUNT(E.RollNo) AS TotalStudents
FROM SOCIETY S
LEFT JOIN ENROLLMENT E
ON S.SocID = E.SID
GROUP BY SocName;
```

---

# Query 24: Find students enrolled in all societies

```sql
SELECT StudentName
FROM STUDENT S
JOIN ENROLLMENT E
ON S.RollNo = E.RollNo
GROUP BY S.RollNo, StudentName
HAVING COUNT(DISTINCT E.SID) = (
    SELECT COUNT(*) FROM SOCIETY
);
```

---

# Query 25: Count societies with more than 5 students enrolled

```sql
SELECT COUNT(*) AS TotalSocieties
FROM (
    SELECT SID
    FROM ENROLLMENT
    GROUP BY SID
    HAVING COUNT(RollNo) > 5
) AS T;
```

---

# Query 26: Add mobile number column with default value

```sql
ALTER TABLE STUDENT
ADD MobileNo VARCHAR(10) DEFAULT '9999999999';
```

---

# Query 27: Find total number of students whose age is greater than 20 years

```sql
SELECT COUNT(*) AS TotalStudents
FROM STUDENT
WHERE TIMESTAMPDIFF(YEAR, DOB, CURDATE()) > 20;
```

---

# Query 28: Find students born in 2001 and enrolled in at least one society

```sql
SELECT DISTINCT StudentName
FROM STUDENT S
JOIN ENROLLMENT E
ON S.RollNo = E.RollNo
WHERE YEAR(DOB) = 2001;
```

---

# Query 29: Count societies whose name starts with S and ends with t and at least 5 students enrolled

```sql
SELECT COUNT(*) AS TotalSocieties
FROM (
    SELECT SocName
    FROM SOCIETY S
    JOIN ENROLLMENT E
    ON S.SocID = E.SID
    WHERE SocName LIKE 'S%t'
    GROUP BY SocName
    HAVING COUNT(E.RollNo) >= 5
) AS T;
```

---

# Query 30: Display society information

```sql
SELECT SocName,
       MentorName,
       TotalSeats AS TotalCapacity,
       COUNT(E.RollNo) AS TotalEnrolled,
       TotalSeats - COUNT(E.RollNo) AS UnfilledSeats
FROM SOCIETY S
LEFT JOIN ENROLLMENT E
ON S.SocID = E.SID
GROUP BY SocName, MentorName, TotalSeats;
```

---

# Database Administration Commands

## Create User

```sql
CREATE USER 'roshan'@'localhost' IDENTIFIED BY 'password123';
```

---

## Create Role

```sql
CREATE ROLE manager;
```

---

## Grant Privileges

```sql
GRANT SELECT, INSERT, UPDATE
ON CollegeDB.*
TO manager;
```

---

## Assign Role to User

```sql
GRANT manager TO 'roshan'@'localhost';
```

---

## Revoke Privileges

```sql
REVOKE INSERT
ON CollegeDB.*
FROM manager;
```

---

## Create Index

```sql
CREATE INDEX idx_studentname
ON STUDENT(StudentName);
```

---

# End of Practical File

