# Предотвращение вторжений

Путь в веб-интерфейсе NGFW: **Правила трафика -> Предотвращение вторжений**

{#top}

{% cut "Получение статуса работы службы" %}

```
GET /ips/status
```

**Ответ на успешный запрос:** 

```json5
[
    {
        "name": "string",
        "status": "active" | "activating" | "deactivating" | "failed" | "inactive" | "reloading",
        "msg": [ "string" ]
    }
]
```

* `name` - название демона;
* `status` - статус;
* `msg` - список сообщений, объясняющий текущее состояние.

{% endcut %}

{#top}

{% cut "Управление статусом работы службы" %}

**Получение текущей настройки включенности модуля**

```
GET /ips/state
```

**Ответ на успешный запрос:**

```json5
{

    "enabled": "boolean"
}
```

* `enabled` - `true` если модуль включен, `false` - если выключен.

**Изменение настройки включенности модуля**

```
PUT /ips/state
```

**Json-тело запроса:**

```json5
{

    "enabled": "boolean"
}
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## Группы сигнатур

{#top}

{% cut "Получение представления групп сигнатур в табличном виде" %}

```
GET /ips/signature_groups/table
```

**Ответ на успешный запрос:** 

```json5
{

    "signature_groups": [
        {
            "classtype": "string",
            "classtype_name": "string",  
            "mitre_tactics": [
                {
                    "mitre_tactic_id": "string",
                    "mitre_tactic_name": "string"  
                },
                ...
            ],
            "count": "integer"
        },
        ...
    ]
}
```

* `classtype` - группа сигнатур;
* `classtype_name` - название группы сигнатур (отображается в интерфейсе Ideco NGFW);
* `mitre_tactics` - тактика согласно матрице MITRE ATT&CK, которой соответствует группа сигнатур:
    * `mitre_tactic_id` - идентификатор тактики;
    * `mitre_tactic_name` - название тактики.
* `count` - количество сигнатур в группе.

{% endcut %}

{#top}

{% cut "Получение представления групп сигнатур в матричном виде MITRE ATT&CK" %}

```
GET /ips/signature_groups/mitre
```

**Ответ на успешный запрос:**

```json5
{

    "signature_groups": [
        {
            "mitre_tactic_id": "string",
            "mitre_tactic_name": "string",  
            "classtypes": [
                {
                    "classtype": "string-admin",
                    "classtype_name": "string",  
                    "count": "integer"
                },
                ...
            ]
        },
        ...
    ]
}
```

* `mitre_tactic_id` - идентификатор тактики согласно матрице MITRE ATT&CK;
* `mitre_tactic_name` - название тактики;
* `classtypes` - группы сигнатур, соответствующие тактике:
    * `classtype` - группа сигнатур;
    * `classtype_name` - название группы сигнатур (отображается в интерфейсе Ideco NGFW);
    * `count` - количество сигнатур в группе.

{% endcut %}

{#top}

{% cut "Получение списка сигнатур определенной группы" %}

```
GET /ips/signatures?filter=[ { "items": [ {"column_name":"classtype","operator":"equals","value":[<classtype нужной группы сигнатур (может быть несколько значений через запятую)>]} ], "link_operator":"or" } ]
```

* `"column_name":"classtype","operator":"equals","value":[<classtype нужной группы сигнатур (может быть несколько значений через запятую)>]` - фильтр. Отбирает из таблицы групп сигнатур только те группы, у которых значение `classtype` соответствует указанным в `value`.

**Ответ на успешный запрос:**

```json5
{

    "signatures": [
        {
            "action": "string",
            "protocol": "string",
            "flow": "string",
            "classtype": "string-admin",
            "sid": "integer",
            "signature_severity": "string",
            "mitre_tactic_id": "string",
            "signature_source": "string",
            "msg": "string",
            "source": "string",
            "source_ports": "string",
            "destination": "string",
            "destination_ports": "string",
            "updated_at": "string"
        },
        ...
    ]
}
```

* `action` - действие для трафика, соответствующего сигнатуре:
  * `pass` - **Пропускать**;
  * `alert` - **Предупреждать**;
  * `drop` - **Блокировать**;
  * `rejectsrc` - **Отправлять RST узлу источника**;
  * `rejectdst` - **Отправлять RST узлу назначения**;
  * `rejectboth` - **Отправлять RST обоим**.
* `protocol` - протокол (`tcp`, `udp`, `icmp`, `ip`). Возможные значения представлены по [ссылке](https://docs.suricata.io/en/latest/rules/intro.html#protocol);
* `flow` - направление трафика (`client2server`, `server2client`, `-`);
* `classtype` - группа, к которой относится сигнатура;
* `sid` - идентификатор сигнатуры;
* `signature_severity` - уровень угрозы;
* `mitre_tactic_id` - тактика согласно матрице MITRE ATT&CK;
* `signature_source` - источник сигнатуры: 
    * `standard` - стандартные правила;
    * `advanced` - правила IPS от Лаборатории Касперского;
    * `custom` - пользовательские правила.
* `msg` - название сигнатуры;
* `source` - источник подключения;
* `source_ports` - порты источника;
* `destination` - назначение;
* `destination_ports` - порты назначения;
* `updated_at` - дата в формате `YYYY-MM-DD` или строка со значением `-`.

{% endcut %}

{#top}

{% cut "Получение оригинального содержания сигнатуры" %}

```
GET /ips/signatures/<sid>
```

* `sid` - идентификатор сигнатуры.
  
**Ответ на успешный запрос:**

```json
{

    "signature": "string"
}
```

* `signature` - содержание сигнатуры.

{% endcut %}

## Пользовательские сигнатуры

{#top}

{% cut "Получение списка пользовательских сигнатур" %}

```
GET /ips/custom
```

**Ответ на успешный запрос:**

```json5
{

    "signatures": [
        {
            "action": "string",
            "protocol": "string",
            "flow": "string",
            "classtype": "string-admin",
            "sid": "integer",
            "signature_severity": "string",
            "mitre_tactic_id": "string",
            "signature_source": "string",
            "msg": "string",
            "source": "string",
            "source_ports": "string",
            "destination": "string",
            "destination_ports": "string",
            "updated_at": "string"
        },
        ...
    ]
}
```

* `action` - действие для трафика, соответствующего сигнатуре:
  * `pass` - **Пропускать**;
  * `alert` - **Предупреждать**;
  * `drop` - **Блокировать**;
  * `rejectsrc` - **Отправлять RST узлу источника**;
  * `rejectdst` - **Отправлять RST узлу назначения**;
  * `rejectboth` - **Отправлять RST обоим**.
* `protocol` - протокол (`tcp`, `udp`, `icmp`, `ip`). Возможные значения представлены по [ссылке](https://docs.suricata.io/en/latest/rules/intro.html#protocol);
* `flow` - направление трафика (`client2server`, `server2client`, `-`);
* `classtype` - группа, к которой относится сигнатура;
* `sid` - идентификатор сигнатуры;
* `signature_severity` - уровень угрозы;
* `mitre_tactic_id` - тактика согласно матрице MITRE ATT&CK;
* `signature_source` - источник сигнатуры: 
    * `standard` - стандартные правила;
    * `advanced` - правила IPS от Лаборатории Касперского;
    * `custom` - пользовательские правила.
* `msg` - название сигнатуры;
* `source` - источник подключения;
* `source_ports` - порты источника;
* `destination` - назначение;
* `destination_ports` - порты назначения;
* `updated_at` - дата в формате `YYYY-MM-DD` или строка со значением `-`.

{% endcut %}

{#top}

{% cut "Создание пользовательской сигнатуры вручную" %}

```
POST /ips/custom
```

**Json-тело запроса:**

```json5
{

    "comment": "string",
    "rule": "string"
}
```

* `comment` - описание, может быть пустым, максимальная длина - 255 символов;
* `rule` - строка с правилом, не более 8196 символов, переводы строк в ней запрещены.

**Ответ на успешный запрос:**

```json5
{

    "id": "string"
}
```

* `id` - идентификатор созданной сигнатуры.

{% endcut %}

{#top}

{% cut "Загрузка пользовательских сигнатур из файла" %}

```
POST /ips/custom_rules_file
```

Файл загружается как тело запроса, он должен иметь текстовый формат text/plain, максимальный размер файла - 32 MB.

**Ответ на успешный запрос:**

```json5
{

    "count": "integer"
}
```

* `count` - количество загруженных правил.

{% endcut %}

{#top}

{% cut "Редактирование пользовательской сигнатуры" %}

```
PATCH /ips/custom/<sid>
```

* `sid` - идентификатор сигнатуры

**Json-тело запроса (все или некоторые поля):**

```json5
{

    "comment": "string",
    "rule": "string"
}
```

* `comment` - описание, может быть пустым, максимальная длина - 255 символов;
* `rule` - строка с правилом, не более 8196 символов, переводы строк в ней запрещены.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Удаление пользовательской сигнатуры" %}

```
DELETE /ips/custom/<sid>
```

* `sid` - идентификатор сигнатуры

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

## Обновление баз

{#top}

{% cut "Получение статуса обновления баз правил Suricata и GeoIP" %}

```
GET /ips/update
```

**Ответ на успешный запрос:**

```json
{

    "status": "up_to_date" | "updating" | "failed_to_update|disabled",
    "msg": "i18n_string",
    "last_update": "float" | "null"
}
```

* `status` - текущий статус обновления баз:
  * `up_to_date` - базы успешно обновлены;
  * `updating` - скачиваются новые базы;
  * `failed_to_update` - последняя попытка обновления баз завершилась неудачно;
  * `disabled` - обновление баз выключено.
* `msg` - текстовое описание статуса обновления баз;
* `last_update` - время последнего успешного обновления баз.

{% endcut %}

{#top}

{% cut "Получение статуса обновления расширенных баз правил Suricata и GeoIP" %}

```
GET /ips/update_advanced
```

**Ответ на успешный запрос:**

```json
{

    "status": "up_to_date" | "updating" | "failed_to_update|disabled",
    "msg": "i18n_string",
    "last_update": "float" | "null"
}
```

* `status` - текущий статус обновления баз:
  * `up_to_date` - базы успешно обновлены;
  * `updating` - скачиваются новые базы;
  * `failed_to_update` - последняя попытка обновления баз завершилась неудачно;
  * `disabled` - обновление баз выключено.
* `msg` - текстовое описание статуса обновления баз;
* `last_update` - время последнего успешного обновления баз.

{% endcut %}

{#top}

{% cut "Запуск принудительного обновления баз" %}

```
POST /ips/update
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## Сети, защищенные от вторжений

{#top}

{% cut "Получение списка локальных подсетей" %}

```
GET /ips/nets
```

**Ответ на успешный запрос:**

```json
[
    {
    "id": "string",
    "address": "string"
  },
  ...
]
```

* `id` - идентификатор подсети;
* `address` - адрес подсети (например, `192.168.0.0/16`).

{% endcut %}

{#top}

{% cut "Добавление новой локальной подсети" %}

```
POST /ips/nets
```

**Json-тело запроса:**

```json
{

    "address": "string"
  }
```

* `address` - адрес подсети (например, `192.168.0.0/16`).

{% endcut %}

{#top}

{% cut "Удаление локальной подсети" %}

```
DELETE /ips/nets/<id локальной подсети>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

