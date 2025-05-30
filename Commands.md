```table-of-contents
title: 
style: nestedList # TOC style (nestedList|nestedOrderedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
includeLinks: true # Make headings clickable
hideWhenEmpty: false # Hide TOC if no headings are found
debugInConsole: false # Print debug info in Obsidian console
```
# Curl
curl базовый протокол HTTP
скачать файл
`--output` [file_name]

```
curl -L www.likegeeks.com --output likegeeks.html
```
 `-L` - (позволяет обработать редирект при скачивании, почти всегда при скачивании файла)
`-C` - (позволяет продолжить прерванное скачивание файла, дефис после показывает с какого места продолжить скачивание)
`-m` [seconds] (по истечению времени прервать выполнение)
`--connect-timeout` [seconds] (сколько секунд держать открытое соединение)
`-u` username:password авторизация при подключении, чаще всего используется при аутентификации на ftp (file transport protocol) сервер
`-x` 192.168.1.1:8080 перед обращением на сайт будет выполняться подключение через прокси сервер
`--cert` path/to/cert.crt:password
`-s` тихий режим, не выводит ничего в ответ
`-O` будет сохранен результат выполнения в текущую директорию
`-I` вывести заголовки
`-H` передача заголовка, при передачи нескольких заголовков перед каждый нужно указать ключ
`-d` передать POST запрос 
```
curl -d 'name=geek&location=usa' http://example.com
```
`-d` @filename передать файл
`-T` загрузить файл через FTP
`-X` "method" с каким методом сделать запрос
`--location` "full_addres"

# wget
wget "link" - скачать файл
- **-b** **(--background)** - работать в фоновом режиме
- **-o** **файл** **(--out-file)** - указать лог файл
- **-d** **(--debug)** - включить режим отладки
- **-v (--verbose)** - выводить максимум информации о работе утилиты
- **-q (--quiet)** - выводить минимум информации о работе
- **-i** **файл (--input-file)** - прочитать URL из файла
- **--force-html** - читать файл указанный в предыдущем параметре как html
- **-t (--tries)** - количество попыток подключения к серверу
- **-O файл** **(--output-document)** - файл в который будут сохранены полученные данные
- **-с (--continue)** - продолжить ранее прерванную загрузку
- **-S (--server-response)** - вывести ответ сервера
- **--spider** - проверить работоспособность URL
- **-T время (--timeout)** - таймаут подключения к серверу
- **--limit-rate** - ограничить скорость загрузки
- **-w (--wait)** - интервал между запросами
- **-Q** **(--quota)** - максимальный размер загрузки
- **-4 (--inet4only)** - использовать протокол ipv4
- **-6 (--inet6only)** - использовать протокол ipv6
- **-U (--user-agent)**- строка USER AGENT отправляемая серверу
- **-r** (**--recursive**)- рекурсивная работа утилиты
- **-l (--level)** - глубина при рекурсивном сканировании
- **-k** **(--convert-links)** - конвертировать ссылки в локальные при загрузке страниц
- **-P (--directory-prefix)** - каталог, в который будут загружаться файлы
- **-m** **(--mirror)** - скачать сайт на локальную машину
- **-p** **(--page-requisites)** - во время загрузки сайта скачивать все необходимые ресурсы
- --http-user=username
- –http-password=password
- --ftp-user=username
- --ftp-password=password

```
wget https://github.com/seladb/PcapPlusPlus/releases/download/v24.09/pcapplusplus-24.09-ubuntu-22.04-intel-2024.2.0-x86_64.tar.gz
```

# zip
**$ zip опции файлы**

**$ unzip опции архив**

- **-d** удалить файл из архива
- **-r** - рекурсивно обходить каталоги
- **-0** - только архивировать, без сжатия
- **-9** - наилучший степень сжатия
- **-F** - исправить zip файл
- **-e** - шифровать файлы
```
zip -r /path/to/files/*
```

```
unzip archive.zip
```

# tar
**tar опцииf файл_для_записи /папка_файлами_для_архива**
- **A** - добавить файл к архиву
- **c** - создать архив в linux
- **d** - сравнить файлы архива и распакованные файлы в файловой системе
- **j** - сжать архив с помощью Bzip
- **z** - сжать архив с помощью Gzip
- **r** - добавить файлы в конец архива
- **t** - показать содержимое архива
- **u** - обновить архив относительно файловой системы
- **x** - извлечь файлы из архива
- **v** - показать подробную информацию о процессе работы
- **f** - файл для записи архива
- **-C** - распаковать в указанную папку
- **--strip-components** - отбросить n вложенных папок

```
 tar -cvf archive.tar /path/to/files
```

разархивировать
```
tar -xvf archive.tar.gz
```

# ls
**$ ls опции /путь/к/папке**

- **-a** - отображать все файлы, включая скрытые, это те, перед именем которых стоит точка;
- **-A** - не отображать ссылку на текущую папку и корневую папку . и ..;
- **--author** - выводить создателя файла в режиме подробного списка;
- **-b** - выводить Escape последовательности вместо непечатаемых символов;
- **--block-size** - выводить размер каталога или файла в определенной единице измерения, например, мегабайтах M, гигабайтах G или килобайтах K;
- **-B** - не выводить резервные копии, их имена начинаются с ~;
- **-c** - сортировать файлы по времени модификации или создания, сначала будут выведены новые файлы;
- **-C** - выводить колонками;
- **--color** - включить цветной режим вывода, автоматически активирована во многих дистрибутивах;
- **-d** - выводить только директории, без их содержимого, полезно при рекурсивном выводе;
- **-D** - использовать режим вывода, совместимый с Emacs;
- **-f** - не сортировать;
- **-F** - показывать тип объекта, к каждому объекту будет добавлен один из специализированных символов */=>@|;
- **--full-time** - показывать подробную информацию, плюс вся информация о времени в формате ISO;
- **-g** - показывать подробную информацию, но кроме владельца файла;
- **--group-directories-first** - сначала отображать директории, а уже потом файлы;
- **-G** - не выводить имена групп;
- **-h** - выводить размеры папок в удобном для чтения формате;
- **-H** - открывать символические ссылки при рекурсивном использовании;
- **--hide** - не отображать файлы, которые начинаются с указанного символа;
- **-i** - отображать номер индекса inode, в которой хранится этот файл;
- **-l** - выводить подробный список, в котором будет отображаться владелец, группа, дата создания, размер и другие параметры;
- **-L** - для символических ссылок отображать информацию о файле, на который они ссылаются;
- **-m** - разделять элементы списка запятой;
- **-n** - выводить UID и GID вместо имени и группы пользователя;
- **-N** - выводить имена как есть, не обрабатывать контролирующие последовательности;
- **-Q** - брать имена папок и файлов в кавычки;
- **-r** - обратный порядок сортировки;
- **-R** - рекурсивно отображать содержимое поддиректорий;
- **-s** - выводить размер файла в блоках;
- **-S** - сортировать по размеру, сначала большие;
- **-t** - сортировать по времени последней модификации;
- **-u** - сортировать по времени последнего доступа;
- **-U** - не сортировать;
- **-X** - сортировать по алфавиту;
- **-Z** - отображать информацию о расширениях SELinux;
- **-1** - отображать один файл на одну строку.

# mv
|                                        |                                                                                                                       |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| _-b_ или _—backup_ или _—backup=МЕТОД_ | Создает копию файлов, которые были перемещены или перезаписаны                                                        |
| _-f_                                   | При активации не будет спрашивать разрешение у владельца файла, если речь идет о перемещении или переименовании файла |
| _-i_                                   | Наоборот, будет спрашивать разрешение у владельца                                                                     |
| _-n_                                   | Отключает перезапись уже существующих объектов                                                                        |
| _—strip-trailing-slashes_              | Удаляет завершающий символ / у файла при его наличии                                                                  |
| _-t ДИРЕКТОРИЯ_                        | Перемещает все файлы в указанную директорию                                                                           |
| _-u_                                   | Осуществляет перемещение только в том случае, если исходный файл новее объекта назначения                             |
| _-v_                                   | Отображает сведения о каждом элементе во время обработки команды                                                      |

```
mv myfile1.txt mydir/
```

# cp
# rm

ip link show - просмотр сетевых интерфейсов
ip -br link show - сокращенная инфромация

find / -type f -name "main.cpp" - поиск файлов в системе

# grep
$ grep [опции] шаблон [<путь к файлу или папке>] - поиск внутри файлов
для поиска начало строки ^
```
grep ‘^T’ poem.txt
```


для поиска в начале слова '\\b ...' or \\<
 для поиска в конце слова '... \\b' or $
```
grep ‘\bd’ poem.txt
grep ‘d\b’ poem.txt
```
-e для расширенных опций
- + - одно или более повторений предыдущего символа
- ? - ноль или более повторения предыдущего символа
- {n, m} - от n до m повторений предыдущего символа
- | - объединение нескольких паттернов
```
grep -E ‘l+|d{1,2}’ poem.txt
```


-i  - не учитывать регистр
-w - поиск полного совпадения
-v - поиск строк без вхождения конкретного символа
-n - вывод строки где совпадение
-r - рекурсивный поиск
-l - пропуск двоичных файлов
-s - пропуск файлов суперпользователей и скрытие ошибок
-include - включение определенных файлов для поиска --include "\*.log"
-exclude - исключить файлы от поиска
-c - узнать общее количество строк вхождения
-l - узнать имя файла где есть вхождение
\-\-color=always - цветное выделение совпадения

grep -w "main.cpp" - поиск внутри файлов

# find

# chmod
**$ chmod опции права /путь/к/файлу**

- **r** - чтение;
- **w** - запись;
- **x** - выполнение;
- **s** - выполнение  от имени суперпользователя (дополнительный);


- **u** - владелец файла;
- **g** - группа файла;
- **o** - все остальные пользователи;

Синтаксис настройки прав такой:

**группа_пользователей:действие:вид_прав**

- **u+x** - разрешить выполнение для владельца;
- **ugo+x** - разрешить выполнение для всех;
- **ug+w** - разрешить запись для владельца и группы;
- **o-x** - запретить выполнение для остальных пользователей;
- **ugo+rwx** - разрешить все для всех;

Права на папку linux такие же, как и для файла. Во время установки прав сначала укажите цифру прав для владельца, затем для группы, а потом для остальных. Например, :

- **744** - разрешить все для владельца, а остальным только чтение;
- **755** - все для владельца, остальным только чтение и выполнение;
- **764** - все для владельца, чтение и запись для группы, и только чтение для остальных;
- **777** - всем разрешено все.

 ```
 -rw-r--r-- 1 crushhh crushhh    0 Feb 22 20:35 chmod_test
 drwxr-xr-x 3 crushhh crushhh 4096 Feb 17 02:29 asd
```

1 символ - значит что файл, d - папка
rw- права владельца
r-- права группы
r-- права остальных пользователей
чтение запись выполнить


# journalctl
**journalctl опции**

- **--full, -l** - отображать все доступные поля;
- **--all, -a** - отображать все поля в выводе full, даже если они содержат непечатаемые символы или слишком длинные;
- **--pager-end, -e** - отобразить только последние сообщения из журнала;
- **--lines, -n** - количество строк, которые нужно отображать на одном экране, по умолчанию 10;
- **--no-tail** - отображать все строки доступные строки;
- **--reverse, -r** - отображать новые события в начале списка;
- **--output, -o** - настраивает формат вывода лога;
- **--output-fields** - поля, которые нужно выводить;
- **--catalog, -x** - добавить к информации об ошибках пояснения, ссылки на документацию или форумы там, где это возможно;
- **--quiet, -q** - не показывать все информационные сообщения;
- **--merge, -m** - показывать сообщения из всех доступных журналов;
- **--boot, -b** - показать сообщения с момента определенной загрузки системы. По умолчанию используется последняя загрузка;
- **--list-boots** - показать список сохраненных загрузок системы;
- **--dmesg, -k** - показывает сообщения только от ядра. Аналог вызова команды dmesg;
- **--identifier, -t** - показать сообщения с выбранным идентификатором;
- **--unit, -u** - показать сообщения от выбранного сервиса;
- **--user-unit** - фильтровать сообщения от выбранной сессии;
- **--priority, -p** - фильтровать сообщения по их приоритету. Есть восемь уровней приоритета, от 0 до 7;
- **--grep, -g** - фильтрация по тексту сообщения;
- **--cursor, -c** - начать просмотр сообщений с указанного места;
- **--since, -S, --until, -U** - фильтрация по дате и времени;
- **--field, -F** - вывести все данные из выбранного поля;
- **--fields, -N** - вывести все доступные поля;
- **--system** - выводить только системные сообщения;
- **--user** - выводить только сообщения пользователя;
- **--machine, -M** - выводить сообщения от определенного контейнера;
- **--header** - выводить заголовки полей при выводе журнала;
- **--disk-usage** - вывести общий размер лог файлов на диске;
- **--list-catalog** - вывести все доступные подсказки для ошибок;
- **--sync** - синхронизировать все не сохраненные журналы с файловой системой;
- **--flush** - перенести все данные из каталога /run/log/journal в /var/log/journal;
- **--rotate** - запустить ротацию логов;
- **--no-pager** - выводить информацию из журнала без возможности листать страницы;
- **-f** - выводить новые сообщения в реальном времени, как в команде tail;
- **--vacuum-time** - очистить логи, давностью больше указанного периода;
- **--vacuum-size** - очистить логи, чтобы размер хранилища соответствовал указанному.


- **Стрелка вниз, Enter, e** или **j** - переместиться вниз на одну строку;
- **Стрелка вверх, y** или **k** - переместиться на одну строку вверх;
- **Пробел** - переместиться на одну страницу вниз;
- **b** - переместиться на одну страницу вверх;
- **Стрелка вправо, стрелка влево** - горизонтальна прокрутка;
- **g** - перейти на первую строку;
- **G** - перейти на последнюю строку;
- **p** - перейти на позицию нужного процента сообщений. Например, 50p перенесет курсор на середину вывода;
- **/** - поиск по журналу;
- **n** - найти следующее вхождение;
- **N** - предыдущее вхождение;
- **q** - выйти. 

```
journalctl -fxeu docker.service
```