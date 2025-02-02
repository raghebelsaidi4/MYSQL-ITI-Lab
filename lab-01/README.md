## 1. **Scenario**
As an instructor at ITI, you need to convert the manual grading process into an electronic database system. The database should store:
- **Student Information**: Name, contact info (email, address), and multiple phone numbers.
- **Subjects**: Name, description, and maximum score.
- **Exams**: Exam date and score achieved by each student.
- **Student-Subject Relationship**: Track which subjects each student is studying.

---

## 2. **ERD (Entity-Relationship Diagram)**

### Entities:
1. **Student**:
   - Attributes: `StudentID` (PK), `Name`, `Email`, `Address`.
2. **Phone**:
   - Attributes: `PhoneID` (PK), `StudentID` (FK), `PhoneNumber`.
3. **Subject**:
   - Attributes: `SubjectID` (PK), `Name`, `Description`, `MaxScore`.
4. **Exam**:
   - Attributes: `ExamID` (PK), `StudentID` (FK), `SubjectID` (FK), `ExamDate`, `Score`.

### Relationships:
- A **Student** can have multiple **Phone** numbers (1-to-Many).
- A **Student** can study multiple **Subjects**, and a **Subject** can be studied by multiple students (Many-to-Many).
- A **Student** can take multiple **Exams**, and an **Exam** is associated with one **Subject**.

---

## 3. **Mapping Schema**

### Tables:
1. **Student**:
   - `StudentID` (Primary Key)
   - `Name`
   - `Email`
   - `Address`

2. **Phone**:
   - `PhoneID` (Primary Key)
   - `StudentID` (Foreign Key referencing `Student.StudentID`)
   - `PhoneNumber`

3. **Subject**:
   - `SubjectID` (Primary Key)
   - `Name`
   - `Description`
   - `MaxScore`

4. **StudentSubject** (Many-to-Many Relationship between Student and Subject):
   - `StudentID` (Foreign Key referencing `Student.StudentID`)
   - `SubjectID` (Foreign Key referencing `Subject.SubjectID`)

5. **Exam**:
   - `ExamID` (Primary Key)
   - `StudentID` (Foreign Key referencing `Student.StudentID`)
   - `SubjectID` (Foreign Key referencing `Subject.SubjectID`)
   - `ExamDate`
   - `Score`

---

## 4. **SQL Script to Create Tables**

```sql
-- Create Student Table
CREATE TABLE Student (
    StudentID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100),
    Address VARCHAR(255)
);

-- Create Phone Table
CREATE TABLE Phone (
    PhoneID INT AUTO_INCREMENT PRIMARY KEY,
    StudentID INT,
    PhoneNumber VARCHAR(15),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID)
);

-- Create Subject Table
CREATE TABLE Subject (
    SubjectID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Description TEXT,
    MaxScore INT
);

-- Create StudentSubject Table (Many-to-Many Relationship)
CREATE TABLE StudentSubject (
    StudentID INT,
    SubjectID INT,
    PRIMARY KEY (StudentID, SubjectID),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (SubjectID) REFERENCES Subject(SubjectID)
);

-- Create Exam Table
CREATE TABLE Exam (
    ExamID INT AUTO_INCREMENT PRIMARY KEY,
    StudentID INT,
    SubjectID INT,
    ExamDate DATE,
    Score INT,
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (SubjectID) REFERENCES Subject(SubjectID)
);
```

---

## 5. **Insert Sample Data**

### Insert Students:
```sql
INSERT INTO Student (Name, Email, Address) VALUES
('Ahmed Mohamed', 'ahmed@example.com', 'Cairo, Egypt'),
('Mona Ali', 'mona@example.com', 'Alexandria, Egypt'),
('Omar Khaled', 'omar@example.com', 'Giza, Egypt'),
('Hana Samir', 'hana@example.com', 'Luxor, Egypt'),
('Youssef Ahmed', 'youssef@example.com', 'Aswan, Egypt');
```

### Insert Phone Numbers:
```sql
INSERT INTO Phone (StudentID, PhoneNumber) VALUES
(1, '01012345678'),
(1, '01123456789'),
(2, '01234567890'),
(3, '01098765432'),
(4, '01187654321');
```

### Insert Subjects:
```sql
INSERT INTO Subject (Name, Description, MaxScore) VALUES
('C', 'Programming in C', 100),
('CPP', 'Object-Oriented Programming in C++', 100),
('HTML', 'Web Development with HTML', 100),
('JS', 'JavaScript Programming', 100);
```

### Insert Student-Subject Relationships:
```sql
INSERT INTO StudentSubject (StudentID, SubjectID) VALUES
(1, 1), (1, 2), -- Ahmed studies C and CPP
(2, 3), (2, 4), -- Mona studies HTML and JS
(3, 1), (3, 3), -- Omar studies C and HTML
(4, 2), (4, 4), -- Hana studies CPP and JS
(5, 1), (5, 4); -- Youssef studies C and JS
```

### Insert Exam Results:
```sql
INSERT INTO Exam (StudentID, SubjectID, ExamDate, Score) VALUES
(1, 1, '2023-10-01', 85),
(1, 2, '2023-10-02', 90),
(2, 3, '2023-10-03', 78),
(2, 4, '2023-10-04', 88),
(3, 1, '2023-10-05', 92);
```

---

## 6. **Query Examples**

### Get All Students and Their Subjects:
```sql
SELECT s.Name, sub.Name AS Subject
FROM Student s
JOIN StudentSubject ss ON s.StudentID = ss.StudentID
JOIN Subject sub ON ss.SubjectID = sub.SubjectID;
```

### Get Exam Results for a Specific Student:
```sql
SELECT s.Name, sub.Name AS Subject, e.ExamDate, e.Score
FROM Exam e
JOIN Student s ON e.StudentID = s.StudentID
JOIN Subject sub ON e.SubjectID = sub.SubjectID
WHERE s.StudentID = 1;
```

### Get Average Score for Each Subject:
```sql
SELECT sub.Name, AVG(e.Score) AS AverageScore
FROM Exam e
JOIN Subject sub ON e.SubjectID = sub.SubjectID
GROUP BY sub.Name;
```
