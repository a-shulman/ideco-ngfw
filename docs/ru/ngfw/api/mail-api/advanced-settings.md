# Расширенные настройки

{#top}

{% cut "Получение настроек" %}

`GET /mail/settings/advanced/general`

**Ответ на успешный запрос:**

```json5
{

  "mail_relay": "string",
  "sender_bcc": "string",
  "recipient_bcc": "string",
  "max_mailbox_size": "integer",
  "max_message_size": "integer",
  "autoexpunge_days": "integer"
}
``` 

* `mail_relay` - внешний SMTP-релей. Если не настроен - `null`. Не может быть пустой строкой;
* `sender_bcc` - копировать исходящую почту на электронный адрес. Если не настроен - `null`;
* `recipient_bcc` - копировать входящую почту на электронный адрес. Если не настроен - `null`;
* `max_mailbox_size` - максимальный размер почтового ящика в байтах, минимум - 1 000 000;
* `max_message_size` - максимальный размер письма в байтах, минимум - 1 000 000;
* `autoexpunge_days` - количество дней (возраст письма), после которого автоматически удалять из корзины письма. Значения от 0 до 60 (0 - не удалять).

{% endcut %}

{#top}

{% cut "Изменение настроек" %}

`PUT /mail/settings/advanced/general`

**Json-тело запроса:**

```json5
{

  "mail_relay": "string",
  "sender_bcc": "string",
  "recipient_bcc": "string",
  "max_mailbox_size": "integer",
  "max_message_size": "integer",
  "autoexpunge_days": "integer"
}
```

* `mail_relay` - внешний SMTP-релей. Если не настроен - `null`. Не может быть пустой строкой;
* `sender_bcc` - копировать исходящую почту на электронный адрес. Если не настроен - `null`. Не может быть пустой строкой;
* `recipient_bcc` - копировать входящую почту на электронный адрес. Если не настроен - `null`. Не может быть пустой строкой;
* `max_mailbox_size` - максимальный размер почтового ящика в байтах, минимум - 1 000 000;
* `max_message_size` - максимальный размер письма в байтах, минимум - 1 000 000;
* `autoexpunge_days` - количество дней (возраст письма), после которого автоматически удалять из корзины письма. Возможно указать значения от 0 до 60 (0 - не удалять).

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

## Безопасность

{#top}

{% cut "Получение состояния переключателей" %}

`GET /mail/settings/advanced/security/state`

**Ответ на успешный запрос:**

```json5
{

  "smtpd_sasl_enabled": "boolean",
  "smtpd_tls_only_auth": "boolean",
  "dnsbl_enabled": "boolean",
  "greylisting_enabled": "boolean",
  "secure_encryption": "boolean"
}
```

* `smtpd_sasl_enabled` - поддержка SASL для аутентификации SMTP-клиентов;
* `smtpd_tls_only_auth` - аутентификация только через защищенное соединение (TLS);
* `dnsbl_enabled` - фильтрация по DNSBL для входящей почты;
* `greylisting_enabled` - фильтрация по серым спискам (greylisting) для входящей почты;
* `secure_encryption` - поддержка только безопасных шифров (TLSv1.2 и выше).

{% endcut %}

{#top}

{% cut "Изменение состояния переключателей" %}

`PATCH /mail/settings/advanced/security/state`

**Json-тело запроса (все или некоторые поля):**

```json5
{

  "smtpd_sasl_enabled": "boolean",
  "smtpd_tls_only_auth": "boolean",
  "dnsbl_enabled": "boolean",
  "greylisting_enabled": "boolean",
  "secure_encryption": "boolean"
}
```

* `smtpd_sasl_enabled` - поддержка SASL для аутентификации SMTP-клиентов;
* `smtpd_tls_only_auth` - аутентификация только через защищенное соединение (TLS);
* `dnsbl_enabled` - фильтрация по DNSBL для входящей почты;
* `greylisting_enabled` - фильтрация по серым спискам (greylisting) для входящей почты;
* `secure_encryption` - поддержка только безопасных шифров (TLSv1.2 и выше).

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Получение списка доверенных сетей" %}

`GET /mail/settings/advanced/security/network`

**Ответ на успешный запрос:**

```json5
{

  "postfix_mynetworks": [
    "string"
  ]
}
```

* `postfix_mynetworks` - список доверенных сетей. Если не настроен - пустой массив. 

{% endcut %}

{#top}

{% cut "Установка списка доверенных сетей" %}

`PUT /mail/settings/advanced/security/network`

**Json-тело запроса:**

```json5
{

  "postfix_mynetworks": [
    "string"
  ]
}
```

* `postfix_mynetworks` - список доверенных сетей. Если не настроен - пустой массив. Ни один элемент массива не может быть пустой строкой или `null`.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

## DKIM-подпись

{#top}

{% cut "Получение состояния DKIM" %}

`GET /mail/settings/advanced/dkim/state`

**Ответ на успешный запрос:**

```json5
{

  "opendkim_enabled": "boolean"
}
```

* `opendkim_enabled` - `true`, когда DKIM-подпись включена, `false` - когда выключена.

{% endcut %}

{#top}

{% cut "Установка состояния DKIM" %}

`PUT /mail/settings/advanced/dkim/state`

**Json-тело запроса:**

```json5
{

  "opendkim_enabled": "boolean"
}
```

* `opendkim_enabled` - `true`, когда DKIM-подпись включена, `false` - когда выключена.

**Ответ на успешный запрос:** 200 ОК

{% endcut %}

{#top}

{% cut "Получение ключей" %}

`GET /mail/settings/advanced/dkim`

**Ответ на успешный запрос:**

```json5
[
  {
    "public_key": "string",
    "selector": "string",
    "dkim_domain_status": "mismatch" | "error" | "missing" | "set",
    "domain": "string"
  }
]
```

* `public_key` - публичный ключ;
* `selector` - строка вида `ics._domainkey.<домен>.`;
* `dkim_domain_status` - статус наличия публичного ключа в DNS-записи;
* `domain` - домен.

{% endcut %}

