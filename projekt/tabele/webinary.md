## webinars
zawiera informacje na temat webinarow

- webinar_id - klucz główny, identyfikator każdego webinaru
- name - nazwa webinaru
- category_id  - referencja do ID kategorii
- start_time - data rozpoczęcia webinaru
- end_time - data zakończenia webinaru
- enrollment_limit - limit zapisanych na webinar
- is_deleted - czy nagranie zostało usunięte
- recording_id - referencja do nagrania z webinaru
- product_id - referencja do ID produktu

```sql
webinar_id       int identity        constraint webinars_pk            primary key        constraint webinars_webinars__fk            references products,
name             varchar(50) not null,
category_id      int         not null        references categories,
start_time       datetime    not null,
end_time         datetime    not null,
enrollment_limit int         not null,
is_deleted       bit,
recording_id     int        constraint webinars_recordings_recording_id_fk            references recordings,
product_id       int        constraint webinars_products_product_id_fk            references products
```
