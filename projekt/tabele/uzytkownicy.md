## roles
zawiera dos†ępne role użytkowników systemu

- role_id - klucz główny, ID roli
- role_name - nazwa dostępnej roli

```sql
role_id   int identity        primary key,
role_name varchar(20) not null
```

## users
zawiera informacje o użytkownikach

- user_id - klucz główny, identyfikator każdego użytkownika
- role_id - referencja do roli przypisanej użytkownikowi
- firstname - imię użytkownika
- lastname - nazwisko użytkownika

```sql
user_id   int identity        primary key,
role_id   int        references roles,
firstname varchar(30) not null,
lastname  varchar(30) not null
```

## user addresses
zawiera informacje o adresach zamieszkania użytkowników

- user_id - referencja do identyfikatora każdego użytkownika
- country - kraj zamieszkania
- city - miasto zamieszkania
- postal_code - kod pocztowy
- street_name - nazwa ulicy
- building_number - numer budynku
- flat_number - numer mieszkania, jesli dotyczy

```sql
user_id         int        references users,
country         varchar(50) not null,
city            varchar(50) not null,
postal_code     int         not null,
street_name     varchar(50) not null,
building_number int         not null,
flat_number     int
```
