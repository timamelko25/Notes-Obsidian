Модуль Advanced Python Scheduler улучшенный планировщик задач

Триггер - содержит логику планирования. ***Когда*** задание должно быть запущенно следующим. Помимо своей первоначальной конфигурации, триггеры не имеют состояния.

Хранилище заданий - содержит запланированные задания. По умолчанию хранит задания в памяти, можно хранить в бд. Данные при записи в бд сериализуются и десериализуются при выгрузке. Хранилища заданий (кроме хранилища по умолчанию) выступают в качестве 

Исполнители - обработчик выполнения задач. Исполнитель отправляет назначенный вызываемый объект (функцию) в задании в поток или пул процессов. Когда задание выполнено, он уведомляет планировщик, который затем генерирует соответствующее событие.

Планировщик - главное связывающее звено. Интерфейс для работы с хранилищами заданий, исполнителями или триггерами. Через него проходит настройка хранилищ заданий, исполнителей и добавление, изменение и удаление задач.

Выбор планировщика задач:
- `BlockingScheduler`: используется, когда в процессе работает только планировщик, например, отдельный скрипт/модуль, заменяющий системный `cron` операционной системы Linux .
- `BackgroundScheduler`: используется, когда не используются фреймворки, указанные ниже и необходимо, чтобы планировщик работал в фоновом режиме внутри какого-то приложения.
- `AsyncIOScheduler`: используется, если приложение использует модуль `asyncio`.
- `GeventScheduler`: используется, если приложение использует `gevent`.
- `TornadoScheduler`: используется, если создается приложение `Tornado`.
- `TwistedScheduler`: используется, если создается приложение `Twisted`.
- `QtScheduler`: используйте, если создается приложение `Qt`.

В ином случае, для большинства задач достаточно `ThreadPoolExecutor`. 

Выбор хранилища заданий

Если задания воссоздаются в начале приложения кодом, то можно использовать значение по умолчанию `MemoryJobStore` для хранения в оперативной памяти. Для сохранения задач после перезапуска или сбоя приложения, можно использовать `SQLAlchemyJobStore`.

Можно объявить default и дополнительно определить дополнительные хранилища

```python
from apscheduler.schedulers.background import BackgroundScheduler
from apscheduler.jobstores.sqlalchemy import SQLAlchemyJobStore

# Для PostgreSQL
postgres_url = 'postgresql://user:password@localhost:5432/dbname'

# Для MySQL
mysql_url = 'mysql+pymysql://user:password@localhost:3306/dbname'

# Для SQLite
sqlite_url = 'sqlite:///jobs.sqlite'

jobstores = {
    'default': SQLAlchemyJobStore(url=postgres_url),
    'redis': RedisJobStore(host='localhost', port=6379)
}

scheduler = BackgroundScheduler(jobstores=jobstores)
scheduler.start()

scheduler.add_job(job_func, trigger='interval', id='job_1', hour=1, 
                   jobstore='default)

```

Триггеры планировщика заданий

Триггер определяет логику, по которой вычисляются даты/время при запуске задания

- `date` - запуск задания ***один раз*** в определенный момент времени

```python
scheduler.add_job(job, 'date', run_date=date(2022, 11, 30),
                  args=['Job 1'], id='job_1')
 
# выполнит функцию `job()` один раз 30.11.2022 г. в 12:00:00
scheduler.add_job(job, 'date', run_date=datetime(2022, 4, 30, 12, 0, 0), 
                  args=['Job 2'], id='job_2')
 
# выполнит функцию `job()` один раз 30.11.2022 г. в 8:00
scheduler.add_job(job, 'date', run_date='2022-4-30 08:00:00',
                  args=['Job 3'], id='job_3')
```

- `interval` - запуск задания через фиксированные интервалы времени

```python
scheduler.add_job(prompt, 'interval', seconds=5)
```

- `cron` - периодический запуск заданий в определенное время

```python
# Выполняется ежедневно, каждые 5 минут, в 17 часов
scheduler.add_job(job, 'cron', hour=17, minute='*/5',
                  args=['job 2'])
```

Параметры триггера cron
- year - принимает значение года в 4-х значном виде
- month - от 1 до 12
- day - от 1 до 31
- week - от 1 до 53
- day_of_week - от 0 до 6 или mon, tue, wed, thu, fri, sat, sun
- hour - от 0 до 23
- minute - от 0 до 59
- start_date - datetime или строка - начала выполнения задания
- end_date - datetime или строка - окончания выполнения задания
- timezone - datetime.tzinfo или строка - часовой пояс, который будет использоваться для вычисления даты и времени 
- jitter - задержать выполнения задания на __ секунд

Типы выражений триггера cron
- `x`: Срабатывает при возникновении `x`. `hour=3`, означает срабатывание на 4-м часу.
- `*`: Срабатывает по каждому значению. `hour='*'` означает каждый час.
- `*/a`: Срабатывает при каждом значении `a`. hour="*/3" означает каждые 3 часа.
- `a-b`: Срабатывает при каждом значении между `a` и `b` включая эти значения.
- `a-b/c`: Срабатывает при каждом значении `c` в диапазоне `a-b`.
- `Xth Y`: Срабатывает при `X` наступлении дня недели `Y` в течение месяца (только для аргумента `day`).
- `last X`: Срабатывает на последнем `X`, где `x` - любой день недели. например: последняя пятница (только для аргумента `day`).
- `last`: Срабатывает в последний день месяца (только для аргумента `day`).
- `x,y,z`: Используется для объединения различных выражений с помощью запятой.

Можно объявить несколько триггеров в один, который срабатывает в моменты времени, согласованные всеми участвующими триггерами, либо когда сработает любой из триггеров

Настройка планировщика

Можно использовать словарь конфигурации или передать параметры в качестве ключевых аргументов. Можно сначала создать экземпляр планировщика, затем добавить задания и настроить планировщик

1  вариант 
``` python
from pytz import utc
from apscheduler.schedulers.background import BackgroundScheduler
from apscheduler.jobstores.sqlalchemy import SQLAlchemyJobStore
from apscheduler.executors.pool import ProcessPoolExecutor

jobstores = {
    'mongo': {'type': 'mongodb'},
    'default': SQLAlchemyJobStore(url='sqlite:///jobs.sqlite')
}
executors = {
    'default': {'type': 'threadpool', 'max_workers': 20},
    'processpool': ProcessPoolExecutor(max_workers=5)
}
job_defaults = {
    'coalesce': False,
    'max_instances': 3
}

# СОЗДАЕМ ФОНОВЫЙ ПЛАНИРОВЩИК 
# ЗАДАНИЙ ПО УМОЛЧАНИЮ
scheduler = BackgroundScheduler()

# настраиваем фоновый планировщик заданий
scheduler.configure(jobstores=jobstores, executors=executors, 
           job_defaults=job_defaults, timezone=utc)

# добавляем задание
scheduler.add_job(job_func, trigger='interval', id='job_1', hour=1, 
                   jobstore='default', executor='default')
```

2 вариант
``` python
from apscheduler.schedulers.background import BackgroundScheduler

# создаем и настраиваем планировщик заданий 
# префикс `apscheduler`  жестко запрограммирован
scheduler = BackgroundScheduler({
    'apscheduler.jobstores.mongo': {
         'type': 'mongodb'
    },
    'apscheduler.jobstores.default': {
        'type': 'sqlalchemy',
        'url': 'sqlite:///jobs.sqlite'
    },
    'apscheduler.executors.default': {
        'class': 'apscheduler.executors.pool:ThreadPoolExecutor',
        'max_workers': '20'
    },
    'apscheduler.executors.processpool': {
        'type': 'processpool',
        'max_workers': '5'
    },
    'apscheduler.job_defaults.coalesce': 'false',
    'apscheduler.job_defaults.max_instances': '3',
    'apscheduler.timezone': 'UTC',
})

# добавляем задание
scheduler.add_job(job_func, trigger='interval', id='job_1', hour=1, 
                   jobstore='mongo', executor='default')
```

3 вариант 
``` python
jobstores = {
    'default': SQLAlchemyJobStore(url='sqlite:///jobs.sqlite')
    'mongo': MongoDBJobStore(),
}
executors = {
    'default': ThreadPoolExecutor(20),
    'processpool': ProcessPoolExecutor(5)
}
job_defaults = {
    'coalesce': False,
    'max_instances': 3
}
# создаем и настраиваем планировщик заданий
scheduler = BackgroundScheduler(jobstores=jobstores, executors=executors, 
                             job_defaults=job_defaults, timezone=utc)

# добавляем задание
scheduler.add_job(job_func, trigger='interval', id='job_1', hour=1, 
                   jobstore='mongo', executor='processpool')
```

Использование декоратора
Можно обернуть функцию в декоратор

```python
job = Scheduler.add_job(func, trigger=None, args=None, kwargs=None, 
                        id=None, name=None, misfire_grace_time=undefined, 
                        coalesce=undefined, max_instances=undefined, 
                        next_run_time=undefined, jobstore='default', executor='default', 
                        replace_existing=False, **trigger_args)

# версия декоратора `Scheduler.add_job()`, 
# за исключением того, что `replace_existing` 
# всегда имеет значение `True`.
@Scheduler.scheduled_job(trigger=None, args=None, kwargs=None, ...)
def job():
    pass
```

Метод Scheduler .add_job() и декоратор @Scheduler.scheduled_job() добавляют данное задание в список заданий
и пробуждает планировщик, если он уже запущен.

Любой аргумент undefined будет установлен в значение по умолчанию. 

Экземпляр триггера принимает аргументы:
- args (list | tuple) - список/кортеж позиционных аргументов для вызова func 
- kwargs (dict) - словарь ключевых аргументов для вызова func
- id - явный идентификатор задания
- name - текстовое описание задания
- misfire_grace_time - время допустимое для запуска при наличии задержки выполнения (или None, чтобы разрешить выполнение задания независимо от того, насколько оно запоздало)
- coalesce (bool) - запускать один раз, даже если планировщик определяет что оно должно выполняться более одного раза
- max_instances - максимальное количество одновременно запущенных экземпляров, разрешенных для этого задания
- next_run_time - когда запускать задание в первый раз, независимо от триггера (None, чтобы добавить задание как приостановленное)
- jobstore - псевдоним хранилища задания для хранения задания
- executor - псевдоним исполнителя с которым выполняется задание
- replace_existing - True для замены существующего задания с тем же идентификатором

Получение списка запланированных заданий 
метод `Scheduler.schedget_jobs()` возвращает список экземпляров Job. Вторым аргументом можно проверить конкретное хранилище заданий

Изменение заданий 

Можно изменить любые атрибуты задания, вызвав либо `Job.modify()` (для экземпляра `Job`, который был получен при добавлении задания `Scheduler.add_job()`), либо Scheduler.modify_job(id) для изменения определенного `id` задания. При этом можно изменить любые атрибуты задания, кроме идентификатора `id` задания.

Удаление заданий
- вызвав `Scheduler.remove_job()` с идентификатором задания и псевдонимом хранилища заданий.
- вызвав `Job.remove()` для экземпляра `Job`, который был получен при добавлении задания `Scheduler.add_job()`.

После отработки последнего триггера задание автоматически удаляется

Приостановка / возобновление работы
Можно приостановить обработку запланированных заданий:

scheduler.pause()

Это приведет к тому, что планировщик не будет пробуждаться до тех пор, пока обработка не будет возобновлена:

scheduler.resume()

Также возможен запуск планировщика в состоянии паузы, то есть без первого пробуждающего вызова:

scheduler.start(paused=True)

Это полезно, когда нужно удалить нежелательные задания, прежде чем они смогут быть запущены.

Завершение работы планировщика
Чтобы закрыть планировщик:

scheduler.shutdown()

По умолчанию планировщик закрывает свои хранилища заданий и исполнителей и ждет, пока не будут завершены все текущие выполняемые задания. Если не надо ждать, то можно сделать следующее:

scheduler.shutdown(wait=False)

Этот код по-прежнему закроет хранилища заданий и исполнителей, но не будет ждать завершения каких-либо запущенных задач.