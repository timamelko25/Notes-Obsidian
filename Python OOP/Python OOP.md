Главные концепция ООП:
- инкапсуляция
- наследование
- полиморфизм
Полиморфизм выделяется 2 типа:
1. Ad hoc (организовывался перегрузкой функции (в Python не используется))
2. Параметрический (можно оперировать разными типами объектов через их единый базовый класс)

Класс это пространство имен.
Сам класс состоит из свойств (данных) и методов (действий). Переменные внутри класса называются атрибутами или свойствами класса. 

```
class Point: 
	color = 'red'
	radius = 2

	def __new__(cls, *args, **kwargs):
		...
		return super(),__new__(cls)

	def __init__(self, a, b):
		self.x = a
		self.y = b

	def set_coords(self, x, y):
		self.x = x
		self.y = y
		print("set")


pt = Point()
```

![[Pasted image 20241104001000.png]]

pt является объектом класса или экземпляром класса.

параметр self в функции передает ссылку на экземпляр класса. Для self в функции можно объявлять локальные свойства

**Инициализатор и финализатор** 

__ имя магического метода __ или же dunder method (double underscope)

\_\_init\_\_(self) - инициализатор объекта класса. вызывается **сразу после создания** объекта класса
1. Создание объекта (метод \_\_new\_\_)
2. Инициализация объекта (метод \_\_init\_\_)
При задании не именных параметров функции требуется обязательное их объявление

\_\_del\_\_(self) - финализатор класса. вызывается **перед уничтожением** экземпляра класса

**Магический метод \_\_new\_\_**

\_\_new\_\_ - вызывается **перед созданием** объекта класса
Обязательный параметр cls, ссылающийся на текущий экземпляр класса. cls ссылается на класс, self ссылается на экземпляр класса.

Начиная с Pyhton 3 все классы автоматически и неявно наследуются от базового класса object. Метод \_\_new\_\_ вызывается из базового класса object. При вызове super() мы получаем ссылку на базовый класс и в базовом классе вызываем магический метод

Например используется в паттерне Singelton. Создается db которая должна быть только одна и повторном создании будет не создаваться новый объект, а передаваться ссылка на уже созданный объект.

**Замыкание функций**

Замыкание (closure) — функция, которая находится внутри другой функции и ссылается на переменные объявленные в теле внешней функции (свободные переменные).

```
def multiply(num1):
    var = 10
    def inner(num2):
        return num1 * num2
    return inner
```


**Декораторы @classmethod и @staticmethod**

Декоратор в Python это функция, которая используется для изменения функции, метода или класса. Декораторы используются для добавления какого-то функционала к функциям/классам.
Более полное определение декоратора: декоратор это вызываемый объект, который используется для изменения других вызываемых объектов.

Декоратор `@classmethod` преобразует обычный метод в метод класса. Это значит, что он привязан к самому классу, а не к его экземплярам. Первым аргументом такого метода всегда является сам класс (обычно обозначается как `cls`), а не экземпляр класса (`self`).

Еще одним видом методов класса в Python являются статические методы, которые отмечаются декоратором `@staticmethod`. Статические методы ведут себя как обычные функции, за исключением того, что они могут быть вызваны на уровне класса.

**Механизм инкапсуляции** 
![[Pasted image 20241104163502.png]]

protected никак явно не ограничивает доступ к переменной извне

private так же можно объявить для переменной или метода
Интерфейсные методы:
- геттер
- сеттер
```
class Point:
	def __init__(self, x = 0, y = 0):
		self.x = x
		self.y = y

	@classmethod
	def __check_value(cls):
		...

	def set_coord(self, x, y):
		self.__x = x
		self.__y = y

	def get_coord(self, x, y):
		return self.__x self.__y

	x, y = property(get_coord, set_coord)
```

в библиотеке accessify есть декораторы методов private, protected которые более строго соблюдают работу извне.

**Свойства property**
Создается для атрибута чтобы работать с закрытыми переменными в коде программы.

**Дескрипторы**



Магические методы 
![[Pasted image 20241105160051.png]]

**Магический метод \_\_call\_\_**

 При создании объекта класса через вызов класса автоматически вызывается метод call. В общем случае обрабатывается данный алгоритм (в упрощенном) виде.  Экземпляры класса подобно функциям вызывать нельзя без методы call.
 
 **Класс который себя так ведет называется функтор**
 ```
 class Point():
	 ...
	def __call__(self, *args, **kwargs):
		obj = self.__new__(self, *args, **kwargs)
		self.__init__(self, *args, **kwargs)
		return obj

x = Point()
x()
```

**Наследование классов**
```
class Geom:
	...

class Line(Geom):
	...
```

![[Pasted image 20241105163801.png]]

self может ссылаться не только на объект дочернего класса но и на родительский. При вызове дочернего объекта класса и вызове метода из родительского класса self будет ссылаться на соответствующий объект дочернего класса

Все стандартные типы данных в Python являются классами

Расширение и переопределение

```
class Geom:
	name = "Geom"

class Line:
	def draw:
		...
```

При добавлении методов в дочернем классе - расширение (extended)


При изменении метода в дочернем классе - переопределение (overriding)

```
class Geom:
	def draw:
	...

class Line:
	def draw:
		...
```


Функция для обращения к базовому классу super(), возвращает объект

Множественные наследование
MRO

Обработка исключений
![[Pasted image 20241110011828.png]]