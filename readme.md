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

    стр 141!

