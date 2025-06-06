Функции и файлы тестирования начинаются с ключевого слова `test_`

Дополнительная информация по тестам: ключ `-v --verbose`

```
# content of test_sample.py
def inc(x):
    return x + 1

def test_answer():
    assert inc(3) == 5
```

`assert` - утверждение для проверки тестовых ожиданий
Тест проходит успешно если `assert` не выбрасывает исключение

Проверка вызова исключений

```
def test_foo_not_implemented():
    def foo():
        raise NotImplementedError

    with pytest.raises(RuntimeError) as excinfo:
        foo()
    assert excinfo.type is RuntimeError
```

Проверить для одного теста сразу с разными параметрами

```
@pytest.mark.parametrize("a,b,expected", [
    (1, 2, 3),
    (0, 0, 0),
    (-1, 1, 0),
])
def test_add_param(a, b, expected):
    assert add(a, b) == expected
```

Фикстуры (fixture) - повторяющаяся подготовка окружения

```
@pytest.fixture
def sample_data():
    return {"name": "Alice", "age": 30}

def test_sample_data(sample_data):
    assert sample_data["age"] == 30
```

Фикстуры можно делать модульными, глобальными, с автозапуском

```
@pytest.fixture(scope="module", autouse=True)
def setup_db():
    print("Connect to DB")
    yield
    print("Disconnect from DB")
```

Мок тесты

```
from unittest.mock import patch
from app.weather import get_temperature

@patch("app.weather.requests.get")
def test_get_temperature(mock_get):
    mock_get.return_value.json.return_value = {"temp": 20}
    assert get_temperature("Paris") == 20
```