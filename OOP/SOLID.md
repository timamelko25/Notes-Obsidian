Принципы SOLID - 5 основных принципов ООП
- Single responsibility - принцип единственной ответственности
- Open-closed - принцип открытости/закрытости
- Liskov substitution - принцип подстановки Лисков
- Interface segregation - Принцип разделения интерфейса
- Dependency inversion - Принцип инверсии зависимостей

## Single responsibility

Каждый класс должен иметь только 1 причину для изменения

Ошибка

```
class Report:
    def __init__(self, data):
        self.data = data

    def generate(self):
        return f"Report: {self.data}"

    def save_to_file(self, filename):
        with open(filename, "w") as f:
            f.write(self.generate())

```

Верно

```
class Report:
    def __init__(self, data):
        self.data = data

    def generate(self):
        return f"Report: {self.data}"

class FileSaver:
    def save(self, content: str, filename: str):
        with open(filename, "w") as f:
            f.write(content)

```

## Open-closed

Классы должны быть открыты для расширения, но закрыты для изменения

Ошибка

```
class Discount:
    def get(self, customer_type):
        if customer_type == "VIP":
            return 0.2
        elif customer_type == "Regular":
            return 0.1
        return 0.0

```

Верно

```
from abc import ABC, abstractmethod

class DiscountStrategy(ABC):
    @abstractmethod
    def get(self):
        pass

class VIPDiscount(DiscountStrategy):
    def get(self):
        return 0.2

class RegularDiscount(DiscountStrategy):
    def get(self):
        return 0.1

def apply_discount(strategy: DiscountStrategy):
    return strategy.get()

```

## Liskov substitution

Подтипы должны заменять базовый тип без ошибок

Ошибка

```
class Bird:
    def fly(self):
        pass

class Ostrich(Bird):
    def fly(self):
        raise Exception("Ostriches can't fly!")

```

Верно

```
class Bird:
    pass

class FlyingBird(Bird):
    def fly(self):
        pass

class Sparrow(FlyingBird):
    def fly(self):
        print("Sparrow flies")

class Ostrich(Bird):
    def walk(self):
        print("Ostrich walks")

```

## Interface segregation

Клиенты не должны зависеть от интерфейсов, которые они реализуют

Ошибка

```
class Worker:
    def work(self): pass
    def eat(self): pass

class Robot(Worker):
    def work(self): pass
    def eat(self):
        raise NotImplementedError("Robots don't eat")

```

Верно

```
class Workable:
    def work(self): pass

class Eatable:
    def eat(self): pass

class Human(Workable, Eatable):
    def work(self): pass
    def eat(self): pass

class Robot(Workable):
    def work(self): pass

```
## Dependency inversion

Модули верхнего уровня не должны зависеть от модулей нижнего уровня. Оба должны зависеть от абстракций

Ошибка

```
class MySQLDatabase:
    def connect(self):
        print("Connected to MySQL")

class App:
    def __init__(self):
        self.db = MySQLDatabase()  # жесткая зависимость

```

Верно

```
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def connect(self):
        pass

class MySQLDatabase(Database):
    def connect(self):
        print("Connected to MySQL")

class App:
    def __init__(self, db: Database):
        self.db = db

# Использование:
db = MySQLDatabase()
app = App(db)

```