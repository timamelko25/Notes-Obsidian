- [Настройка имени пользователя и почты глобально](#%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0%20%D0%B8%D0%BC%D0%B5%D0%BD%D0%B8%20%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8F%20%D0%B8%20%D0%BF%D0%BE%D1%87%D1%82%D1%8B%20%D0%B3%D0%BB%D0%BE%D0%B1%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE)
- [Инициализация репозитория](#%D0%98%D0%BD%D0%B8%D1%86%D0%B8%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F%20%D1%80%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D1%8F)
- [Проверка статуса репозитория](#%D0%9F%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0%20%D1%81%D1%82%D0%B0%D1%82%D1%83%D1%81%D0%B0%20%D1%80%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D1%8F)
- [Добавление файлов в область подготовленных файлов (staging area)](#%D0%94%D0%BE%D0%B1%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5%20%D1%84%D0%B0%D0%B9%D0%BB%D0%BE%D0%B2%20%D0%B2%20%D0%BE%D0%B1%D0%BB%D0%B0%D1%81%D1%82%D1%8C%20%D0%BF%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8B%D1%85%20%D1%84%D0%B0%D0%B9%D0%BB%D0%BE%D0%B2%20(staging%20area))
- [Создание коммита](#%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D0%B0)
- [Просмотр истории коммитов](#%D0%9F%D1%80%D0%BE%D1%81%D0%BC%D0%BE%D1%82%D1%80%20%D0%B8%D1%81%D1%82%D0%BE%D1%80%D0%B8%D0%B8%20%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D0%BE%D0%B2)
- [Просмотр изменений в конкретном коммите](#%D0%9F%D1%80%D0%BE%D1%81%D0%BC%D0%BE%D1%82%D1%80%20%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D0%B9%20%D0%B2%20%D0%BA%D0%BE%D0%BD%D0%BA%D1%80%D0%B5%D1%82%D0%BD%D0%BE%D0%BC%20%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D0%B5)
- [Работа с ветками](#%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20%D0%B2%D0%B5%D1%82%D0%BA%D0%B0%D0%BC%D0%B8)
	- [Удаление ветки](#%D0%A3%D0%B4%D0%B0%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%B2%D0%B5%D1%82%D0%BA%D0%B8)
- [Слияние веток](#%D0%A1%D0%BB%D0%B8%D1%8F%D0%BD%D0%B8%D0%B5%20%D0%B2%D0%B5%D1%82%D0%BE%D0%BA)
- [Работа с удалёнными репозиториями](#%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20%D1%83%D0%B4%D0%B0%D0%BB%D1%91%D0%BD%D0%BD%D1%8B%D0%BC%D0%B8%20%D1%80%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D1%8F%D0%BC%D0%B8)
	- [Отправка изменений в удалённый репозиторий](#%D0%9E%D1%82%D0%BF%D1%80%D0%B0%D0%B2%D0%BA%D0%B0%20%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D0%B9%20%D0%B2%20%D1%83%D0%B4%D0%B0%D0%BB%D1%91%D0%BD%D0%BD%D1%8B%D0%B9%20%D1%80%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%B9)
	- [Получение изменений из удалённого репозитория](#%D0%9F%D0%BE%D0%BB%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D0%B9%20%D0%B8%D0%B7%20%D1%83%D0%B4%D0%B0%D0%BB%D1%91%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D1%80%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D1%8F)
	- [Слияние удалённого репозитория с локальным](#%D0%A1%D0%BB%D0%B8%D1%8F%D0%BD%D0%B8%D0%B5%20%D1%83%D0%B4%D0%B0%D0%BB%D1%91%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D1%80%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D1%8F%20%D1%81%20%D0%BB%D0%BE%D0%BA%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%BC)
- [Rebase (перемещение коммитов)](#Rebase%20(%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D1%89%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D0%BE%D0%B2))
- [Работа с временным сохранением (Stash)](#%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20%D0%B2%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D0%BC%20%D1%81%D0%BE%D1%85%D1%80%D0%B0%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5%D0%BC%20(Stash))
- [Удалить файл из коммита](#%D0%A3%D0%B4%D0%B0%D0%BB%D0%B8%D1%82%D1%8C%20%D1%84%D0%B0%D0%B9%D0%BB%20%D0%B8%D0%B7%20%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D0%B0)


https://learngitbranching.js.org/
## Настройка имени пользователя и почты глобально

```
git config --global user.name "Tara Routray"
git config --global user.email "dev@tararoutray.com"
```

Эти команды задают имя и почту для всех коммитов в системе.

## Инициализация репозитория

```
git init
```

Создаёт новый локальный репозиторий в текущей директории.

## Проверка статуса репозитория

```
git status
```

Отображает состояние файлов в репозитории: отслеживаемые, изменённые, неотслеживаемые.

## Добавление файлов в область подготовленных файлов (staging area)

Добавить **все** файлы:

```
git add .
```

Добавить **конкретный** файл:

```
git add foo.py
```

## Создание коммита

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

## Просмотр истории коммитов

```
git log
```

Просмотр детальных изменений в каждом коммите:

```
git log -p
```

## Просмотр изменений в конкретном коммите

```
git show hash_commit
```

## Работа с ветками

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

## Слияние веток

Слияние текущей ветки с другой:

```
git merge existing_branch_name
```

Прервать слияние при конфликте:

```
git merge --abort
```

## Работа с удалёнными репозиториями

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

## Rebase (перемещение коммитов)

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

## Работа с временным сохранением (Stash)

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

## Удалить файл из коммита
```
git rm --cached test
```


To complete this level, let's detach HEAD from `bugFix` and attach it to the commit instead.

Specify this commit by its hash. The hash for each commit is displayed on the circle that represents the commit.