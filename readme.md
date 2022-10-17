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

* Пример использования нескольких поисковых выражений.

    ```
    SELECT lase_name, firat_name
    -> FROM customer
    -> WHERE last_name LIKE '%Q' OR last_name LIKE 'Y%';
    ```
    Ответ на запрос:
    ```
    +-------------+------------+
    | last_name   | first_name |
    +-------------+------------+
    | QUALLS      | STEPHEN    |
    | QUIGLEY     | TROY       |
    | QUINTANILLA | ROGER      |
    | YANEZ       | LUIS       |
    | YEE         | MARVIN     |
    | YOUNG       | CYNTHIA    |
    +-------------+------------+
    6 rows in set (0.00 sec)
    ```
Название книги по регуляркам -> Изучаем регулярные выражения (Диалектика, 2019),
### Использование регулярных выражений.
    Пример придыдущего запроса с использованием регулярного выражения:
    ```
    SELECT last_name, first_name
    -> FROM customer
    -> WHERE last_name REGEXP '^[QY]';
    ```
    Ответ на запрос: 
    ```
    +-------------+------------+
    | last_name   | first_name |
    +-------------+------------+
    | QUALLS      | STEPHEN    |
    | QUIGLEY     | TROY       |
    | QUINTANILLA | ROGER      |
    | YANEZ       | LUIS       |
    | YEE         | MARVIN     |
    | YOUNG       | CYNTHIA    |
    +-------------+------------+
    6 rows in set (0.00 sec)
    ```
    Оператор REGEXP принемает регулярное выражение '^[QY]' и применяет его к выражению в левой части условия. 'last_name'

### Значение null.

    Значение null -> это отсутствие значения;
    Чтобы проверить, является ли выражение null, необходимо использовать 
    оператор is null.
    ```
    SELECT rental_id, customer_id
    -> FROM rental
    -> WHERE rental_date IS NULL;
    ```
    Запрос который находит фильмыб взфтые на прокат, которые не были возвращены.

    Пример запроса с возвращенными ыильмами и нет в определенном диапазоне:
    ```
    SELECT rental_id, customer_id, return_date
    -> FROM rental
    -> WHERE rturn_date IS NULL
    -> OR return_date NOT BERWEEN '2005-05-01' AND '2005-09-01';
    ```

# Глава 5
## Запросы к нескольким таблицам.

### Что такое соединение

Соединение -> это механизм получения данных из двух таблиц.

### Декартово произведение. 
    Пример запроса:
    ```
    SELECT c.first_name, c.last_name, a.address
    -> FROM customer c JOIN address a;
    ```
    Ответ:
    ```
    +----------+-----------+------- +
    |first name | last name |address|
    +----------+-----------+--------+
    361197 rows in set (0.09 sec)
    ```
    По скольку в запросе не указанно, как именно должны быть соеденны таблици, сервер базы данных сгенерировал декартово произведение, которое представляет собой все возможные сочетания записий из двух таблиц (599 клиектов * 603 адреса = 361197 сочитаний).
    Этот тип соединения известен как перекрестное соединение (cross join).
### Внутреннее соединение:
    
    Пример запроса:
    ```
    SELECT c.first_name, c.last_name, a.address
    -> FROM customer c JOIN address a
    ->  ON c.address_id = a.address_id;
    ```
    Благодоря предложению ON которое указывает серверу необходимоть соединение таблиц costomer b address, использую столбец address_id мы молучаем из 361197 строк 599.

    Пример запроса с внутренним соединением:
    ```
    SEECT c.first_name, c.last_name, a.address
    -> FROM customer c INNER JOIN address a
    ->  ON c.address_id = a.address_id;
    ```
    INNER JOIN -> внутреннее соединение.

### Синтаксис соединения ANSI:
    страница 120!
    Пример запросa:
    ```
    SELECT c.first_name, c.last_name, a.address
    -> FROM customer c INNER JOIN address a
    -> ON c.address_id = a.address_id
    -> WHERE a.postal_code = 52137;
    ```

### Соединение трех и более стр.
    Пример запроса: 
    ```
    SELECT c.first_name, c.last_name, ct.city
    ->  FROM customer c
    ->  INNER JOIN address a
    ->  ON c.address_id = a.address_id
    ->  INNER city ct
    ->  ON a.city_id = ct.city_id;

    SELECT c.first_name, c.last_name, ct.city
    -> FROM city ct
    -> INNER JOIN address a
    -> ON a.city_id = ct.city_id
    -> INNER JOIN customer c
    -> ON c.address_id = a.address_id;

    SELECT c.first_name, c.last_name, ct.city
    -> FROM address a
    -> ON a.city_id = ct.city_id
    -> INNER JOIN customer c
    -> ON c.address_id = a.address_id;
    ```

### Использование подзапросов в качестве таблиц.
    Пример Запроса: 
    ```
    SELECT c.first_name, c.last_name, addr.address, addr.city
    ->  FROM customer c
    ->  INNER JOIN
    ->      (SELECT a.address_id, a.address, ct.city
    ->          FROM address a
    ->              INNER JOIN city ct 
    ->              ON a.city_id = ct.city_id 
    ->          WHERE a.district = 'California'
    ->        ) addr
    ->       ON c.address_id = addr.address_id;
    ```
    Подзапрос из предыдущего запроса 'Отработка самого по себе!':
    ```
    SELECT a.address_id, a.address, ct.city
    ->  FROM address a
    ->   INNER JOIN city ct
    ->   ON a.city_id = ct.city_id
    -> WHERE a.dictrict = 'california';
    ```
    Ответ на запрос: 
    ```
    +------------+------------------------+----------------+
    | address_id | address                | city           |
    +------------+------------------------+----------------+
    |          6 | 1121 Loja Avenue       | San Bernardino |
    |         18 | 770 Bydgoszcz Avenue   | Citrus Heights |
    |         55 | 1135 Izumisano Parkway | Fontana        |
    |        116 | 793 Cam Ranh Avenue    | Lancaster      |
    |        186 | 533 al-Ayn Boulevard   | Compton        |
    |        218 | 226 Brest Manor        | Sunnyvale      |
    |        274 | 920 Kumbakonam Loop    | Salinas        |
    |        425 | 1866 al-Qatif Avenue   | El Monte       |
    |        599 | 1895 Zhezqazghan Drive | Garden Grove   |
    +------------+------------------------+----------------+
    ```
### Использование одной таблици дважды:

    Пример запроса:
    ```
    SELECT f.title
    ->  FROM film f
    ->  INNER JOIN film_actor fa
    ->  ON f.film_id = fa.film_id
    ->  INNER JOIN actor a
    ->  ON fa.actor_id = a.actor_id
    ->  WHERE ((a.first_name = 'CATE' AND a.last_name = 'MCQUEEN')
    ->  OR (a.first_name = 'CUBA' AND a.last_name = 'BIRCH'));
    ```
    Запрос который возвращает фильмы в которых снялись два актера:
    ```
    SELECT f.title
    -> FROM film f
    -> INNER JOIN film_actor fa1
    -> ON f.film_id = fa1.film_id
    -> INNER JOIN actor a1
    -> ON fa1.actor_id = a1.actor_id
    -> INNER JOIN actor a2
    -> ON fa2.actor_id = a2.actor_id
    -> WHERE (a1.first_name = 'CATE' AND a1.last_name = 'MCQUEEN')
    ->  AND (a2.firts_name = 'CUBA' AND a2.last_name = 'BRICH');
    ```
    Ответ:
    ```
    +------------------+
    | title            |
    +------------------+
    | BLOOD ARGONAUTS  |
    | TOWERS HURRICANE |
    +------------------+
    ```
### Самосоединение:

    самоссылающийся внешний ключ -> это означает что в таблици имеется столбецб ссылающийся на первичный ключ в тойже таблице.

### Проверь знание ЗАДАЧКИ!

    № 5.1
    ```
    SELECT c.first_name, c.last_name, a.address, ct.city
    ->  FROM customer c
    ->  INNER JOIN address a
    ->  ON c.address_id = a.address_id
    ->  INNER JOIN city ct
    ->  ON a.city_id = ct.city_id
    -> WHERE a.district = 'California';
    ```

    Ответ на запрос:

    ```
    +------------+-----------+------------------------+----------------+
    | first_name | last_name | address                | city           |
    +------------+-----------+------------------------+----------------+
    | PATRICIA   | JOHNSON   | 1121 Loja Avenue       | San Bernardino |
    | BETTY      | WHITE     | 770 Bydgoszcz Avenue   | Citrus Heights |
    | ALICE      | STEWART   | 1135 Izumisano Parkway | Fontana        |
    | ROSA       | REYNOLDS  | 793 Cam Ranh Avenue    | Lancaster      |
    | RENEE      | LANE      | 533 al-Ayn Boulevard   | Compton        |
    | KRISTIN    | JOHNSTON  | 226 Brest Manor        | Sunnyvale      |
    | CASSANDRA  | WALTERS   | 920 Kumbakonam Loop    | Salinas        |
    | JACOB      | LANCE     | 1866 al-Qatif Avenue   | El Monte       |
    | RENE       | MCALISTER | 1895 Zhezqazghan Drive | Garden Grove   |
    +------------+-----------+------------------------+----------------+
    ```
    № 5.2

    ```
    SELECT f.title
    -> FROM film f
    ->  INNER JOIN film_actor fa
    ->  ON f.film_id = fa.film_id
    ->  INNER JOIN actor a
    ->  ON fa.actor_id = a.actor_id
    ->  WHERE a.first_name = 'JOHN';
    ```



# Глава 6. Работа с множествами.

### Теория множеств на практике.

    ```
    SELECT 1 num, 'abc' str
    ->  UNION
    ->  SELECT 9 num, 'xyz' str;
    ```
* Данный запрос сообщяет серверу что необходимо объединить два столбца.
    (UNION) -> Оператор объединения множества.

    Ответ на запрос:
    ```
    +-----+-----+
    | num | str |
    +-----+-----+
    |   1 | abc |
    |   9 | xyz |
    +-----+-----+
    ```

## Операторы для работы с множествами.

### Оператор Union.

    Операторы UNION и UNION ALL позволяет объединить несколько наборов данных. Разница между ними заключается в том, что UNION сортирует объединенный набор и удаляет дубликаты, тогда как UNION ALL не делает этого.
    Первая версия запроса:
    ```
    SELECT 'CUST' typ, c.first_name, c.last_name
    ->  FROM customer c
    ->  UNION ALL
    ->  SELECT 'ACTR' typ, a.first_name, a.last_name
    ->  FROM actor a;
    ```

    Второй пример запроса с идентичными предложениями.

    ```
    SELECT 'ACTR' typ, a.first_name, a.last_name
    ->  FROM actor a
    ->  UNION ALL
    ->  SELECT 'ACTR' typ, a.first_name, a.last_name
    ->  FROM actor a;
    ```

    Вот составной запрос возвращающий повторяющиеся данные:
    ```
    SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
    -> UNION ALL
    -> SELECT a.first_name, a.last_name
    -> FROM actor a
    -> WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%';
    ```
    Ответ на запрос:

    ```
    +------------+-----------+
    | first_name | last_name |
    +------------+-----------+
    | JENNIFER   | DAVIS     |
    | JENNIFER   | DAVIS     |
    | JUDY       | DEAN      |
    | JODIE      | DEGENERES |
    | JULIANNE   | DENCH     |
    +------------+-----------+
    ```
    Запрос без дубликатов (UNION):

    ```
    SELECT c.first_name, c.last_name
    ->  FROM customer c
    ->  WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
    ->  UNION
    ->  SELECT a.first_name, a.last_name
    ->  FROM actor a
    ->  WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%';
    ```
    Ответ на запрос:

    ```
    +------------+-----------+
    | first_name | last_name |
    +------------+-----------+
    | JENNIFER   | DAVIS     |
    | JUDY       | DEAN      |
    | JODIE      | DEGENERES |
    | JULIANNE   | DENCH     |
    +------------+-----------+
    ```

## Правила применения операторов для работы с множествами.

### 1. Сортировка результатов составного запроса.

    Пример запроса:
    ```
    SELECT a.first_name fname, a.last_name lname
    ->  FRPM actor a
    ->  WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    ->  UNION ALL
    ->  SELECT c.first_name, c.last_name
    ->  FROM customer c
    ->  WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
    ->  ORDER BY lname, fname;
    ```
    В составном запросе часто совпадают имена столбцов, но эо не обязательное условие.

### 2. Приоритеты операций над множествами.

    Пример запроса:
    ```
    SELECT a.first_name, a.last_name
    ->  FROM actor a
    ->  WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    ->  UNION ALL
    ->  SELECT a.first_name, a.last_name
    ->  FROM actor a
    ->  WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'
    ->  UNION
    ->  SELECT c.first_name, c.last_name
    ->  FROM customer c
    ->  WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
    ```
    Этот пример составного запроса возвращает набор неуникальных имен.

    ```
    SELECT a.first_name, a.last_name
    ->  FROM actor a
    ->  WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    ->  UNION
    ->  SELECT a.first_name, a.last_name
    ->  FROM actor a
    ->  WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'
    ->  UNION ALL
    ->  SELECT c.first_name, c.last_name
    ->  FROM customer c
    ->  WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
    ```

### Проверь сои знания:
    Напишите составной запрос, который ноходит именя и  фамилии всех актеров и клиентов, чьи фамилии начинаются с буквы L.

    ```
    SELECT a.first_name, a.last_name
    ->  FROM actor a
    ->  WHERE a.last_name LIKE 'L%'
    ->  UNION ALL
    ->  SELECT c.first_name, c.last_name
    ->  FROM customer c
    ->  WHERE c.last_name LIKE 'L%';
    ```

# Глава 7: Генерация, обработка и переобразование данных.


* CHAR -> Хранит строки фиксированной длинны с заполнением знако мест пробелами.

* varchar -> Содержит строки переменной длинны. MySQL позволяет использовать в столбце varchar до 65535 символов.

* text -> Содержит очень большие строки  переменной длинны (обычно в этом контексте именнуемые документами).

    MySQL имеет несколько текстовых типов:
    * tinytext
    * text
    * mediumtext
    * longtext

### Пример запроса на создания таблицы:
    ```
    CREATE TABLE string_tbl
    -> (char_fld CHAR(30),
    -> vchar_fld VARCHAR(30),
    -> text_fld TEXT);
    ```
## Генерация строк:
    INSERT INTO string_tbl (char_fld, vchar_fld, text_fld)
    ->  VALUE ('This is char data',
    ->  'This is varchar data',
    ->  'This is text data');

## Включение одинаковых кавычек:
* Пример запроса:
    Просто ставим еще один опостров.
    ```
    UPDATE string_tg1
    -> SET text_fld = 'This string didn''t work, but it does now';
    ```
* Для извлечения строки и использования в другой программе, используем функцию quote()
    Пример запроса с использованием функции quote():
    ```
    SELECT quote(text_fld)
    ->  FROM string_tg1;
    ```

## Включение сппециальных символов

* Пример запроса:
    ```
    SELECT 'abcdefg', CHAR(97,98,99,100,101,102,103);
    ```
    CONCAT() -> Функция конкатинации строк.
    ascii()  -> Функция возвращающая номер символа в ascii кодировке.
    char()   -> Функция преобразующая номер символа в символ.

## Манипуляция строками.
* Удаляем и Наполняем зановов таблицу:
    ```
    DELETE FROM string_tb1;

    INSERT INTO string_tg1 (char_fld, vchar_fld, text_fld)
    ->  VALUE ('This string is 28 characters',
    ->      'This string is 28 characters',
    ->      'This string is 28 characcters');
    ```

### Строковые функции, возвращающие числовые значения.
    Length() -> Функция возвращающая кол-во элементов в строке.
* Пример запроса:
    ```
    SELECT LENGTH(char_fld) char_length,
    ->  LENGTH(vchar_fld) varchar_length,
    ->  LENGTH(text_fld) text_length
    ->FROM string_tg1;
    ```

    position() -> Функция возвращающая местоположение подстрок внутри строки.

* Пример запроса:
    ```
    SELECT POSITION('characters' IN vchar_fld)
    -> FROM string_tg1;
    ```
    
    Для поиска с определенной позииции отличающейся от первой использует функцию locate().

* Пример запроса:
    ```
    SELECT LOCATE('is', vchar_fld, 5)
    -> FROM string_tg1;
    ```
Еще одна функция которая примимает числовые значения, - это функция сравнения строк strcmp() -> данная функция не чувствительна к регистру.

    Она принимает в качестве фргументов две строки и возвращает одно из следующих значений:
* -1 -> если первая строка предшествует второй в порядке сортировки;
*  0 -> если строки идентичны;
*  1 -> если первая строка в порядке сортировки идет после второй.
Следующий запрос выполняет шесть сравнения между пятью разными строками.

    ```
    SELECT STRCMP('12345', '12345') 12345_12345,
    -> STRCMP('abcd', 'xyz') abcd_xyz,
    -> STRCMP('abcd', 'QRSTUV') abcd_QRSTUV,
    -> STRCMP('qrstuv', 'QRSTUV') qrstuv_QRSTUV,
    -> STRCMP('12345', 'xyz') 12345_xyz,
    -> STRCMP('xyz', 'qrstuv') xyz_qrstuv;
    ```
    Ответ на запрос:
    ```
    +-------------+----------+-------------+---------------+-----------+------------+
    | 12345_12345 | abcd_xyz | abcd_QRSTUV | qrstuv_QRSTUV | 12345_xyz | xyz_qrstuv |
    +-------------+----------+-------------+---------------+-----------+------------+
    |           0 |       -1 |          -1 |             0 |        -1 |          1 |
    +-------------+----------+-------------+---------------+-----------+------------+
    1 row in set (0.00 sec)
    ```
    Пример запроса с select и регулярки:

    ```
    SELECT name, name, LIKE '%y' ends_in_y
    ->  FROM category;
    ```
    Если имя заканчиваается буквой 'y' -> 1, в противном случае -> 0.
    
    Второй столбец этого запроса возвращает 1, если значение хранится в столбце name, соответствует данному регулярному выражению.
    ```
    SELECT name, name REGEXP 'y$' ends_in_y
    ->  FROM category;
    ```

## Строковые функции, возвращающие строки.

* Функция concat() -> Добавить к хронмой строке дополнителные символы.
    ```
    UPDATE string_tg1
    -> SET text_fld = CONCAT(text_fld, ', but now it is longer');
    ```
* Содержимое строки выглядит вот так:
    ```
    SELECT text_fld
    -> FROM string_tg1;
    ```
    ```
    +-----------------------------------------------------+
    | text_fld                                            |
    +-----------------------------------------------------+
    | This string was 29 characters, but now it is longer |
    +-----------------------------------------------------+
    ```
* Построение строки из отдельных фрагментов данных.
    ```
    SELECT concat(first_name, ' ', last_name,
    ->  ' has been a customer since ',
    ->  date(create_date)) cust_narrative
    ->  FROM customer; 
    ```

    Функция insert которая принемает четыре аргуманта:
    Исходную строку -> 'goodbye world'
    Позицию с которой следует начать вставку -> 9
    Кол-во вставляемых символов -> 0
    Вставляемую строку -> 'cruel '

    Пример запроса:
    ```
    SELECT INSERT('goodbye world', 9, 0, 'cruel ') string;
    ```

    Если третий аргумант больше нуля, то он указывает на кол-во элементов которые заменяются вставляемой строкой.
    ```
    SELECT INSERT('goodbye world', 1, 7, 'hello') string;
    ```
    Придыдущий пример с использованием функции replase()
    ```
    SELECT REPLACE('goodbye world', 'goodbye', 'hello')
    ->  FROM dual;
    ```
    По мимо свтавки символов в строку нам может потребоваться извлечение подстроки из строки.
    В этом нам помможет функция substring()

    ```
    SELECT SUBSTRING('goodbye cruel world', 9, 5);
    ```
    Название книги ->  Джеймс Грофф, Пол Вайнберг, Эндрю Оппель. SQL. Полное руководство.— СПб. : ООО “Диалектика”, 2020.

### Работа с числовыми данными.

    Пример запроса:
    ```
    SELECT (37 * 59) / (78 - (8 * 6));
    ```
### Выполнение матаматических функций:

    Остаток от делений:
    ```
    SELECT MOD(10, 4);
    ```

    ```
    SELECT MOD(22.75, 5);
    ```
    Возведение в степень:
    ```
    SELECT POW(2, 8); -> Эквивалентко 2 в 8 степени.
    ```
    Функция pow() может быть удобным средством определения точности количества байтов в определенном объеме памяти.

    ```
    SELECT POW(2,20) kilobyte, POW(2,20) megabyte,
    ->  POW(2,30) gigabyte, POW(2,40) terabyte;
    ```
    +----------+----------+------------+---------------+
    | kilobyte | megabyte | gigabyte   | terabyte      |
    +----------+----------+------------+---------------+
    |     1024 |  1048576 | 1073741824 | 1099511627776 |
    +----------+----------+------------+---------------+

### Управление точночтью чисел.
    Функция ceil -> округляет в большую сторону.
    Функция floor -> округляет в меньшую сторону.
    Функция round -> округляет к ближайшему целому числу.

    Второй параметр round говорит нам о том с какой точностью округлить.
    Пример запроса:
    ```
    SELECT ROUND(72.0909, 1), ROUND(72.0909, 2), ROUND(72.0909, 3);
    ```
    +------------------+------------------+------------------+
    | ROUND(72.0909,1) | ROUND(72.0909,2) | ROUND(72.0909,3) |
    +------------------+------------------+------------------+
    |             72.1 |            72.09 |           72.091 |
    +------------------+------------------+------------------+
    1 row in set (0.00 sec)

    Функция truncate() -> Оставляет кол-во цифр после запятой.

    ```
    SELECT TRUNCATE(72.0909, 1), TRUNCATE(72.0909, 2),
    ->  TRUNCATE(72.0909, 3);
    ```
    +----------------------+----------------------+----------------------+
    | TRUNCATE(72.0909, 1) | TRUNCATE(72.0909, 2) | TRUNCATE(72.0909, 3) |
    +----------------------+----------------------+----------------------+
    |                 72.0 |                72.09 |               72.090 |
    +----------------------+----------------------+----------------------+
    1 row in set (0.00 sec)

### Работа со знаковыми данными.

### Преобразование строки в дату Функция CAST.

    ```
    SELECT CATS('2019-09-17 15:30:00' AS DATETIME);
    ```

    ```
    SELECT CAST('2019-09-17' AS DATE) date_field,
    ->  CAST('108-17-57' AS TIME) time_field;
    ```

### Функция генерации дат.

    str_to_date() -> возвращает значение datetime, date или time в зависимости от содержимого строки формата.

    ```
    UPDATE rental
    ->  SET return_date = STR_TO_DATE('September 17, 2019', 'M% d% %Y')
    ->  WHERE rental_id = 99999;
    ```
    Для того чтобы сгенерировать текущую дату вызываем функции представленные в запросе приведенном ниже.
    ```
    SELECT CURRENT_DATE(), CURRENT_TIME(), CURRENT_TIMESTAMP();
    ```
    Ответ сервера:
    ```
    +----------------+----------------+---------------------+
    | CURRENT_DATE() | CURRENT_TIME() | CURRENT_TIMESTAMP() |
    +----------------+----------------+---------------------+
    | 2022-09-26     | 12:04:31       | 2022-09-26 12:04:31 |
    +----------------+----------------+---------------------+
    ```

### Манипуляции временными данными

    Добавления к текущей дате 5 дней:
    ```
    SELECT DATE_ADD(CURRENT_DATE(), INTERVAL 5 DAY);
    ```
    ```
    +------------------------------------------+
    | DATE_ADD(CURRENT_DATE(), INTERVAL 5 DAY) |
    +------------------------------------------+
    | 2022-10-01                               |
    +------------------------------------------+
    1 row in set (0.00 sec)
    ```

    ```
    UPDATE rental
    -> SET return_date = DATE_ADD(return_date, INTERVAL '3:27:11' HOUR_SECOND)
    -> WHERE rental_id = 99999;
    ```
    В этом примере функция date_add ( ) принимает значение из столбца
return_date и добавляет к нему 3 часа 27 минут и 11 секунд. Затем это значение используется для изменения значения столбца return_date.

    Если клиент запрашивает перевод 17 сентября 2019 года, можно узнать последний день сентября следующим образом:

    ```
    SELECT LAST_DAY('2019-09-17');
    ```
### Функции, возвращающие строки.

    DAYNAME -> возвращающая название дня.

    ```
    SELECT DAYNAME('2019-09-18');
    ```
    ```
    +-----------------------+
    | DAYNAME('2019-09-18') |
    +-----------------------+
    | Wednesday             |
    +-----------------------+
    1 row in set (0.00 sec)
    ```
    Например, чтобы извлечь из значения datetime только часть года можно сделать следующее:
    ```
    SELECT EXTRACT(YEAR FROM '2019-09-18 22:19:05');
    ```

    ```
    +------------------------------------------+
    | EXTRACT(YEAR FROM '2019-09-18 22:19:05') |
    +------------------------------------------+
    |                                     2019 |
    +------------------------------------------+
    1 row in set (0.00 sec)
    ```

### Функции, возвращающие числовые значения

    Функция возвращает колво полных дней между двумя датами.
    ```
    SELECT DATEDIFF('2022-09-27', '2022-08-27');
    ```

    Ответ на запрос:
    ```
    +--------------------------------------+
    | DATEDIFF('2022-09-27', '2022-08-27') |
    +--------------------------------------+
    |                                   31 |
    +--------------------------------------+
    ```
    Если аргументы поменять местами то функция вернет отрицательное значение.

    Также можно задавать время: 
    ```
    SELECT DATEDIFF('2019-09-03 23:59:59', '2019-06-21 00:00:01');
    ```

### Функции преобразования:
    Функция cast() -> преобразует стрококвое значение в число.
    ```
    SELECT CAST('1456328' AS SIGNED INTEGER);
    ```
    Функция cast()-> пытается преобразовать всю строчку в число с лева на права если в строке обнаружется символ не подходящий под чисто преобразование остановится без возращается ошибки.

    ```
    SELECT CAST('999ABC111' AS UNSIGNED INTEGER);
    ```

    ```
    +---------------------------------------+
    | CAST('999ABC111' AS UNSIGNED INTEGER) |
    +---------------------------------------+
    |                                   999 |
    +---------------------------------------+
    1 row in set, 1 warning (0.00 sec)
    ```

### Задание 7.1
    ```
    SELECT SUBSTRING('Please frind the substring in this string', 17, 25);
    ```
### Задание 7.2

    ```
    SELECT ROUND(ABS(-25.76823),2)
    ```
### Задание 7.3

    ```
    SELECT EXTRACT(YEAR FROM '2019-09-27 15:30:00');
    ```

# Глава 8 Групировка и Огрегания.
    
    Агрегатная функция для подсчета строк для каждого пользователя.
    ```
    SELECT customer_id, count(*)
    ->  FROM rental
    ->  GROUP BY customer_id;
    ```
    Агригатная функция count() подсчитывает кол-во строк в каждой группе, a звездочка сообщает серверуб что нужно считать все, что есть в группе.
    ```
    |         592 |       29 |
    |         593 |       26 |
    |         594 |       27 |
    |         595 |       30 |
    |         596 |       28 |
    |         597 |       25 |
    |         598 |       22 |
    |         599 |       19 |
    +-------------+----------+
    ```

    ```
    SELECT customer_id, count(*)
    ->  FROM rental
    ->  GROUP BY customer_id
    ->  ORDER BY 2 DESC;
    ```
    Запрос возвращающий пользователей которые брали от 40 и более фильмов.

    ```
    SELECT customer_id, count(*)
    -> FROM rental
    -> GROUP BY customer_id
    -> HAVING count(*) >= 40;
    ```

    ```
    +-------------+----------+
    | customer_id | count(*) |
    +-------------+----------+
    |          75 |       41 |
    |         144 |       42 |
    |         148 |       46 |
    |         197 |       40 |
    |         236 |       42 |
    |         469 |       40 |
    |         526 |       45 |
    +-------------+----------+
    7 rows in set (0.00 sec)
    ```
### Агрегатные функции.

    max() -> Возвращает мфксимальное значение в наборе.

    min() -> Возвращает минимальное значение в наборе.

    avg() -> Возвращает усредненное значение в наборе.

    sum() -> Возвращает сумму значений в наборе.

    count() -> Возвращает колличество значений в наборе.

    Пример запроса:
    ```
    SELECT MAX(amount) max_amt,
    ->  MIN(amount) min_amt,
    ->  AVG(amount) avg_amt,
    ->  SUM(amount) tot_amt,
    ->  COUNT(*) num_payments
    ->  FROM payment;
    ```

    ```
    +---------+---------+----------+----------+--------------+
    | max_amt | min_amt | avg_amt  | tot_amt  | num_payments |
    +---------+---------+----------+----------+--------------+
    |   11.99 |    0.00 | 4.201356 | 67406.56 |        16044 |
    +---------+---------+----------+----------+--------------+
    1 row in set (0.04 sec)
    ```

### Неявная и явная групировка.
    ```
    SELECT customer_id,
    ->  MAX(amount) max_amt,
    ->  MIN(amount) min_amt,
    ->  AVG(amount) avg_amt,
    ->  SUM(amount) tot_amt,
    ->  COUNT(*) num_payments,
    ->FROM payment;
    ```

    ```
    SELECT customer_id,
    ->  MAX(amount) max_amt,
    ->  MIN(amount) min_amt,
    ->  AVG(amount) avg_amt,
    ->  SUM(amount) tot_amt,
    ->  COUNT(*) num_payments
    ->  FROM payment
    ->  GROUP BY customer_id;
    ```
### Подсчет различных значений.
    В первом столбце запроса просто подсчитывается кол-во строк в таблице payment, в то время как во втором столбце исследуется значение в столбце customer_id и подсчитывается только кол-во уникальных значений.
    ```
    SELECT COUNT(customer_id) num_rows,
    ->  COUNT(DISTINCT customer_id) num_customers
    ->  FROM payment;
    ```
    ```
    +----------+---------------+
    | num_rows | num_customers |
    +----------+---------------+
    |    16044 |           599 |
    +----------+---------------+
    1 row in set (0.01 sec)
    ```
### Использование выражений
    ```
    SELECT MAX(datediff(return_date, rental_date))
    ->  FROM rental;
    ```
    ```
    +----------------------------------------+
    | MAX(datediff(return_date,rental_date)) |
    +----------------------------------------+
    |                                     10 |
    +----------------------------------------+
    1 row in set (0.00 sec)
    ```
### Обработка значений null.
    ```
    SELECT COUNT(*) num_rows,
    ->  COUNT(val) num_vals,
    ->  SUM(val) total,
    ->  MAX(val) max_val,
    ->  AVG(val) avg_val,
    ->FROM number_tb1;
    ```
    ```
    +----------+----------+-------+---------+---------+
    | num_rows | num_vals | total | max_val | avg_val |
    +----------+----------+-------+---------+---------+
    |        3 |        3 |     9 |       5 |  3.0000 |
    +----------+----------+-------+---------+---------+
    ```
    После добавления объекта типа null напишем то же запрос.

    ```
    INSERT INTO number_tb1 VALUES (NULL);
    ```
    ```
    SELECT COUNT(*) num_rows,
    ->  COUNT(val) num_vals,
    ->  SUM(val) total,
    ->  MAX(val) max_val,
    ->  AVG(val) avg_val
    ->  FROM number_tg1;
    ```
    Ответ получаем практически идентичный за исключением первого столбца т.к. он отображает кол-во столбцов. Объекты null данными функция игнорируются.

## Генерация групп.

### Групировка по одному столбцу.
    Группы одного столбца -> самый простой и наиболее распростороненный тип групировки.
    ```
    SELECT actor_id, count(*)
    ->  FROM film_actor
    ->  GROUP BY actor_id;
    ```
    Данный запрос генерит 200 групп, по одной для кождого актераб а затем суммирует кол-во фильмов для каждого участника группы.

### Многостолбцовая группировка.

    ```
    SELECT fa.actor_id, f.rating, count(*)
    ->  FROM film_actor fa
    ->  INNER JOIN film f
    ->  ON fa.film_id = f.film_id
    ->  GROUP BY fa.actor_id, f.rating
    ->  ORDER BY 1,2;
    ```

### Групировка с помощью выражений
    ```
    SELECT extract(YEAR FROM rental_date) year;
    ->  COUNT(*) how_many
    ->FROM rental
    ->GROUP BY extract(YEAR FROM rental_date);
    ```
     этом запросе применено довольно простое выражение, которое использует функцию extract ( ) для того, чтобы вернуть из даты только год для соответствующей группировки строк в таблице rental

### Генерация итоговых значений.
    ```
    SELECT fa.actor_id, f.rating, count(*)
    ->  FROM film_actor fa
    ->  INNER JOIN film f
    ->  ON fa.film_id = f.film_id
    -> GROUP BY fa.actor_id, f.rating WITH ROLLUP
    -> ORDER BY 1,2;
    ```
### Условия группового фильтра.
    ```
    SELECT fa.actor_id, f.rating, count(*)
    ->  FROM film_actor fa
    ->  INNER JOIN film f
    ->  ON fa.film_id = f.film_id
    ->  WHERE f.rating IN ('G', 'PG')
    ->  GROUP BY fa.actor_id, f.rating
    ->  HAVING count(*) > 9;
    ```
    Этот запрос имеет два условия фильтрации: одно — в предложении where,которое отфильтровывает любые фильмы с рейтингом, отличным от G или PG, и еще одно — в предложении having, которое отфильтровывает всех актеров, снявшихся менее чем в 10 фильмах.

### Упражнение 8.1

    ```
    SELECT COUNT(*) num_rows,
    ->  FROM payment;
    ```
    ```
    +----------+
    | num_rows |
    +----------+
    |    16044 |
    +----------+
    ```
### Упражнение 8.2
    
    ```
    SELECT customer_id, count(*), sum(amount)
    ->  FROM payment
    ->  GROUP BY customer_id;
    ```
### Упражнение 8.3
    ```
    SELECT customer_id, count(*), sum(amount)
    ->  FROM payment
    ->  GROUP BY customer_id;
    ->  HAVING count(*) >= 40;
    ```

# Глава 9 Позапросы.
    Пример простого подзапроса:
    ```
    SELECT customet_id, first_name, last_name
    FROM customer
    WHERE customer_id = (SELECT MAX(customer_id) FROM customer);
    ```
    Подзапрос возвращает макс. значение customer_id, а далее инф. об этом клиенте.

### Типы подзапросов.

    Некоррелированный подзапрос -> это тип подзапроса который может быть выполнен отдельно и не ссылается ни на что из содержащей инструкции.
    Следующий запрос возвращает города которые находятся НЕ в Индии, Подзапрос в свою очередь возвращает id Индии.
    
    ```
    SELECT city_id, city
    -> FROM city
    -> WHERE country_id <>
    ->  (SELECT country_id FROM country WHERE country = 'India');
    ```
    Второй пример запроса: 
    ```
    SELECT city_id, city
    -> FROM city
    -> WHERE country_id <>
    -> (SELECT country_id FROM country WHERE country <> 'India');
    ```
    Выбрасывается вот такое исключение:
    ERROR 1242 (21000): Subquery returns more than 1 row
    Если написать только подзапрос мы увидем множества городов:
    ```
    SELECT country_id FROM country WHERE country <> 'India';
    ```
    Это значит что нельзя прировнять одну вещь к набору вещей.

### Подзапросы с несколькими строками и одним столбцем.

#### Операторы in и not in
    ```
    SELECT country_id
    ->  FROM country
    ->  WHERE country IN ('Canada','Mexico');
    ```
    Оператор in проверяет, входит ли строка из столбца counrty в набор, если да то строка добавляется в результирующий набор.

    Того-же самого результата можно добиться с помощью следующего запроса.
    ```
    SELECT country_id
    ->  FROM country
    ->  WHERE country = 'Canada' OR country = 'Mexico';
    ```

    Данный запрос возвращает все города которые находятся в Канаде и Мексике.
    ```
    SELECT city_id, city
    -> FROM city
    -> WHERE country_id IN
    ->  (SELECT country_id
    ->   FROM country
    ->   WHERE country IN ('Canada','Mexico'));
    ```
### Оператор all
    Оператор all позваляет срвнить отдельное зачение с каждым значением в наборе.

    ```
    SELECT first_name, last_name
    ->  FROM customer
    ->  WHERE customer <> ALL
    ->  (SELECT customer_id
    ->  FROM payment
    ->  WHERE amount = 0);
    ```
    Подзапрос возвращает набор индентификаторов клиентов, которые заплатили 0 долларов за прокат фильма. А запров возвращает имена всех клиентов, чьи идентификаторы не указанны в результирующем наборе.
    Пример запроса без all
    ```
    SELECT first_name, last_name
    ->  FROM customer
    ->  WHERE customer_id NOT IN
    ->  (SELECT customer_id
    ->   FROM payment
    ->   WHERE amount = 0);
    ```
    Вот еще один пример запроса с использование оператора all
    ```
    SELECT customer_id, count(*)
    ->  FROM rental
    ->  GROUP BY customer_id
    ->  HAVING count(*) > ALL
    ->   (SELECT count(*)
    ->     FROM rental r
    ->      INNER JOIN customer c
    ->      ON r.customer_id = c.customer_id
    ->      INNER JOIN address a
    ->      ON c.address_id = a.address_id
    ->      INNER JOIN city ct
    ->      ON a.city_id = ct.city_id
    ->      INNER JOIN country co
    ->      ON ct.country_id = co.country_id
    ->   WHERE co.country_id IN ('United States', 'Mexico', 'Canada')
    ->   GROUP BY r.customer_id
    ->);
    ```
    Подзапрос в этом примере общеекол-во прокатов фильмов для каждого клиента в Севереной Амекике, a содержащий запрос возвращает всех клиентов, общее количество прокатов фильмов у которых превышает значение у лубого из североамериканских клиентов.

### Оператор Any.
    ```
    SELECT customer_id, sum(amount)
    ->  FROM payment
    ->  GROUP BY customet_id
    ->  HAVING sum(amount) > ANY
    ->  (SELECT sum(p.amount)
    ->   FROM payment p
    ->      INNER JOIN customer c
    ->      ON p.customer_id = c.customer_id
    ->      INNER JOIN address a
    ->      ON c.address_id = a.address_id
    ->      INNER JOIN city ct
    ->      ON a.city_id = ct.city_id
    ->      INNER JOIN country co
    ->      ON ct.country_id = co.country_id
    -> WHERE co.country IN ('Bolivia', 'Paraguay', 'Chile')
    -> GROUP BY co.country
    ->);
    ```
    Подзапрос возвращает общую стоимость проката фильмов для всех клиентов в Боливии, Парагвае и Чили, a содержащий запрос возвращает всех клиентов , которые изрсходовали суммуб превышающую разходы клиентов, хотя бы из этих трех стран.

### Многостолбцевые подзапросы.
    ```
    SELECT fa.actor_id, fa.film_id
    ->  FROM film_actor fa
    ->  WHERE fa.actor_id IN
    ->  (SELECT actor_id FROM actor WHERE last_name = 'MONROE')
    ->  AND fa.film_id IN
    ->  (SELECT film_id FROM film WHERE rating = 'PG');
    ```
    Эта версия запроса выполняет ту же функцию, что и в предыдущем примере, но с использованием одного подзапроса, который возвращает два столбца вместо двух подзапросов,каждый из которых возвращает один столбец.

### Коррелированные подзапросы.
    Коррелированный подзапрос зависит от содержащий его инструкции.

    Следующий подзапрос использует коррелированный подзапрос для подсчета кол-во прокатов фильмов для каждого клиента, a затем сожержащий запрос извлекает тех клиентовб которые взяли на прокат ровно 20 фильмов.

    ```
    SELECT c.first_name, c.last_name
    ->  FROM customer c
    ->  WHERE 20 =
    ->  (SELECT count(*) FROM rental r
    ->   WHERE r.customer_id = c.customer_id);
    ```
    Запрос с условием диапозона:
    ```
    SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE
    -> (SELECT sum(p.amount) FROM payment p
    -> WHERE p.customer_id = c.customer_id)
    -> BETWEEN 180 AND 240;
    ```
    Этот вариант запроса возвращает всех клиентов чьи общие платежи за все прокаты фильмов состовляют от 180 до 240 долларов.

### Оператор exists

    Оператор exists используется, когда необходимо опредилить существование связи безотносительно к колличеству.
```
SELECT c.first_name, c.last_name
->  FROM customer c
->  WHERE EXISTS
->  (SELECT 1 FROM rental r
->  WHERE r.customer_id = c.customer_id
->  AND date(r.rental_date) < '2005-05-25');
```
    Использую оператор exists, ваш подзапрос может возвращать нуль, одну строку или несколькоб и условие просто проверяетб вернул ли подзапрос хотя бы одну строку.


```
SELECT c.first_name, c.last_name
->  FROM customer c
->  WHERE EXISTS
->  (SELECT r.rental_date, r.customer_id, 'ABCD' str, 2*3/7 nmbr
->  FROM rental r
->   WHERE r.customer_id = c.customer_id
->   AND date(r.rental_date) < "2005-05-25");
```
    При использовании exists принято указывать либо select 1 либо select *

    Вы так-же можете использовать not exixst для отбора подзапросов, которые не возвращают строки, как показанно ниже.
```
SELECT a.first_name, a.last_name
->  FROM actor a
->  WHERE NOT EXISTS
->  (SELECT 1
->   FROM film_actor fa
->      INNER JOIN film f ON f.film_id = fa.film_id
->   WHERE fa.actor_id = a.actor_id
->      AND f.rating = 'R');
```
    Этот запрос находит всех актерев которые никогда не снималить в фильмах с рейтингом "R".

### Работа с данными с помощью коррелированных подзапросов.
    Коррелированные рапросы можно использовать инструкции update, delete, insert
    Вот пример коррелилованного подсапроса, используемого для изменения столбца last_update в таблице customer.
    ```
    UPDATE customer c
    ->  SET c.last_update =
    ->  (SELECT max(r.rental_date) FROM rental r
    ->   WHERE r.customer_id = c.customer_id);
    ```
    Эта функция изменит каждуэ строку в таблице клиентов (Поскольку в ней нет предложения where)

    ```
    UPDATE customer c
    ->  SET c.last_update =
    ->  (SELECT max(r.rental_date) FROM rental r
    ->      WHERE r.customer_id = c.customer_id)
    ->  WHERE EXISTS
    -> (SELECT 1 FORM rental r
    ->  WHERE r.customer_id = c.customer_id);
    ```
    ```
    DELETE FROM customer c
    -> WHERE 365 < ALL
    ->  (SELECT datediff(now(), r.rental_date) days_since_last_rental
    ->  FROM rental
    ->  WHERE r.customer_id = c.customer_id);
    ```

## Применение подзапросов
### Подзапросы как источник данных

    ```
    SELECT c.first_name, c.last_name,
    ->  pymnt.num_rentals, pymnt.tot_payments
    ->FROM customer c
    ->  INNER JOIN
    ->  (SELECT customer_id,
    ->      count(*) num_rentals, sum(amount) tot_payments
    ->  FROM payment
    ->  GROUP BY customer_id
    ->  ) pymnt
    ->  ON c.customer_id = pymnt.customer_id;
    ```

### Соединение данных.
    Чтобы сформировать три группы в рамках одного запроса, требуется способ опредилить эти группы. Первым шагом является создание запроса, который генерирует определения групп:

    ```
    SELECT 'Small Fly' name, 0 low_limit, 74.99 higt_limit
    ->  UNION ALL
    ->  SELECT 'Average Joes' name, 75 low_limit, 149.99 hith_limit
    ->  UNION ALL
    -> SELECT 'Heavy Hitters' name, 150 low_limit, 9999999.99 high_limit;
    ```
    Каждый запрос извлекает три литерала, а результаты трех запросов объединяются для создания результирующего набора с тремя строками и тремя столбцами.
    Теперь пропишим данный запрос в предложение FROM.

    ```
    SELECT pymnt_grps.name, count(*) num_customers
    ->  FROM
    ->    (SELECT customer_id,
    ->      count(*) num_rentals, sum(amount) tot_payments
    ->  FROM payment
    ->  GROUP BY customer_id
    ->  ) pymnt
    ->  INNER JOIN
    ->    (SELECT 'Small Fry', name, 0 low_limit, 74.99 high_limit
    ->      UNION ALL
    ->      SELECT 'Average Joes' name, 75 low_limit, 149.99 high_limit
    ->      UNION ALL
    ->      SELECT 'Heavy Hitters' name, 150 low_limit, 9999999.99 high_limit
    ->  ) pymnt_grps
    ->   ON pymnt.tot_payments
    ->      BETWEEN pymnt_grps.low_limit AND pymnt_grps.high_limit
    ->  GROUP BY pymnt_grps.name;
    ```
    Предложение FROM содержит два подзапроса; первый подзапрос, с именем 
    pymnt, возвращает общее кол-во прокатов фильмов и общие платяжи для каждого клиента, в то время как второй второй подзапрос б с именем pymnt_grps, генерирует три группы клиентов.

### Подзапрсы, ориентированные на задачу.
    ```
    SELECT c.first_name, c.last_name, ct.city,
    ->  sum(p.amount) tot_payments, count(*) tot_rentals
    ->  FROM payments p
    ->    INNER JOIN customer c
    ->    ON p.customer_id = c.customer_id
    ->    INNER JOIN address a
    ->    ON c.address_id = a.address_id
    ->    INNER JOIN city ct
    ->    ON a.city_id =ct.city_id
    ->  GROUP BY c.first_name, c.last_name, ct.city
    ```
    Таким образом мы можем выделить задачу создания групп в подсапрос, а затем присоединмть остальные три таблици к таблице, сгенеринной подзапросом.
    ```
    SELECT customer_id,
    ->  count(*) tot_payments, sum(amount) tot_payments
    ->  FROM payment
    ->  GROUP BY customer_id;
    ```

    ```
    SELECT c.first_name, c.last_name,
    -> ct.city,
    -> pymnt.tot_payments, pymnt.tot_rentals
    -> FROM
    -> (SELECT customer_id,
    -> count(*) tot_rentals, sum(amount) tot_payments
    -> FROM payment
    -> GROUP BY customer_id
    -> ) pymnt
    -> INNER JOIN customer c
    -> ON pymnt.customer_id = c.customer_id
    -> INNER JOIN address a
    -> ON c.address_id = a.address_id
    -> INNER JOIN city ct
    -> ON a.city_id = ct.city_id;
    ```

### Обобщенные табличные выражения:

    CTE (Common table expressions) -> это именнованный подзапросб который который появляется в верхней части запроса в предложении with.
    ```
    WITH actors_s AS
    -> (SELECT actor_id, first_name,last_name
    -> FROM actor
    -> WHERE last_name LIKE 'S%'
    -> ),
    -> actors_s_pg AS
    -> (SELECT s.actor_id, s.first_name, s.last_name,
    -> f.film_id, f.title
    -> FROM actors_s s
    -> INNER JOIN film_actor fa
    -> ON s.actor_id = fa.actor_id
    -> INNER JOIN film f
    -> ON f.film_id = fa.film_id
    -> WHERE f.rating = 'PG'
    -> ),
    -> actors_s_pg_revenue AS
    -> (SELECT spg.first_name, spg.last_name, p.amount
    -> FROM actors_s_pg spg
    -> INNER JOIN inventory i
    -> ON i.film_id = spg.film_id
    -> INNER JOIN rental r
    -> ON i.inventory_id = r.inventory_id
    -> INNER JOIN payment p
    -> ON r.rental_id = p.rental_id
    -> )
    -> SELECT spg_rev.first_name, spg_rev.last_name,
    -> sum(spg_rev.amount) tot_revenue
    -> FROM actors_s_pg_revenue spg_rev
    -> GROUP BY spg_rev.first_name, spg_rev.last_name
    -> ORDER BY 3 desc;
    ```
    Этот запрос вычисляет общий доход от проката тех фильмов с рейтингом
PG, актерский состав которых включает актера, фамилия которого начинается с S. 
Первый подзапрос (actors_s) находит всех актеров, чьи фамилии начинаются с S,
второй подзапрос (actors_s_pg) соединяет этот набор данных
с таблицей film и фильтрует фильмы с рейтингом PG, а третий подзапрос
(actors_s_pg_revenue) соединяет этот набор данных с таблицей payment,
чтобы узнать суммы, уплаченные за аренду любого из этих фильмов.
Последний запрос просто группирует данные из actors_s_pg_revenue по имени/фамилии и суммирует доходы.


### Подзапросы как генераторы выражений.
    ```
    SELECT
    ->(SELECT c.first_name FROM customer c
    ->  WHERE c.customer_id = p.customer_id
    ->) first_name,
    ->(SELECT c.last_name FROM customer c
    ->  WHERE c.customer_id = p.customer_id
    ->) last_name,
    ->(SELECT ct.city
    ->FROM customer c
    ->INNER JOIN address a
    -> ON c.address_id = a.address_id
    ->INNER JOIN city ct
    ->  ON a.city_id = ct.city_id
    -> WHERE c.customer_id = p.customer_id
    ->) city,
    ->  sum(p.amount) tot_payments,
    -> count(*) tot_rentals
    ->  FROM payment p
    -> GROUP BY p.customer_id;
    ```

К таблице customer выполняется три обращения, потому что скалярные
подзапросы могут возвращать только один столбец и строку, поэтому, если
нам нужны три столбца, связанные с клиентом, необходимо использовать
три разных подзапроса.

    ```
    SELECT a.actor_id, a.first_name, a.last_name
    ->  FROM actor a
    ->  ORDER BY
    ->  (SELECT count(*) FROM film_actor fa
    ->   WHERE fa.actor_id = a.actor_id) DESC;
    ```
    Этот запрос использует коррелированный скалярный подзапрос в предложении order by только для возврата количества фильмов, и это значение
    используется исключительно для сортировки.
    Наряду с коррелированными скалярными подзапросами в инструкциях
    select можно использовать некоррелированные скалярные подзапросы для
    генерации значений для инструкции insert. Пусть, например, вы   собираетесь создать новую строку в таблице film actor и у вас имеются следующие данные:
    • имя и фамилия актера;
    • название фильма.

    ```
    INSERT INRO film_actor (actor_id, film_id, last_update)
    ->VALUES (
    ->  (SELECT actor_id FROM actor
    ->  WHERE first_name = 'JENNIFER' AND last_name = 'DAVIS'),
    ->  (SELECT film_id FROM film
    ->  WHERE title = 'ACE GOLDFINGER'),
    -> now()
    ->);
    ```

### Упражнение 9.1
    ```
    SELECT title
    -> FROM film
    -> WHERE film_id IN
    ->  (SELECT fc.film_id
    ->      FROM film_categoty fc INNER JOIN category c
    ->          ON fc.category_id = c.category_id
    ->      WHERE c.name = 'Action');
    ```
### Упражнение 9.2
    ```
    SELECT f.title
    -> FROM film f
    ->  WHERE ESISTS
    ->  (SELECT 1
    ->   FROM film_categoty fc INNER JOIN category
    ->      ON fc.category_id = c.category_id
    ->   WHERE c.name = 'Action'
    ->      AND fc.film_id = f.film_id);
    ```
### Упражнение 9.3
    ```
     SELECT actr.actor_id, grps.level
    -> FROM
    -> (SELECT actor_id, count(*) num_roles
    -> FROM film_actor
    -> GROUP BY actor_id
    -> ) actr
    -> INNER JOIN
    -> (select 'Hollywood Star' level, 30 min_roles, 99999 max_roles
    -> UNION ALL
    -> SELECT 'Prolific Actor' level, 20 min_roles, 29 max_roles
    -> UNION ALL
    -> SELECT 'Newcomer' level, 1 min_roles, 19 max_roles
    -> ) grps
    -> ON actr.num_roles BETWEEN grps.min_roles AND grps.max_roles;
    ```
# Глава 10. Соединения

### Внешние сщкдинения.

    ```
    SELECT f.film_id, f.title, count(*) num_copies
    -> FROM film f
    ->  INNER JOIN inventory i
    ->      ON f.film_if = i.film_id
    -> GROUP BY f.film_id, f.title;
    ```
Если вы хотите, чтобы запрос возвращал все 1000 фильмов, независимо от
того, имеются ли соответствующие строки в таблице inventory, можете использовать внешнее соединение, которое, по сути, делает условие соединения
необязательным:
    ```
    SELECT f.film_id, f.title, count(i.inventory_id) num_copies
    -> FROM film f
    ->   LEFT OUTER JOIN inventory i
    ->      ON f.film_id = i.film_id
    -> GROUP BY f.film_id, f.title;
    ```
    Запрос без внешнего соединения.
    ```
    SELECT f.film_id, f.title, i.inventory_id
    -> FROM film f
    ->      INNER JOIN inventory i
    ->        ON f.film_id = i.film_id
    ->  WHERE f.film_if BETWEEN 13 AND 15;
    ```
    Тот-же запрос с внешним соединением.
    ```
    SELECT f.first_id, f.title, i.inventory_id
    -> FROM film f
    ->  LEFT OUTER JOIN inventory i
    ->      ON f.film_id = i.film_id
    -> WHERE f.film_id BETWEEN 13 AND 15;
    ```
    Пример правого внешнего соединения вместо левого:
    ```
    SELECT f.film_id, f.title, i.inventory_id
    -> FROM inventory i
    ->   RIGTH OUTER JOIN film f
    ->      ON f.film_id = i.film_id
    -> WHERE f.film_id BETWEEN 13 AND 15;
    ```
    Заметим, что обе версии запроса выполняют внешнее соединение
    ключевые слова left и right служат только для того, чтобы сообщить
    серверу, в
    какой таблице разрешено иметь пробелы в данных. Если вы хотите внешне
    соединить таблицы А и В и хотите, чтобы в результирующем наборе
    присут -
    ствовали все строки из А (с дополнительными столбцами из В всякий
    раз,
    когда есть соответствующие данные), то можете указать либо A left
    outer
    join В, либо В right outer join А.


### Трехсторонние внешнее соединение.

    Внекоторых случаях может понадобиться внешнее соединение одной
    таблици с двумя другими, аот пример:
    ```
    SELECT f.film_id, f.title, i.inventory_id, r.rental_date
    ->  FROM film f
    ->      LEFT OUTER JOIN inventory i
    ->      ON f.film_id = i.film_id
    ->      LEFT OUTER JOIN rental r
    ->      ON i.inventory_id = r.inventory_id
    ->  WHERE f.film_id BETWEEN 13 AND 15;
    ```
Результаты включают в себя все фильмы, имеющиеся в наличии, но у
фильма Alice Fantasia в столбцах из обеих таблиц, соединенных внешним
соединением, находятся значения null.

### Перекрестные соединения:

    ```
    SELECT c.name category_name, lname language_name
    ->  FROM category c
    ->      CROSS JOIN language l;
    ```
    Этот запрос генерирует декартово произведение таблици category и language, в результате чего получается результирующий набор из 96 строк (16 строк category * 6 строк language)

```
SELECT days.dt, COUNT(r.rental_id) num_rentals
    -> FROM rental r
    -> RIGHT OUTER JOIN
    -> (SELECT DATE_ADD('2005-01-01',
    -> INTERVAL (ones.num + tens.num + hundreds.num) DAY) dt
    -> FROM
    -> (SELECT 0 num UNION ALL
    -> SELECT 1 num UNION ALL
    -> SELECT 2 num UNION ALL
    -> SELECT 3 num UNION ALL
    -> SELECT 4 num UNION ALL
    -> SELECT 5 num UNION ALL
    -> SELECT 6 num UNION ALL
    -> SELECT 7 num UNION ALL
    -> SELECT 8 num UNION ALL
    -> SELECT 9 num) ones
    -> CROOS JOIN
    -> (SELECT 0 num UNION ALL
    -> SELECT 10 num UNION ALL
    -> SELECT 20 num UNION ALL
    -> SELECT 30 num UNION ALL
    -> SELECT 40 num UNION ALL
    -> SELECT 50 num UNION ALL
    -> SELECT 60 num UNION ALL
    -> SELECT 70 num UNION ALL
    -> SELECT 80 num UNION ALL
    -> SELECT 90 num) tens
    -> CROSS JOIN
    -> (SELECT 0 num UNION ALL
    -> SELECT 100 num UNION ALL
    -> SELECT 200 num UNION ALL
    -> SELECT 300 num) hundreds
    -> WHERE DATE_ADD('2005-01-01',
    -> INTERVAL (ones.num + tens.num + hundreds.num) DAY)
    -> <'2006-01-01'
    -> ) days
    -> ON days.dt = date(r.rental_date)
    -> GROUP BY days.dt
    -> ORDER BY 1;
```

### Естественные соединения.
    ```
    SELECT cust.first_name, cust.last_name, date(r.rental_date)
    ->  FROM
    ->  (SELECT customer_id, first_name, last_name
    ->  FROM customer
    ->  ) cust
    ->  NATURAL JOIN rental r;
    ```
### Упражнение 10.1
    ```
    SELECT c.customer_id, c.first_name, c.last_name
    ->  FROM customer c
    ```
    Дописать!!!!
    Page -> 238/

# Условная логика:
    Пример запроса с условным оператором.
    ```
    SELECT first_name, last_name,
    ->  CASE
    ->  WHEN active = 1 THEN 'ACTIVE'
    ->  ELSE 'INACTIVE'
    ->  END activity_type
    ->  FROM customer;
    ```
## Вырахение CASE:

### Поисковые выражения case:

Выше расположенный запрос являеся примером поискового выражения:
Ниже представлен пример поискового запроса "Не работает -> илюстрация!"

```
CASE
-> WHEN category.name IN ('Children', 'Family', 'Sport', 'Animation')
-> THEN 'ALL Ages'
-> WHEN category.name = 'Horror'
-> THEN 'Adult'
-> WHEN category.name IN ('Music', 'Games')
-> THEN 'Teens'
-> ELSE 'Other'
-> END
```

Вот еще одна версия показанного выше запроса, в которой
для возврата количество прокатов — но только для активных клиентов!
используется подзапрос:
```
SELECT c.first_name, c.last_name,
->  CASE
->      WHEN active = 0 THEN 0
->  ELSE
->      (SELECT count(*) FROM rental r
->       WHERE r.customer_id = c.customer_id)
->  END num_rentals
->  FROM customer c;
```
Эта версия запроса использует коррелированный подзапрос для
получения количества прокатов для каждого активного клиента. В
зависимости от
процента активных клиентов использование этого подхода может быть
более
эффективным, чем соединение таблиц customer и rental и группировка по
столбцу customer_id.

### Простые выражения case:
Пример синтаксиса выражени ```CASE```

```
CASE V0
    WHEN V1 THEN E1
    WHEN V2 THEN E2
    ...
    WHEN VN THEN EN
    [ELSE ED]
END
```

Пример простого выражения ```CASE```

```
CASE category.name
WHEN 'Children 1
THEN 'All Ages
WHEN 'Family' THEN 'All Ages'
WHEN 'Sports' THEN 'All Ages'
WHEN 'Animation' THEN 'All Ages
WHEN 'Horror' THEN 'Adult
WHEN ’Music' THEN 'Teens’
WHEN 'Games' THEN 'Teens'
ELSE 'Other
END
```

### Преобразование результирующего набора

Пример запроса:

```
SELECT monthname(rental_date) rental_month,
->  count(*) num_rentals
->FROM rental
->WHERE rental_date BETWEEN '2005-05-01' AND '2005-08-01'
->GROUP BY monthname(rental_date);
```
Пример запроса в одну строку данных с тремя столбцами.

```
SELECT
-> SUM(CASE WHEN monthname(rental_date) = "May" THEN 1
->      ELSE 0 END) May_rentals,
->SUM(CASE WHEN monthname(rental_date) = 'June' THEN 1
->      ELSE 0 END) June_rentals,
->SUM(CASE WHEN nomthname(rental_date) = "July" THEN 1
->      ELSE 0 END) July_rentals
->  FROM rental
->  WHERE rental_date BETWEEN '2005-05-01' AND '2005-08-01';
```
### Проверка существования:

Пример запроса проверки сущ.

```
SELECT a.first_name, a.last_name,
->  CASE
->      WHEN EXISTS (SELECT 1 FROM film_actor fa
->      INNER JOIN film f ON fa.film_actor = f.film_id
->      WHERE fa.actor_id = a.actor_id
->          AND f.rating = 'G') THEN 'Y'
->  ELSE 'N'
->END g_actor,
->  CASE
->      WHEN EXISTS (SELECT 1 FROM film_actor fa
->      INNER JOIN film ON fa.film_id = f.film_id
->      WHERE fa.actor_id = a.actor_id
->      and f.rating = 'PG') THEN 'Y'
->  ELSE 'N'
->END pg_actor,
->  CASE
->      WHEN EXIST (SELECT 1 FROM film_actor fa
->      INNER JOIN film f ON fa.film_id = f.film_id
->      WHERE fa.actor_id = a.actor_id
->      AND f.rating = 'NC-17') THEN 'Y'
->  ELSE 'N'
->END nc17_actor
->  FROM actor a
->  WHERE a.last_name LIKE 'S%' OR a.first_name LIKE 'S%';
```
аждое выражение case включает коррелированный подзапрос к таблицам
film_actor и film; один из них ищет фильмы с рейтингом G, второй —
фильмы с рейтингом PG и третий — фильмы с рейтингом NC-17. Поскольку
в каждом предложении when используется оператор exists, условия
вычисляются как истинные, если актер появился как минимум в одном фильме
с соответствующим рейтингом.

выражение case используется для подсчета количества копий в прокате для
каждого фильма, возвращая строки ' Out Of Stock ' (нет в прокате),
Scarce ' (редкий),'Available ' (доступный) или ' Common ' (обычный (в
большом количестве)):

```
SELECT f.title,
->  CASE (SELECT count(*) FROM inventory i
->      WHERE i.film_id = f.film_id)
->  WHEN 0 THEN 'Out Of Stock'
->  WHEN 1 THEN 'Scarce'
->  WHEN 2 THEN 'Scarce'
->  WHEN 3 THEN 'Available'
->  WHEN 4 THEN 'Available'
->  ELSE 'Common'
->END film_availability
->FROM film f
->;
```
### Ошибка деления на нуль

```
SELECT 100/0;
```
Ответ сервера:
```
+---------+
| 100 / 0 |
+---------+
|    NULL |
+---------+
```
Чтобы защитить свои расчеты от ошибок или, что еще хуже, от таинственным
образом появляющихся значений null,следует “завернуть” все знаменатели в
условную логику, как демонстрируется в следующем примере:

Пример:
```
SELECT c.first_name, c.last_name,
->  sum(p.amount) tot_payment_amt,
->  count(p.amount) mun_payments,
->  sum(p.amount) /
->      CASE WHEN count(p.amount) = 0 THEN 1
->          ELSE count(p.amount)
->      END avg_payment
->  FROM customer c
->      LEFT OUTER JOIN payment p
->      ON c.customer_id = p.customer_id
->  GROUP BY c.first_name, c.last_name;
```
тот запрос вычисляет среднюю сумму платежа для каждого клиента.
Поскольку некоторые клиенты могут быть новыми и еще не брали напрокат ни
одного фильма, лучше включить выражение case, чтобы знаменатель никогда
не был равен нулю.

### Условные обновления:

```
UPDATE customer
    -> SET active =
    -> CASE
    -> WHEN 90 <= (SELECT datediff(now(), max(rental_date))
    -> FROM rental r
    -> WHERE r.customer_id = customer.customer_id)
    -> THEN 0
    -> ELSE 1
    -> END
    -> WHERE active = 1;
```
Этот оператор использует коррелированный подзапрос для определения
количества дней с момента последнего взятия фильма напрокат для каждого
клиента, и полученное значение сравнивается со значением 90; если
возвращенное подзапросом значение равно 90 или больше, клиент помечается
как неактивный.

### Обратный вызов null:
Page -> 250.

```
SELECT name,
    -> CASE
    -> WHEN name IN ('English', 'Italian', 'French', 'German')
    -> THEN 'latin1'
    -> WHEN name IN ('Japanese', 'Mandarin')
    -> THEN 'utf8'
    -> ELSE 'Unknown'
    -> END character_set
    -> FROM language;
```

Перепишите запрос так чтобы чтбы результирующий запрос содержал одну строку с 5-ю столбцами.
```
SELECT rating, count(*)
-> FROM film
-> GROUP BY rating;
```
