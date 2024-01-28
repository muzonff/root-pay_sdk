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

Возвращает список активных методов получения платежей.

### balance

```python
balance = await api.balance()
```

Возвращает баланс кассы.

### get_payments

```python
payments = await api.get_payments(limit=24)
```

Возвращает список платежей с указанным лимитом. Параметр `limit` задает лимит количества операций для выгрузки (по умолчанию 10).

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

Возвращает `bool`, в котором `False` означает, что инвойс не оплачен, а `True` - что оплачен.

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
Возвращает сумму платежа.

## Переменные из этого класса

### Ссылка на платеж
```python
pay_link = payment.link
```
Вернет ссылку на платеж 
