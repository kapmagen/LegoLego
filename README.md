# Создание и анализ проекта базы данных PostgreSQL

Этот проект создает базу данных PostgreSQL, используя различные команды SQL для определения таких таблиц, как `colors`, `themes`, `part_categories`, `parts`, `sets`, `inventories`, `inventory_sets` и `inventory_parts`. Цель этой базы данных - хранить информацию и анализировать различные части наборов LEGO, такие как цвета, категории, темы и инвентарь.

### Таблицы
- **colors**: Эта таблица содержит информацию о различных цветах деталей LEGO. Колонки в этой таблице включают `id`, `name`, `rgb` и `is_trans`.
- **themes**: Эта таблица содержит информацию о различных темах наборов LEGO. Столбцы этой таблицы включают `id`, `name` и `parent_id`, который является ссылкой на другую строку в той же таблице.
- **part_categories**: Эта таблица содержит информацию о различных категориях деталей LEGO. Столбцы этой таблицы включают `id` и `name`.
- **parts**: Эта таблица содержит информацию о деталях LEGO. Столбцы этой таблицы включают `part_num`, `name`, `part_cat_id`, который является ссылкой на строку в таблице `part_categories`.
- **sets**: Эта таблица содержит информацию о наборах LEGO. Столбцы этой таблицы включают `set_num`, `name`, `year`, `theme_id`, который является ссылкой на строку в таблице `themes`, и `num_parts`.
- **inventories**: Эта таблица содержит информацию о запасах LEGO. Столбцы этой таблицы включают `id`, `version` и `set_num`, который является ссылкой на строку в таблице `sets`.
- **inventory_sets**: Эта таблица содержит информацию об инвентаризации наборов LEGO. Столбцы этой таблицы включают `inventory_id`, `set_num`, который является ссылкой на строку в таблице `sets`, и `quantity`.
- **inventory_parts**: Эта таблица содержит информацию о запасах деталей LEGO. Столбцы этой таблицы включают `inventory_id`, `part_num`, который является ссылкой на строку в таблице `parts`, `color_id`, который является ссылкой на строку в таблице `colors`, `quantity` и `is_spare`.

### Запросы
Проект включает в себя различные запросы для получения информации из базы данных. Ниже приведены примеры различных запросов, которые можно выполнить к этой базе данных:

- `SELECT count(*) from colors`: Возвращает количество различных цветов, доступных в базе данных.
- `SELECT min(year), max(year) from sets`: Возвращает минимальный и максимальный год доступности наборов LEGO в базе данных.
- `SELECT count(*) from sets`: Возвращает общее количество наборов LEGO в базе данных.
- `SELECT count(*) from parts`: Возвращает общее количество деталей LEGO в базе данных.
- `SELECT year, count(*) as amount from sets group by year order by amount desc`: Возвращает лучший год доступности наборов LEGO в базе данных.
- `SELECT year, avg(num_parts) as avg from sets group by year order by avg desc`: Возвращает среднее количество деталей в наборе LEGO по годам.
- `SELECT year, count(distinct theme_id) as themes from sets group by year order by themes desc`: Возвращает количество уникальных тем наборов LEGO, доступных по годам.
- `SELECT part_categories.name as category, count(*) as amount from parts left join part_categories on parts.part_cat_id = part_categories.id group by category order by amount desc`: Этот запрос возвращает различные категории деталей LEGO и общее количество каждой из них.
- `SELECT sum(quantity) FROM inventory_sets`: Возвращает общее количество наборов LEGO, имеющихся в различных инвентарных списках.
- `SELECT sum(quantity) FROM inventory_parts`: Возвращает общее количество различных деталей LEGO, имеющихся в различных инвентарях.
- `SELECT colors.name as color, sum(quantity) as quantity from inventory_parts left join colors on inventory_parts.color_id = colors.id group by color order by quantity desc`: Возвращает самые популярные цвета деталей LEGO в различных инвентарных списках.
- `SELECT themes.name, count(*) as amount from sets left join themes on sets.theme_id = themes.id group by themes.name order by amount desc`: Возвращает количество доступных наборов LEGO для разных тем.
- `SELECT themes.name, sum(num_parts) as amount from sets left join themes on sets.theme_id = themes.id group by themes.name order by amount desc`: Возвращает общее количество деталей, доступных для разных тем.
- `SELECT themes.name, sets.name, max(num_parts) as max from sets left join themes on sets.theme_id = themes.id group by themes.name, sets.name order by max desc`: Возвращает набор LEGO с наибольшим количеством деталей для каждой темы.
- `SELECT part_categories.name as name, sum(quantity) as amount from inventory_parts left join parts on inventory_parts.part_num = parts.part_num left joinpart_categories on parts.part_cat_id = part_categories.id group by part_categories.name order by amount desc`: Возвращает общее количество каждой категории деталей LEGO, доступных в различных инвентарях.
- `SELECT themes.name as name, sum(quantity) as amount from inventory_sets left join sets on inventory_sets.set_num = sets.set_num left join themes on themes.id = sets.theme_id group by themes.name order by amount desc`: Возвращает количество каждого доступного набора LEGO для разных тем.
- `WITH total as (SELECT year as year, count(*) as amount from sets group by year) SELECT total.year, total.amount, sum(total.amount) over (order by year asc) from total order by year asc`: Возвращает суммарное количество доступных наборов LEGO по годам.
- `SELECT (case when t1.parent_id is null then t1.name else concat(t2.name, ': ', t1.name ) end) as them from themes as t1 left join themes as t2 on t1.parent_id = t2.id`: Возвращает иерархию тем LEGO, включенных в базу данных.

### Заключение
Проект `Создание и анализ проекта базы данных PostgreSQL` предоставляет полезный ресурс для анализа и управления информацией о наборах и деталях LEGO. Включенные таблицы и запросы позволяют пользователям выполнять различные анализы и получать ценные сведения о LEGO. 
