```table-of-contents
title: 
style: nestedList # TOC style (nestedList|nestedOrderedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
includeLinks: true # Make headings clickable
hideWhenEmpty: false # Hide TOC if no headings are found
debugInConsole: false # Print debug info in Obsidian console
```



## 🔹 Настройка имени пользователя и почты глобально

```
git config --global user.name "Tara Routray"
git config --global user.email "dev@tararoutray.com"
```

Эти команды задают имя и почту для всех коммитов в системе.

## 🔹 Инициализация репозитория

```
git init
```

Создаёт новый локальный репозиторий в текущей директории.

## 🔹 Проверка статуса репозитория

```
git status
```

Отображает состояние файлов в репозитории: отслеживаемые, изменённые, неотслеживаемые.

## 🔹 Добавление файлов в область подготовленных файлов (staging area)

Добавить **все** файлы:

```
git add .
```

Добавить **конкретный** файл:

```
git add foo.py
```

## 🔹 Создание коммита

Добавление коммита с сообщением:

```
git commit -m "Описание изменений"
```

Изменение последнего коммита (например, исправление сообщения):

```
git commit --amend -m "Обновлённое описание коммита"
```

Добавление файлов в последний коммит **без изменения** его сообщения:

```
git add new_file.py
git commit --amend --no-edit
```

## 🔹 Просмотр истории коммитов

```
git log
```

Просмотр детальных изменений в каждом коммите:

```
git log -p
```

## 🔹 Просмотр изменений в конкретном коммите

```
git show hash_commit
```

## 🔹 Работа с ветками

Создать новую ветку:

```
git branch new_branch_name
```

Создать и сразу перейти в новую ветку:

```
git checkout -b new_branch_name
```

Вывести список всех веток:

```
git branch
```

Список с удалёнными ветками:

```
git branch -a
```

### Удаление ветки

Локально:

```
git branch -d existing_branch_name
```

Принудительное удаление:

```
git branch -D existing_branch_name
```

В удалённом репозитории:

```
git push origin --delete existing_branch_name
```

## 🔹 Слияние веток

Слияние текущей ветки с другой:

```
git merge existing_branch_name
```

Прервать слияние при конфликте:

```
git merge --abort
```

## 🔹 Работа с удалёнными репозиториями

Добавить удалённый репозиторий:

```
git remote add <имя_удаленного_реп> <URL_репозитория>
```

```
git remote add origin https://github.com/someurl.git
```

Просмотр всех удалённых репозиториев:

```
git remote -v
```

### Отправка изменений в удалённый репозиторий

```
git push origin main
```

Отправить новую ветку:

```
git push -u origin new_branch
```

Принудительная отправка (если есть конфликты):

```
git push -f
```

### Получение изменений из удалённого репозитория

```
git pull
```

Подробный вывод изменений:

```
git pull --verbose
```

### Слияние удалённого репозитория с локальным

```
git merge origin
```

## 🔹 Rebase (перемещение коммитов)

Перенос `HEAD` ветки `main` на начало коммитов другой ветки:

```
git rebase branch_name
```

Прервать `rebase` при ошибке:

```
git rebase --abort
```

Продолжить `rebase` после исправления конфликтов:

```
git rebase --continue
```

## 🔹 Работа с временным сохранением (Stash)

Сохранить текущие изменения **без коммита**:

```
git stash
```

Просмотреть список сохранённых изменений:

```
git stash list
```

Применить последние сохранённые изменения:

```
git stash apply
```

Применить **и удалить** последние сохранённые изменения:

```
git stash pop
```

Удалить все сохранённые изменения:
```
git stash clear
```

## 🔹 Удалить файл из коммита
```
git rm --cached test
```