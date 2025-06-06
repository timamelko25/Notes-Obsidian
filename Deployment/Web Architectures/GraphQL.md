GraphQL - язык запросов и серверная среда для API с открытым исходным кодом. Запросы делаются на основе дерева, где узлы графа представляют собой объекты, а ребра - связи между этими объектами

Типы запросов  GraphQL
- Query
- Mutation
- Subscription

Query
Query - получение данных с сервера. (аналог GET в REST) Запросы - строки, которые отправляются в теле HTTP POST-запроса. В ответ на запрос сервер присылает JSON.

Все типы запросов в GraphQL отправляются через POST. Но это если мы говорим про обмен по HTTP, и это самый распространённый вариант. Но GraphQL также может работать и через Веб-сокеты, и через gRPC, и поверх других транспортных протоколов.

Запрос
```
query {  
	users {    
		fname  
		age 
	}
}
```

Ответ (если ошибки то с ключом `errors`)

```
{
    "errors": [
        {
            "message": "Ошибка: Поле 'age' имеет недопустимое значение 'test'.",
            "locations": [
                {
                    "line": 5,
                    "column": 5
                }
            ],
            "path": [
                "users",
                0,
                "age"
            ]
        }
    ],
    "data": {
        "users": [
            {
                "fname": "Alice",
                "age": "test"
            },
            {
                "fname": "Bob",
                "age": 32
            }
        ]
    }
}
```

Mutation
С помощью мутаций можно добавлять данные в базу. (Аналог POST PUT в REST)

Запрос
```
mutation createUser{
  addUser(fname: "Richie", age: 22) {
    id
    }
}
```

Ответ (id новой записи)

```
data : {
	addUser : "a36e4h"
}
```

Subscription

Подписка на прослушивание данных в бд в реальном времени. Под капотом используются веб-сокеты. Например для отслеживания лайков на посте, при обновлении количества лайков будет прислан ответ

Запрос
```
subscription listenLikes {
  listenLikes {
    fname
    likes
  }
}
```

Ответ
```
data: {
listenLikes: {
    "fname": "Richie",
    "likes": 245
  }
}
```

Концепция запросов

1. Поля (Fields) - поле `user` возвращает объект в котором есть поле типа String 
```
{
  user {
    name
  }
}
```
2. Аргументы (Arguments) - передаем аргументы для конкретного поля. `limit` ограничения на получение ответа
```
{
  user(id: "1") {
    name
    followers(limit: 50)
  }
}
```
3. Псевдонимы (Aliases) - переименовывание полей в ответе запроса. Например для получения данных из нескольких полей с одинаковым именем
```
query {
  products {
    name
    description
  }
  users {
    userName: name
    userDescription: description
  }
}
```
4. Фрагменты (Fragments) - для избежания дублирования кода когда необходимо использовать один и тот же набор полей в нескольких местах запроса
```
{
  leftComparison: tweet(id: 1) {
    ...comparisonFields
  }
  rightComparison: tweet(id: 2) {
    ...comparisonFields
  }
}

fragment comparisonFields on tweet {
  userName
  userHandle
  date
  body
  repliesCount
  likes
}
```
5. Переменные (Variables) - способ динамического указания значения которое используется в запросе. Указание `!` делает поле обязательным
```
query GetAccHolder($id: String !) {
  accholder: user(id: $id) {
    fullname: name
  }
}

{
  "id": "1"
}
```
6. Директивы (Directives) - помогают динамически изменять структуру и форму наших запросов с помощью переменных. **@include** и **@skip** – две директивы, доступные в GraphQL. `@include(if: Boolean)` — Включить поле, если значение boolean-переменной = true. `@skip(if: Boolean)` —  Пропустить поле, если значение boolean-переменной = `true`
```
query GetFollowers($id: String) {
  user(id: $id) {
    fullname: name,
    followers: @include(if: $getFollowers) {
      name
      userHandle
      tweets
    }
  }
}

{
  "id": "1",
  "$getFollowers": false
}
```

