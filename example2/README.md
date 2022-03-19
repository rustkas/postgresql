```
CREATE TABLE items(item_pid SERIAL PRIMARY KEY, item_id UUID NOT NULL DEFAULT uuid_generate_v4(), item_name TEXT NOT NULL, item_description TEXT NOT NULL DEFAULT '', item_price NUMERIC(10,2) NOT NULL, item_registration TIMESTAMP NOT NULL DEFAULT now());

ALTER TABLE items ADD CHECK(length(item_name) <= 200);
ALTER TABLE items ADD CHECK(length(item_description) <= 200);
```

```
unixway1=# \d items
                                                       Таблица "public.items"
      Столбец      |             Тип             | Правило сортировки | Допустимость NULL |              По умолчанию
-------------------+-----------------------------+--------------------+-------------------+-----------------------------------------
 item_pid          | integer                     |                    | not null          | nextval('items_item_pid_seq'::regclass)
 item_id           | uuid                        |                    | not null          | uuid_generate_v4()
 item_name         | text                        |                    | not null          |
 item_description  | text                        |                    | not null          | ''::text
 item_price        | numeric(10,2)               |                    | not null          |
 item_registration | timestamp without time zone |                    | not null          | now()
Индексы:
    "items_pkey" PRIMARY KEY, btree (item_pid)
Ограничения-проверки:
    "items_item_description_check" CHECK (length(item_description) <= 200)
    "items_item_name_check" CHECK (length(item_name) <= 200)
```

```
ALTER TABLE items ADD CHECK(item_price > 0);
```
