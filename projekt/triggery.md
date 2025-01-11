### Zapobieżenie zapisu studenta na kurs, gdy ten się już zaczął
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
go
```
