Middleware (Промежуточный слой)

"Middleware" это функция, которая выполняется с каждым запросом до его обработки какой-либо конкретной _операцией пути_. А также с каждым ответом перед его возвращением.

- Она принимает каждый поступающий **запрос**.
- Может что-то сделать с этим **запросом** или выполнить любой нужный код.
- Затем передает **запрос** для последующей обработки (какой-либо _операцией пути_).
- Получает **ответ** (от _операции пути_).
- Может что-то сделать с этим **ответом** или выполнить любой нужный код.
- И возвращает **ответ**.


CORSMiddleware

`CORSMiddleware` использует для параметров "запрещающие" значения по умолчанию, поэтому нужно явным образом разрешить использование отдельных источников, методов или заголовков, чтобы браузеры могли использовать их в кросс-доменном контексте.

Поддерживаются следующие аргументы:

- `allow_origins` - Список источников, на которые разрешено выполнять кросс-доменные запросы. Например, `['https://example.org', 'https://www.example.org']`. Можно использовать `['*']`, чтобы разрешить любые источники.
- `allow_origin_regex` - Регулярное выражение для определения источников, на которые разрешено выполнять кросс-доменные запросы. Например, `'https://.*\.example\.org'`.
- `allow_methods` - Список HTTP-методов, которые разрешены для кросс-доменных запросов. По умолчанию равно `['GET']`. Можно использовать `['*']`, чтобы разрешить все стандартные методы.
- `allow_headers` - Список HTTP-заголовков, которые должны поддерживаться при кросс-доменных запросах. По умолчанию равно `[]`. Можно использовать `['*']`, чтобы разрешить все заголовки. Заголовки `Accept`, `Accept-Language`, `Content-Language` и `Content-Type` всегда разрешены для простых CORS-запросов.
- `allow_credentials` - указывает, что куки разрешены в кросс-доменных запросах. По умолчанию равно `False`. Также, `allow_origins` нельзя присвоить `['*']`, если разрешено использование учётных данных. В таком случае должен быть указан список источников.
- `expose_headers` - Указывает любые заголовки ответа, которые должны быть доступны браузеру. По умолчанию равно `[]`.
- `max_age` - Устанавливает максимальное время в секундах, в течение которого браузер кэширует CORS-ответы. По умолчанию равно `600`.

HTTPSRedirectMiddleware

Проверяет все входящие запросы на `https` `wss`. Все запросы `http` `ws` будет перенаправлены на защитную схему

```
from fastapi.middleware.httpsredirect import HTTPSRedirectMiddleware

app.add_middleware(HTTPSRedirectMiddleware)
```

TrustedHostMiddleware

Обеспечивает, чтобы все входящие запросы имели правильно заданный заголовок Host, чтобы защититься от атак на HTTP Host Header.

```
from fastapi.middleware.trustedhost import TrustedHostMiddleware

app.add_middleware( TrustedHostMiddleware, allowed_hosts=["example.com", "*.example.com"] )
```

GZipMiddleware

Обрабатывает GZip-ответы для любого запроса, содержащего "gzip" в заголовке Accept-Encoding.

Промежуточное ПО будет обрабатывать как стандартные, так и потоковые ответы.

```
from fastapi.middleware.gzip import GZipMiddleware

app.add_middleware(GZipMiddleware, minimum_size=1000, compresslevel=5)
```

Form

Используйте `Form` для объявления входных параметров данных формы.
Обычно способ, которым HTML-формы (`<form></form>`) отправляют данные на сервер, использует "специальное" кодирование для этих данных, отличное от JSON.

```
from typing import Annotated

from fastapi import FastAPI, Form

app = FastAPI()


@app.post("/login/")
async def login(username: Annotated[str, Form()], password: Annotated[str, Form()]):
    return {"username": username}
```

pydantic form

```
from typing import Annotated

from fastapi import FastAPI, Form
from pydantic import BaseModel

app = FastAPI()


class FormData(BaseModel):
    username: str
    password: str


@app.post("/login/")
async def login(data: Annotated[FormData, Form()]):
    return data
```