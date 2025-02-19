[[Pydantic]]
[[Alembic]]
[[mypy статический типизатор]]
```table-of-contents
title: 
style: nestedList # TOC style (nestedList|nestedOrderedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
includeLinks: true # Make headings clickable
hideWhenEmpty: false # Hide TOC if no headings are found
debugInConsole: false # Print debug info in Obsidian console
```

**SQLAlchemy** — это библиотека Python для работы с реляционными базами данных.

Поддержка 2-ух типов:
- **core** - низкоуровневый подход для выполнения запросов с помощью SQL
- **ORM** (object-renational mapping: объектно-реляционное отображение) - работа с объектами

Работа с ORM включает в себя основные понятия:
1. Модели таблиц - класс, содержащий в себе структуру таблицы (название, колонки, типы данных, типы связей с другими таблицами)
2. Сессии - объекты, через которые происходит взаимодействие с бд. Запрашивается доступ к бд, выполняется запрос, доступ закрывается. Сессия управляет транзакциями и следит за состояниями объектов. Не создается прямое подключение, а происходит абстракция этого процесса
3. Фабрика сессий - шаблон для создания сессий

# Mapping (Маппер)

**Маппер** - определение связи между классом и таблицей бд

Методы задания маппера:
1. декларативное
	1. Создайте базовый класс для декларативного отображения с помощью функции `declarative_base`. Этот базовый класс будет использоваться в качестве базы для всех ваших классов отображения.
	2. Определите класс отображения для каждой таблицы в вашей базе данных. Класс отображения должен быть подклассом базового класса и определять столбцы таблицы с помощью объектов `Column`. Вы также можете использовать атрибут `tablename` для указания имени таблицы.
	3. Используйте метод `create_all` объекта Base.metadata для создания таблиц в базе данных.
	4. Используйте функцию `sessionmaker` для создания сеанса для запроса к базе данных.
	5. Используйте метод запроса сеанса для запроса базы данных и извлечения экземпляров классов сопоставления.
	6. Используйте метод `add` сеанса для добавления новых экземпляров классов сопоставления в базу данных.
	7. Используйте метод `commit` сеанса, чтобы сохранить изменения в базе данных.

```python
class Base(AsyncAttrs, DeclarativeBase):

    """

    Базовый класс для всех моделей SQLAlchemy.

    """

    __abstract__ = True

    id: Mapped[int_pk]
```

2. классическое
	1. Импортируйте функции таблицы, сопоставления и отношений из SQLAlchemy.
	2. Определите структуру таблицы с помощью объекта `Table`. Объект `Table` должен иметь столбцы, определенные с помощью объектов `Column`.
	3. Создайте класс Python для представления таблицы. Класс должен иметь атрибуты, соответствующие столбцам таблицы.
	4. Используйте функцию `mapper` для сопоставления класса Python с объектом `Table`.
	5. Используйте функцию отношения для определения отношений между таблицами.

``` pythonwrap
CityUser = Table(

    "city_user",

    Base.metadata,

    Column("user_id", Integer, ForeignKey("user.id"), primary_key=True),

    Column("city_id", Integer, ForeignKey("city.id"), primary_key=True)

    )
```

# Транзакции

Транзакция в базе данных — **это серия из одной или нескольких операций, выполняемых как единая атомарная единица работы**. Это означает, что либо все операции в транзакции завершаются успешно, либо ни одна из них не применяется к базе данных.

Транзакции используются для **обеспечения согласованности и целостности данных**. Они гарантируют, что база данных останется согласованной даже в случае системных сбоев или ошибок. Транзакция достигающего своего нормального завершения EOT - end of transaction и тем самым фиксирующая свои результаты сохраняет согласованность бд

**Примеры операций**, которые могут быть частью транзакции: вставка, обновление или удаление данных в таблице, создание или изменение таблицы, создание или изменение индекса. 

**Отмена транзакции** называется откатом. Она может иметь два исхода: первый — изменения данных, произведённые в ходе её выполнения, успешно зафиксированы в базе данных, второй — транзакция отменяется, и отменяются все изменения, выполненные в её рамках

# ACID

Набор требований к транзакционной системе **ACID** (atomicity (атомарность), consistency (согласованность), isolation (изоляция), durability (устойчивость))

- **Atomicity** (атомарность)
Атомарность гарантирует что каждая транзакция будет выполнена полностью или не выполнена совсем без промежуточных операций

- **Consistency** (согласованность)
Транзакция не допускает промежуточных результатов, тем самым остается согласованной

- **Isolation** (изоляция)
Определяет как транзакции могут взаимодействовать между собой, насколько сильно могут пересекаться и мешать параллельной работе. Разные уровни изоляции допускают или не допускают разные аномалии при параллельной работе транзакций

- Durability (устойчивость)
Сохранность выполненной операции, при любом исходе. 

## Isolation

4 основные уровня изоляции:
- **READ UNCOMMITTED**
Самый слабый уровень изоляции. Транзакция может видеть результаты других транзакций, даже если они не закоммичены

**Проблемы**:
1. **Dirty read** (грязное чтение)
2. **Non-repeaeable read** / **Fuzz read**
3. **Phantom read** (фантомное чтение) 
4. **Lost update**
5. **Out-of-order read** (неупорядоченное чтение) 

- **READ COMMITTED**
Транзакция может читать только те изменения в других параллельных транзакциях которые уже были закоммичены

**Проблемы**:
- **Dirty read** решается
- **Fuzz read** остается
- **Phantom read** остается
- **Lost update** остается
- **Out-of-order read** остается

- **REPEATABLE READ**
Пока транзакция не завершится никто параллельно не может изменять или удалять строки, которые транзакция уже прочитала

**Проблемы**:
- **Dirty read** решается
- **Fuzz read** решается
- **Phantom read** остается

- **SERIALIZABLE**
самый жесткий, тяжелый, медленный для обработки запросов. Блокировка любых действий при запущенной транзакции

**Проблемы**: 
- Все решается 

![[Pasted image 20250204231442.png]]

*Редкие методы изоляции*:
- **READ STABILITY** - данные прочитанные первой транзакцией не будут изменены другой транзакцией до завершения первой транзакции. нет проблемы неповторяющегося чтения но есть фантомные чтения
- **CURSOR STABILITY** - блокирует строку прочитанную через курсор
- **SNAPSHOT ISOLATION**  многоверсионная изоляция - (SQL SERVER) - делается снимок бд на момент начала транзакции, нет проблемы фантомного чтения и неповторяющегося чтения, не блокируются данные при чтении, обеспечивается параллелизм. нет полной сериализации, остается Lost update. Работает через MVCC
- **SERIALIZABLE SNAPSHOT ISOLATION SSI** (PosrgeSQL) - улучшает SERIALIZABLE. использует MVCC

! Уровни изоляции по умолчанию в postgreSQL READ COMMITTED, SQlite SERIALIZED

**Проблемы**:
1. **Dirty read** (грязное чтение) - данные при чтении, можно откатить до завершения транзакции

- Селект баланса
- Изменение баланса без коммита
- Селект вернет новый баланс
- Откат баланса
- Селект вернет измененный (неверный) баланс

2. **Non-repeaeable read** / **Fuzz read** - прочитанные данные несколько раз возвращают разные результаты из-за коммитов другой транзакции
3. **Phantom read** (фантомное чтение) - прочитанные данные несколько раз возвращают разное количество строк из-за добавления и удаления другой транзакцией
4. **Lost update** - две транзакции изменяют одну строку без учета изменений друг друга, одна модификация теряется
5. **Out-of-order read** (неупорядоченное чтение) - несколько чтений в произвольном порядке могут привести к неправильному результату в транзакциях

**MVCC** (Multiversion Concurrency Control) - метод управления конкурентным доступом к данным в БД который позволяет нескольким транзакциям работать с данными одновременно без конфликтов. Поддерживает высокую производительность, изоляцию транзакций, минимизация блокировок, улучшенный параллелизм.
Работает в каждой СУБД с разной реализацией
Используемые технологии:
- многоверсионность - создается новая версия данных вместо изменения строки в бд и она меняется
- изоляция - транзакция видит только те данные которые были зафиксированы до ее начала. предотвращает грязное чтение, неповторяющееся чтение и фантомное чтение
- отсутствие блокировок для чтения и записи. повышает производительность
- управление конфликтами


# Normalization databases

Связи между таблицами:
1. **Один к одному**
2. **Многие к одному**
3. **Один ко многим**
4. **Многие ко многим**

*Основы нормализации бд*:
**Нормализация** - процесс организации данных в таблицах. Необходима для оптимального расхода памяти, изменение данных, согласованности данных. Третья форма является самым высоким уровнем. На практике разбиение на большое количество таблиц является целесообразным при частом изменении данных.

**Первая нормальная форма**:
- Все атрибуты должны быть атомарными (т. е. не содержать списков, массивов или вложенных структур).
- В таблице не должно быть повторяющихся строк (записей).
- Должен быть определен первичный ключ, который уникально идентифицирует каждую запись.

**Вторая нормальная форма**:
- Должна быть выполнена 1NF.
- Все **неключевые атрибуты** должны **полностью зависеть** от **первичного ключа**.
- Если таблица имеет составной первичный ключ, то нельзя, чтобы часть этого ключа влияла только на часть данных (если такое есть — нужно выносить данные в отдельную таблицу).

**Третья нормальная форма**:
- Должна быть выполнена 2NF.
- **Не должно быть транзитивных зависимостей** (когда неключевые атрибуты зависят друг от друга, а не только от первичного ключа).
- Каждый неключевой атрибут должен зависеть **только** от первичного ключа.

**Иные формы**:
- Четвертая нормальная форма (BCNF):
	- Должна быть выполнена 3NF.
	- Каждый **детерминант** (атрибут, от которого зависят другие) должен быть **кандидатным ключом**.
	- Устраняются аномалии, когда один ключ частично определяет другой.
- Пятая нормальна форма - максимальное разбиение таблиц для избегания избыточности данных
	- Должна быть выполнена BCNF.
	- Таблица должна быть разложена так, чтобы можно было воссоздать исходные данные через соединение (JOIN), не порождая избыточность.
	- Часто применяется, когда данные содержат сложные связи многие-ко-многим.

## Пример нормализации

- *Первая нормальная форма*
❌ **Ненормализованная таблица (нет 1NF)**:

| ID  | Имя   | Телефон          |
| --- | ----- | ---------------- |
| 1   | Иван  | 111-222, 333-444 |
| 2   | Ольга | 555-666          |

✅ **Приводим к 1NF** (разбиваем многозначные атрибуты):

| ID  | Имя   | Телефон |
| --- | ----- | ------- |
| 1   | Иван  | 111-222 |
| 1   | Иван  | 333-444 |
| 2   | Ольга | 555-666 |

- *Вторая нормальная форма*

❌ **Нарушение 2NF** (данные о курсе зависят только от `Курс_ID`, а не от всего составного ключа):

|Студент_ID|Курс_ID|Название курса|Оценка|
|---|---|---|---|
|1|101|Математика|5|
|2|101|Математика|4|
|1|102|Физика|3|

✅ **Приводим к 2NF** (разносим данные в отдельные таблицы):

**Таблица "Оценки"**:

|Студент_ID|Курс_ID|Оценка|
|---|---|---|
|1|101|5|
|2|101|4|
|1|102|3|

**Таблица "Курсы"**:

| Курс_ID | Название курса |
| ------- | -------------- |
| 101     | Математика     |
| 102     | Физика         |

- *Третья нормальная форма*

❌ **Нарушение 3NF** (поле "Город" зависит от "Отдел_ID", а не от "Сотрудник_ID"):

|Сотрудник_ID|Имя|Отдел_ID|Отдел|Город|
|---|---|---|---|---|
|1|Иван|10|IT|Москва|
|2|Ольга|20|HR|Санкт-Петербург|

✅ **Приводим к 3NF** (выносим "Отдел" в отдельную таблицу):

**Таблица "Сотрудники"**:

|Сотрудник_ID|Имя|Отдел_ID|
|---|---|---|
|1|Иван|10|
|2|Ольга|20|

**Таблица "Отделы"**:

| Отдел_ID | Отдел | Город           |
| -------- | ----- | --------------- |
| 10       | IT    | Москва          |
| 20       | HR    | Санкт-Петербург |

- *Четвертая нормальная форма*

❌ **Нарушение BCNF** (у одного курса может быть несколько преподавателей и аудиторий, но преподаватель зависит только от курса):

|Курс|Преподаватель|Аудитория|
|---|---|---|
|Математика|Иван Петров|101|
|Математика|Ольга Иванова|102|

✅ **Приводим к BCNF** (разбиваем зависимости):

**Таблица "Курсы-Преподаватели"**:

|Курс|Преподаватель|
|---|---|
|Математика|Иван Петров|
|Математика|Ольга Иванова|

**Таблица "Аудитории"**:

| Курс       | Аудитория |
| ---------- | --------- |
| Математика | 101       |
| Математика | 102       |

- *Пятая нормальная форма*

❌ **Нарушение 5NF** (трёхсторонняя зависимость, таблица неразделена):

|Проект|Разработчик|Навык|
|---|---|---|
|CRM|Иван|Python|
|CRM|Иван|SQL|
|CRM|Ольга|Java|
|AI|Ольга|Python|

✅ **Приводим к 5NF** (разбиваем на три таблицы):

**Таблица "Проект-Разработчик"**:

|Проект|Разработчик|
|---|---|
|CRM|Иван|
|CRM|Ольга|
|AI|Ольга|

**Таблица "Разработчик-Навык"**:

|Разработчик|Навык|
|---|---|
|Иван|Python|
|Иван|SQL|
|Ольга|Java|
|Ольга|Python|

**Таблица "Проект-Навык"**:

| Проект | Навык  |
| ------ | ------ |
| CRM    | Python |
| CRM    | SQL    |
| CRM    | Java   |
| AI     | Python |

# Code examples

### Пример `config.py` для создания настроек по подключению к бд

```python
from pydantic_settings import BaseSettings
from sqlalchemy.ext.asyncio import async_sessionmaker, create_async_engine
from sqlalchemy.exc import SQLAlchemyError, InvalidRequestError
from sqlalchemy import text
from functools import wraps
from typing import Optional
class Settings(BaseSettings):
    """
    Класс конфигурации для хранения настроек базы данных.
    Наследуется от `BaseSettings` и предоставляет параметры подключения
    к базе данных как для синхронного, так и для асинхронного использования.
    Атрибуты:
        DB_SQLITE_SYNC (str): URL для синхронного подключения к базе данных SQLite.
        DB_SQLITE_ASYNC (str): URL для асинхронного подключения к базе данных SQLite.
        DB_USER: str
		DB_PASSWORD: str
		DB_HOST: str
		DB_PORT: int
		DB_NAME: str
    """
	DB_USER: str
	DB_PASSWORD: str
	DB_HOST: str
	DB_PORT: int
	DB_NAME: str
	
    # DB_SQLITE_SYNC: str = 'sqlite+pysqlite:///db.sqlite3'
    # DB_SQLITE_ASYNC: str = 'sqlite+aiosqlite:///db.sqlite3'

    # def get_db_sync_url(self):
         # return self.DB_SQLITE_SYNC

    # def get_db_async_url(self):
        # return self.DB_SQLITE_ASYNC

	model_config = SettingsConfigDict(
		env_file=os.path.join(os.path.dirname(os.path.abspath(__file__)), ".env")
	)

	# postrgresql
	def get_db_url(self):
		return f"postgresql+asyncpg://{self.DB_USER}:{self.DB_PASSWORD}@"
			f"{self.HOST}:{self.PORT}/{self.DB_NAME}"


settings = Settings()
DB_ASYNC_URL = settings.get_db_async_url()
DB_SYNC_URL = settings.get_db_sync_url()

engine = create_async_engine(url=DB_ASYNC_URL)
async_session_maker = async_sessionmaker(engine, expire_on_commit=False)

def connection(isolation_level: Optional[str] = None, commit: bool = True):
    """
    Декоратор для управления соединением с базой данных в асинхронном режиме.
    Открывает новую асинхронную сессию, выполняет переданную функцию в этой сессии
    и управляет транзакцией (коммит или откат в случае ошибки).
    :param isolation_level: Уровень изоляции транзакции (опционально).
    Возможные значения: "READ UNCOMMITTED", "READ COMMITTED", "REPEATABLE READ", "SERIALIZABLE".
    :param commit: Определяет, нужно ли коммитить транзакцию после выполнения функции.
	По умолчанию `True`, если `False`, изменения не фиксируются.
    :return: Декорированная функция, работающая в контексте асинхронной сессии.
    """

    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            async with async_session_maker() as session:
                try:
                    if isolation_level:
                        await session.execute(text(f"SET TRANSACTION ISOLATION LEVEL {isolation_level}"))
                    result = await func(session=session, *args, **kwargs)
                    if commit:
                        await session.commit()
                    return result
                except SQLAlchemyError as e:
                    await session.rollback()
                    raise e
        return wrapper
    return decorator
```

#### Применение @connection()

```python
@connection()
async def add_city_to_user(cls, city: CityScheme, user_id: int, session: AsyncSession) -> dict:
...

@connaction(isolation_level="READ COMMITTED", commit=False):
async def get_current_user(self, session, user_id:int):
...
```
### Файл `database.py` для создания базового класса

```python
from sqlalchemy.ext.asyncio import AsyncAttrs
from sqlalchemy.orm import declared_attr, DeclarativeBase, Mapped, mapped_column
from typing import Annotated

int_pk = Annotated[int, mapped_column(primary_key=True, autoincrement=True)]

class Base(AsyncAttrs, DeclarativeBase):
    """
    Базовый класс для всех моделей SQLAlchemy.
    """

    __abstract__ = True
    id: Mapped[int_pk]
	created_at: Mapped[datetime] = mapped_column(server_default=func.now())
	updated_at: Mapped[datetime] = mapped_column(server_default=func.now(), onupdate=func.now())

    @declared_attr
    def __tablename__(cls) -> str:
        return cls.__name__.lower()
```


`Mapped` - способ аннотировать типы данных для колонок в моделях. `Mapped[int]` указание что колонка типа int

`mapped_column` - функция для создания колонки. 
Важные аргументы для передачи
- `primary_key`
- `nullable`
- `default` (параметр для задания значения по умолчанию на уровне приложения (код программы). при создании объекта при пустом значении поля будет указано значение default.)
- `server_default` (параметр для задания значения по умолчанию на уровне бд (прямой запрос sql). при создании объекта при пустом значении поля будет указано значение `server_default`. отличие от `default`: это значение применяется, если запись добавляется в таблицу напрямую, например, через SQL-запросы, минуя приложение)
- `autoincrement`
- `unique`

Указание `__tablename__`    указывает название для обращения к таблице в программе

### Файл `service.py` базовый сервис для работы с бд

```python
from sqlalchemy import update
from sqlalchemy.future import select
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy.orm import class_mapper

from app.config import connection  

class BaseService:
    """
    Базовый сервис для работы с моделями SQLAlchemy.
    Определяет стандартные CRUD-методы:
    - `find_all` — получить все записи по фильтру.
    - `find_one_or_none` — получить одну запись или None по фильтру.
    - `add` — добавить новую запись в базу данных.
    - `update` — обновляет запись в базе данных.
    - 'to_dict'  - Универсальный метод для конвертации объекта SQLAlchemy в словарь
    """

    model = None
    
    @classmethod
    @connection()
    async def find_all(cls, session: AsyncSession, **filter_by):
        query = select(cls.model).filter_by(**filter_by)
        result = await session.execute(query)
        return result.scalars().all()

    @classmethod
    @connection()
    async def find_one_or_none(cls, session: AsyncSession, **filter_by):
        query = select(cls.model).filter_by(**filter_by)
        result = await session.execute(query)
        return result.scalar_one_or_none()

    @classmethod
    @connection()
    async def add(cls, session: AsyncSession, **values):
        new_instance = cls.model(**values)
        session.add(new_instance)
        await session.flush()
        return new_instance

    @classmethod
    @connection()
    async def update(cls, session: AsyncSession, filter_by, **values):
        query = (
            update(cls.model)
            .where(*[getattr(cls.model, k) == v for k, v in filter_by.items()])
            .values(**values)
        .execution_options(synchronize_session="fetch")
        )
        result = await session.execute(query)
        await session.flush()
        return result.rowcount

    def to_dict(self) -> dict:          
    # class_mapper(self.\_\_class__).columns - метод возвращает объект маппера SQLAlchemy, который содержит информацию о всех колонках модели
        columns = class_mapper(self.__class__).columns  
        return {column.key: getattr(self, column.key) for column in columns}
```

Процесс добавления: 
1. Открытие сессии
2. Создание инстанса (экземпляра) 
3. Формирование запроса
4. Коммит

Основные методы `session`:
1. `commit()` - подтверждение изменений
2. `flush()` - отправление изменений без окончательной фиксации. можно использовать для генерации полей и сразу их использовать без отправки в бд. Транзакция остается открытой 
3. `rollback()` - возврат бд к предыдущему состоянию
4. `execute()` - выполнение запроса к бд

#### **Получение данных**
Для выборки данных используется `select()`
Метод `get()` - быстрое получение одной записи по первичному ключу. Поддерживает асинхронный режим 

Фильтры для получения данных:
1. `where()` / `filter()` - используется с операторами сравнения и логическими операторами. работает с классом
```python
query = select(User).where(User.age > 18, User.active == True)
```
2.` filter_by()` - принимает именованные аргументы, соответствующие колонкам модели
```python
query = select(User).filter_by(id=user_id)
```

#### Связь данных организуется через options()

**Основные подгрузки**:
1. `joinloaded` - LEFT OUTER JOIN (=== lazy='joined')
```python
query = select(Users).options(joinloaded(User.profile))
```
2. `subqueryload` -  загружает связные данные через отдельный подзапрос. полезно когда JOIN не работает при больших связных данных
3. `selectinload` - загружает связные данные через запрос с оператором IN, особенно эффективно при загрузке больших объемов связных данных

#### Методы для обработки результата
1. `scalars()` - ожидание получить одну колонку результата, а не несколько полей. Преобразует результат в объекты модели
2. `scalars().all()` - возвращает список всех записей. вместе со scalars() получаем список всех объектов модели
3. `scalars.first()` - возвращение первой записи объекта модели. None если ничего не найдет
4. `scalar()` - одна запись и одно поле в результате запроса. если вернется больше то вызовется исключение 
5. `scalar_one()` / `scalar_one_or_none()` - возвращает одно значение, если запрос вернет больше одной строки то произойдет ошибка / будет одна запись или ничего
# Типы связей таблицы examples
## ForeignKey

```python
class User(Base):
	__tablename__ = 'users'    
	
	id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True)    name: Mapped[str]    

class Post(Base):   
	__tablename__ = 'posts'    
	
	id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True)    title: Mapped[str]    
	content: Mapped[Text]    
	user_id: Mapped[int] = mapped_column(ForeignKey('users.id'))  # Внешний ключ
```

**ForeignKey** (внешний ключ) - связь одной таблицы с другой

## Relaionship

Описание зависимостей (relationship)
### Связь один к одному

```python
class Users:
	__tablename__ = 'users'

	profile: Mapped["Profiles"] = relationship(
	"Profiles", # указание формата данных
	back_populates="users", # указание обратной связи в модели "Profiles"
	uselist=False, # определение свзяи один к одному. По умолчанию True для связи многие ко многим
	lazy="joined" # автоматическая подгрузка данных 
	)

class Profiles:
	__tablename__ = 'profiles'

	user: Mapped["Users"] = relationship(
	"Users",
	back_populates="profiles",
	uselist=False
	)
```

### Связь многие к одному

``` python
class User(Base):   
	# Поля пользователя...     
	posts: Mapped[list["Post"]] = relationship(  
	"Post",   
	back_populates="user",    
	cascade="all, delete-orphan"  # Удаляет посты при удалении пользователя    
	) 

class Post(Base):
	# Поля поста...    
	user_id: Mapped[int] = mapped_column(ForeignKey('users.id'))
	user: Mapped["User"] = relationship(        
	"User",  
	back_populates="posts"   
	)
```

### Связь многие ко многим

```python

CityUser = Table(
    "city_user",
    Base.metadata,
    Column("user_id", Integer, ForeignKey("user.id"), primary_key=True),
    Column("city_id", Integer, ForeignKey("city.id"), primary_key=True)
    )

class CitiesModel(Base):
    __tablename__ = 'city'
    name: Mapped[str] = mapped_column(nullable=False)
    longitude: Mapped[float]
    latitude: Mapped[float]


    users: Mapped[List["UsersModel"]] = relationship( # type: ignore
        "UsersModel",
        secondary="city_user",
        back_populates="cities",
        lazy='selectin'
        )

class UsersModel(Base):
    __tablename__ = 'user'
    login: Mapped[str] = mapped_column(unique=True, nullable=False)
    cities: Mapped[List["CitiesModel"]] = relationship( # type: ignore
        "CitiesModel",
        secondary="city_user",
        back_populates="users",
        lazy="selectin",
        )
```
