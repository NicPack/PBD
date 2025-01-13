
# Procedury

## Dodawanie nowego zamówienia

```sql
CREATE PROCEDURE AddOrder
    @user_id INT,
    @cart_id INT
AS
BEGIN
    -- Stworzenie zmiennej do przechowania wartosci zamowienia
    DECLARE @total_value INT;

    -- Obliczenie wartosci zamowienia
    SELECT @total_value = SUM(price * quantity)
    FROM shopping_carts_products
    WHERE cart_id = @cart_id;

    -- Insert do tabeli invoices
    INSERT INTO invoices (currency, value, exchange_rate, is_exempted, user_id)
    VALUES ('PLN', @total_value, 1, 0, @user_id);

    -- Zwroc id nowej faktury
    SELECT SCOPE_IDENTITY() AS invoice_id;
END;

```

## Dodawanie nowego kursu

```sql
CREATE PROCEDURE AddCourse
@CourseName varchar(50),
@CoursePrice float,
@CourseStartDate int,
@CourseEndDate datetime,
@CourseEnrollmentLimit int,
@ProductId int
AS
BEGIN
-- Sprawdź, czy istnieje kurs o podanej nazwie
IF EXISTS (
SELECT 1
FROM Courses
WHERE name = @CourseName
)
BEGIN
RAISERROR('Kurs o podanej nazwie już istnieje.', 16, 1);
END
-- Sprawdź czy istnieje produkt o danym ID
IF NOT EXISTS (
SELECT 1
FROM Products
WHERE product_id = @ProductId
)
BEGIN
RAISERROR('Kurs o podanym ID nie istnieje.', 16, 1);
END
-- Sprawdź czy kurs o podanym productID już istnieje
IF EXISTS (
SELECT 1
FROM Courses
WHERE product_id = @ProductId
)
BEGIN
RAISERROR('Kurs o podanym ID nie istnieje.', 16, 1);
END
    -- Wstaw nowy kurs do tabeli Courses
    INSERT INTO Courses (name, category_id, price, start_time, end_time, enrollment_limit, product_id)
    VALUES (@CourseName, 2, @CoursePrice, @CourseStartDate, @CourseEndDate, @CourseEnrollment, @ProductId);

    PRINT 'Kurs dodany pomyślnie.';
END;
```

## Dodawanie nowego modułu

```sql
CREATE PROCEDURE AddModule
@ModuleName varchar(50),
@ModuleMinAttendance int
AS
BEGIN
-- Sprawdź, czy istnieje moduł o podanej nazwie
IF EXISTS (
SELECT 1
FROM modules
WHERE name = @ModuleName
)
BEGIN
RAISERROR('Moduł o podanej nazwie już istnieje.', 16, 1);
END
    -- Wstaw nowy moduł do tabeli Modules
    INSERT INTO modules (name, min_attendance)
    VALUES (@ModuleName, @ModuleMinAttendance);

    PRINT 'Moduł dodany pomyślnie.';
END;
```

## Dodawanie modułu do istniejącego kursu

```sql
CREATE PROCEDURE AddModuleToCourse
@ModuleID int,
@CourseID int,
@OnlineMeetingLink varchar(30),
@ClassName varchar(30)
AS
BEGIN
    -- Sprawdź, czy istnieje kurs o podanym CourseID
    IF NOT EXISTS (SELECT 1 FROM courses WHERE course_id = @CourseID)
    BEGIN
        RAISERROR('Kurs o podanym ID nie istnieje.', 16, 1);
    END

    -- Sprawdź, czy istnieje moduł o podanym ModuleID
    IF NOT EXISTS (SELECT 1 FROM modules WHERE module_id = @ModuleID)
    BEGIN
        RAISERROR('Moduł o podanym ID nie istnieje.', 16, 1);
    END

    -- Wstaw nowy moduł do tabeli Modules
    INSERT INTO course_modules (module_id, course_id, online_meeting_link, class_name)
    VALUES (@ModuleID, @CourseID, @OnlineMeetingLink, @ClassName)

    PRINT 'Moduł pomyślnie dodany do kursu.';
END;
```

## Dodawanie studiów

```sql
CREATE PROCEDURE AddStudy
@StudyName varchar(50),
@StudyCategoryID int,
@StudyPrice float,
@StartTime datetime,
@EndTime datetime,
@StudyEnrollmentLimit int,
@ProductID int
AS
BEGIN
-- Sprawdź czy istnieje podana kategoria
IF NOT EXISTS (
SELECT 1 FROM categories WHERE category_id = @StudiesCategoryID
)
BEGIN
RAISERROR('Podana kategoria studiów nie istnieje.', 16, 1);
END

-- Sprawdź czy podany kierunek istnieje jako produkt
IF NOT EXISTS (
SELECT 1 FROM products WHERE product_id = @ProductID
)
BEGIN
RAISERROR('Studia o podanym ID produktu nie istnieją.', 16, 1);
END

    INSERT INTO studies(name,category_id,price,start_time,end_time,enrollment_limit,product_id)
    VALUES (@StudyName, @StudyCategoryID, @StudyPrice, @StartTime, @EndTime, @StudyEnrollmentLimit, @ProductID)
END
```

## Dodawanie nowego przemiotu studiów

```sql
CREATE PROCEDURE AddSubject
@SubjectName varchar(50)
AS
BEGIN
-- Sprawdź, czy istnieje przemiot o podanej nazwie
IF EXISTS (
SELECT 1
FROM subjects
WHERE name = @SubjectName
)
BEGIN
RAISERROR('Przedmiot o podanej nazwie już istnieje.', 16, 1);
END
  
    INSERT INTO subjects (name)
    VALUES (@SubjectName)

    PRINT 'Przedmiot dodany pomyślnie.';
END;
```


## Dodawanie przedmiotów w ramach studiów

```sql
CREATE PROCEDURE AddSubjectToStudies
@StudyID int,
@SubjectID int,
@OnlineMeetingLink varchar(30),
@Classroom varchar(30),
@MinAttendance int
AS
BEGIN
-- Sprawdz czy istnieje kierunek o danym StudyID
IF NOT EXISTS (
SELECT 1 FROM studies WHERE study_id = @StudyID
)
BEGIN
RAISERROR('Nie znaleziono studiów', 16, 1);
END

-- Sprawdz czy istnieje przedmiot o danym SubjectID
IF NOT EXISTS (
SELECT 1 FROM subjects WHERE subject_id = @SubjectID
)
BEGIN
RAISERROR('Nie znaleziono przedmiotu', 16, 1);
END

    INSERT INTO study_subjects(study_id,subject_id,online_meeting_link,classroom,min_attendance)
    VALUES (@StudyID, @SubjectID, @OnlineMeetingLink, @Classroom, @MinAttendance)
END
```

## Dodawanie nowego stażu

```sql
CREATE PROCEDURE AddInternship
@StudyID int,
@StudentID int
AS
BEGIN
IF EXISTS (
SELECT 1 FROM internship_attendance WHERE student_id = @StudentID)
BEGIN
RAISERROR('Student o podanym ID ma już odbyty staż', 16, 1);
END

INSERT INTO Internship_attendance(study_id, student_id)
VALUES (@StudyID, @StudentID)
END
```

## Dodawanie nowego użytkowinika

```sql
CREATE PROCEDURE AddUser
@UserRoleID int,
@UserFirstName varchar(30),
@UserLastName varchar(30)
AS
BEGIN
INSERT INTO users (role_id, firstname, lastname)
VALUES (@UserRoleID, @UserFirstName, @UserLastName)
END;
```

## Dodanie szczegółów użytkowinika

```sql
CREATE PROCEDURE AddUserDetails
@UserID int,
@UserCountry varchar(50),
@UserCity varchar(50),
@UserPostalCode int,
@UserStreetName varchar(50),
@UserBuildingNumber int,
@UserFlatNumber int
AS
BEGIN

INSERT INTO user_address (user_id, country, city, postal_code, street_name, building_number, flat_number)
VALUES (@UserID, @UserCountry, @UserCity, @UserPostalCode,@UserStreetName, @UserBuildingNumber, @UserFlatNumber)
END;
```

## Dodawanie nowego webinaru

```sql
CREATE PROCEDURE AddWebinar
@Name varchar(50),
@CategoryID int,
@Price float,
@StartTime datetime,
@EndTime datetime,
@EnrollmentLimit int,
@IsDeleted bit,
@RecordingId int,
@ProductId int
AS
BEGIN
-- Sprawdz czy istnieje kategoria o danym ID
IF NOT EXISTS (SELECT 1 FROM categories WHERE category_id = @CategoryID)
BEGIN
RAISERROR('Nie znaleziono danej kategorii webinarów.', 16, 1);
END

-- Sprawdz czy istnieje produkt o danym ID
IF NOT EXISTS (SELECT 1 FROM products WHERE product_id = @ProductID)
BEGIN
RAISERROR('Nie znaleziono webinaru o danym ID produktu.', 16, 1);
END

    INSERT INTO Webinars (name,category_id,price,start_time,end_time,enrollment_limit,is_deleted,recording_id,product_id)
    VALUES (@Name,@CategoryID,@Price,@StartTime,@EndTime,@EnrollmentLimit,@IsDeleted,@RecordingID,@ProductID);
END;
```

## Usuń dostęp do nagrań po 30 dniach

```sql
CREATE PROCEDURE MarkOldWebinarsAsDeleted
AS
BEGIN
    UPDATE webinars
    SET is_deleted = 1
    WHERE end_time < DATEADD(DAY, -30, GETDATE());
END;
GO
```

## Wyślij dyplomy do uczestniów którzy ukończyli studia z pozytywnym rezultatem

```sql
CREATE PROCEDURE CreateDiplomas
AS
BEGIN
    INSERT INTO diplomas (student_id, study_id)
    SELECT DISTINCT
        fe.student_id,
        fe.study_id
    FROM
        final_exams fe
    WHERE
        fe.result > 80
        AND NOT EXISTS (
            SELECT 1
            FROM diplomas d
            WHERE d.student_id = fe.student_id
              AND d.study_id = fe.study_id
        );
END;
GO
```
