## categories
zawiera kategorie do jakich należą produkty: kursy, webinary, studia
- category_id - 
- category_name - nazwa kategorii
```sql
category_id int identity primary key,
category_name varchar(50) not null
```

## courses
zawiera informacje o kursach
- course_id - klucz główny, ID każdego kursu
- name - nazwa kursu
- category_id - referencja do kategorii produktu
- price - cena kursu
- start_time - data rozpoczęcia kursu
- end_time - data zakończenia kursu
- enrollment_limit - limit zapisanych na kurs
- product_id - referencja do ID produktu

```sql
course_id        int identity        constraint courses_pk            primary key,
name             varchar(50) not null,
category_id      int         not null,
price            float       not null,
start_time       datetime    not null,
end_time         datetime    not null,
enrollment_limit int         not null,
product_id       int        constraint courses_products_product_id_fk            references products
```

## course_modules
zawiera informacje o modułach będących częścią w danym kursie
- course_module_id - klucz główny, ID przypisania modułu do kursu
- module_id - referencja do ID modułu
- course_id - referencja do ID kursu, do którego moduł przynależy
- online_meeting_link - link do modułu, jeśli ten jest w formule zdalnej lub hybrydowej
- class_name - nazwa klasy, w której odbywają się zajęcia

```sql
course_module_id    int identity        constraint course_modules_pk            primary key,
module_id           int not null        constraint course_modules_modules_module_id_fk            references modules,
course_id           int not null        constraint course_modules_courses_course_id_fk            references courses,
online_meeting_link varchar(30),
class_name          varchar(30)
)
```

## course_details
zawiera szczegóły danego kursu
- course_id - ID kursu, do którego się odwołuje
- enrolled - liczba zapisanych kursantów
- attendance_mode - formuła, w jakiej odbywają się zajęcia
```sql
course_id       int         references courses,
enrolled        int         not null,
attendance_mode varchar(20) not null
```

## course_enrollment
zawiera informacje o zapisanych na dany kurs
- enrollment_id - klucz główny
- user_id - referencja do użytkownika zapisanego na kurs
- course_id - referencja do kursu, na który użytkownik został zapisany

```sql
enrollment_id int not null        constraint course_enrollment_pk            primary key,
user_id       int        constraint course_enrollment_users_user_id_fk            references users,
course_id     int        constraint course_enrollment_courses_course_id_fk            references courses
```

## course attendace
zawiera informacje o uczestnictwie kursantów w zajęciach

- attendance_id - klucz główny, ID obecności
- date - data odbywania się kursu
- student_id - referencja do ID kursanta
- attended - informacje czy kursant zdobył obeność na zajęciach
- course_module_in - referencja do modułu kursu, dla której liczona jest obecność

```sql
attendance_id    int identity        primary key,
date             datetime not null,
student_id       int      not null        references users,
attended         binary   not null,
course_module_id int      not null        constraint module_attendance_course_modules_course_module_id_fk            references course_modules
```
## module_online_meeting
zawiera informacje o modułach odbywających się zdalnie

- module_online_meeting - klucz główny, ID spotkania online
- course_module_id - referencja do modułu zawierającego się w kursie
- recording_id - referencja do ID nagrania
```sql
module_online_meeting_id int identity        constraint module_online_meeting_pk            primary key,
course_module_id         int not null        constraint module_online_meeting_course_modules_course_module_id_fk            references course_modules, 
recording_id             int                 references recordings
```
