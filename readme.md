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


## Связи таблиц

    Механизм связывания двух таблиц называется (соединением (join))

    Пример запроса:
```
    SELECT customer.first_name, customer.last_name,
    ->  time(rental.rental_date) rental_time
    -> FROM customer
    ->  INNER JOIN rental
    ->  ON customer.customer_id = rental.costomer_id
    -> WHERE date(rental.rental_date) = '2005-06-14';
```
    Пример Ответа:

```
    +------------+-----------+-------------+
    | first_name | last_name | rental_time |
    +------------+-----------+-------------+
    | CATHERINE  | CAMPBELL  | 23:17:03    |
    | JOYCE      | EDWARDS   | 23:16:26    |
    | AMBER      | DIXON     | 23:42:56    |
    | JEANETTE   | GREENE    | 23:54:46    |
    | MINNIE     | ROMERO    | 23:00:34    |
    | GWENDOLYN  | MAY       | 23:16:27    |
    | SONIA      | GREGORY   | 23:50:11    |
```

## Определение псевдопимов таблиц.
    Псевдоним -> сокращенное название таблици.

    Пример запрова с псевданимами:
```
SELECT c.first_name, c.last_name,
    -> time(r.rental_date) rental_time
    -> FROM customer c
    -> INNER JOIN rental r
    -> ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) = '2005-06-14';
```

Аналогичный пример с ключевым словом AS:

```
SELECT с.first_name, c.last_name,
    -> time(r.rental_date) rental_time
    -> FROM customer AS c
    -> INNER JOIN rental AS r
    -> ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) = '2005-06-14';
```

## Предложение where:
    Предложение where - это механизм фильтрации нежелательных строк из нашего результирующего набора.

    Пример запроса с предложение WHERE:
    ```
    SELECT title
    -> FROM film
    -> WHERE rating = 'G' AND rental_duration >= 7;
    ```
    Ответ на запрос:
```
    +-------------------------+
    | title                   |
    +-------------------------+
    | BLANKET BEVERLY         |
    | BORROWERS BEDAZZLED     |
    | BRIDE INTRIGUE          |
    | CATCH AMISTAD           |
    | CITIZEN SHREK           |
    | COLDBLOODED DARLING     |
    | CONTROL ANTHEM          |
    ...
```
    Пример разделенного скобками '()' запроса:

    ```
    SELECT title, rating, rental_duration
    -> FROM film
    -> WHERE (rating = 'G' AND rental_duration >= 7)
    ->  OR (rating = 'PG-13' AND rental_duration < 4);
    ```
## Предложение order by
    Предложение order by -> это механизм для сщртировки вашего результируещего набора с использованием любого необратимого столбца данных или выражения на основе данных столбца.

    Пример Запроса:
    ```
    SELECT c.first_name, c.last_name,
    ->  time(r.rental_date) rental_tame
    -> FROM customer c
    -> INNER JOIN rental r
    -> ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) = '2005-06-14';
    ```

# Глава 4 Фильтрация!

### Условия раветства 'условие=выражение'

    Примеры: 
    ```
    title = 'RIVER OUTLAW'
    fid_id = '111-111-1111'
    amount = 375.25
    film_id = (SELECT film_id FROM film WHERE title = 'RIVER OUTLAW')
    ```
    Пример запроса:
    SELECT c.email
    ->  FROM customer c
    -> INNER JOIN rental r
    -> ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) = '2005-06-14';
    В данном запросе присудствует два условия равенства:
    Первое условие равентсва  ON (условие соединение)
    Второе условие равенства WHERE (условие фильтрации)

### Условия неравенства.

    Данное условие проверяет что два выражения НЕ равны!
    Пример запроса с условием неравенства: 
    ```
    SELECT c.email
    ->  FROM customer c
    -> INNER JOIN rental r
    -> ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) != '2005-06-14';
    ```
### Условия диапазона.
    Данное условие проверяет попадает ли выражение в определенный диапазон.
    Пример запроса с условием диапазона:
    ```
    SELECT customer_id, rental_date
    -> FROM rental
    -> WHERE rental_date < '2005-05-25';
    ```
    Пример запроса с условием более узкого дианазона:
    ```
    SELECT customer_id, rental_date
    -> FROM rental
    -> WHERE rental_date <= '2005-06-16'
    ->  AND rental_date >= '2005-06-14';
    ```
### Оператор beetween
    Тоже самое что и предыдущий раздел.
    Необходимо запомнить что после оператора BEETWEN указывается нижний предел
    а после AND верхний.

    ```
    SELECT customer_id, rental_date
    ->  FROM rental
    ->  WHERE rental_date BETWEEN '2005-06-14' AND '2005-06-16';
    ```

    Пример запроса с числовым диапазонам с использованием оператора BETWEEN:
    ```
    SELECT customer_id, payment_date, amount
    -> FROM payment
    -> WHERE amount BETWEEN 10.0 AND 11.99;
    ```

    Пример условия с использованием строкового диапазона:
    ```
    SELECT last_name, first_name
    -> FROM customer
    -> WHERE last_name BETWEEN 'FA' AND 'FR';
    ```

### Условие членства.

    Пример запроса членства при помощи оператора OR.
    ```
    SELECT title, rating
    -> FROM film
    -> WHERE rating = 'G' OR rating = 'PG';
    ```

    Пример запроса членства при помощи оператора IN.
    ```
    SELECT title, rating
    ->  FROM film
    ->  WHERE rating IN ('G', 'PG');
    ```

    При помощи данного оператора вы можете написать одно условие
    не зависимо от того сколько выражений входит в набор.

### Использование подзапросов.

    Пример использования подзапроса:
    ```
    SELECT title, rating
    -> FROM film
    -> WHERE rating IN (SELECT rating FROM film WHERE title LIKE '%PET%');
    ```
    Данный запрос считает что любой фильм название которого включает
    строку 'PET', будет безопастным для семейного просмотра.
    Подзапрос возвращает набор 'G' и 'PG', а основной запрос проверяет
    находится ли значение столбца rating в наборе, возвращаемом подзапросом.

### Оператор not in.
    Мы можем добится такого-же результата используя оператор NOT IN:
    ```
    SELECT title, rating
    -> FROM film
    -> WHERE rating NOT IN ('PG-13', 'R', 'NC-17');
    ```
    Этот запрос находит все записи, не помеченные рейтингом 'PG-13', 'R', 'NC-17'.

### Условия соответствия:
    Пример запроса чьи фамилии начинаются на Q:
    ```
    SELECT last_name, first_name
    -> FROM customer
    -> WHERE left(last_name, 1) = 'Q';
    ```
    Ответ:
    ```
    +-------------+------------+
    | last_name   | first_name |
    +-------------+------------+
    | QUALLS      | STEPHEN    |
    | QUINTANILLA | ROGER      |
    | QUIGLEY     | TROY       |
    +-------------+------------+
    3 rows in set (0.00 sec)
    ```
### Использование подстановочных знаков.

    Пример Запроса:
    ```
    SELECT last_name, first_name
    ->  FROM customer
    ->  WHERE last_name LIKE '_A_T%S';
    ```
    Выражение поиска в предыдущем примере определяется строки, содержащие букву А во второй позиции и T - в четвертой позиции, за которым следует любое кол-во символов, заканчивается буквой S.

* Примеры выражений поиска.
    F% -> Строка начинающаяся с F
    %t -> Строка, заканчивается t
    %bas% -> Строка, содержащая 'bas'
    __t_ -> Строка из 4 символов с t в третей позиции
    ___-__-____ -> Строка из 11 символов с дефисами в четвертой и седьмой позициях.


