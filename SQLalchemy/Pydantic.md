- [Field](#field)
- [Наследование Pydantic](#%D0%9D%D0%B0%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-pydantic)

\- библиотека для валидации (проверка входных данных на соответствие ожидаемым типам и ограничениям) и трансформации данных (автоматическое приведение данных у нужному формату)

Модели наследуются от базового класса `BaseModel`

**Типизация**:
- *базовая* типизация с указанием типов: name: str
- *расширенная*, аннотация полей с дополнительными параметрами: email: str = Field(...)

**Валидация**:
- *Минимальная валидация полей*: использование встроенных типов 
- *Валидаторы*: расширенные валидаторы из библиотеки (**EmailStr**, **FilePath**, **DirectoryPath**, **Color**, **HttpUrl**, **IPvAnyAddress**)

Создание **кастомного валидатора** с помощью декоратора `@field_validator` (аргумент mode='before'/'after' - выполнние валидации до или после создания объекта)

`@computed_field` - поле, которое вычисляется на основе других данных в модели. используется для автоматический генераций значений и валидации. Сразу записывает в словарь вычисленный значения

```python
class SUser(Basemodel):
	age: int
	name: str
	surname: str
	
	@field_validator('age')
	def check_age(cls, value):
		if value < 18:
			raise ValueError("Age must be under 18")
		return value

	@computed_field
	def full_name(self) -> str:
		return f"{self.name} {self.surname}"

```

Для валидации и трансформации данных полученных из бд при работе с ORM 

```python
from pydantic import BaseModel, ConfigDict

class SUser(BaseModel):
	config = ConfigDict(from_attributes=True)
	
	age: int
	
```

Создание модели Pydantioc из ORM используется метод `from_orm`:

```python
user = user.from_orm(orm_instance)
```

`dict()` (old) / `model_dump()` (new) - преобразование модели в словарь

`json()` /` model_dump_json()` - преобразование модели в **JSON** строку

# Field
Дополнение метаданных к полям моделей и настройки, которые Pydantic использует для валидации, сериализации (процесс преобразования структуры данных в линейную форму, обратное преобразование - десериализация) и документирования

Основные параметры:
- `default` - значение по умолчанию
- `default_factory` - функция возвращающая значение по умолчанию
- `alias` - альтернативное имя поля для сериализации и десериализации
- `title` - заголовок поля в доках
- `description` 
- `exclude` - исключает поле из сериализации (например при преобразовании в словарь или json)
- `repr` - определяет будет ли поле включено в строковое представление модели
- `min_length`
- `max_length`
- `regex` - проверка по регулярному выражению
- `gt` (greater than) больше чем
- `ge` (greater than or equal) больше чем или равно
- `lt` (less than) меньше чем
- `le` (less than or equal) меньше чем или равно

Современное представление в полях через `Annotated`

```python
class SUser(BaseModel):
	name: Annotated[str, Field(min_length(3))]
```

`ConfigDict` основные опции

- `from_attributes=True` — позволяет создавать объект модели напрямую из атрибутов Python-объектов (например, когда поля модели совпадают с атрибутами другого объекта). Чаще всего опция применяется для преобразования моделей ORM к моделям Pydantic. (обычно ожидается словарь, но так можно отдавать атрибутов класса)
- `str_to_lower, str_to_upper` - преобразование всех строк модели в нижний или верхний регистр
- `str_strip_whitespace` - cледует ли удалять начальные и конечные пробелы для типов str (аналог strip)
- `str_min_length, str_max_length` - задает максимальную и минимальную длину строки для всех строковых полей
- `use_enum_values` - cледует ли заполнять модели значениями, выбранными из перечислений, вместо того чтобы использовать необработанные значения? Это часто требуется при работе с моделями ORM, в которых колонки определены как перечисления (ENUM).

# Наследование Pydantic
Общие поля в модели определяются в базовой модели
Дочерние модели могут добавлять новые поля и методы

SQLAlchemy & Pydantic

1. Описание модели таблицы в SQLAlchemy
2. Создание Pydantic-модели для работы с полученными данными
3. Запрос данных из таблицы через SQLAlchemy
4. Преобразование объекта SQLAlchemy в объект Pydantic
5. Использование методов `model_dump` или `model_dump_json` для получения данных в нужном формате

Проверка данных полученных данных
```python
def get_user(user_id: int): 
	with Session() as session:     
		user = session.select(Users).filter_by(id = user_id).scalar_one_or_none()    
		# Шаг 4: Преобразование объекта SQLAlchemy в Pydantic    
		if user:          
			user_pydantic = SUser.model_validate(user)    
			# Шаг 5: Получение данных в нужном формате     
			return user_pydantic.model_dump()
		return None
```

Scheme User.model_validate(user)
можно указать в model_validate(user, from_attributes=True)

Атрибуты `model_validate`
- strict: bool | None, True - строгая валидация типов
- context: Any | None - дополнительный контекст для валидатора
- from_attributes: bool | None - извлечение данных атрибутов моделей 