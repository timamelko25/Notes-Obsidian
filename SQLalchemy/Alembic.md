Средство для создания и обновления таблиц на основе моделей, выполнение миграций

1. Выполнение команды в /app

```
alembic init -t async migration
```

2. move alembic.ini > /
3. migration/env.py
	- импорт подключения бд, базовый класс
	- импорт всех моделей
```python
from database import Base, DATABASE_URL
```

	- указание конфига для подключения

```python
config = config.config
config.set_main_option("sqlalchemy.url", DATABASE_URL)
```

	- указание где искать информацию по всем моделям

```python
target_metadata = Base.metadata
```

Создание миграции

```
alembic revision --autogenerate -m "__name__"
```

Обновление до последней миграции

```
alembic upgrade head
```

Переход на предыдущую версию

```
alembic downgrade -1
```
