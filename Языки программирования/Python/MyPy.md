- [Простые примеры аннотации типов](#%D0%9F%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%B5-%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80%D1%8B-%D0%B0%D0%BD%D0%BD%D0%BE%D1%82%D0%B0%D1%86%D0%B8%D0%B8-%D1%82%D0%B8%D0%BF%D0%BE%D0%B2)
- [Стандартные дженерики:](#%D0%A1%D1%82%D0%B0%D0%BD%D0%B4%D0%B0%D1%80%D1%82%D0%BD%D1%8B%D0%B5-%D0%B4%D0%B6%D0%B5%D0%BD%D0%B5%D1%80%D0%B8%D0%BA%D0%B8)
	- [Составные (контейнерные) типы](#%D0%A1%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%BD%D1%8B%D0%B5-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%BD%D1%8B%D0%B5-%D1%82%D0%B8%D0%BF%D1%8B)
	- [Другие контейнеры](#%D0%94%D1%80%D1%83%D0%B3%D0%B8%D0%B5-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B)
	- [Объединение и опциональный типы](#%D0%9E%D0%B1%D1%8A%D0%B5%D0%B4%D0%B8%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B8-%D0%BE%D0%BF%D1%86%D0%B8%D0%BE%D0%BD%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B9-%D1%82%D0%B8%D0%BF%D1%8B)
	- [Обобщенные типы (Generics)](#%D0%9E%D0%B1%D0%BE%D0%B1%D1%89%D0%B5%D0%BD%D0%BD%D1%8B%D0%B5-%D1%82%D0%B8%D0%BF%D1%8B-generics)


MyPy - статический типизатор
```python
def greet(name: str) -> str:
    return f"Hello, {name}!"

def add_numbers(x: int, y: int) -> int:
    return x + y

def get_length(s: str) -> int:
    return len(s)

def is_even(n: int) -> bool:
    return n % 2 == 0
```

# Простые примеры аннотации типов 

После имени аргументы через двоеточие указывается тип аргумента 
Через стрелку после аргументов функции указывается тип возвращаемого значения

Стандартные скалярные типы (не коллекции):
- `int`
- `float`
- `bool`
- `str`
- `bytes` - последовательность байт
- `bytearray` - изменяемый массив байт
- `None`

# Стандартные дженерики:

## Составные (контейнерные) типы
- `List[T]` - массив типа T
- `Tuple[T1, T2, ...]` - первый элемент типа T1, второй T2
- `Tuple[T1, ...]` - вариант переменной длины
- `Set[T]` - множество типа T
- `FrozenSet[T]` - неизменяемое множество
- `Dict[K, V]` - ключ типа K, значение типа V
## Другие контейнеры
- `Iterable[T]` - любой итерируемый объект типа T
```python
def t(items: Iterable[str]) -> None:
	for item in items:
		print(item)
```
- `Itertator[T]` - итератор выдающий элементы типа T
```python
def t(n: int) -> Iterator[int]:
	i = 0
	while i < n:
		yield i
		i += 1
```
- `Sequence[T]` - любая упорядоченная коллекция (список, кортеж), поддерживающая индексирование
```python
from typing import Sequence

def first_item(items: Sequence[int]) -> int:
    return items[0]

```
- `Mapping[K, V]` - абстрактное отображение (словарь)
```python
from typing import Mapping

def lookup(mapping: Mapping[str, int], key: str) -> int:
    return mapping[key]

```

## Объединение и опциональный типы

 - `Union[T1, T2, ..., TN]` - любой из перечисленных типов
```python
from typing import Union

def stringify(num: Union[int, float]) -> str:
    return str(num)

```
 
- `Optional[T, V]` - тип T или None, аналогично `Union[T, None]`
```python
from typing import Optional

def find_user(user_id: int) -> Optional[str]:
    # Возвращает имя пользователя или None, если не найдено
    return None

```

Функциональные типы и типизация функций
- `Callable\[\[T1, T2, ...], \[K]]` - функция с указанными типами аргументов и возвращаемого значения
```python
from typing import Callable

def apply_func(func: Callable[[int, int], int], a: int, b: int) -> int:
    return func(a, b)

```

- `@overload` - перегрузка функций
```python
from typing import overload

@overload
def parse(data: str) -> int: ...

@overload
def parse(data: bytes) -> int: ...

def parse(data):
    if isinstance(data, bytes):
        return int(data.decode())
    return int(data)
```

## Обобщенные типы (Generics)
- `TypeVar` - параметризированный тип позволяющий создавать обобщенные функции и классы
```python
from typing import TypeVar, List

T = TypeVar('T')

def first_element(lst: List[T]) -> T:
    return lst[0]

```
- `Generic` - базовый класс для создания обобщенных классов
```python
from typing import Generic, TypeVar

T = TypeVar('T')

class Stack(Generic[T]):
    def __init__(self) -> None:
        self.items: List[T] = []
    
    def push(self, item: T) -> None:
        self.items.append(item)
    
    def pop(self) -> T:
        return self.items.pop()

```


```python
T = TypeVar("T")
  
class LinkedList(Generic[T]):
    data: T
    next: "LinkedList[T]"
    def __init__(self, data: T):
        self.data = data
head_int: LinkedList[int] = LinkedList(1)
head_int.next = LinkedList(2)
head_int.next = 2  # error: Incompatible types in assignment (expression has type "int", variable has type "LinkedList[int]")
head_int.data += 1
head_int.data.replace("0", "1")  # error: "int" has no attribute "replace"
head_str: LinkedList[str] = LinkedList("1")
head_str.data.replace("0", "1")
head_str = LinkedList[str](1)  # error: Argument 1 to "LinkedList" has incompatible type "int"; expected "str"
```
- `NewType` - создание нового типа на основе существующего, полезно для семантической типизации

**Семантическая типизация** (или **типизация по смыслу**) — это техника в статической типизации, при которой создаются новые типы данных на основе существующих, но с разным **смысловым** значением.

```python
from typing import NewType

UserId = NewType('UserId', int)

def get_user_name(user_id: UserId) -> str:
    return "User" + str(user_id)

```
- `Final` / `@final` - переменная не должна быть изменена, класс или метод - переопределен
```python
from typing import Final

MAX_SIZE: Final[int] = 100

from typing import final

@final
class Base:
    pass

```
- `Annotated` - позволяет прикрепить метаданные к типу. `Annotated[основной тип, метаданные (строки, числа, функции, классы)]`
```python
from typing import Annotated

Age = Annotated[int, "Возраст должен быть неотрицательным"]

def set_age(age: Age) -> None:
    if age < 0:
        raise ValueError("Возраст не может быть отрицательным")

```