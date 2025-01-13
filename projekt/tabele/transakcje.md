## invoices
zawiera informacje o zakupach, które zakończyły się wystawieniem faktury

- invoice_id - klucz główny, ID każdej faktury
- currency - waluta, wktórej odbywa się zakup
- value - wartość zamówienia w walucie
- exchange_rate - wymienialność waluty na złotówki
- is_exempted - jeśli dyrektor postanowi dać komuś indywidualną zniżkę
- user_id - referencja do użytkownika, dokonującego zakupu
- payment_deadline - data, do której należy uiścić zapłatę
- product_id - referencja do produktu znajdującego się na fakturze
- payment_link - link do zapłaty
- is_paid - czy faktura została już opłacona

```sql
invoice_id       int identity        primary key,
currency         varchar(20) not null,
value            float,
exchange_rate    float,
is_exempted      binary      not null,
user_id          int         not null        references users,
payment_deadline datetime,
product_id       int         not null        constraint invoices_products_product_id_fk            references products,
payment_link     varchar(80) not null,
is_paid          bit         not null
```

## shopping_carts
zawiera klucz główny koszyka i referencję do użytkownika dokonującego zakupu

- cart_id - klucz główny, ID koszyka
- user_id - referencja do użytkownika tworzącego koszyk

```sql
cart_id int identity        primary key,
user_id int        references users
```

## shopping_carts_products
zawiera produkty znajdujące się w danym koszyku

- cart_id - referencja do koszyka zakupowego
- product_id - referencja do ID produktu w koszyku
- price - cena produktu
- quantity  - liczba sztuk danego produktu w koszyku

```sql
cart_id    int        references shopping_carts,
product_id int        references products,
price      int not null,
quantity   int not null
```
