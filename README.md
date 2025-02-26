# ktum

# Криптовалютный Трекер
Этот проект представляет собой простой сервис для отслеживания цен на криптовалюты через REST API. Пользователи могут получать актуальные данные о курсах различных криптовалют, используя запросы к API.

## Функционал

- Получение актуальных курсов криптовалют.
- Возможность получения истории изменения курса за определённый период.
- Поддержка нескольких источников данных.

## Установка

Для установки и запуска приложения выполните следующие шаги:

1. Установите необходимые зависимости:
   ```bash
   pip install -r requirements.txt
   ```

2. Запустите приложение:
   ```bash
   python app.py
   ```

3. Откройте браузер и перейдите по адресу `http://localhost:5000` для доступа к API.

## Использование

API предоставляет несколько конечных точек:

### Получить текущий курс криптовалют

```
GET /api/currencies
```

Пример ответа:
```json
{
  "BTC": {
    "USD": 12345.67,
    "EUR": 9876.54
  },
  "ETH": {
    "USD": 3456.78,
    "EUR": 2345.67
  }
}
```

### Получить историю изменений курса

```
GET /api/history?currency=BTC&start_date=2023-01-01&end_date=2023-12-31
```

Пример ответа:
```json
[
  {
    "date": "2023-01-01",
    "price": 10000.00
  },
  {
    "date": "2023-02-01",
    "price": 12000.00
  }
]
```

## Лицензия

Этот проект распространяется под лицензией MIT. См. файл `LICENSE` для подробной информации.

---

**Автор:** Ваше Имя  
**Контакт:** ваш_почтовый_адрес@example.com
```

#### Файл `app.py`
```python
from flask import Flask, jsonify
import requests
from datetime import datetime

app = Flask(__name__)

@app.route('/api/currencies')
def get_current_prices():
    response = requests.get('https://example-api.com/current-prices').json()
    return jsonify(response)

@app.route('/api/history')
def get_historical_data():
    currency = request.args.get('currency', 'BTC')
    start_date = request.args.get('start_date', '2023-01-01')
    end_date = request.args.get('end_date', '2023-12-31')
    
    # Логика получения исторических данных
    historical_data = []
    for date in range(start_date, end_date):
        price = get_price_from_db(currency, date)
        historical_data.append({
            'date': date,
            'price': price
        })
        
    return jsonify(historical_data)

if __name__ == '__main__':
    app.run(debug=True)
```

#### Файл `requirements.txt`
```
requests==2.28.1
Flask==2.2.2
```

#### Тестовый файл `test_app.py`
```python
import unittest
from app import app

class TestApp(unittest.TestCase):

    def setUp(self):
        self.app = app.test_client()

    def test_get_current_prices(self):
        response = self.app.get('/api/currencies')
        self.assertEqual(response.status_code, 200)
        data = response.json
        self.assertIn('BTC', data)
        self.assertIn('USD', data['BTC'])

    def test_get_historical_data(self):
        response = self.app.get('/api/history?currency=BTC&start_date=2023-01-01&end_date=2023-02-01')
        self.assertEqual(response.status_code, 200)
        data = response.json
