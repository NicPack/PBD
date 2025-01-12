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
