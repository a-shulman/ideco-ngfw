# Описание хендлеров

{#top}

{% cut "Авторизация" %}

```
POST /web/auth/login
```

**Json тело запроса:**

```
{

    "login": "string",    
    "password": "string",    
    "rest_path": "string" (по умолчанию строка со слэшем "/")
}

```
После успешной авторизации, сервер Ideco UTM передает в заголовках куки. Пример значений:

```
set-cookie: insecure-ideco-session=02428c1c-fcd5-42ef-a533-5353da743806
set-cookie: __Secure-ideco-3ea57fca-65cb-439b-b764-d7337530f102=df164532-b916-4cda-a19b-9422c2897663:1663839003
```

Эти куки нужно передавать при каждом запросе после авторизации в заголовке запроса Cookie.

{% endcut %}

{#top}

{% cut "Разавторизация" %}

```
DELETE /web/auth/login
```
После успешной разавторизации, сервер Ideco UTM передает в заголовках куки. Пример значений:

```
set-cookie: insecure-ideco-session=""; expires=Thu, 01 Jan 1970 00:00:00 GMT; Max-Age=0; Path=/
set-cookie: __Secure-ideco-b7e3fb6f-7189-4f87-a4aa-1bdc02e18b34=""; HttpOnly; Max-Age=0; Path=/; SameSite=Strict; Secure
```

{% endcut %}

{#top}

{% cut "Получение информации о лицензии" %}

```
GET /license
```

**Пример ответа на успешный запрос:**

```
{

    "modules": {
        "active_directory": {
            "available": true,
            "expiration_date": 1713103784.0
        },
        "kaspersky_av_for_web": {
            "available": true,
            "expiration_date": 1713103784.0
        },
        "kaspersky_av_for_mail": {
            "available": true,
            "expiration_date": 1713103784.0
        },
        "application_control": {
            "available": true,
            "expiration_date": 1713103784.0
        },
        "suricata": {
            "available": true,
            "expiration_date": 1713103784.0
        },
        "advanced_content_filter": {
            "available": true,
            "expiration_date": 1713103784.0
        },
        "standard_content_filter": {
            "available": false,
            "expiration_date": 0
        },
        "ips_advanced_rules": {
            "available": true,
            "expiration_date": 1713103784.0
        },
        "icsd": {
            "available": true,
            "max_users_count": 10000
        }
    },
    "general": {
        "available": true,
        "reason": "",
        "not_upgrade_after": 1713103784.0,
        "tech_support_end": 1713103784.0,
        "start_date": 1709647784.511906,
        "expiration_date": 1713103784.0
    },
    "license_type": "enterprise-demo",
    "license_id": "UTM-1880610351",
    "server_name": "UTM",
    "last_update_time": 1709650768.1893554,
    "company_id": "Ideco",
    "server_id": "AOW8rosllirTUx94EPLb8p0atP3fmOJcXrFu3ksfak-K",
    "registered": true,
    "unreliable": false,
    "has_connection": true,
    "license_server": "https://my.ideco.ru"
}
```

**Если лицензия для данного сервера отсутствует:**

```
{

    "registered": false,
    "has_connection": true,
    "license_server": "https://my.ideco.ru"
}
```

{% endcut %}

## Управление объектами

{#top}

{% cut "Создание объекта IP-адрес" %}

```
POST /aliases/ip_addresses
```

**Json тело запроса:**

```
{

    "comment": "string",    
    "title": "string",
    "value": "string"
}
```

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Cписок IP-адресов" %}

```
POST /aliases/ip_address_lists
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "values": [ "string" ]
}
```

**Ответ на успешный запрос:**

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Диапазон IP-адресов" %}

```
POST /aliases/ip_ranges
```

**Json тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "start": "string",
    "end": "string"
}
```

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Список IP-объектов" %}

```
POST /aliases/lists/addresses
```

**Json тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "values": ["string"] (идентификаторы объектов IP-адреса, через запятую)
}
```

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Подсеть" %}

```
POST /aliases/networks
```

**Json-тело запроса:**

```
{

    "title": "string", (максимальная длина 42 символа)
    "comment": "string", (может быть пустым, максимальная длина 256 символов)
    "value": "string" (адрес подсети в формате `192.168.0.0/24` либо `192.168.0.0/255.255.255.0`)
}
```

**Ответ на успешный запрос:**

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Домен" %}

```
POST /aliases/domains
```

**Json-тело запроса:**

```
{

    "title": "string", (максимальная длина 42 символа)
    "comment": "string", (может быть пустым, максимальная длина 256 символов)
    "value": "string" (домен)
}
```

**Ответ на успешный запрос:**

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Порт" %}

```
POST /aliases/ports
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "value": integer (номер порта)
}
```

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Диапазон портов" %}

```
POST /aliases/port_ranges
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "start": integer, (первый порт диапазона)
    "end": integer (последний порт диапазона)
}
```

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Порты" %}

```
POST /aliases/lists/ports
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "values": [ "string" ] (список портов)
}
```

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Время" %}

```
POST /aliases/time_ranges
```

**Json тело запроса:**

```
{

    "title":"string",
    "comment":"string",
    "weekdays":[int], (список дней недели, где 1-пн, 2-вт ... 7-вс)
    "start":"string", (начало временного отрезка в формате ЧЧ:ММ)
    "end":"string"(конец временного отрезка в формате ЧЧ:ММ)
}
```

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Создание объекта Расписание" %}

```
POST /aliases/lists/times
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "values": [ "string" ] (список id объектов Время)
}
```

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Получение ID объектов" %}

```
GET /aliases
```

**Ответ на успешный запрос:**

```
[
    {
        comment: "string"
        title: "string"
        type: "string"
        values: [
            "string" | integer
            "string" | integer
        ],
        id: "type.id.1"
    }, 
{

        comment: "string"
        title: "string"
        type: "string"
        value: "string" | integer
        id: "type.id.1"
    },
    ...
]  
```

В качестве ответа будет возвращен список всех объектов, существующих в UTM:

* "protocol.ah" - протокол AH;
* "protocol.esp" - протокол ESP;
* "protocol.gre" - протокол GRE;
* "protocol.icmp" - протокол ICMP;
* "protocol.tcp" - протокол TCP;
* "protocol.udp" - протокол UDP;
* "quota.exceeded"- IP-адреса пользователей, которые превысили квоту;
* "any" - допускается любое значение в этом поле;
* "interface.external_any" - все внешние интерфейсы (равно таблице *Подключение к провайдеру* в веб-интерфейсе и включает в себя подключения к провайдеру по Ethernet/VPN);
* "interface.external_eth" - внешние Ethernet-интерфейсы;
* "interface.external_vpn" - внешние VPN-интерфейсы;
* "interface.local_any" - все локальные интерфейсы;
* "group.id." - идентификатор группы пользователей;
* "interface.id." - идентификатор конкретного интерфейса;
* "security_group.guid." - идентификатор группы безопасности AD;
* "user.id." - идентификатор пользователя;
* "domain.id." - идентификатор домена;
* "ip.id." - идентификатор IP-адреса;
* "iplist." - идентификатор объекта *GeoIP (Страна)*;
* "list_of_iplists." - идентификатор объекта *Список стран*;
* "ip_range.id." - идентификатор объекта *Диапазон IP-адресов*;
* "address_list.id." - идентификатор объекта *Список IP-объектов*;
* "port_list.id." - идентификатор объекта *Список портов*;
* "time_list.id." - идентификатор объекта *Расписание*;
* "subnet.id." - идентификатор объекта *Подсеть*;
* "port_range.id." - идентификатор объекта *Диапазон портов*;
* "port.id." - идентификатор объекта *Порт*;
* "time_range.id." - идентификатор объекта *Время*;
* "zero_subnet" - сеть `0.0.0.0/0`.

{% endcut %}

{#top}

{% cut "Редактирование объекта IP-адрес" %}

```
PUT /aliases/ip_addresses/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "value": "string"
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Список IP-адресов" %}

```
PUT /aliases/ip_address_lists/{id}
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "values": [ "string" ]
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Диапазон IP-адресов" %}

```
PUT /aliases/ip_ranges/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "start": "string",
    "end": "string"
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Список IP-объектов" %}

```
PUT /aliases/lists/addresses/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "values": [ "string" ]
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Подсеть" %}

```
PUT /aliases/networks/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "value": "string"
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Домен" %}

```
PUT /aliases/domains/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "value": "string"
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Порт" %}

```
PUT /aliases/ports/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "value": "integer"
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Диапазон портов" %}

```
PUT /aliases/port_ranges/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "start": "integer",
    "end": "integer"
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Порты" %}

```
PUT /aliases/lists/ports/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "values": [ "string" ]
}
```

**Ответ на запрос пустой.**

{% endcut %}

{#top}

{% cut "Редактирование объекта Время" %}

```
PUT /aliases/time_ranges/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "weekdays": [ int ],
    "start": "string",
    "end": "string",
    "period": {"first": int, "last": int} | null
}
```

**Ответ на запрос пустой**

{% endcut %}

{#top}

{% cut "Редактирование объекта Расписание" %}

```
PUT /aliases/lists/times/id
```

**Json-тело запроса:**

```
{

    "title": "string",
    "comment": "string",
    "values": [ "string" ]
}
```

**Ответ на запрос пустой.**

{% endcut %}

## Пользовательские категории Контент-фильтра

{#top}

{% cut "Создание пользовательской категории" %}

```
POST /content-filter/users_categories
```

**Json тело запроса:**

```
{

    "name": "string",
    "description": "string",
    "urls": [ "string" ]
}
```

**urls** - список url. Либо полный путь до страницы, либо только доменное имя. В пути могут присутствовать, означающие любое количество любых символов на этом месте.

**Ответ на успешный запрос:** 

```
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Получение пользовательских категорий" %}

```
GET /content-filter/users_categories
```

**Json ответ на запрос:**

```
[
    {
        "id": "string", (номер категории, вида - users.id.1)
        "name": "string", (название категории, не пустая строка)
        "description": "string",
        "urls": ["string"] 
    },
    ...
]
```

**urls** - список url. Либо полный путь до страницы, либо только доменное имя. В пути могут присутствовать, означающие любое количество любых символов на этом месте.

{% endcut %}

{#top}

{% cut "Редактирование" %}

```
PUT /content-filter/users_categories/{category_id}
```

**Json тело запроса:**

```
{

    "name": "string",
    "description": "string",
    "urls": ["string"]
}
```

**Ответ на успешный запрос:**

```
{

    "id": "string",
    "name": "string",
    "description": "string",
    "urls": [ "string" ]
}
```

{% endcut %}

## Распространенные статусы

* **200** OK – Операция успешно завершена;
* **302** Found – Запрашиваемая страница была найдена / временно перенесена на другой URL;
* **400** Bad Request – Сервер не смог понять запрос из-за недействительного синтаксиса;
* **401** Unauthorized – Запрещено. Сервер понял запрос, но он не выполняет его из-за ограничений прав доступа к указанному ресурсу;
* **404** Not Found – Запрашиваемая страница не найдена. Сервер понял запрос, но не нашёл соответствующего ресурса по указанному URL;
* **405** Method Not Allowed – Mетод не поддерживается. Запрос был сделан методом, который не поддерживается данным ресурсом;
* **502** Bad Gateway – Ошибка шлюза. Сервер, выступая в роли шлюза или прокси-сервера, получил недействительное ответное сообщение от вышестоящего сервера;
* **542** – Валидация не пропустила тело запроса.