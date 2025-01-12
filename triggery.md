## Limit uczestników kursu

```sql
CREATE TRIGGER trg_LimitCoursesEnrollment
ON course_enrollment
INSTEAD OF INSERT
AS
BEGIN
IF EXISTS (
SELECT 1
FROM inserted
WHERE (
    SELECT COUNT(*)
    FROM course_enrollment ce
    WHERE ce.course_id = inserted.course_id
    ) >= (select enrollment_limit from courses where course_id = inserted.course_id)
)
BEGIN
    RAISERROR('Courses enrollment limit reached!', 16, 1)
    RETURN
END
INSERT INTO course_enrollment
SELECT * FROM  inserted
END;
```

## Limit uczestników studiów

```sql
CREATE TRIGGER trg_LimitStudiesEnrollment
ON study_enrollment
INSTEAD OF INSERT
AS
BEGIN
IF EXISTS (
SELECT 1
FROM inserted
WHERE (
    SELECT COUNT(*)
    FROM study_enrollment se
    WHERE se.study_id = inserted.study_id
    ) >= (select enrollment_limit from studies where studies.study_id = inserted.study_id)
)
BEGIN
    RAISERROR('Studies enrollment limit reached!', 16, 1)
    RETURN
END
INSERT INTO study_enrollment
SELECT * FROM  inserted
END;
```

## Zapobieganie spóźnionego zapisu na kursy

```sql
CREATE TRIGGER trg_PreventLateEnrollment
ON course_enrollment
INSTEAD OF INSERT
AS
BEGIN
    IF EXISTS (
        SELECT 1
        FROM inserted i
        JOIN courses c ON i.course_id = c.course_id
        WHERE GETDATE() > c.start_time
    )
    BEGIN
        RAISERROR('Can"t enroll after course had started', 16, 1);
    END
    ELSE
    BEGIN
        INSERT INTO course_enrollment (enrollment_id, user_id, course_id)
        SELECT *
        FROM inserted;
    END
END;
```

## Zapobieganie spóźnionego zapisu na studia

```sql
CREATE TRIGGER trg_PreventLateStudyEnrollment
ON study_enrollment
INSTEAD OF INSERT
AS
BEGIN
    IF EXISTS (
        SELECT 1
        FROM inserted i
        JOIN studies s ON i.study_id = s.study_id
        WHERE GETDATE() > s.start_time
    )
    BEGIN
        RAISERROR('Can"t enroll after studies have already started', 16, 1);
    END
    ELSE
    BEGIN
        INSERT INTO study_enrollment (user_id, study_id)
        SELECT *
        FROM inserted;
    END
END;
```
