### 1. Insert a new student and their exam scores in different subjects as a transaction.

```sql
START TRANSACTION;

-- Insert a new student
INSERT INTO Students (student_id, name, age, birthdate, contact_info)
VALUES (1, 'John Doe', 20, '2003-05-15', 'ragheb.amr@example.com');

-- Insert exam scores for the student
INSERT INTO Exam_Results (student_id, subject, score, exam_date)
VALUES 
(1, 'Math', 85, '2023-10-01'),
(1, 'Science', 90, '2023-10-01'),
(1, 'History', 78, '2023-10-01');

COMMIT;
```

---

### 2. Display the date of the exam in the format: day 'month name' year.

```sql
SELECT DATE_FORMAT(exam_date, '%d %M %Y') AS formatted_exam_date
FROM Exam_Results;
```

---

### 3. Display the name and age of each student.

```sql
SELECT name, age
FROM Students;
```

---

### 4. Display the name of students with their rounded score in each exam.

```sql
SELECT s.name, er.subject, ROUND(er.score) AS rounded_score
FROM Students s
JOIN Exam_Results er ON s.student_id = er.student_id;
```

---

### 5. Display the name of students with the year of their birthdate.

```sql
SELECT name, YEAR(birthdate) AS birth_year
FROM Students;
```

---

### 6. Add a new exam result, using `NOW()` for the date column.

```sql
INSERT INTO Exam_Results (student_id, subject, score, exam_date)
VALUES (1, 'Geography', 88, NOW());
```

---

### 7. Create a "Hello World" function that takes a username and returns a welcome message.

```sql
DELIMITER //

CREATE FUNCTION HelloWorld(username VARCHAR(50))
RETURNS VARCHAR(100)
DETERMINISTIC
BEGIN
    RETURN CONCAT('Welcome, ', username, '!');
END //

DELIMITER ;

-- Usage:
SELECT HelloWorld('John Doe');
```

---

### 8. Create a multiply function that takes two numbers and returns their product.

```sql
DELIMITER //

CREATE FUNCTION Multiply(a INT, b INT)
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN a * b;
END //

DELIMITER ;

-- Usage:
SELECT Multiply(5, 10); -- Returns 50
```

---

### 9. Create a function that takes a student ID and exam ID and returns the student's score in that exam.

```sql
DELIMITER //

CREATE FUNCTION GetStudentScore(student_id INT, exam_id INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE exam_score INT;
    SELECT score INTO exam_score
    FROM Exam_Results
    WHERE student_id = student_id AND exam_id = exam_id;
    RETURN exam_score;
END //

DELIMITER ;

-- Usage:
SELECT GetStudentScore(1, 1); -- Returns the score for student 1 in exam 1
```

---

### 10. Create a function that takes a subject name and returns the maximum grade for that subject.

```sql
DELIMITER //

CREATE FUNCTION GetMaxGrade(subject_name VARCHAR(50))
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE max_grade INT;
    SELECT MAX(score) INTO max_grade
    FROM Exam_Results
    WHERE subject = subject_name;
    RETURN max_grade;
END //

DELIMITER ;

-- Usage:
SELECT GetMaxGrade('Math'); -- Returns the maximum grade in Math
```

---

### 11. Create a table called `Deleted_Students` to hold the deleted students' info.

```sql
CREATE TABLE Deleted_Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    birthdate DATE,
    contact_info VARCHAR(100),
    deletion_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

### 12. Create a trigger to save the deleted student from the `Students` table to `Deleted_Students`.

```sql
DELIMITER //

CREATE TRIGGER SaveDeletedStudent
AFTER DELETE ON Students
FOR EACH ROW
BEGIN
    INSERT INTO Deleted_Students (student_id, name, age, birthdate, contact_info)
    VALUES (OLD.student_id, OLD.name, OLD.age, OLD.birthdate, OLD.contact_info);
END //

DELIMITER ;
```

---

### 13. Create a trigger to save newly added students to a `Backup_Students` table.

```sql
CREATE TABLE Backup_Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    birthdate DATE,
    contact_info VARCHAR(100),
    backup_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

DELIMITER //

CREATE TRIGGER BackupNewStudent
AFTER INSERT ON Students
FOR EACH ROW
BEGIN
    INSERT INTO Backup_Students (student_id, name, age, birthdate, contact_info)
    VALUES (NEW.student_id, NEW.name, NEW.age, NEW.birthdate, NEW.contact_info);
END //

DELIMITER ;
```

---

### 14. (Bonus) Create a trigger to track changes in the `Contact_Info` table.

```sql
CREATE TABLE Contact_Info_Logs (
    log_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT,
    old_contact_info VARCHAR(100),
    new_contact_info VARCHAR(100),
    action_type VARCHAR(10), -- 'INSERT' or 'UPDATE'
    action_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

DELIMITER //

CREATE TRIGGER TrackContactInfoChanges
AFTER INSERT OR UPDATE ON Contact_Info
FOR EACH ROW
BEGIN
    IF NEW.contact_info IS NOT NULL THEN
        INSERT INTO Contact_Info_Logs (student_id, old_contact_info, new_contact_info, action_type)
        VALUES (NEW.student_id, OLD.contact_info, NEW.contact_info, 
                CASE WHEN OLD.contact_info IS NULL THEN 'INSERT' ELSE 'UPDATE' END);
    END IF;
END //

DELIMITER ;
```
