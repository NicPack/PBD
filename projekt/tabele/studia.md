## studies
zawiera informacje na temat kierunków studiów

- study_id - klucz główny, identyfikator kierunku
- name - nazwa kierunku
- category_id - referencja do ID kategorii produktu
- start_time - data rozpoczęcia zajęć kierunku
- end_time - data zakończenia zajęć kierunku
- enrollment_limit - limit zapisanych na kierunek
- product_id - referencja do ID produktu

```sql
study_id         int identity        constraint studies_pk            primary key,
name             varchar(50) not null,
category_id      int         not null,
start_time       datetime    not null,
end_time         datetime    not null,
enrollment_limit int         not null,
product_id       int        constraint studies_products_product_id_fk            references products
```

## subjects
zawiera informacje o przedmiotach

- subject_id - klucz główny, identyfikator przedmiotu
- name - nazwa przedmiotu

```sql
subject_id int identity        constraint subjects_pk            primary key,
name       varchar(30) not null
```

## study_subjects
zawiera informacje o przedmiotach na danych kierunkach

- study_subject_id - klucz główny, identyfikator przedmiotu na studiach
- study_id - referencja do ID kierunku studiów
- subject_id - referencja do ID przedmiotu
- online_meeting_link - link do zajęć, jeśli są online
- classroom - nazwa klasy, jeśli zajęcia są stacjonarnie
- min_attendance - minimalna liczba studentów na przedmiocie

```sql
study_subject_id    int identity        constraint study_subjects_pk            primary key,
study_id            int not null        constraint study_subjects_studies_study_id_fk            references studies,
subject_id          int not null        constraint subject_id            references subjects,
online_meeting_link varchar(30),
classroom           varchar(30),
min_attendance      int not null
```


## study enrollment
zawiera informacje o zapisanych na studia

- study_enrollment_id - klucz główny, ID każdego wpisu na studia
- user_id - referencja do ID studenta
- study_id - referencja do ID kierunku

```sql
study_enrollment_id int identity        constraint study_enrollment_pk            primary key,
user_id             int not null        constraint study_enrollment_users_user_id_fk            references users,
study_id            int not null        constraint study_enrollment_studies_study_id_fk            references studies
```

## study_subject_attendance
zawiera obecności na danych zajęciach

- attendance_id - klucz główny, ID każdej obecności na zajęciach
- user_id - referencja do ID użytkownika
- class_id - referencja do ID przedmiotu

```sql
attendance_id int identity        constraint study_subject_attendance_pk            primary key,
user_id       int not null        constraint study_subject_attendance_users_user_id_fk            references users,
class_id      int not null        constraint study_subject_attendance_study_subject_classes_study_subject_class_id_fk            references study_subject_classes
```

## study_subject_classes
zawiera informacje odbywających się zajęciach w danych grupach dziekańskich

- study_subject_class_id - klucz główny, ID połączenia kierunku studiów, przedmiotu i grupy dziekańskiej
- study_subject_id - referencja do ID kierunku studiów i przedmiotu
- date - data odbycia się zajęć
- classroom - zawiera nazwę klasy jeśli zajęcia odbywają się na miejscu
- online_meeting_link - zawiera link do spotkania jeśli zajęcia prowadzone są zdalnie

```sql
study_subject_class_id int identity        constraint study_subject_classes_pk            primary key,
date                   datetime not null,
study_subject_id       int      not null        constraint study_subject_classes_study_subjects_study_subject_id_fk            references study_subjects,
classroom              varchar(30),
online_meeting_link    varchar(30)
```

## classes
Tabela przechowująca informacje o zajęciach
- class_id - klucz główny, ID zajęć
- class_name - nazwa zajęć
- study_id - referencja do kierunku, na którym są dane zajęcia
- enrollment_limit - limit uczniów zapisanych na zajęcia

```sql
class_id         int identity primary key,
class_name       varchar(50),
study_id         int references studies,
enrollment_limit int not null
```

## lecturers
zawiera informacje o wykładowcach

- lecturer_id - klucz główny, ID wykładowcy
- first_name - imię wykładowcy
- last_name - nazwisko wykładowcy

```sql
lecturer_id int identity        primary key,
first_name  varchar(30) not null,
last_name   varchar(30) not null
```

## lecture_details
zawiera szczegóły wykładów
- lecture_id - referencja do ID produktu
- lecturer_id - referencja do wykładowcy
- langugage - język w jakim wykład jest prowadzony
- is_translated - czy wykład jest tłumaczony

```sql
lecture_id    int        constraint lecture_datails_products__fk            references products,
lecturer_id   int        references lecturers,
language      varchar(30) not null,
is_translated bit         not null
```

## class_attendance
Przechowuje obecność studenta na zajęciach

- attendance_id - klucz główny, ID każdej obecności
- class_id - ID klasy, dla jakiej zliczona jest obecność
- date - data zanotowania obecności
- location - lokalizacja zajęć
- student_id - ID studenta, któremu została przypisana obecność

```sql
attendance_id int identity primary key,
class_id      int references classes,
date          datetime not null,
location      varchar(50),
student_id    int references users
```

## internship_attendance
zawiera informacje o odbytych praktykach

- attendance_id - klucz główny, ID każdej zaliczonej praktyce
- student_id - referencja do studenta, który odbył praktyki
- study_id - referencja do kierunku studiów

```sql
attendance_id int identity        primary key,
student_id    int        references users,
study_id      int        references studies
```


## final_exams
zawiera informacje o egzaminach końcowych

- exam_id - klucz główny, ID egzaminu,
- student_id - referencja do studenta podchodzącego do egzaminu
- study_id - referencja do kierunku, na którym jest egzamin

```sql
exam_id    int identity        primary key,
student_id int        references users,    result     float not null,
study_id   int   not null        constraint final_exams_studies_study_id_fk            references studies
```

## diplomas
zawiera informacje o dyplomach wydawanych studentom

- diploma_id - klucz główny, ID każdego dyplomu
- student_id - referencja do studenta, któremu jest wydawany dyplom
- study_id - referencja do kierunku, na którym student studiował
```sql
diploma_id int identity        primary key,
student_id int        references users,
study_id   int not null        constraint diplomas_studies_study_id_fk            references studies
```
