# Управление пользователями и группами

{% note info %}

Длина комментариев (`comment`) при API-запросах ограничена 255 символами.

{% endnote %}

## Управление пользователями

{#top}

{% cut "Получение списка пользователей" %}

```
GET /user_backend/users
```

**Ответ на успешный запрос:**

```json5
[
    {
        "id": "string",
        "name": "string",
        "login": "string",
        "parent_id": "string",
        "enabled": "boolean",
        "domain_type": "local" | "ad" | "ald" | "radius" | "device",
        "domain_name": "string",
        "ldap_guid": "string",
        "phone_number": "string",
        "comment": "string"
    },
    ...
]
```

* `id` - идентификатор пользователя;
* `name` - имя пользователя;
* `login` - логин пользователя;
* `parent_id` - идентификатор группы;
* `enabled` - соответствует опции **Запретить доступ**: `true` - включена, `false` - выключена;
* `domain_type` - тип пользователя:
    * `local` - локальный пользователь Ideco NGFW;
    * `ad` - пользователь, импортированный из Active Directory;
    * `ald` - пользователь, импортированный из  ALD Pro;
    * `radius` - пользователь RADIUS-сервера;
    * `device` - клиентское устройство, подключающееся через Ideco Client в режиме Device VPN.
* `domain_name` - имя домена, из которого импортирован пользователь;
* `ldap_guid` - идентификатор объекта AD;
* `phone_number` - номер телефона пользователя;
* `comment` - комментарий.

{% endcut %}

{#top}

{% cut "Создание пользователя" %}

```
POST /user_backend/users
```

**Json-тело запроса:**

```json5
{

    "name": "string",
    "login": "string",
    "psw": "string",
    "parent_id": "string",
    "phone_number": "string" | null,
    "comment": "string"
}
```

* `name` - имя пользователя;
* `login` - логин пользователя;
* `psw` - пароль пользователя;
* `parent_id` - идентификатор группы;
* `phone_number` - номер телефона пользователя, не обязательно;
* `comment` - комментарий, может быть пустым.

**Ответ на успешный запрос:**

```json5
{

    "id": "integer"
}
```

* `id` - идентификатор добавленного пользователя.

Если пользователь с указанным логином или именем существует, то исключение с описанием ошибки.

{% endcut %}

{#top}

{% cut "Изменение одного пользователя" %}

```
PUT /user_backend/users/<id пользователя>
```

**Json-тело запроса:**

```json5
{

    "name": "string",
    "login": "string",
    "parent_id": "string",
    "enabled": "boolean",
    "domain_type": "string",
    "domain_name": "string",
    "ldap_guid": "string",
    "phone_number": "string" | null,
    "comment": "string"
}
```

* `name` - имя пользователя;
* `login` - логин пользователя;
* `parent_id` - идентификатор группы;
* `enabled` - соответствует опции **Запретить доступ**: `true` - включена, `false` - выключена;
* `domain_type` - тип пользователя:
    * `local` - локальный пользователь Ideco NGFW;
    * `ad` - пользователь, импортированный из Active Directory;
    * `ald` - пользователь, импортированный из  ALD Pro;
    * `radius` - пользователь RADIUS-сервера;
    * `device` - клиентское устройство, подключающееся через Ideco Client в режиме Device VPN.
* `domain_name` - имя домена, из которого импортирован пользователь;
* `ldap_guid` - идентификатор объекта AD;
* `phone_number` - номер телефона пользователя;
* `comment` - комментарий, может быть пустым.

**Важно!** Для пользователя со значением `domain_type`: `radius` можно изменить только значения полей `enabled`, `comment` и `name`. Для пользователя со значением `domain_type`: `device` нельзя изменить никакие значения.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление пользователя" %}

```
DELETE /user_backend/users/<id пользователя>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Смена пароля пользователя" %}

```
PUT /user_backend/change_password/<id пользователя>
```

**Json-тело запроса:**

```json5
{

    "password": "string"
}
```

* `password` - новый пароль пользователя, не может быть пустым.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

## Управление группами пользователей

{#top}

{% cut "Получение групп пользователей" %}

```
GET /user_backend/groups
```

**Ответ на успешный запрос:**

```json5
[
    {
        "id": "string",
        "name": "string",
        "parent_id": "string",
        "domain_type": "string",
        "domain_name": "string",
        "ldap_guid": "string"
    }
]
```

* `id` - идентификатор группы;
* `name` - имя группы;
* `parent_id` - идентификатор родительской группы;
* `domain_type` - тип группы пользователей:
    * `local` - локальная группа Ideco NGFW;
    * `ad` - группа, импортированная из Active Directory;
    * `ald` - группа, импортированная из  ALD Pro;
    * `radius` - группа RADIUS-сервера;
    * `device` - группа Device VPN.
* `domain_name` - имя домена, из которого импортирована группа;
* `ldap_guid` - идентификатор объекта AD.

{% endcut %}

{#top}

{% cut "Создание группы пользователей" %}

```
POST /user_backend/groups
```

**Json-тело запроса:**

```json5
{

    "name": "string",
    "parent_id": "string"
}
```

* `name` - имя группы;
* `parent_id` - идентификатор группы.

**Ответ на успешный запрос:**

```json5
{

    "id": "integer"
}
```

* `id` - идентификатор добавленной группы.

Если группа с указанным именем у указанного предка существует, то код ответа 542 c описанием ошибки.

{% endcut %}

{#top}

{% cut "Изменение группы" %}

```
PUT /user_backend/groups/<id группы>
```

**Json-тело запроса:**

```json5
{

    "name": "string",
    "parent_id": "string",
    "domain_type": "string",
    "domain_name": "string",
    "ldap_guid": "string"
}
```

* `name` - имя группы;
* `parent_id` - идентификатор родительской группы;
* `domain_type` - тип группы пользователей:
    * `local` - локальная группа Ideco NGFW;
    * `ad` - группа, импортированная из Active Directory;
    * `ald` - группа, импортированная из  ALD Pro;
    * `radius` - группа RADIUS-сервера;
    * `device` - группа Device VPN.
* `domain_name` - имя домена, из которого импортирована группа;
* `ldap_guid` - идентификатор объекта AD.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление группы" %}

```
DELETE /user_backend/groups/<id группы>
```

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

## Настройки RADIUS-авторизации администраторов

{#top}

{% cut "Получение статуса RADIUS-авторизации" %}

```
GET /admins-radius-auth/state
```

**Ответ на успешный запрос:**

```json5
{

  "enabled": "boolean"
}
```

* `enabled` - если `true`, то RADIUS-авторизация включена, `false` - выключена.

{% endcut %}

{#top}

{% cut "Изменение статуса RADIUS-авторизации" %}

```
PATCH /admins-radius-auth/state
```

**Json-тело запроса:**

```json5
{

  "enabled": "boolean"
}
```

* `enabled` - включить (`true`) или выключить (`false`) RADIUS-авторизацию.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Получение настроек RADIUS-авторизации" %}

```
GET /admins-radius-auth/settings
```

**Ответ на успешный запрос:**

```json5
{

    "primary": {
      "server": "string",
      "port": "integer",
      "secret": "string"
    },
    "secondary": {
      "server": "string",
      "port": "integer",
      "secret": "string"
    }
}
```

* `primary` - основной сервер RADIUS-авторизации:
    * `server` - IP-адрес или домен основного RADIUS-сервера, может быть пустой строкой;
    * `port` - порт основного RADIUS-сервера. Целое число от 1 до 65535, по умолчанию `1812`, может быть null;
    * `secret` - секрет основного RADIUS-сервера. Строка с максимальной длиной 128 символов, может быть пустой.
* `allow_external` - резервный сервер RADIUS-авторизации, нельзя настроить без основного:
    * `server` - IP-адрес или домен основного RADIUS-сервера, может быть пустой строкой;
    * `port` - порт резервного RADIUS-сервера. Целое число от 1 до 65535, может быть null;
    * `secret` - секрет основного RADIUS-сервера. Строка с максимальной длиной 128 символов, может быть пустой.

Интеграция с RADIUS-сервером считается настроенной, если заполнены поля `server` и `secret`.

{% endcut %}

{#top}

{% cut "Изменение настроек RADIUS-авторизации" %}

```
PATCH /admins-radius-auth/settings
```

**Json-тело запроса:**

```json5
{

    "primary": {
      "server": "string",
      "port": "integer",
      "secret": "string"
    },
    "secondary": {
      "server": "string",
      "port": "integer",
      "secret": "string"
    }
}
```

* `primary` - основной сервер RADIUS-авторизации:
    * `server` - IP-адрес или домен основного RADIUS-сервера, может быть пустой строкой;
    * `port` - порт основного RADIUS-сервера. Целое число от 1 до 65535, по умолчанию `1812`, может быть null;
    * `secret` - секрет основного RADIUS-сервера. Строка с максимальной длиной 128 символов, может быть пустой.
* `allow_external` - резервный сервер RADIUS-авторизации, нельзя настроить без основного:
    * `server` - IP-адрес или домен основного RADIUS-сервера, может быть пустой строкой;
    * `port` - порт резервного RADIUS-сервера. Целое число от 1 до 65535, может быть null;
    * `secret` - секрет основного RADIUS-сервера. Строка с максимальной длиной 128 символов, может быть пустой.

Интеграция с RADIUS-сервером считается настроенной, если заполнены поля `server` и `secret`.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

