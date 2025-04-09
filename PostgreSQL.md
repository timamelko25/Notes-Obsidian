Используется клиент-серверная архитектура.


```
sudo pacman -S postgresql
```

В результате будет установлен postgres и созданы группа и пользователь `postgres`.

для входа от имени postgres
```
sudo -iu postgres
```

инициализация кластера баз данных 
```
initdb --locale=ru_RU.UTF-8 --encoding=UTF8 -D /var/lib/postgres/data --data-checksums
```

установить автозапуск pg и запустить

создание бд

```
createdb названиеБд
```

выполнение команды от другого пользователя

```
sudo -u postgres 
```

`psql` - нативная консоль
`\c` - подключение к бд
`create database name` - создание бд
`\q` - выход из бд
`\l` - список всех бд
`\du` - вывод всех пользователей
`create user name` - создание нового юзера
`\password name` - указание пароля для пользователя 
`create user *name* with login password 'password'` - создание пользователя сразу с паролем
`drop user name` - удаление пользователя
`drop database name` - удаление бд

`psql db_name user_name` - войти бд определенным пользователем

`SELECT * FROM pg_catalog.pg_tables` - просмотр всех таблиц в текущем подключении
`\dt` - просмотр созданных схем

