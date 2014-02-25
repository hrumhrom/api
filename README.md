# Hrumhrom RESTful API

<p>Hrumhrom RESTful API позволяет получать и изменять объекты на карте Hrumhrom. Для использование Hrumhrom RESTful API необходимо получить api ключ для команды.</p>

## Получение ключа

<p>Ключ команды может быть получен на странице: http://hrumhrom.ru/#team/profile_team_self_api</p>

<p>Ключ может получить только капитан, если вы не являетесь капитаном команды обратитесь к вашему капитану для получения api ключа.</p>

## Вызов методов API

<p>Методы API могут быть вызваны по средствам `http запросов` с `basic access authentication`.</p>

<p>Точка входа API:</p>

```
http://hrumhrom.ru/public/api/1/%ресурс%
```

| Ресурс  | Описание      |
|---------|---------------|
| targets | Белые кролики |
| users   | Игроки        |
| zones   | Игровые зоны  |

Используйте в качестве логина `hrumhrom` и в качестве пароля `ваш api ключ команды`. Пример вызова API с помощью библиотеки [curl](http://curl.haxx.se/):

```
curl http://hrumhrom.ru/public/api/1/targets --user hrumhrom:%ваш_api_ключ_команды%
```

Ответом всегда является валидный [JSON](http://json.org/). Пример:

```javascript
[
    {
        "id": 4873,
        "index": 109,
        "description": "91, Профсоюзная улица, Коньково, район Коньково, Москва",
        "geolocation": {
            "latitude": 55.640786487,
            "longitude": 37.527236938
        },
        "icon": {
            "id": 14
        },
        "created_at": 1379449976,
        "modified_at": 1379449976
    },
    {
        "id": 4879,
        "index": 114,
        "description": "30 к2, Веерная улица, район Очаково-Матвеевское, Москва",
        "geolocation": {
            "latitude": 55.710092713,
            "longitude": 37.478485107
        },
        "icon": {
            "id": 3
        },
        "created_at": 1379973561,
        "modified_at": 1379973561
    }
]
```

## API в примерах

#### Белые кролики

* [Получение всех Белых кроликов](#exampleTargetsGet)
* [Создание Белого кролика на карте](#exampleTargetsPost)
* [Удаление Белого кролика](#exampleTargetsDelete)
* [Удаление всех Белых кроликов](#exampleTargetsDeleteRoot)

#### Игроки

* [Получение всех игроков](#exampleUsersGet)
* [Перемещение игрока на карте](#exampleUsersPost)

#### Игровые зоны

* [Создание игровой зоны](#exampleZonesPost)
* [Удаление всех игровых зон](#exampleZonesDelete)

<a name="exampleTargetsGet"></a>
### Получение всех Белых кроликов

```
curl http://hrumhrom.ru/public/api/1/targets --user hrumhrom:%ваш_api_ключ_команды%
```

Пример ответа:

```javascript
[
    {
        "id": 4873, // Уникальный идентификатор Белого кролика
        "index": 109, // Порядковый номер
        "description": "91, Профсоюзная улица, Коньково, район Коньково, Москва",
        "geolocation": {
            "latitude": 55.640786487,
            "longitude": 37.527236938
        },
        "icon": {
            "id": 14 // Иконка (см. таблицу доступных иконок ниже)
        },
        "created_at": 1379449976,
        "modified_at": 1379449976
    }
    ...
]
```

<a name="exampleTargetsPost"></a>
### Создание Белого кролика на карте

```
curl -X POST -d %данные_белого_кролика% http://hrumhrom.ru/public/api/1/targets --user hrumhrom:%ваш_api_ключ_команды%
```

Данные Белого кролика:

```javascript
{
    // Описание: адрес и т. д.
    "description": "улица Победы, дом 15. Заброс на обочине.",

    // Широта и долгота
    "geolocation": {
        "latitude": 55.57849,
        "longitude": 37.9023786
    },

    // Иконка (см. таблицу доступных иконок ниже)
    "icon": {
        "id": 1
    }
}
```

Пример ответа:

```javascript
{
    "id": 5023, // Уникальный идентификатор
    "description": "улица Победы, дом 15. Заброс на обочине.",
    "geolocation": {
        "latitude": 55.57849,
        "longitude": 37.9023786
    },
    "icon": {
        "id": 1
    },
    "created_at": 1379448045,
    "modified_at": 1379448045
}
```

<a name="exampleTargetsDelete"></a>
### Удаление Белого кролика

```
curl -X DELETE http://hrumhrom.ru/public/api/1/targets/%id_белого_кролика% --user hrumhrom:%ваш_api_ключ_команды%
```

<a name="exampleTargetsDeleteRoot"></a>
### Удаление всех Белых кроликов

```
curl -X DELETE http://hrumhrom.ru/public/api/1/targets --user hrumhrom:%ваш_api_ключ_команды%
```

<a name="exampleUsersGet"></a>
### Получение всех игроков

```
curl http://hrumhrom.ru/public/api/1/users --user hrumhrom:%ваш_api_ключ_команды%
```

Пример ответа:

```javascript
[
    {
        "id": 1, // Уникальный идентификатор игрока
        "name": "GREEN.POINT",
        "realname": "Mr Vitaliy Green",
        "avatar": "http:\/\/hrumhrom.ru\/users\/avatars\/default.jpg",
        "phone": "+7 903 1234567",
        "icq": "473581560",
        "skype": "green_point",

        // Место, где находится игрок
        "place": {
            "id": 255, // Уникальный идентификатор экипажа или -1 для штаба и -2 для сумрака
            "name": "GREEN.POINT мобиль", // Название экипажа
            "color": "ff0000", // Цвет экипажа
            "seat": 0 // Номер сидения в экипаже от 0 - 4
        },

        // Местоположения игрока на карте
        "geolocation": {
            "source": "device", // Источник координат
            "latitude": 55.7774001,
            "longitude": 37.7244277,
            "accuracy": 0, // Точность определения координат в метрах
            "heading": 45.5, // Направление в градусах относительно севера
            "speed": 35.2, // Скорость в м/с
            "timestamp": 1393178904 // Время обновления координат в unix timestamp
        }
    }
    ...
]
```

<a name="exampleUsersPost"></a>
### Перемещение игрока на карте

```
curl -X POST -d %данные_игрока% http://hrumhrom.ru/public/api/1/users/%id_игрока% --user hrumhrom:%ваш_api_ключ_команды%
```

Данные игрока:

```javascript
{
    "geolocation": {
        // Источник координат (обязательно).
        // `device` - координаты определены устройством (например GPS приемником)
        // `user_action` - координаты задаются вручную
        "source": "device",

        // Широта (обязательно)
        "latitude": 55.7774001,

        // Долгота (обязательно)
        "longitude": 37.7244277,

        // Точность определения координат в метрах (обязательно)
        "accuracy": 0

        // Направление в градусах относительно севера (опционально)
        "heading": 45.5,

        // Скорость в м/с (опционально)
        "speed": 35.2
    }
}
```

<a name="exampleZonesPost"></a>
### Создание игровой зоны

```
curl -X POST -d %данные_игровой_зоны% http://hrumhrom.ru/public/api/1/zones --user hrumhrom:%ваш_api_ключ_команды%
```

Данные игровой зоны:

```javascript
{
    "points": [
        { "latitude": 55.3459045, "longitude": 37.423789 },
        { "latitude": 55.9043231, "longitude": 37.589021 },
        { "latitude": 55.5901201, "longitude": 37.219402 }
    ]
}
```

<a name="exampleZonesDeleteRoot"></a>
### Удаление всех игровых зон

```
curl -X DELETE http://hrumhrom.ru/public/api/1/zones --user hrumhrom:%ваш_api_ключ_команды%
```

## Иконки Белых кроликов

| Код | Описание    |
|-----|-------------|
| 1   | Стандартная |
| 2   | Рей-Бенc    |
| 3   | Идея        |
| 4   | Кисс        |
| 5   | Вор         |
| 6   | Вуду        |
| 7   | Элвис       |
| 8   | Циклоп      |
| 9   | Кинг        |
| 10  | Эмо         |
| 11  | Кости       |
| 12  | Нуар        |
| 13  | Коп         |
| 14  | Джин        |
| 15  | Эппл        |
| 16  | Джейми      |

## Обработка ошибок

| Код | Описание                                |
|-----|-----------------------------------------|
| 401 | Неверный api ключ                       |
| 404 | Сущность не существует или была удалена |
| 500 | Внутрення ошибка сервера                |
| 501 | Метод API еще не реализован             |
