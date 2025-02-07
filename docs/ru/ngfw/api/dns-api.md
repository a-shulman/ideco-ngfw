# DNS-сервер

{% note info %}

Длина комментариев (`comment`) при API-запросах ограничена 255 символами.

{% endnote %}

{#top}

{% cut "Получение статуса службы DNS" %}

```
GET /dns/status
```

**Ответ на успешный запрос:**

```json5
[
  {
    "name": "string",
    "status": "active" | "activating" | "deactivating" | "failed" | "inactive" | "reloading",
    "msg": [ "string" ]
  },
  ...
]
```

* `name` - название службы;
* `status` - состояние службы;
* `msg` - список ошибок, может быть пустым.

{% endcut %}

## Настройки

{#top}

{% cut "Получение настроек DNS-сервера" %}

```
GET /dns/settings
```

**Ответ на успешный запрос:**

```json5
{

	"intercept_enabled": "boolean"
}
```

* `intercept_enabled` - перехватывать DNS-запросы на серверы в интернет.

{% endcut %}

{#top}

{% cut "Включение/выключение DNS-сервера" %}

```
PUT /dns/settings
```

**Json-тело запроса:**

```json5
{

    "intercept_enabled": "boolean"
}
```

* `intercept_enabled` - перехватывать DNS-запросы на серверы в интернет.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Включение/выключение переадресации DNS для безопасного поиска" %}

### Получение настроек:

```
GET /dns/safesearch
```

**Ответ на успешный запрос:**

```json5
{

	"enabled": "boolean"
}
```

* `enabled` - переадресовывать DNS-запросы на безопасные домены поиска Google, Yandex, YouTube, Bing, DuckDuckGo, Qwant и Pixabay.

### Изменение настроек:

```
PUT /dns/safesearch
```

**Json-тело запроса:**

```json5
{

    "enabled": "boolean"
}
```

* `enabled` - переадресовывать DNS-запросы на безопасные домены поиска Google, Yandex, YouTube, Bing, DuckDuckGo, Qwant и Pixabay.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## Управление внешними DNS-серверами

{#top}

{% cut "Получение списка" %}

```
GET /dns/zones/root
```

**Ответ на успешный запрос:**

```json5
[
    {
    "id": "string",
    "enabled": "boolean",
    "type": "ip" | "interface",
    "object": "string",
    "comment": "string"
},
...
]
```

* `id` - идентификатор объекта;
* `enabled` - если `true`, то элемент включен, `false` - выключен;
* `type` - принимает два значения:
  * `ip` - IP-адрес DNS-сервера, заданного вручную;
  * `interface` - идентификатор алиаса подключения к провайдеру (DNS-серверы, выданные подключению). Тип алиаса - `isp`.
* `object` - IP-адрес, если тип `ip`, или идентификатор алиаса подключения к провайдеру, если тип `interface`;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

{% endcut %}

{#top}

{% cut "Добавление корневого DNS-сервера" %}

```
POST /dns/zones/root
```

**Json-тело запроса:** 

```json5
{

    "enabled": "boolean",
    "type": "ip" | "interface",
    "object": "string",
    "comment": "string"
}
```

* `enabled` - если `true`, то элемент включен, `false` - выключен;
* `type` - принимает два значения:
  * `ip` - IP-адрес DNS-сервера, заданного вручную;
  * `interface` - идентификатор алиаса подключения к провайдеру (DNS-серверы, выданные подключению). Тип алиаса - `isp`.
* `object` - IP-адрес, если тип `ip`, или идентификатор алиаса подключения к провайдеру, если тип `interface`;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

**Ответ на успешный запрос:**

```json5
{

    "id": "string"
}
```

* `id` - идентификатор DNS-сервера.

{% endcut %}

{#top}

{% cut "Редактирование корневого DNS-сервера" %}

```
PATCH /dns/zones/root/<id DNS-сервера>
```

**Json-тело запроса (все или некоторые поля):** 

```json5
{

    "enabled": "boolean",
    "type": "ip" | "interface",
    "object": "string",
    "comment": "string"
}
```

* `enabled` - если `true`, то элемент включен, `false` - выключен;
* `type` - принимает два значения:
  * `ip` - IP-адрес DNS-сервера, заданного вручную;
  * `interface` - идентификатор алиаса подключения к провайдеру (DNS-серверы, выданные подключению). Тип алиаса - `isp`.
* `object` - IP-адрес, если тип `ip`, или идентификатор алиаса подключения к провайдеру, если тип `interface`;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление корневого DNS-сервера" %}

```
DELETE /dns/zones/root/<id DNS-сервера>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## Управление Forward-зонами

{#top}

{% cut "Получение списка" %}

```
GET /dns/zones/forward
```

**Ответ на успешный запрос:**

```json5
[
    {
    "id": "string",
    "name": "string",
    "enabled": "boolean",
    "servers": [ "string" ],
    "comment": "string",
},
    ...
]
```

* `id` - идентификатор объекта;
* `name` - название зоны;
* `enabled` - если `true`, то зона включена, `false` - выключена;
* `servers` - список IP-адресов DNS-серверов, заданных вручную;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

{% endcut %}

{#top}

{% cut "Добавление Forward-зоны" %}

```
POST /dns/zones/forward
```

**Json-тело запроса:**

```json5
{

    "name": "string",
    "enabled": "boolean",
    "servers": [ "string" ],
    "comment": "string"
}
```

* `name` - название зоны;
* `enabled` - если `true`, то зона включена, `false` - выключена;
* `servers` - список IP-адресов DNS-серверов, заданных вручную;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

**Ответ на успешный запрос:**

```json5
{

    "id": "string"
}
```

* `id` - идентификатор Forward-зоны.

{% endcut %}

{#top}

{% cut "Редактирование Forward-зоны" %}

```
PATCH /dns/zones/forward/<id Forward-зоны>
```

**Json-тело запроса (все или некоторые поля):** 

```json5
{

    "name": "string",
    "enabled": "boolean",
    "servers": [ "string" ],
    "comment": "string"
}
```

* `name` - название зоны;
* `enabled` - если `true`, то зона включена, `false` - выключена;
* `servers` - список IP-адресов DNS-серверов, заданных вручную;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление Forward-зоны" %}

```
DELETE /dns/zones/forward/<id Forward-зоны>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## Управление Master-зонами

{#top}

{% cut "Получение списка" %}

```
GET /dns/zones/master
```

**Ответ на успешный запрос:**

```json5
[
    {
    "id": "string",
    "name": "string",
    "enabled": "boolean",
    "config": "string",
    "comment": "string",
},
    ...
]
```

* `id` - идентификатор объекта;
* `name` - уникальное название зоны, имеет вид домена example.com;
* `enabled` - если `true`, то зона включена, `false` - выключена;
* `config` - текст с параметрами зоны, не может быть пустым. Максимальная длина - 10000 символов;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

Подробнее о формате записей для настройки Master-зоны - в статье [Master-зоны](../../ngfw/settings/services/dns/master-zon.md).

{% endcut %}

{#top}

{% cut "Добавление Master-зоны" %}

```
POST /dns/zones/master
```

**Json-тело запроса:** 

```json5
{

    "name": "string",
    "enabled": "boolean",
    "config": "string",
    "comment": "string",
}
```

* `name` - уникальное название зоны, имеет вид домена example.com;
* `enabled` - если `true`, то зона включена, `false` - выключена;
* `config` - текст с параметрами зоны, не может быть пустым. Максимальная длина - 10000 символов;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

**Ответ на успешный запрос:**

```json5
{

    "id": "string"
}
```

* `id` - идентификатор Master-зоны.

{% endcut %}

{#top}

{% cut "Редактирование Master-зоны" %}

```
PATCH /dns/zones/master/<id Master-зоны>
```

**Json-тело запроса (все или некоторые поля):** 

```json5
{

    "name": "string",
    "enabled": "boolean",
    "config": "string",
    "comment": "string",
}
```

* `name` - уникальное название зоны, имеет вид домена example.com;
* `enabled` - если `true`, то зона включена, `false` - выключена;
* `config` - текст с параметрами зоны, не может быть пустым. Максимальная длина - 10000 символов;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление Master-зоны" %}

```
DELETE /dns/zones/master/<id Master-зоны>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## DDNS

{% note info %}

DDNS в Ideco NGFW реализован через интеграцию с хостингом RU-CENTER. Перед настройкой DDNS зарегистрируйтесь на сайте [RU-CENTER](https://www.nic.ru) и приобретите [DNS-хостинг](https://www.nic.ru/catalog/for-domain-use/dns-hosting).

Подробнее о DDNS - в [статье](../../ngfw/settings/services/dns/ddns.md).

{% endnote %}

{#top}

{% cut "Получение состояния" %}

```
GET /dns/ddns/state
```

**Ответ на успешный запрос:**

```json5
{

    "enabled": "boolean"
}
```

* `enabled` - состояние DDNS: `true` - включено, `false` - выключено.

{% endcut %}

{#top}

{% cut "Включение/выключение DDNS" %}

```
PUT /dns/ddns/state
```

**Json-тело запроса:**

```json5
{

    "enabled": "boolean"
}
```

* `enabled` - состояние DDNS: `true` - включено, `false` - выключено.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Получение настроек" %}

```
GET /dns/ddns
```

**Ответ на успешный запрос:**

```json5
{

    "domain": "string",
    "service_login": "string",
    "service_password": "string"
}
```

* `domain` - домен, который администратор хочет видеть в адресной строке. Формат: `domain.com` (без `https://` и `www`);
* `service_login` - логин для доступа к API сервиса DDNS;
* `service_password` - пароль для доступа к API сервиса DDNS, до 42 символов.

{% endcut %}

{#top}

{% cut "Изменение настроек" %}

```
PUT /dns/ddns
```

**Json-тело запроса:**

```json5
{

    "domain": "string",
    "service_login": "string",
    "service_password": "string"
}
```

* `domain` - домен, который администратор хочет видеть в адресной строке. Формат: `domain.com` (без `https://` и `www`);
* `service_login` - логин для доступа к API сервиса DDNS;
* `service_password` - пароль для доступа к API сервиса DDNS, до 42 символов.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

