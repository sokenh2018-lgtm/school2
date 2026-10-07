# منهج SQL Server المتكامل لتدريس الطلاب

## 1) مقدمة

هذا المنهج مصمم لتدريس SQL Server للطلاب من الصفر إلى المستوى المتقدم، مع تطبيق عملي على منظومة المدرسة. يركز المنهج على الفهم العملي لكيفية إنشاء قواعد البيانات وتصميم الجداول وكتابة الاستعلامات وتطوير نظام مدرسة كامل.

## 2) أهداف المنهج

بعد انتهاء الطالب من هذا المنهج، سيكون قادراً على:

- فهم أساسيات قواعد البيانات
- إنشاء قاعدة بيانات SQL Server
- تصميم الجداول والعلاقات
- كتابة استعلامات SQL الأساسية والمتقدمة
- استخدام JOIN وGROUP BY وSubquery
- إدخال وتحديث وحذف البيانات
- إنشاء Views وStored Procedures وFunctions وTriggers
- تحسين الأداء والأمان
- بناء مشروع كامل لنظام المدرسة

## 3) مدة المنهج

- 12 إلى 16 أسبوع
- حصتان أسبوعياً
- تمرين عملي أسبوعي
- مشروع نهائي في نهاية الدورة

## 4) محتوى المنهج

### الوحدة الأولى: مقدمة لقواعد البيانات وSQL Server

- مفهوم قاعدة البيانات
- ما هو SQL؟
- ما هو SQL Server؟
- لماذا نستخدم قواعد البيانات؟
- أدوات SQL Server
- SSMS
- إنشاء قاعدة بيانات جديدة
- أنواع البيانات
- القيود: PK, FK, NOT NULL, UNIQUE, CHECK

### الوحدة الثانية: تصميم قاعدة البيانات

- مفهوم الـ ERD
- العلاقات الأساسية
- One-to-One
- One-to-Many
- Many-to-Many
- المفتاح الأساسي Primary Key
- المفتاح الخارجي Foreign Key
- Normalization

### الوحدة الثالثة: أساسيات SQL

- SELECT
- FROM
- WHERE
- ORDER BY
- DISTINCT
- TOP
- LIKE
- IN
- BETWEEN
- AND / OR
- IS NULL / IS NOT NULL

### الوحدة الرابعة: الوظائف التجميعية

- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()
- GROUP BY
- HAVING

### الوحدة الخامسة: الانضمام بين الجداول

- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL OUTER JOIN
- CROSS JOIN

### الوحدة السادسة: استعلامات متقدمة

- Subquery
- EXISTS
- CASE WHEN
- UNION و UNION ALL
- CTE (Common Table Expression)

### الوحدة السابعة: إدخال وتحديث وحذف البيانات

- INSERT
- UPDATE
- DELETE
- TRUNCATE
- MERGE

### الوحدة الثامنة: Views

- مفهوم View
- إنشاء View
- استخدام View في التقارير

### الوحدة التاسعة: Stored Procedures

- ما هي Procedure؟
- إنشاء Procedure بإدخال بارامترات
- إضافة سجل جديد
- تحديث سجل
- حذف سجل

### الوحدة العاشرة: Functions

- Scalar Function
- Table-Valued Function

### الوحدة الحادية عشرة: Triggers

- Trigger بعد INSERT
- Trigger بعد UPDATE
- Trigger بعد DELETE
- استخدامات المشغلات

### الوحدة الثانية عشرة: الأمان والأداء

- Indexes
- Execution Plan
- Permissions
- GRANT و REVOKE

## 5) تصميم قاعدة بيانات المدرسة

### الجداول الأساسية

#### 1. Class

- Class_ID
- Class_Name
- Level

#### 2. Student

- Student_ID
- Name
- BirthDate
- Gender
- Address
- Class_ID

#### 3. Teacher

- Teacher_ID
- Name
- Specialization
- Phone

#### 4. Subject

- Subject_ID
- Subject_Name
- Grade
- Teacher_ID

#### 5. Registration

- Student_ID
- Subject_ID
- Registration_Date

#### 6. Teaches_Student

- Teaches_Student_ID
- Teacher_ID
- Student_ID

## 6) العلاقات بين الجداول

- الطالب ينتمي إلى صف واحد
- الصف يحتوي على عدة طلاب
- المعلم يدرّس عدة مواد
- الطالب يسجل في عدة مواد
- المادة يدرّسها مدرس واحد
- المعلم يدرّس عدة طلاب

## 7) إنشاء قاعدة البيانات

```sql
CREATE DATABASE SchoolDB;
GO

USE SchoolDB;
GO
```

## 8) إنشاء الجداول

```sql
CREATE TABLE Class (
    Class_ID INT PRIMARY KEY IDENTITY(1,1),
    Class_Name NVARCHAR(100) NOT NULL,
    Level NVARCHAR(50) NOT NULL
);

CREATE TABLE Student (
    Student_ID INT PRIMARY KEY IDENTITY(1,1),
    Name NVARCHAR(100) NOT NULL,
    BirthDate DATE NOT NULL,
    Gender NVARCHAR(10) CHECK (Gender IN ('Male', 'Female')),
    Address NVARCHAR(255),
    Class_ID INT NOT NULL,
    CONSTRAINT FK_Student_Class FOREIGN KEY (Class_ID)
        REFERENCES Class(Class_ID)
);

CREATE TABLE Teacher (
    Teacher_ID INT PRIMARY KEY IDENTITY(1,1),
    Name NVARCHAR(100) NOT NULL,
    Specialization NVARCHAR(100),
    Phone NVARCHAR(20)
);

CREATE TABLE Subject (
    Subject_ID INT PRIMARY KEY IDENTITY(1,1),
    Subject_Name NVARCHAR(100) NOT NULL,
    Grade NVARCHAR(20) NOT NULL,
    Teacher_ID INT NOT NULL,
    CONSTRAINT FK_Subject_Teacher FOREIGN KEY (Teacher_ID)
        REFERENCES Teacher(Teacher_ID)
);

CREATE TABLE Registration (
    Student_ID INT NOT NULL,
    Subject_ID INT NOT NULL,
    Registration_Date DATE NOT NULL,
    CONSTRAINT PK_Registration PRIMARY KEY (Student_ID, Subject_ID),
    CONSTRAINT FK_Registration_Student FOREIGN KEY (Student_ID)
        REFERENCES Student(Student_ID),
    CONSTRAINT FK_Registration_Subject FOREIGN KEY (Subject_ID)
        REFERENCES Subject(Subject_ID)
);

CREATE TABLE Teaches_Student (
    Teaches_Student_ID INT PRIMARY KEY IDENTITY(1,1),
    Teacher_ID INT NOT NULL,
    Student_ID INT NOT NULL,
    CONSTRAINT FK_Teaches_Student_Teacher FOREIGN KEY (Teacher_ID)
        REFERENCES Teacher(Teacher_ID),
    CONSTRAINT FK_Teaches_Student_Student FOREIGN KEY (Student_ID)
        REFERENCES Student(Student_ID)
);
GO
```

## 9) إدراج بيانات تجريبية

```sql
INSERT INTO Class (Class_Name, Level)
VALUES
('A1', 'Primary'),
('B2', 'Middle'),
('C3', 'Secondary');

INSERT INTO Student (Name, BirthDate, Gender, Address, Class_ID)
VALUES
('Ali Ahmed', '2012-05-10', 'Male', 'Riyadh', 1),
('Sara Khaled', '2011-08-14', 'Female', 'Jeddah', 1),
('Mohammed Saleh', '2010-02-20', 'Male', 'Dammam', 2),
('Layla Hassan', '2009-11-15', 'Female', 'Makkah', 2),
('Omar Nasser', '2008-07-05', 'Male', 'Abha', 3),
('Noura Salem', '2007-06-18', 'Female', 'Tabuk', 3);

INSERT INTO Teacher (Name, Specialization, Phone)
VALUES
('Mr. Abdullah', 'Mathematics', '0551111111'),
('Mrs. Huda', 'Science', '0552222222'),
('Mr. Faisal', 'Arabic', '0553333333'),
('Mrs. Noor', 'English', '0554444444');

INSERT INTO Subject (Subject_Name, Grade, Teacher_ID)
VALUES
('Mathematics', 'A', 1),
('Science', 'B', 2),
('Arabic', 'A', 3),
('English', 'B', 4);

INSERT INTO Registration (Student_ID, Subject_ID, Registration_Date)
VALUES
(1, 1, '2025-09-01'),
(1, 2, '2025-09-01'),
(2, 3, '2025-09-02'),
(2, 4, '2025-09-02'),
(3, 1, '2025-09-03'),
(3, 2, '2025-09-03'),
(4, 3, '2025-09-04'),
(5, 4, '2025-09-05'),
(6, 1, '2025-09-05'),
(6, 2, '2025-09-05');

INSERT INTO Teaches_Student (Teacher_ID, Student_ID)
VALUES
(1, 1),
(1, 3),
(1, 6),
(2, 1),
(2, 4),
(3, 2),
(3, 4),
(4, 2),
(4, 5),
(4, 6);
```

## 10) الاستعلامات الأساسية

### عرض جميع الطلاب

```sql
SELECT * FROM Student;
```

### عرض أسماء الطلاب مع اسم الصف

```sql
SELECT s.Student_ID, s.Name, c.Class_Name, c.Level
FROM Student s
JOIN Class c ON s.Class_ID = c.Class_ID;
```

### عرض المواد مع اسم المعلم

```sql
SELECT sub.Subject_Name, t.Name AS Teacher_Name
FROM Subject sub
JOIN Teacher t ON sub.Teacher_ID = t.Teacher_ID;
```

### عرض تسجيل الطلاب في المواد

```sql
SELECT s.Name AS Student_Name, sub.Subject_Name, r.Registration_Date
FROM Registration r
JOIN Student s ON r.Student_ID = s.Student_ID
JOIN Subject sub ON r.Subject_ID = sub.Subject_ID;
```

## 11) الاستعلامات المتقدمة

### عدد الطلاب في كل صف

```sql
SELECT c.Class_Name, COUNT(s.Student_ID) AS Student_Count
FROM Class c
LEFT JOIN Student s ON c.Class_ID = s.Class_ID
GROUP BY c.Class_Name;
```

### عدد المواد لكل معلم

```sql
SELECT t.Name, COUNT(sub.Subject_ID) AS Subject_Count
FROM Teacher t
LEFT JOIN Subject sub ON t.Teacher_ID = sub.Teacher_ID
GROUP BY t.Name;
```

### الطالب الذي سجل في أكبر عدد من المواد

```sql
SELECT TOP 1 s.Name, COUNT(r.Subject_ID) AS Number_Of_Subjects
FROM Student s
JOIN Registration r ON s.Student_ID = r.Student_ID
GROUP BY s.Name
ORDER BY Number_Of_Subjects DESC;
```

## 12) View

```sql
CREATE VIEW vw_Student_Class AS
SELECT s.Student_ID, s.Name AS Student_Name, c.Class_Name, c.Level
FROM Student s
JOIN Class c ON s.Class_ID = c.Class_ID;
GO

SELECT * FROM vw_Student_Class;
```

## 13) Stored Procedure

```sql
CREATE PROCEDURE sp_AddStudent
    @Name NVARCHAR(100),
    @BirthDate DATE,
    @Gender NVARCHAR(10),
    @Address NVARCHAR(255),
    @Class_ID INT
AS
BEGIN
    INSERT INTO Student (Name, BirthDate, Gender, Address, Class_ID)
    VALUES (@Name, @BirthDate, @Gender, @Address, @Class_ID);
END;
GO

EXEC sp_AddStudent 'Hassan Ali', '2013-01-10', 'Male', 'Jazan', 1;
```

## 14) Function

```sql
CREATE FUNCTION fn_CountStudentsInClass(@Class_ID INT)
RETURNS INT
AS
BEGIN
    DECLARE @Count INT;
    SELECT @Count = COUNT(*)
    FROM Student
    WHERE Class_ID = @Class_ID;
    RETURN @Count;
END;
GO

SELECT dbo.fn_CountStudentsInClass(1) AS Student_Count;
```

## 15) Trigger

```sql
CREATE TRIGGER trg_StudentAudit
ON Student
AFTER INSERT
AS
BEGIN
    PRINT 'New student added';
END;
GO
```

## 16) Indexes

```sql
CREATE INDEX IX_Student_ClassID ON Student(Class_ID);
GO
```

## 17) تمارين للطلاب

1. أوجد أسماء جميع الطلاب
2. أوجد الطلاب في الصف الأول
3. أوجد عدد الطلاب في كل صف
4. أوجد المواد التي يدرّسها كل معلم
5. أوجد الطالب الذي سجل في أكبر عدد من المواد
6. أوجد الطلاب الذين يدرّسهم مدرس محدد
7. أنشئ View يوضح كل طالب مع الصف
8. أنشئ Procedure لإضافة طالب جديد
9. أنشئ Function لحساب عدد الطلاب في صف معين
10. أضف Trigger لتسجيل إضافة طالب

## 18) أسئلة تقييم

1. ما هو الفرق بين Primary Key و Foreign Key؟
2. ما هي الفائدة من INNER JOIN؟
3. ما هو دور الـ View؟
4. ما الفرق بين Table و View؟
5. لماذا نستخدم Index؟

## 19) الخلاصة

هذا المنهج يعلّم الطالب SQL Server بشكل عملي ومتكامل، مع تطبيق مباشر على منظومة المدرسة، ويُعد مناسباً للمبتدئين والمتوسطين. الطالب خلال هذا المنهج يتعلم تصميم قواعد البيانات وكتابة الاستعلامات وبناء مشروع كامل.

## 20) نصائح للمعلم

- ابدأ من أمثلة عملية
- استخدم نفس قاعدة البيانات طوال الدورة
- اجعل كل درس يحتوي على تمرين عملي
- أطلب من الطلاب كتابة الاستعلامات بأنفسهم
- اجعل المشروع النهائي على شكل نظام مدرسة كامل

## 21) مشروع نهائي مقترح

يمكن توسيع قاعدة البيانات لاحقاً لإضافة:

- Attendance
- Exams
- Results
- Fees
- Parents
- Employees
- Login System

## 22) النهاية

هذا المنهج مناسب للطلاب الذين يرغبون في تعلم SQL Server بشكل صحيح ومبني على تطبيق عملي واقعي.

إذا رغبت، أستطيع في الخطوة التالية أن أجهّز لك:

- خطة تدريس شهرية أو أسبوعية
- اختبار نهاية الوحدة
- أسئلة وتطبيقات
- نسخة جاهزة للطباعة في Word
- ملف SQL كامل جاهز للتنفيذ
