# Бэкапы и возврат к предыдущей версии

{% note info %}

Длина комментариев (`comment`) при API-запросах ограничена 255 символами.

{% endnote %}

{#top}

{% cut "Получение настроек бэкапов" %}

```
GET /backup/settings
```

**Ответ на успешный запрос:**

```json5
{

   "common": {
      "hour": "integer",
      "rotate": "weekly" | "monthly"
   },
   "ftp": {
      "enabled": "boolean",
      "server": "string",
      "login": "string",
      "password": "string",
      "remote_dir": "string"
   },
   "cifs": {
      "enabled": "boolean",
      "server": "string",
      "login": "string",
      "password": "string",
      "remote_dir": "string"
   }
}
```

* `common` - общие настройки бэкапов;
  * `hour` - час, в который делается автоматический бэкап, число от 0 до
    23;
  * `rotate` - удалять бэкапы старше недели (`weekly`) или месяца (`monthly`).
* `ftp` - настройки выгрузки бэкапов на FTP:
  * `enabled` - если `true`, то выгрузка включена, `false` - выключена;
  * `server` - адрес сервера, валидный домен или IP-адрес;
  * `login` - логин, не пустая строка;
  * `password` - пароль, не пустая строка, до 42 символов;
  * `remote_dir` - удаленный каталог, не пустая строка.
* `cifs` - настройки выгрузки бэкапов в общую папку CIFS:
  * `enabled` - если `true`, то выгрузка включена, `false` - выключена;
  * `server` - адрес сервера, валидный домен или IP-адрес;
  * `login` - логин, не пустая строка;
  * `password` - пароль, не пустая строка, до 42 символов;
  * `remote_dir` - удаленный каталог, не пустая строка.

{% endcut %}

{#top}

{% cut "Изменение настроек бэкапов и настройка выгрузки на FTP-сервер или в общую папку CIFS" %}

```
PUT /backup/settings
```

**Json-тело запроса:**

```json5
{

   "common": {
      "hour": "integer",
      "rotate": "weekly | monthly"
   },
   "ftp": {
      "enabled": "boolean",
      "server": "string",
      "login": "string",
      "password": "string",
      "remote_dir": "string"
   },
   "cifs": {
      "enabled": "boolean",
      "server": "string",
      "login": "string",
      "password": "string",
      "remote_dir": "string"
   }
}
```

* `common` - общие настройки бекапов:
  * `hour` - час, в который делается автоматический бекап, число от 0 до 23;
  * `rotate` - удалять бекапы старше недели (`weekly`) или месяца (`monthly`).
* `ftp` - настройки выгрузки бекапов на FTP:
  * `enabled` - если `true`, то выгрузка включена, `false` - выключена;
  * `server` - адрес сервера, валидный домен или IP-адрес;
  * `login` - логин, не пустая строка;
  * `password` - пароль, не пустая строка, до 42 символов;
  * `remote_dir` - удаленный каталог, не пустая строка.
* `cifs` - настройки выгрузки бекапов в общую папку CIFS:
  * `enabled` - если `true`, то выгрузка включена, `false` - выключена;
  * `server` - адрес сервера, валидный домен или IP-адрес;
  * `login` - логин, не пустая строка;
  * `password` - пароль, не пустая строка, до 42 символов;
  * `remote_dir` - удаленный каталог, не пустая строка.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## Управление бэкапами

{#top}

{% cut "Создание бэкапа" %}

```
POST /backup/backups
```

**Json-тело запроса:**

```json5
{

   "comment": "string"
}
```

* `comment` - комментарий, произвольный текст.

**Ответ на успешный запрос:**

```json5
{

    "id": "string"
}
```

* `id` - идентификатор бэкапа.

{% endcut %}

{#top}

{% cut "Получение списка бэкапов" %}

```
GET /backup/backups
```

**Ответ на успешный запрос:**

```json5
[
   {
      "id": "string",
      "version": {
         "major": "integer",
         "minor": "integer",
         "build": "integer",
         "timestamp": "integer",
         "vendor": "Ideco",
         "product": "UTM" | "CC",
         "kind": "FSTEK" | "VPP" | "STANDARD" | "BPF",
         "release_type": "release" | "beta" | "devel"
      },
      "timestamp": "float",
      "comment": "string",
      "md5": "string",
      "size": "integer",
      "fast_restore_allowed": "boolean"
   }
...
]
```

* `id` - идентификатор бэкапа;
* `version` - версия системы:
  * `major` - мажорный номер версии;
  * `minor` - минорный номер версии;
  * `build` - номер сборки;
  * `timestamp` - время выхода версии;
  * `vendor` - вендор ("Ideco");
  * `product` - код продукта;
  * `kind` - вид продукта;
  * `release_type` - тип релиза;
* `timestamp` - дата/время создания бэкапа в формате UNIX timestamp;
* `comment` - комментарий, произвольный текст;
* `md5` - контрольная сумма файла бэкапа (`data.tar`);
* `size` - размер бэкапа, байт;
* `fast_restore_allowed` - можно ли выполнить быстрое восстановление из данного бэкапа (версия идентична системной).

{% endcut %}

{#top}

{% cut "Скачивание бэкапа" %}

```
GET /backup/download/<id бэкапа>
```

**Ответ на успешный запрос:** тело бэкапа.

{% endcut %}

{#top}

{% cut "Загрузка бэкапа на Ideco NGFW из файла" %}

```
POST /backup/upload
```

Используйте стандартный POST-запрос на загрузку файла. Название поля в форме должно
быть `backup_file`.

**Ответ на успешный запрос:**

```json5
{

   "id": "string"
}
```

* `id` - идентификатор бэкапа.

{% endcut %}

{#top}

{% cut "Восстановление из бэкапа" %}

```
POST /backup/backups/<id бэкапа>/apply
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Быстрое восстановление из бэкапа" %}

```
POST /backup/backups/<id бэкапа>/apply/fast
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

{#top}

{% cut "Удаление бэкапа" %}

```
DELETE /backup/backups/<id бэкапа>
```

**Ответ на успешный запрос:** 200 OK

{% endcut %}

## Возврат к предыдущей версии Ideco NGFW

{#top}

{% cut "Получение информации о наличии предыдущей версии" %}

```
GET /system_management/change_sys_root
```

**Ответ на успешный запрос:**

```json5
{

   "previous_version": {
      "major": "integer",
      "minor": "integer",
      "build": "integer",
      "timestamp": "integer",
      "vendor": "string",
      "product": "UTM" | "CC",
      "kind": "FSTEK" | "NGFW-FSTEK" | "VPP" | "STANDARD" | "BPF",
      "release_type": "release" | "beta" | "devel"
   }
}
```

* `major` - мажорный номер версии;
* `minor` - минорный номер версии;
* `build` - номер сборки;
* `timestamp` - время сборки версии в формате UNIX timestamp;
* `vendor` - вендор продукта. Значения могут быть произвольными;
* `product` - название продукта;
* `kind` - вид продукта;
* `release_type` - тип редакции.

Если предыдущей версии не было, то значение поля `previous_version` будет `null`.

{% endcut %}

{#top}

{% cut "Возврат к предыдущей версии" %}

```
POST /system_management/change_sys_root
```

**Json-тело запроса:**

```json5
{

   "version": {
      "major": "integer",
      "minor": "integer",
      "build": "integer",
      "timestamp": "integer",
      "vendor": "string",
      "product": "UTM" | "CC",
      "kind": "FSTEK" | "NGFW-FSTEK" | "VPP" | "STANDARD" | "BPF",
      "release_type": "release" | "beta" | "devel"
   }
}
```

* `major` - мажорный номер версии;
* `minor` - минорный номер версии;
* `build` - номер сборки;
* `timestamp` - время сборки версии в формате UNIX timestamp;
* `vendor` - вендор продукта. Значения могут быть произвольными;
* `product` - название продукта;
* `kind` - вид продукта;
* `release_type` - тип редакции.

**Ответ на успешный запрос:** 200 OK

{% endcut %}

