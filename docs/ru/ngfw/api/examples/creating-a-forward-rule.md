# Создание правила Forward

Задача: создать правило Forward для протокола TCP и отредактировать, указав время действия. \
Для правила нужно создать:  
* Диапазона IP-адресов (192.168.0.1-192.168.0.20);
* Списка адресов в качестве источника (9.9.9.9, 9.9.9.10);
* Время действия (с 09:00 по 18:00, с понедельника по пятницу).

**1\.** Авторизуйте администратора: 

```
curl -k -c /tmp/cookie -b /tmp/cookie -X POST https://178.154.205.107:8443/web/auth/login --data '{"login": "логин", "password": "пароль", "recaptcha": "", "rest_path": "/"}'
```

Ответ: статус 200.

**2\.** Создайте объект Диапазон IP-адресов c 192.168.0.1 по 192.168.0.20:

```
curl -k -c /tmp/cookie -b /tmp/cookie -X POST https://178.154.205.107:8443/aliases/ip_ranges --data '{"title": "test ip range", "comment": "test ip range", "start": "192.168.0.1", "end": "192.168.0.20"}'
```

Ответ: статус 200.

Тело ответа:

```
{

    "id": "ip_range.id.2"
}
```

**3\.** Создайте объект Список адресов:

* Создайте объекты "IP-адрес" для IP `9.9.9.9` и повторите действие для IP `9.9.9.10`. Пример: 

```
curl -k -c /tmp/cookie -b /tmp/cookie -X POST https://178.154.205.107:8443/aliases/ip_addresses --data '{"comment": "комментарий", "title": "название", "value": "9.9.9.9"}'
```

Ответ: статус 200.

Тело ответа:

```
{

    "id": "ip.id.3"
}
```

* Создайте объект типа Список адресов указав в `values` полученные в прошлом шаге `id` (например: `ip.id.2` и `ip.id.3`): 

```
curl -k -c /tmp/cookie -b /tmp/cookie -X POST https://178.154.205.107:8443/aliases/lists/addresses --data '{"title": "название", "comment": "комментарий", "values": ["ip.id.2", "ip.id.3"]}'
```

Ответ: статус 200

```
{

    "id": "address_list.id.2"
}
```

**4\.** Создайте объект Время:

```
curl -k -c /tmp/cookie -b /tmp/cookie -X POST https://178.154.205.107:8443/aliases/time_ranges --data '{"title":"Рабочее время","comment":"пн-пт 09:00-18:00","weekdays":[1,2,3,4,5],"start":"09:00","end":"18:00"}'
```

Ответ: статус 200

Тело ответа:

```
{

    "id": "time_range.id.3"
}
```

**5\.** Создайте правило файрвола используя *id* из пунктов 2 и 3:

```
curl -k -c /tmp/cookie -b /tmp/cookie -X POST https://178.154.205.107:8443/firewall/rules/forward --data '{"action": "drop", "comment": "комментарий", "destination_addresses": ["address_list.id.2", "ip_range.id.2"], "destination_ports": ["any"], "incoming_interface": "any", "outgoing_interface": "any", "protocol": "any", "source_addresses": ["any"], "timetable": ["any"], "enabled": true}'
```

Значение `action`: 
* accept - принять пакет; 
* drop - отклонить пакет.

Ответ: статус 200.

Тело ответа:

```
{"action": "drop", "destination_addresses": ["address_list.id.2", "ip_range.id.2"], "destination_ports": ["port.id.1"], "enabled": true, "incoming_interface": "any", "outgoing_interface": "any", "protocol": "protocol.tcp", "source_addresses": ["any"], "timetable": ["any"], "comment": "", "id": 2}

```

**6\.** Отредактируйте созданное правило, указав время действия:

```
curl -k -c /tmp/cookie -b /tmp/cookie -X PUT https://51.250.72.140:8443/firewall/rules/forward/1 --data '{"action": "drop", "comment": "", "destination_addresses": ["any"], "destination_ports": ["port.id.1"], "incoming_interface": "any", "outgoing_interface": "any", "protocol": "protocol.tcp", "source_addresses": ["any"], "timetable": ["time_range.id.1"], "enabled": true, "id": 2}'
```

Ответ: статус 200.

Тело ответа:

```
{"action": "drop", "comment": "", "destination_addresses": ["any"], "destination_ports": ["port.id.1"], "incoming_interface": "any", "outgoing_interface": "any", "protocol": "protocol.tcp", "source_addresses": ["any"], "timetable": ["time_range.id.3"], "enabled": true, "id": 2}

```
