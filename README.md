# Python
1. 
```
6
67
```

2. 
```Python
sorted_dict = sorted(d.items(), key=lambda item: item[1], reverse=True)
print(sorted_dict)
```

3. 
```Python
def get_balls_number(n: int) -> int:
    """Returns the number of balls per iteration n.
    Formula: Q_n = Q_n-1 + 5 * n

    Args:
        n (int): Iteration.

    Raises:
        ValueError: n must be greater than 0.

    Returns:
        int: Number of balls.
    """

    if n < 0:
        raise ValueError("n must be greater than 0.")

    if n == 0:
        return 1

    return (5 * n) + get_balls_number(n - 1)


print(get_balls_number(-1))
```

4. 
```Python
nums_dict = {         # из названия не ясно, что содержит словарь. Переименовал в nums_dict
    '1': 1,
    '2': 2,
    '3': 3,         # добавил запятую
}

# был нарушен PEP8
# функция не отражала свое назначение
# отсутствовали комментарии
# функция содержала ошибки в алгоритме сравнения
# при выбросе исключения не предпринималось никаких действий
# для параметров и возвр. значения не указаны типы
# custom_max, чтобы не возникло конфликта с max
# параметры переименовал для соответствия pylint
def custom_max(a_num: int, b_num: int, c_num: int) -> int:
    """Returns the max value.

    Args:
        a_num (int)
        b_num (int)
        c_num (int)

    Returns:
        int: the max value.
    """
    try:
        if a_num > b_num:
            if a_num > c_num:
                return a_num

            # if a_num <= c_num, then
            return c_num

        # if a_num <= b_num, then
        if b_num > c_num:
            return b_num

        # if b_num <= c_num, then
        return c_num

    except TypeError:
        return "The parameters must be a number."


result = custom_max(2, 2, 3)
print(result)
```

5. 
В файле F025Dialog.py происходит попытка обращения к isTypeOMS, который не был импортирован. Решение: 
```
from modile_name import isTypeOMS
```

---

# SQL
В задании была не указана СУБД, сделал на PostgreSQL.

1. 
```sql
-- 1. Написать SQL-запрос, который выведет количество действий с именем «МРТ» для всех пациентов за 2020 год.
SELECT count(*) FROM "action" a 
	JOIN event e ON a.event_id = e.id 
	JOIN event_type et on et.id = e.event_type_id 
	WHERE et.name = 'МРТ'
	AND (a.beg_date >= '2020-01-01' AND a.end_date < '2021-01-01');
```

2. 
```sql
-- 2. Добавить колонку «Пол» в таблицу с пациентами.
ALTER TABLE client ADD column sex varchar(6) NOT NULL DEFAULT 'male';
```

3. 
```sql
-- 3. Доработать SQL-запрос из задания 1 так, чтобы количество услуг выводилось для пациентов мужского пола и женского пола отдельно.
SELECT count(*), c.sex FROM "action" a 
	JOIN event e ON a.event_id = e.id 
	JOIN event_type et ON et.id = e.event_type_id 
	JOIN client c ON e.client_id = c.id
	WHERE et.name = 'МРТ'
	AND (a.beg_date >= '2020-01-01' AND a.end_date < '2021-01-01') GROUP BY c.sex;
```

4. 
```sql
-- 4. Создать таблицу «Организации», в которой будет хранится основная информация существующей организации (Название, ИНН, адрес, контактный телефон, флаг-признак страховой организации).
CREATE TABLE organization (
	id SERIAL PRIMARY KEY,
	title varchar(255) NOT NULL,
	inn varchar(10) UNIQUE NOT NULL,
	address text,						-- в задании указано поле "адрес", но для нормализации стоило бы разбить адрес на улицу, дом, квартиру
	phone_number varchar(11),			-- исходим из стандарта, принятого в РФ
	flag varchar(2),					-- из задания не ясно, что такое флаг-признак страховой организации. Предположил, что это некий необязательный двузначный код
	CONSTRAINT valid_inn CHECK (inn::int <= 10)
);
```

5. 
```sql
-- 5. Добавить внешний ключ в таблицу Contract. (Contract.payer_id → Organisation.id)
ALTER TABLE contract ADD CONSTRAINT fk_organization FOREIGN KEY (id) REFERENCES organization(id) ON DELETE restrict ON UPDATE cascade;
```
