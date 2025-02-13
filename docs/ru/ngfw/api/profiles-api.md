# Управление профилями безопасности

## Предотвращение вторжений

Путь в веб-интерфейсе NGFW: **Профили безопасности -> Предотвращение вторжений**

{#top}

{% cut "Получение списка профилей" %}

```
GET /ips/profiles
```

**Ответ на успешный запрос:**

```json5
[
  {
    "id": "string",
    "name": "string",
    "comment": "string"
  },
  ...
]
```
* `id` - идентификатор профиля;
* `name` - название профиля, максимальная длина - 42 символа;
* `comment` - комментарий, максимальная длина - 255 символов.

{% endcut %}

{#top}

{% cut "Создание профиля" %}

```
POST /ips/profiles
```
**Json-тело запроса:**:

```json5
{

    "name": "string",
    "comment": "string"
}
```

* `name` - название профиля, максимальная длина - 42 символа;
* `comment` - комментарий, максимальная длина - 255 символов.

**Ответ на успешный запрос:**

```json5
{

  "id": "string"
}
```

* `id` - идентификатор профиля.

{% endcut %}

{#top}

{% cut "Изменение профиля" %}

```
PATCH /ips/profiles/<id профиля>
```

**Json-тело запроса:**

```json5
{

    "name": "string",
    "comment": "string"
}
```

* `name` - название профиля, максимальная длина - 42 символа;
* `comment` - комментарий, максимальная длина - 255 символов.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Удаление профиля" %}

```
DELETE /ips/profiles/<id профиля>
```

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Получение списка правил профиля" %}

```
GET /ips/profiles/<profile_id>/rules
```

* `profile_id` - идентификатор профиля, список правил которого запрашивается (без скобок и кавычек).

**Ответ на успешный запрос:**

```json5
[
  {
    "id": "integer",
    "filters": [
      {
        "key": "sid" | "mitre_tactic_id" | "protocol" | "signature_severity" | "flow" | "classtype",
        "operator": "equals",
        "values": [ "string" | "integer" ],
     },
    ...
    ]
    "action": "default" | "alert" | "drop" | "pass",
    "comment": "string"
  },
  ...
]
```

* `id` - номер правила выбора сигнатур;
* `filters` - список фильтров правила (список не может быть пустым):
    * `key` - поле фильтра (`sid` - идентификатор, `mitre_tactic_id` - тактика MITRE, `protocol` - протокол, `signature_severity` - уровень серьезности, `flow` - направление, `classtype` - класс);
    * `operator` - оператор, только `equals`;
    * `values` - список значений, которые должны принимать поля `key` (если `key` - `sid`, то `values` - число).
* `action` - строка с действием при срабатывании правила;
* `comment` - комментарий, максимальная длина 255 символов.

{% endcut %}

{#top}

{% cut "Создание правила в профиле" %}

```
POST /ips/profiles/<profile_id>/rules?anchor_item_id=<integer>&insert_after=<true|false>
```

* `profile_id` - идентификатор профиля, в котором создается правило (без скобок и кавычек);
* `anchor_item_id` - идентификатор правила, ниже или выше которого нужно создать новое;
* `insert_after` - вставка до или после. Если `true` или отсутствует, то вставить правило сразу после указанного в `anchor_item_id`. Если `false`, то на месте указанного в `anchor_item_id`.

**Json-тело запроса:**

```json5
{

    "filters": [
       {
        "key": "sid" | "mitre_tactic_id" | "protocol" | "signature_severity" | "flow" | "classtype",
        "operator": "equals",
        "values": [ "string" | "integer" ]
      },
      ...
    ],
    "action": "default" | "alert" | "drop" | "pass",
    "comment": "string"
}
```

* `filters` - список фильтров правила (список не может быть пустым):
    * `key` - поле фильтра (`sid` - идентификатор, `mitre_tactic_id` - тактика MITRE, `protocol` - протокол, `signature_severity` - уровень серьезности, `flow` - направление, `classtype` - класс);
    * `operator` - оператор, только `equals`;
    * `values` - список значений, которые должны принимать поля `key` (если `key` - `sid`, то `values` - число).
* `action` - строка с действием при срабатывании правила;
* `comment` - комментарий, максимальная длина - 255 символов.

**Ответ на успешный запрос:** 

```json5
{

  "id": "integer"
}
```

* `id` - номер правила выбора сигнатур.

{% endcut %}

{#top}

{% cut "Изменение правила в профиле" %}

```
PATCH /ips/profiles/<profile_id>/rules/<rule_id>
```

* `profile_id` - идентификатор профиля, в котором изменяется правило;
* `rule_id` - идентификатор правила в профиле.

**Json-тело запроса:** (некоторые или все поля объекта)

```json5
{

    "filters": [
      {
        "key": "sid" | "mitre_tactic_id" | "protocol" | "signature_severity" | "flow" | "classtype",
        "operator": "equals",
        "values": [ "string" | "integer" ]
       },
      ...
    ],
    "action": "default" | "alert" | "drop" | "pass",
    "comment": "string"
}
```

* `filters` - список фильтров правила (список не может быть пустым):
    * `key` - поле фильтра (`sid` - идентификатор, `mitre_tactic_id` - тактика MITRE, `protocol` - протокол, `signature_severity` - уровень серьезности, `flow` - направление, `classtype` - класс);
    * `operator` - оператор, только `equals`;
    * `values` - список значений, которые должны принимать поля `key` (если `key` - `sid`, то `values` - число).
* `action` - строка с действием при срабатывании правила;
* `comment` - комментарий, максимальная длина - 255 символов.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Перемещение правила в профиле" %}

```
PATCH /ips/profiles/<profile_id>/rules/<rule_id>/move
```

* `profile_id` - идентификатор профиля, в котором перемещается правило;
* `rule_id` - идентификатор правила в профиле.

**Json-тело запроса:**:

```json5
{

    "anchor_item_id": "string",
    "insert_after": "boolean"
}
```

* `anchor_item_id` - идентификатор правила, выше или ниже которого нужно разместить `rule_id`;
* `insert_after` - вставить до (`false`) или после (`true`) правила `anchor_item_id`.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Удаление правила в профиле" %}

```
DELETE /ips/profiles/<profile_id>/rules/<rule_id>
```

* `profile_id` - идентификатор профиля, в котором удаляется правило;
* `rule_id` - идентификатор правила в профиле.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Создание профиля с правилами" %}

```
POST /ips/profiles-create-with-rules
```

**Json-тело запроса:**: 

```json5
{

    "name": "string",
    "comment": "string",
    "rules": [
          {
          "filters": [
                {
                "key": "sid" | "mitre_tactic_id" | "protocol" | "signature_severity" | "flow" | "classtype",
                "operator": "equals",
                "values": [ "string" | "integer" ]
                  },
                  ...
                ],
          "action": "default" | "alert" | "drop" | "pass",
          "comment": "string"
          },
          ...
        ]
    }
```

* `name` - название профиля, максимальная длина - 42 символа;
* `comment` - комментарий, максимальная длина - 255 символов;
* `rules` - список правил профиля:
  * `filters` - список фильтров правила (список не может быть пустым):
    * `key` - поле фильтра (`sid` - идентификатор, `mitre_tactic_id` - тактика MITRE, `protocol` - протокол, `signature_severity` - уровень серьезности, `flow` - направление, `classtype` - класс);
    * `operator` - оператор, только `equals`;
    * `values` - список значений, которые должны принимать поля `key` (если `key` - `sid`, то `values` - число).
  * `action` - строка с действием при срабатывании правила;
  * `comment` - комментарий, максимальная длина 255 символов.

**Ответ на успешный запрос:**

```json5
{

    "id": "string"
}
```

* `id` - идентификатор созданного профиля с правилами.

{% endcut %}

{#top}

{% cut "Копирование профиля с правилами" %}

```
POST /ips/profiles/<id профиля>/copy
```

**Ответ на успешный запрос:**

```json5
{

    "id": "string"
}
```

* `id` - идентификатор созданного профиля.

{% endcut %}

{#top}

{% cut "Получение списка сигнатур в определенном профиле" %}

```
GET /ips/profiles/<id профиля>/signatures
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

{% cut "Получение количества сигнатур профиля по действиям для всех профилей" %}

```
GET /ips/profiles/actions-counts 
```

**Ответ на успешный запрос:**

```json5
{

    "profile_id": {
      "pass": "integer",
      "alert": "integer",
      "drop": "integer",
      "rejectsrc": "integer",
      "rejectdst": "integer",
      "rejectboth": "integer"
    },
    ...
}
```

* `profile_id` - идентификатор профиля:
  * `pass` - **Пропускать**;
  * `alert` - **Предупреждать**;
  * `drop` - **Блокировать**;
  * `rejectsrc` - **Отправлять RST узлу источника**;
  * `rejectdst` - **Отправлять RST узлу назначения**;
  * `rejectboth` - **Отправлять RST обоим**.
 

{% endcut %}

{#top}

{% cut "Получение количества сигнатур профиля по действиям для конкретного профиля" %}

```
GET /ips/profiles/<id профиля>/actions-counts
```

**Ответ на успешный запрос:**

```json5
{

    "rule_id": {
      "pass": "integer",
      "alert": "integer",
      "drop": "integer",
      "rejectsrc": "integer",
      "rejectdst": "integer",
      "rejectboth": "integer"
    },
    ...
}
```

* `rule_id` - идентификатор правила в профиле:
  * `pass` - **Пропускать**;
  * `alert` - **Предупреждать**;
  * `drop` - **Блокировать**;
  * `rejectsrc` - **Отправлять RST узлу источника**;
  * `rejectdst` - **Отправлять RST узлу назначения**;
  * `rejectboth` - **Отправлять RST обоим**.
 

{% endcut %}

{#top}

{% cut "Получение профилей Предотвращения вторжений, которые содержат определенную сигнатуру" %}

```
GET /ips/signatures/<sid>/profiles
```

* `sid` - идентификатор сигнатуры.

**Ответ на успешный запрос:**

```json
{

    "id": "string",
    "name": "string"
}
```

* `id` - идентификатор профиля;
* `name` - название профиля.
  

{% endcut %}

## Профили Web Application Control (WAF)

{#top}

{% cut "Получение списка профилей" %}

```
GET /reverse_proxy_backend/waf/profiles
```

**Ответ на успешный запрос:**

```json5
[
  {
    "id": "string",
    "name": "string",
    "detection_only": "boolean",
    "disabled_categories": ["string"],
    "exceptions": [
      {
        "id": "string",
        "rule_id": "integer",
        "comment": "string",
        "enabled": "boolean"
      },
      ...
    ],
    "server_tokens": "boolean",
    "comment": "string",
    "central_console": "boolean"
  },
  ...
]
```

* `id` - идентификатор профиля;
* `name` - название профиля, максимальная длина - 42 символа;
* `detection_only` - режим работы: `true` - только обнаружение, `false` - обнаружение и блокировка;
* `disabled_categories` - список идентификаторов категорий для исключения, максимальная длина - 128 символов;
* `exceptions` - исключенные правила:
  * `id` - идентификатор исключенного правила;
  * `rule_id` - идентификатор исключенного правила в профиле;
  * `comment` - комментарий, максимальная длина - 255 символов;
  * `enabled` - статус: `true` - включено, `false` - выключено.
* `server_tokens` - статус HTTP-заголовка Server: `true` - показывать, `false` - скрывать;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов;
* `central_console` - `true`, если профиль создан в Центральной консоли, только для чтения.

{% endcut %}

{#top}

{% cut "Создание профиля" %}

```
POST /reverse_proxy_backend/waf/profiles
```

**Json-тело запроса:**

```json5
{

    "name": "string",
    "detection_only": "boolean",
    "disabled_categories": ["string"],
    "server_tokens": "boolean",
    "comment": "string",
    "central_console": "boolean"
}
```

* `name` - название профиля, максимальная длина - 42 символа;
* `detection_only` - режим работы: `true` - только обнаружение, `false` - обнаружение и блокировка;
* `disabled_categories` - список идентификаторов категорий для исключения, максимальная длина - 128 символов, при создании профиля может быть пустым;
* `server_tokens` - статус HTTP-заголовка Server: `true` - показывать, `false` - скрывать;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов;
* `central_console` - `true`, если профиль создан в Центральной консоли, только для чтения.

**Ответ на успешный запрос:**

```json5
{

    "id": "string"
}
```

{% endcut %}

{#top}

{% cut "Изменение профиля" %}

```
PATCH /reverse_proxy_backend/waf/profiles/<id профиля>
```

**Json-тело запроса:**

```json5
{

    "name": "string",
    "detection_only": "boolean",
    "disabled_categories": ["string"],
    "server_tokens": "boolean",
    "comment": "string",
    "central_console": "boolean"
}
```

* `name` - название профиля, максимальная длина - 42 символа;
* `detection_only` - режим работы: `true` - только обнаружение, `false` - обнаружение и блокировка;
* `disabled_categories` - список идентификаторов категорий правил, которые были отключены, максимальная длина - 128 символов;
* `server_tokens` - статус HTTP-заголовка Server: `true` - показывать, `false` - скрывать;
* `comment` - комментарий, может быть пустым, максимальная длина - 255 символов;
* `central_console` - `true`, если профиль создан в Центральной консоли, только для чтения.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Удаление профиля" %}

```
DELETE /reverse_proxy_backend/waf/profiles/<id профиля>
```

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Получение списка категорий правил" %}

```
GET /reverse_proxy_backend/waf/categories
```

**Ответ на успешный запрос:**

```json5
[
  {
    "id": "string",
    "title": "string",
    "description": "string"
  },
  ...
]
```

* `id` - идентификатор категории, максимальная длина - 42 символа;
* `title` - название категории, максимальная длина - 42 символа;
* `description` - описание категории, максимальная длина - 255 символов.

{% endcut %}

{#top}

{% cut "Получение списка категорий правил в профиле" %}

```
GET /reverse_proxy_backend/waf/profiles/<id профиля>/categories
```

**Ответ на успешный запрос:**

* Список категорий правил, которые включены в профиле.

```json5
[
  {
    "id": "string",
    "title": "string",
    "description": "string"
  },
  ...
]
```

* `id` - идентификатор категории, максимальная длина - 42 символа;
* `title` - название категории, максимальная длина - 42 символа;
* `description` - описание категории, максимальная длина - 255 символов.

{% endcut %}

{#top}

{% cut "Добавление или удаление категории правил в профиле" %}

```
PATCH /reverse_proxy_backend/waf/profiles/<id профиля>/categories/<id категории>
```

**Json-тело запроса:**

```json5
{

    "enabled": "boolean"
}
```

**Ответ на успешный запрос:**

* Если профиль или категория не найдены по идентификатору, то код ответа - 542;
* Если enabled - `true`, то категория, идентификатор которой указан в запросе, будет удалена из списка `disabled_categories` профиля. Ответ - 200 ОК;
* Если enabled - `false`, то категория, идентификатор которой указан в запросе, будет добавлена в список `disabled_categories` профиля. Ответ - 200 ОК.

{% endcut %}

{#top}

{% cut "Получение белых и черных списков в профиле" %}

```
GET /reverse_proxy_backend/waf/profiles/<id профиля>/rules
```

**Ответ на успешный запрос:**

```json5
[
  {
    "aliases": ["string"],
    "aliases_negate": "boolean",
    "action": "block" | "pass",
    "comment": "string",
    "enabled": "boolean",
    "id": "integer"
  },
  ...
]
```

* `aliases` - список алиасов IP-адресов, подсетей, стран, списков стран и континентов;
* `aliases_negate` - инверсия правила;
* `action` - действие:
  * `block` - блокировать запросы;
  * `pass` - пропускать запросы.
* `comment` - комментарий, максимальная длина - 255 символов;
* `enabled` - статус: `true` - включено, `false` - выключено;
* `id` - номер правила.

{% endcut %}

{#top}

{% cut "Создание белых и черных списков в профиле" %}

```
POST /reverse_proxy_backend/waf/profiles/<id профиля>/rules?anchor_item_id=<integer>&insert_after=<true|false>
```

* `anchor_item_id` - идентификатор правила, ниже или выше которого нужно создать новое;
* `insert_after` - вставка до или после. Если `true` или отсутствует, то вставить правило сразу после указанного в `anchor_item_id`. Если `false`, то на месте указанного в `anchor_item_id`.

**Json-тело запроса:**

```json5
{

    "aliases": ["string"],
    "aliases_negate": "boolean",
    "action": "block" | "pass",
    "comment": "string",
    "enabled": "boolean"
}
```

* `aliases` - список алиасов IP-адресов, подсетей, стран, списков стран и континентов;
* `aliases_negate` - инверсия правила;
* `action` - действие:
  * `block` - блокировать запросы;
  * `pass` - пропускать запросы.
* `comment` - комментарий, максимальная длина - 255 символов;
* `enabled` - статус: `true` - включено, `false` - выключено.

**Ответ на успешный запрос:**

```json5
{

    "id": "integer"
}
```

{% endcut %}

{#top}

{% cut "Изменение белых и черных списков в профиле" %}

```
PATCH /reverse_proxy_backend/waf/profiles/<id профиля>/rules/<id правила в профиле>
```

**Json-тело запроса:**

```json5
{

    "aliases": ["string"],
    "aliases_negate": "boolean",
    "action": "block" | "pass",
    "comment": "string",
    "enabled": "boolean"
}
```

* `aliases` - список алиасов IP-адресов, подсетей, стран, списков стран и континентов;
* `aliases_negate` - инверсия правила;
* `action` - действие:
  * `block` - блокировать запросы;
  * `pass` - пропускать запросы.
* `comment` - комментарий, максимальная длина - 255 символов;
* `enabled` - статус: `true` - включено, `false` - выключено.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Перемещение белых и черных списков в профиле" %}

```
PATCH /reverse_proxy_backend/waf/profiles/<id профиля>/rules/move
```

**Json-тело запроса:**

```json5
{

  "params": {
    "id": "integer",
    "anchor_item_id": "integer",
    "insert_after": "boolean",
  },
}
```

* `id` - идентификатор перемещаемого правила;
* `anchor_item_id` - идентификатор правила, относительно которого будет перемещено правило;
* `insert_after` - вставить правило до или после `anchor_item_id`. Если `true` или отсутствует, то вставить правило сразу после указанного в `anchor_item_id`. Если `false`, то на месте указанного в `anchor_item_id`.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление белых и черных списков в профиле" %}

```
DELETE /reverse_proxy_backend/waf/profiles/<id профиля>/rules/<id правила>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Получение списка исключенных правил профиля" %}

```
GET /reverse_proxy_backend/waf/profiles/<id профиля>/exceptions
```

**Ответ на успешный запрос:**

```json5
[
  {
    "id": "string",
    "rule_id": "integer",
    "comment": "string",
    "enabled": "boolean"
  },
  ...
]
```

* `id` - идентификатор исключенного правила;
* `rule_id` - идентификатор правила;
* `comment` - комментарий, максимальная длина - 255 символов;
* `enabled` - статус: `true` - включено, `false` - выключено.

{% endcut %}

{#top}

{% cut "Создание исключенного правила в профиле" %}

```
POST /reverse_proxy_backend/waf/profiles/<id профиля>/exceptions
```

**Json-тело запроса:**

```json5
{

    "rule_id": "integer",
    "comment": "string",
    "enabled": "boolean"
}
```

* `rule_id` - идентификатор правила. Можно найти в журнале в разделе **Отчеты и журналы -> События безопасности -> [Web Application Firewall](../../ngfw/settings/reports/security-events.md#web-application-firewall)**;
* `comment` - комментарий, максимальная длина - 255 символов;
* `enabled` - статус: `true` - включено, `false` - выключено.

**Ответ на успешный запрос:**

```json5
{

    "id": "integer"
}
```

{% endcut %}

{#top}

{% cut "Изменение исключенного правила в профиле" %}

```
PATCH /reverse_proxy_backend/waf/profiles/<id профиля>/exceptions/<id исключенного правила>
```

**Json-тело запроса:**

```json5
{

    "rule_id": "integer",
    "comment": "string",
    "enabled": "boolean"
}
```

* `rule_id` - идентификатор правила. Можно найти в журнале в разделе **Отчеты и журналы -> События безопасности -> [Web Application Firewall](../../ngfw/settings/reports/security-events.md#web-application-firewall)**;
* `comment` - комментарий, максимальная длина - 255 символов;
* `enabled` - статус: `true` - включено, `false` - выключено.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление исключенного правила в профиле" %}

```
DELETE /reverse_proxy_backend/waf/profiles/<id профиля>/exceptions/<id исключенного правила>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## Контроль приложений

{#top}

{% cut "Получение списка профилей" %}

```
GET /api/application_control/profiles
```

**Ответ на успешный запрос:**

```json5
[
  {
    "id": "string",
    "name": "string",
    "comment": "string",
    "protocols": [
      {
        "id": "string",
        "action": "deny" | "allow"
      },
      ...
    ],
  }
]
```

* `id` - идентификатор профиля;
* `name` - название профиля;
* `comment` - комментарий к профилю;
* `protocols` - список протоколов, выбранных для профиля:
    * `id` - строковый идентификатор алиаса протокола с префиксом `id.l7`. Например, `id.l7.ftp_protocol`;
    * `action` - действие, применяемое к протоколу (`deny` - запретить, `allow` - разрешить).      

{% endcut %}

{#top}

{% cut "Создание профиля" %}

```
POST /api/application_control/profiles
```

**Json-тело запроса:**

```json5

{

  "name": "string",
  "comment": "string",
  "protocols": [
    {
      "id": "string",
      "action": "deny" | "allow"
    },
    ...
    ],
}
```

* `name` - название профиля;
* `comment` - комментарий к профилю;
* `protocols` - список протоколов, выбранных для профиля:
    * `id` - строковый идентификатор алиаса протокола с префиксом `id.l7`. Например, `id.l7.ftp_protocol`;
    * `action` - действие, применяемое к протоколу (`deny` - запретить, `allow` - разрешить).

**Ответ на успешный запрос:**

```json5
[
  {
    "id": "string",
    "name": "string",
    "comment": "string",
    "protocols": [
      {
        "id": "string",
        "action": "deny" | "allow"
      },
      ...
      ],
  }
]
```

* `id` - идентификатор профиля;
* `name` - название профиля;
* `comment` - комментарий к профилю;
* `protocols` - список протоколов, выбранных для профиля:
    * `id` - строковый идентификатор алиаса протокола с префиксом `id.l7`. Например, `id.l7.ftp_protocol`;
    * `action` - действие, применяемое к протоколу (`deny` - запретить, `allow` - разрешить).

{% endcut %}

{#top}

{% cut "Копирование профиля" %}

```
POST /api/application_control/profiles/<id профиля>/copy
```

**Ответ на успешный запрос:** 

```json5
{

  "id": "integer"
}
```

* `id` - идентификатор копии профиля.

{% endcut %}

{#top}

{% cut "Редактирование профиля" %}

```
PATCH /api/application_control/profiles/<id профиля>
```

**Json-тело запроса:**

```json5
[
  {
    "name": "string",
    "comment": "string",
    "protocols": [
      {
        "id": "string",
        "action": "deny" | "allow"
      },
      ...
      ],
  }
]
```

* `name` - название профиля;
* `comment` - комментарий к профилю;
* `protocols` - список протоколов, выбранных для профиля:
    * `id` - строковый идентификатор алиаса протокола с префиксом `id.l7`. Например, `id.l7.ftp_protocol`;
    * `action` - действие, применяемое к протоколу (`deny` - запретить, `allow` - разрешить).

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление профиля" %}

```
DELETE /api/application_control/profiles/<id профиля>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

