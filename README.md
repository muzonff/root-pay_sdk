__Аннотация__
Есть у меня друг который постоянно заказывает у меня ботов, и в один из дней он попросил добавить платежку root-pay.app , поискал либы и понял что их тупо нет. Сейчас руки дошли сделать полноценную асинхронную библиотеку в которой есть абсолютно все методы из api. 



# Класс RootPayApi()

Класс `RootPayApi` предоставляет методы для работы с API сервиса RootPay.

## Инициализация

```python
api = RootPayApi(api_key: str)
```

`api_key` - это API ключ полученный в телеграмм боте rootpay`а

## Методы

### get_active_methods

```python
active_methods = await api.get_active_methods()
```

Возвращает список активных методов получения платежей:
```python
['QIWI', 'CARD', 'USDT', 'ETH', 'SBP', 'YOOMONEY']
```

### balance

```python
balance = await api.balance()
```

Возвращает баланс кассы:
```python
0
```

### get_payments

```python
payments = await api.get_payments(limit=24)
```
Возвращает список платежей с указанным лимитом. Параметр `limit` задает лимит количества операций для выгрузки (по умолчанию 10):
```json
[
	{'amount': '100', 'created_at': '2024-01-28 22:59', 'expired_at': '2024-01-28 23:09', 'method': 'QIWI', 'session_id': 'pb1zeMvxSM8E', 'status': 'check', 'total_amount': '103.57'}, 
	{'amount': '100', 'created_at': '2024-01-28 22:59', 'expired_at': '2024-01-28 23:09', 'method': 'QIWI', 'session_id': 'sin6MhigFskys', 'status': 'check', 'total_amount': '103.53'},
	{'amount': '100', 'created_at': '2024-01-28 22:58', 'expired_at': '2024-01-28 23:08', 'method': 'QIWI', 'session_id': 'iCE3llJXso552', 'status': 'check', 'total_amount': '104.02'},
	{'amount': '100', 'created_at': '2024-01-28 20:27', 'expired_at': '2024-01-29 20:37', 'method': 'QIWI', 'session_id': 'bjjdXvMqjKGwq', 'status': 'check', 'total_amount': '103.38'},
	{'amount': '110', 'created_at': '2024-01-28 18:26', 'expired_at': '2024-01-29 18:36', 'method': 'QIWI', 'session_id': 'pY4oIt8SszQdXiC', 'status': 'check', 'total_amount': '113.78'},
	{'amount': '110', 'created_at': '2024-01-28 18:25', 'expired_at': '2024-01-29 18:35', 'method': 'CARD', 'session_id': 'WQ7zLrWoY9eM', 'status': 'check', 'total_amount': '114.71'}, 
	{'amount': '110', 'created_at': '2024-01-28 18:23', 'expired_at': '2024-01-29 18:33', 'method': 'CARD', 'session_id': 'UNOL30lFv8', 'status': 'check', 'total_amount': '115.70'}, 
	{'amount': '1100', 'created_at': '2024-01-28 18:23', 'expired_at': '2024-01-29 18:33', 'method': 'card', 'session_id': 'IZZkh1YIXftJoG', 'status': 'check', 'total_amount': '1144.70'}, 
	{'amount': '100', 'created_at': '2024-01-28 18:22', 'expired_at': '2024-01-29 18:32', 'method': 'CARD', 'session_id': 'x6huvpZ3Q6AW', 'status': 'check', 'total_amount': '104.59'}, 
	{'amount': '1100', 'created_at': '2024-01-28 18:22', 'expired_at': '2024-01-29 18:32', 'method': 'card', 'session_id': '5vsTm46rmYqtYRZ', 'status': 'check', 'total_amount': '1144.71'}
]
```
### create_payoff

```python
status = await api.create_payoff(method="USDT", wallet="тут мог быть кошелек, но мне не задонатят")
```

Создает запрос на вывод средств. Возвращает статус операции (удачный (True) или нет (False) вывод). Параметры:

-   `method`: платежная система кошелька вывода (usdt, card, sbp, qiwi)
-   `wallet`: реквизиты кошелька получения

### create_pay_link

```python
payment = await api.create_pay_link(method: str, amount: int, subtitle: str = None, comment: str = None)
```

Создает ссылку для оплаты. Возвращает инстанс класса `Payment`. Параметры:

-   `method`: метод оплаты (можно получить в функции `get_active_methods`)
-   `amount`: целочисленная сумма оплаты
-   `subtitle`: описание платежа, видит покупатель. Не обязательный параметр
-   `comment`: дополнительная информация, любая полезная вам нагрузка. Например: Telegram userid покупателя. Не обязательный параметр



# Класс Payment()

Класс `Payment` предоставляет методы для работы с платежами в сервисе RootPay.

## Инициализация

```python
payment = Payment(session_id: str, api_key: str)
```

`session_id` - это айди платежа, который используется для инициализации класса под конкретный платеж. `api_key` - это API ключ сервиса RootPay. В случае библиотеки в этот класс переходят новые инвойсы, т.е то что вернет функция __create_pay_link__ будет являться экземпляром этого класса 


## Методы

### is_paid

```python
status = await payment.is_paid()
```

Возвращает `bool`, в котором `False` означает, что инвойс не оплачен, а `True` - что оплачен:
```python
True
```

### full_info

```python
info = await payment.full_info()
```

Возвращает полную информацию о платеже в формате:

```json
{
    "amount": "1500",
    "created_at": "2023-02-28 19:36",
    "expired_at": "2023-02-28 19:57",
    "method": "CARD",
    "session_id": "puKHMyu4XxF5",
    "status": "paid",
    "total_amount": "1506.12"
}
```

### get_amount

```python
amount = await payment.get_amount()
```
Возвращает сумму платежа:
```python
100
```

## Переменные из этого класса

### Ссылка на платеж
```python
pay_link = payment.link
```
Вернет ссылку на платеж:
```python
https://root-pay.app/iCE3llJXso552
```
