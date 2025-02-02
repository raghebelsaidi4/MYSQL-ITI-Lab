## 1. **Modify the Student Table**

### Add `Gender` and `BirthDate` Columns:
```sql
ALTER TABLE Student
ADD COLUMN Gender ENUM('Male', 'Female') NOT NULL,
ADD COLUMN BirthDate DATE;
```

### Replace `Name` with `FirstName` and `LastName`:
```sql
ALTER TABLE Student
DROP COLUMN Name,
ADD COLUMN FirstName VARCHAR(50) NOT NULL,
ADD COLUMN LastName VARCHAR(50) NOT NULL;
```

### Replace `Address` and `Email` with `ContactInfo` (JSON Data Type):
```sql
ALTER TABLE Student
DROP COLUMN Address,
DROP COLUMN Email,
ADD COLUMN ContactInfo JSON;
```

---

## 2. **Add Foreign Key Constraints with `ON DELETE CASCADE`**

### Modify `Phone` Table:
```sql
ALTER TABLE Phone
ADD CONSTRAINT fk_phone_student
FOREIGN KEY (StudentID) REFERENCES Student(StudentID)
ON DELETE CASCADE;
```

### Modify `StudentSubject` Table:
```sql
ALTER TABLE StudentSubject
ADD CONSTRAINT fk_studentsubject_student
FOREIGN KEY (StudentID) REFERENCES Student(StudentID)
ON DELETE CASCADE,
ADD CONSTRAINT fk_studentsubject_subject
FOREIGN KEY (SubjectID) REFERENCES Subject(SubjectID)
ON DELETE CASCADE;
```

### Modify `Exam` Table:
```sql
ALTER TABLE Exam
ADD CONSTRAINT fk_exam_student
FOREIGN KEY (StudentID) REFERENCES Student(StudentID)
ON DELETE CASCADE,
ADD CONSTRAINT fk_exam_subject
FOREIGN KEY (SubjectID) REFERENCES Subject(SubjectID)
ON DELETE CASCADE;
```

---

## 3. **Update Student Information**

### Update `Gender`, `BirthDate`, `FirstName`, `LastName`, and `ContactInfo`:
```sql
UPDATE Student
SET
    Gender = 'Male',
    BirthDate = '1990-05-15',
    FirstName = 'Ahmed',
    LastName = 'Mohamed',
    ContactInfo = '{"Address": "Cairo, Egypt", "Email": "ahmed@example.com"}'
WHERE StudentID = 1;

UPDATE Student
SET
    Gender = 'Female',
    BirthDate = '1992-08-20',
    FirstName = 'Mona',
    LastName = 'Ali',
    ContactInfo = '{"Address": "Alexandria, Egypt", "Email": "mona@example.com"}'
WHERE StudentID = 2;

UPDATE Student
SET
    Gender = 'Male',
    BirthDate = '1991-03-10',
    FirstName = 'Omar',
    LastName = 'Khaled',
    ContactInfo = '{"Address": "Giza, Egypt", "Email": "omar@example.com"}'
WHERE StudentID = 3;

UPDATE Student
SET
    Gender = 'Female',
    BirthDate = '1993-11-25',
    FirstName = 'Hana',
    LastName = 'Samir',
    ContactInfo = '{"Address": "Luxor, Egypt", "Email": "hana@example.com"}'
WHERE StudentID = 4;

UPDATE Student
SET
    Gender = 'Male',
    BirthDate = '1994-07-30',
    FirstName = 'Youssef',
    LastName = 'Ahmed',
    ContactInfo = '{"Address": "Aswan, Egypt", "Email": "youssef@example.com"}'
WHERE StudentID = 5;
```

---

## 4. **Queries**

### 7. **Display All Students’ Information:**
```sql
SELECT StudentID, FirstName, LastName, Gender, BirthDate, ContactInfo
FROM Student;
```

### 8. **Display Male Students Only:**
```sql
SELECT StudentID, FirstName, LastName, Gender, BirthDate, ContactInfo
FROM Student
WHERE Gender = 'Male';
```

### 9. **Display the Number of Female Students:**
```sql
SELECT COUNT(*) AS FemaleCount
FROM Student
WHERE Gender = 'Female';
```

### 10. **Display Students Born Before 1992-10-01:**
```sql
SELECT StudentID, FirstName, LastName, BirthDate
FROM Student
WHERE BirthDate < '1992-10-01';
```

### 11. **Display Male Students Born Before 1991-10-01:**
```sql
SELECT StudentID, FirstName, LastName, BirthDate
FROM Student
WHERE Gender = 'Male' AND BirthDate < '1991-10-01';
```

### 12. **Display Subjects and Their Max Score Sorted by Max Score:**
```sql
SELECT Name, MaxScore
FROM Subject
ORDER BY MaxScore DESC;
```

### 13. **Display the Subject with the Highest Max Score:**
```sql
SELECT Name, MaxScore
FROM Subject
ORDER BY MaxScore DESC
LIMIT 1;
```

### 14. **Display Students’ Names That Begin with ‘A’:**
```sql
SELECT FirstName, LastName
FROM Student
WHERE FirstName LIKE 'A%';
```

### 15. **Display the Number of Students Named “Mohammed”:**
```sql
SELECT COUNT(*) AS MohammedCount
FROM Student
WHERE FirstName = 'Mohammed';
```

### 16. **Display the Number of Males and Females:**
```sql
SELECT Gender, COUNT(*) AS Count
FROM Student
GROUP BY Gender;
```

### 17. **Display Repeated First Names and Their Counts (if Count > 2):**
```sql
SELECT FirstName, COUNT(*) AS Count
FROM Student
GROUP BY FirstName
HAVING Count > 2;
```

### 18. **Display Students’ Names, Their Score, and Subject Name:**
```sql
SELECT s.FirstName, s.LastName, e.Score, sub.Name AS Subject
FROM Exam e
JOIN Student s ON e.StudentID = s.StudentID
JOIN Subject sub ON e.SubjectID = sub.SubjectID;
```

### 19. **Delete Students with Scores Lower Than 50 in a Particular Subject Exam:**
```sql
DELETE FROM Exam
WHERE Score < 50 AND SubjectID = 1; -- Replace 1 with the desired SubjectID
```
