# Macież uprawnień

Uprawnienia nadane każdej roli.
- Read- odczytywanie
- Write- nadpisywanie
- Insert- tworzenie nowych wpisów
- full- wszystkie 3 

| rola  |  raporty finansowe | lista „dłużników”  | informacje o kursach  |  lista obecności |  raport bilokacji |zapis na kurs   | umożenie długu  |  nadanie rabatu | 
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|  admin |  full | full  | full  | full  | full  |   |   |  |   
|  registrar |   |  full |  full |  full |  full | full  |   |   |   
|  faculty |   |   | Read, Write  | full |   |   |   |   |   
|  student |   |   |  Read | Read  |   |   |   |   |  
|  director |   |  Read |  Read |   |   |   | full  |  full |   
| finance team  | full  | Read  |   |   |   |   |   |   |   

## studenci

```sql
grant execute on AddOrder to student
grant select on studies to student
grant select on courses to student
grant select on webinars to student
grant select on lectures to student
grant select on subjects to student
grant select on products to student
grant select on recordings to student
```

## pracownicy dziekanatu

```sql
grant select on BiLocation to registrar
grant select on DebtList to registrar
grant select on EventAttendance to registrar
grant select on FutureEventEnrollment to registrar
grant execute on CreateDiplomas to registrar
grant execute on AddCourse to registrar
grant execute on AddWebinar to registrar
grant execute on AddModuleToCourse to registrar
grant execute on AddStudy to registrar
grant select, insert , update on studies to registrar
grant select, insert , update on courses to registrar
grant select, insert , update on classes to registrar
grant select, insert , update on course_enrollment to registrar
grant select, insert , update on course_modules to registrar
grant select, insert , update on diplomas to registrar
grant select, insert , update on internship_attendance to registrar
grant select, insert , update on lecture_details to registrar
grant select, insert , update on module_attendance to registrar
grant select, insert , update on module_online_meeting to registrar
grant select, insert , update on modules to registrar
grant select, insert , update on open_lectures to registrar
grant select, insert , update on products to registrar
grant select, insert , update on recordings to registrar
grant select, insert , update on study_enrollment to registrar
grant select, insert , update on study_subject_attendance to registrar
grant select, insert , update on study_subject_classes to registrar
grant select, insert , update on subjects to registrar
grant select, insert , update on user_address to registrar
grant select, insert , update on webinars to registrar
```

## wykładowcy

```sql
grant select on EventAttendance to faculty
grant select on FutureEventEnrollment to faculty
grant select, insert , update on studies to faculty
grant select, insert , update on courses to faculty
grant select, insert , update on classes to faculty
grant select, insert , update on course_modules to faculty
grant select, insert , update on lecture_details to faculty
grant select, insert , update on module_attendance to faculty
grant select, insert , update on module_online_meeting to faculty
grant select, insert , update on modules to faculty
grant select, insert , update on open_lectures to faculty
grant select, insert , update on study_subject_attendance to faculty
grant select, insert , update on study_subject_classes to faculty
grant select, insert , update on subjects to faculty
grant select, insert , update on webinars to faculty
```

## finanse

```sql
grant select on FinanceReport to finance
grant select on DebtList to finance
grant select, update on shopping_carts to finance
grant select, update on shopping_carts_products to finance
grant all privileges on invoices to finance
grant select on users to finance
grant select on user_address to finance
```

## dyrektor

```sql
grant select on DebtList to director
grant select on EventAttendance to director
grant select on FutureEventEnrollment to director
grant select, update on invoices to director
grant select, update on shopping_carts to director
grant select, update on shopping_carts_products  to director
```
