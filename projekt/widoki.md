## Raporty finansowe – zestawienie przychodów dla każdego webinaru/kursu/studium.

```sql
    (
        SELECT
            max(products.name),
            products.price * COUNT(course_enrollment.enrollment_id) as profit
        FROM courses
        INNER JOIN course_enrollment ON courses.course_id = course_enrollment.course_id
        INNER JOIN products ON courses.product_id = products.product_id
        GROUP BY products.product_id, products.price
    )
    union (
        SELECT
            max(products.name),
            products.price * COUNT(study_enrollment.study_enrollment_id) as profit
        FROM studies
        INNER JOIN study_enrollment ON studies.study_id = study_enrollment.study_id
        INNER JOIN products ON studies.product_id = products.product_id
        GROUP BY products.product_id, products.price
    )
    union (
        select
            max(webinars.name),
            sum(invoices.value) as profit
        from webinars
        inner join dbo.products p on webinars.product_id = p.product_id
        inner join invoices on p.product_id = invoices.product_id
        group by webinars.webinar_id
    )
```

## Lista „dłużników” – osoby, które skorzystały z usług, ale nie uiściły opłat.

```sql
WITH outstanding_invoices AS (
    SELECT
        user_id,
        value,
        exchange_rate
    FROM invoices
    WHERE is_paid = 0
    AND payment_deadline < getdate()
)
SELECT
    max(firstname) + ' ' + max(lastname),
    SUM(value * exchange_rate) as total
FROM outstanding_invoices
join users on outstanding_invoices.user_id = users.user_id
GROUP BY outstanding_invoices.user_id
ORDER BY total DESC;
```

## Ogólny raport dotyczący liczby zapisanych osób na przyszłe wydarzenia (z informacją, czy wydarzenie jest stacjonarnie, czy zdalnie).

```sql
select
    users.firstname + ' ' + users.lastname as student_name,
    'Course' as type,
    courses.name as name
from course_enrollment
inner join users on course_enrollment.user_id = users.user_id
inner join courses on course_enrollment.course_id = courses.course_id
where courses.start_time > getdate()
union
select
    users.firstname + ' ' + users.lastname as student_name,
    'Studies' as type,
    studies.name as name
from study_enrollment
inner join users on study_enrollment.user_id = users.user_id
inner join studies on study_enrollment.study_id = studies.study_id
where studies.start_time > getdate()
```

## Ogólny raport dotyczący frekwencji na zakończonych już wydarzeniach.

```sql
SELECT
    'Course' as type,
    MAX(courses.name) as name,
    MAX(modules.name) as class_name,
    MAX(users.firstname) + ' ' + MAX(users.lastname) as username,
    IIF((
        SELECT COUNT(*)
        FROM course_modules
        INNER JOIN module_attendance ma2 ON course_modules.course_module_id = ma2.course_module_id
        WHERE ma2.student_id = ma.student_id and ma2.attended = 0x01
    ) >= max(modules.min_attendance), 'Passed', 'Not passed') as passed
FROM courses
INNER JOIN course_modules cm ON courses.course_id = cm.course_id
INNER JOIN modules ON cm.module_id = modules.module_id
INNER JOIN module_attendance ma ON cm.course_module_id = ma.course_module_id
INNER JOIN users ON ma.student_id = users.user_id
WHERE end_time < GETDATE()
GROUP BY cm.course_module_id, ma.student_id
union
SELECT
    'Study' as type,
    MAX(studies.name) as name,
    MAX(subjects.name) as class_name,
    MAX(users.firstname) + ' ' + MAX(users.lastname) as username,
    IIF((
        SELECT COUNT(*)
        FROM study_subjects
        INNER JOIN study_subject_classes on study_subjects.study_subject_id = study_subject_classes.study_subject_id
        INNER JOIN study_subject_attendance ssa2 on study_subject_classes.study_subject_class_id = ssa2.class_id
        WHERE ssa2.user_id = ssa.user_id
    ) >= max(ss.min_attendance), 'Passed', 'Not passed') as passed
FROM studies
INNER JOIN study_subjects ss on studies.study_id = ss.study_id
INNER JOIN subjects on ss.subject_id = subjects.subject_id
INNER JOIN study_subject_classes on ss.study_subject_id = study_subject_classes.study_subject_id
INNER JOIN study_subject_attendance ssa on study_subject_classes.study_subject_class_id = ssa.class_id
INNER JOIN users on ssa.user_id = users.user_id
WHERE end_time < GETDATE()
GROUP BY ssa.user_id, ss.study_subject_id
```

## Lista obecności dla każdego szkolenia z datą, imieniem, nazwiskiem i informacją czy uczestnik był obecny, czy nie.

```sql
select
    'Course' as type,
    max(courses.name) as name,
    max(modules.name) as class_name,
    module_attendance.student_id as student_id,
    module_attendance.date as date,
    max(module_attendance.attended) as attended
from courses
inner join course_modules cm on courses.course_id = cm.course_id
inner join modules on cm.module_id = modules.module_id
inner join module_attendance on cm.course_module_id = module_attendance.course_module_id
where end_time < getdate()
group by cm.course_module_id, module_attendance.student_id, module_attendance.date
union
select
    'Study' as type,
    max(studies.name) as name,
    max(subjects.name) as class_name,
    study_subject_attendance.user_id as student_id,
    max(study_subject_classes.date) as date,
    1 as attended
from studies
inner join study_subjects ss on studies.study_id = ss.study_id
inner join subjects on ss.subject_id = subjects.subject_id
inner join study_subject_classes on ss.study_subject_id = study_subject_classes.study_subject_id
inner join study_subject_attendance on study_subject_classes.study_subject_class_id = study_subject_attendance.class_id
where end_time < getdate()
group by ss.study_subject_id, study_subject_attendance.user_id, study_subject_classes.study_subject_class_id
```

## Raport bilokacji: lista osób, które są zapisane na co najmniej dwa przyszłe szkolenia, które ze sobą kolidują czasowo.

```sql
with user_timetable as (
    select
    users.firstname + ' ' + users.lastname as username,
    start_time,
    end_time
from course_enrollment
inner join courses on course_enrollment.course_id = courses.course_id
inner join users on course_enrollment.user_id = users.user_id
union
select
    users.firstname + ' ' + users.lastname as username,
    start_time,
    end_time
from study_enrollment
inner join studies on study_enrollment.study_id = studies.study_id
inner join users on study_enrollment.user_id = users.user_id
),
user_timetable_numbered as (
  select *, row_number() over (order by username) as id
  from user_timetable
)
select DISTINCT username
from user_timetable_numbered u1
where exists(
    select 1
    from user_timetable_numbered u2
    where u2.start_time <= u1.end_time and u2.end_time >= u1.start_time and u2.username = u1.username and u1.id != u2.id
)
```
