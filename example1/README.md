# Перезагрузка файла конфигурации
sudo systemctl reload postgresql

# Проверка состояния сервисам СУБД
sudo systemctl status postgresql

# Лог системы
vim /var/log/syslog
ps aux | grep postgres

# Узнать размер директории (папки)
du -hs /var/lib/postgresql/12/main/

# Поменять пользователя
su postgres
# Запустить psql


sudo -i -u postgres
psql


Вывод списка баз данных:
psql=# \l

# создать раздел
CREATE DATABASE unixway1;
CREATE DATABASE unixway1;

# создать пользователей
CREATE USER unixway1user WITH ENCRYPTED PASSWORD 'password1';
GRANT ALL PRIVILEGES ON DATABASE unixway1 TO unixway1user;

# Поменять владельца раздела
ALTER DATABASE unixway1 OWNER TO unixway1user;


# создать пользователей
CREATE USER unixway2user WITH ENCRYPTED PASSWORD 'password2';
GRANT ALL PRIVILEGES ON DATABASE unixway2 TO unixway2user;
# Поменять владельца раздела
ALTER DATABASE unixway2 OWNER TO unixway2user;

# убрать доступ к базам данных извне сервера
REVOKE CONNECT ON DATABASE unixway1 FROM PUBLIC;
REVOKE CONNECT ON DATABASE unixway2 FROM PUBLIC;

#Подключиться к базе данных с помощью локального пользователя
psql -h 127.0.0.1 -p 5432 -U unixway1user unixway1

# Подключиться к базе с помощью пользователя и пароля
psql -d unixway1 -U unixway1user -W
Password: password1

Добавить строку в файл pg_hba.conf
host replication all 192.168.0.103/24 md5

Обновить конфигурацию postgres:
systemctl reload postgresql

Перезапустите PostgreSQL:

ubuntu@ubuntu-standard-2-4-40gb:~$ sudo systemctl restart postgresql.service


Переподключиться с помощью другой базе данных
unixway2=> \c unixway1


# Создание таблицы
CREATE TABLE users(user_pid SERIAL PRIMARY KEY, user_name TEXT NOT NULL, user_email TEXT NOT NULL, user_balance INTEGER NOT NULL);
unixway2=> \d

Просмотр списка талиц:
unixway1=> \d users
unixway1=> \c unixway1

#Добавим данные:
INSERT INTO users(user_name, user_email,user_balance) VALUES ('Jerry Kinn','jkinn@gmail.com',1000);
INSERT INTO users(user_name, user_email,user_balance) VALUES ('Adam Smith','asmith@gmail.com',1000);
INSERT INTO users(user_name, user_email,user_balance) VALUES ('Billy Jonnes','bjonnes@gmail.com',1000);

# Печать содержимого таблицы:
SELECT * FROM users;

unixway1=> \d 

# Удалить талицу
unixway1=> DROP TABLE users;

# Активировать дополнение - использование типа данных uuid
unixway1=> CREATE EXTENSION IF NOT EXITS "uuid-ossp";
# Активировать расширение нужно только один раз для каждого раздела, где он будет использоваться

# Создадим таблицу, используя UUID
CREATE TABLE users(user_pid SERIAL PRIMARY KEY, user_id UUID NOT NULL DEFAULT uuid_generate_v4(), user_name TEXT NOT NULL, user_email TEXT NOT NULL, user_balance INTEGER NOT NULL, user_registration TIMESTAMP NOT NULL DEFAULT now());

# Демонстрация работы функции uuid_generate_v4()
SELECT uuid_generate_v4();

#Добавим данные:
INSERT INTO users(user_name, user_email,user_balance) VALUES ('Jerry Kinn','jkinn@gmail.com',1000);
INSERT INTO users(user_name, user_email,user_balance) VALUES ('Adam Smith','asmith@gmail.com',1000);
INSERT INTO users(user_name, user_email,user_balance) VALUES ('Billy Jonnes','bjonnes@gmail.com',1000);

# Печать содержимого таблицы:
SELECT * FROM users;

# Попытка добавить неполные данные:
INSERT INTO users(user_name,user_email) VALUES ('Martha Longdale','mlngdale@gmail.com');

# Модификация базы данных
ALTER TABLE users ALTER COLUMN user_balance SET DEFAULT 0;


# Модификация базы данных
ALTER TABLE users ADD CHECK(user_balance >= 0);

# Модификация базы данных
ALTER TABLE users ADD CHECK(length(user_name) <= 100);
ALTER TABLE users ADD CHECK(length(user_email) <= 100);

# Попытка добавить неполные данные:
INSERT INTO users(user_name,user_email) VALUES ('Niels Bourre','nbourre@gmail.com', -100);

# Модификация базы данных
ALTER TABLE users ADD COLUMN user_enabled BOOLEAN NOT NULL DEFAULT true;


# Модификация базы данных. Удалим колонку user_enabled
ALTER TABLE users DROP COLUMN user_enabled;

# Обновить данные пользователя
UPDATE users SET user_balance = user_balance + 100 WHERE user_pid = 2;
UPDATE users SET user_balance = user_balance + 100 WHERE user_id = '691967ea-5648-4529-8d70-1a0843683c3d';
UPDATE users SET user_balance = user_balance + 10;
UPDATE users SET user_balance = user_balance - 100;
UPDATE users SET user_balance = user_balance + 100 WHERE user_balance < 100;
UPDATE users SET user_balance = 0;

# Удалить все строки таблицы users
DELETE FROM users;

SELECT COUNT(*) FROM users;


sudo apt install python3.8-venv

Активация виртуального окружения python:

1. Создание виртуальной среды:
pythonЗ -m venv /tmp/temp_postgres01

2. Просмотр
ls -1 /tmp/tempenv/bin/

3. Активация
source envpath/bin/activate or envpath/Scripts/activate.bat

4. Установка нужных пакетов:
$ python3.6 -m pip install requests
$ pip install requests

$ pip install psycopg2-binary
$ pip install faker


Деактивация
source envpath/bin/deactivate

Удаление
rm -rf envpat

/etc/postgresql/12/main

psql -h 192.168.0.103 -p 5432 -d unixway1 -U postgres
unixway1=# \! chcp 1251
