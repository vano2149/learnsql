### Page -> 79

### Для использовани базы данных используем комманду USE database name.
### Для просмотра таблици используем команду DESC table name.
```
    SET FOREIGN_KEY_CHECKS = 0;

    /* DO WHAT YOU NEED HERE */

    SET FOREIGN_KEY_CHECKS = 1;
```
### INSERT TO -> добавить в таблицу.

### ORDER BY -> упорядочить по.

### "High Performance MySQL" -> После этой книги посмотреть вот эту книгу!

### Глава 3.
0. Узнаем путь до базы данных ->  SELECT @@datadir;  

1. SELECT -> Oпределяет какие столбци следует включить в результирующий набор запроса.

2. SELECT DISTINCT -> удаление повторяющихся элементов в таблице.

3. FROM ->  Определяет таблициб из которых следует брать инф.

4. WHERE -> Отсеивает ненужные данные.

5. GROUP BY -> Используется для групировки строк по общим значениям столбцов

6. HAVING -> Отсеивает ненужные данные.

7. ORDER BY -> Сортирует строки окончательного результирующего набора по одному или несколькими столбцами.

### Подзапрос
    Подзапрос это запросб содержащий в другом заросе. Подзапросы окружены круглыми скобками и могут встречаться в различных частях инструкции SELECT.

    Пример запроса:
    ```
    SELECT concat(cust.last_name, ", ", cust.first_name) fullname
        -> FROM
        ->(SELECT first_name, last_name, email
        -> FROM customer
        -> WHERE first_name = 'JESSIE'
        -> ) cust;
    ```
    Ответ Запроса:
    ```
    +---------------+
    | full_name     |
    +---------------+
    | BANKS, JESSIE |
    | MILAM, JESSIE |
    +---------------+
    ```
### Временные таблици
    Таблица -> которая исчезает по истечению определенного промежутка времени. Обычно исчезают в конце транзакции или при закрытии сеанса базы данных.

    Пример создания временной таб.
    ```
    CREATE TEMPORARY TABLE actors_j
        -> (actor_id cmallint(5),
        -> first_name varchar(45),
        -> last_name varchar(45));
    
    Query OK, 7 rows affected (0.03 sec)
    Records: 7 Dublicates: 0 Warnings: 0
    ```
    Заполнения временной таблициб
    ```
    INSERT INTO actors_j
    -> SELECT actor_id, first_name, last_name
    -> FROM actor
    -> WHERE last_name LIKE 'J%';

    Query OK, 7 rows affected (0.03 sec)
    Records: 7 Duplicates: 0 Warnings: 0
    ```
    Просматриваем что во временной табл.
    ```
    SELECT * FROM actors_j;
    ```
    Получаем ответ:
    ```
    +----------+------------+-----------+
    | actor_id | first_name | last_name |
    +----------+------------+-----------+
    |      119 | WARREN     | JACKMAN   |
    |      131 | JANE       | JACKMAN   |
    |        8 | MATTHEW    | JOHANSSON |
    |       64 | RAY        | JOHANSSON |
    |      146 | ALBERT     | JOHANSSON |
    |       82 | WOODY      | JOLIE     |
    |       43 | KIRK       | JOVOVICH  |
    +----------+------------+-----------+
    ```
Эти семь строк временно хроняться в памяти и исчезнут после закрытия подключения к базе данных.

### Представление.
    Представление -> это запрос, который храниться в словаре данных. Он выглядит и действует как таблица.

    Пример создания предстовления: 
    ```
    CREATE VIEW cust_vw AS
    ->  SELECT customer_id, first_name, last_name, active
    ->  FROM customer;
    Query OK, 0 rows affected (0.12 sec)
    ```
    Пример написанны выше откладывает инструкцию select для использования в будующем.

    Пример использования представления в запросе.
    ```
    SELECT first_name, last_name
    -> FROM cust_vw
    -> WHERE active = 0;
    ```
    Ответ: 
    ```
    +------------+-----------+
    | first_name | last_name |
    +------------+-----------+
    | SANDRA     | MARTIN    |
    | JUDITH     | COX       |
    | SHEILA     | WELLS     |
    | ERICA      | MATTHEWS  |
    | HEIDI      | LARSON    |
    ```
    Представления создаются по разным причинам, в том числе и для того, что-бы скрыть столбцы от пользователей и упрастить сложные конструкции бaз данных.





