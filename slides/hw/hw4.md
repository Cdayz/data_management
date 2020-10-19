# Домашнее задание #4

### ETL

ETL - процесс выгрузки данных, обработки и их дальнейшеней загрузки. В рамках домашней работы нужно проделать все три этапа

#### Extract

Проверьте, что в директории data присутствует файл с ключевыми словами по фильмам

<pre>
ls $NETOLOGY_DATA/raw_data | grep keywords
</pre>

Наша задача - загрузить это файл в Postgres. 

Напишите команду создания таблички keywords у неё должно быть 2 поля - id(числовой) и tags (текстовое).  Пример такой команды можно подсмотреть в файле postgres_interactions/load_data.sh
<pre>
psql -U postgres -c "ВАША КОМАНДА"
</pre>

Напишите команду копирования данных из файла в созданную вами таблицу
<pre>
psql -U postgres -c "ВАША КОМАНДА"
</pre>

Подключитесь к контейнеру
<pre>
psql -U postgres
</pre>

Проверьте, что в таблице есть записи
<pre>
SELECT  COUNT(*) FROM keywords;
</pre>

Результат запроса
<pre>
count
-------
 46419
</pre>

#### Transform

Мы загрузили данные в табличку, теперь нужно их преобразовать для дальнейшего использования. Мы ходитм узнать, какие теги у фильмов, которые сильно нравятся пользователям.

- Сформируйте запрос (назовём его ЗАПРОС1) к таблице ratings, в котором будут 2 поля
-- movieId
-- avg_rating - средний рейтинг, который ставят этому контенту пользователи
В выборку должны попасть те фильмы, которым поставили оценки более чем 50 пользователей
Список должен быть отсортирован по убыванию по полю avg_rating и по возрастанию по полю movieId
Из этой выборки оставить первое 150 элементов

Теперь мы хотим добавить к выборке хороших фильмов с высоким рейтингов информацию о тегах. Воспользуемся Common Table Expressions. Для этого нужно написать ЗАПРОС2, который присоединяет к выборке таблицу keywords

<pre>
WITH top_rated as ( ЗАПРОС1 ) ЗАПРОС2;
</pre>

#### Load

Мы обогатили выборку популярного контента внешними данными о тегах. Теперь мы можем сохранить эту информацию в таблицу для дальнейшего использования

Сохраним нашу выборку в новую таблицу top_rated_tags. Для этого мы модифицируем ЗАПРОС2 - вместо простого SELECT сделаем SELECT INTO.

Назовём всю эту конструкцию ЗАПРОС3
<pre>
WITH top_rated as ( ЗАПРОС1 )  SELECT movieId, top_rated_tags INTO имя_таблицы FROM top_rated ...;
</pre>

Теперь можно выгрузить таблицу в текстовый файл - пример см. в лекции. Внимание: Поля в текстовом файле нужно разделить при помощи табуляции ( символ E`\t`).

Решением домашки будет скрипт hw3.sql вида:

<pre>
"ВАША КОМАНДА СОЗДАНИЯ ТАБЛИЦЫ";

"ВАША КОМАНДА ЗАЛИВКИ ДАННЫХ В ТАБЛИЦу";

"ЗАПРОС3";

"ВАША КОМАНДА ВЫГРУЗКИ ТАБЛИЦЫ В ФАЙЛ"
</pre>