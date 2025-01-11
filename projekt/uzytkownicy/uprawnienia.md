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
