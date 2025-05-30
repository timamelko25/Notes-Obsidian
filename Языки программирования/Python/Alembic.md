Средство для создания и обновления таблиц на основе моделей, выполнение миграций

1. Выполнение команды в /app

```
alembic init -t async migration
```

2. move alembic.ini > /
3. migration/env.py
	- импорт URL бд, базовый класс
	- импорт всех моделей
```python
from app.db.database import PG_URL, Base

from app.db.models import User, Key, Server
```

	- указание конфига для подключения

```python
config = context.config
config.set_main_option("sqlalchemy.url", DATABASE_URL)
```

	- указание где искать информацию по всем моделям

```python
target_metadata = Base.metadata
```

	- в файле alembic.ini изменить `script_location` на путь к миграции

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
