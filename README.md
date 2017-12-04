# rest-burger
## Запуск
1. Установка зависимостей
```bash
pip install -r requirements.txt
```
2. Сборка статки
```bash
./manage.py collectstatic --noinput
```
3. Миграции
```bash
./manage.py migrate
```
4. Запуск gunicorn
```bash
gunicorn application.wsgi
```

## API
### 1. Авторизация
POST 
```
/auth
```
в теле запроса передаем JSON:
```json
{
  "login": "%user_login%",
  "password": "%user_password%"
}
```
В ответе получаем токен
```json
{
  "token": "%auth_token%"
}
```
### 2. Получить меню
GET запрос (передаем полученный токен после авторизации)
```
/get_menu?token=%auth_token%
```
или POST запрос
```
/get_menu
```
в теле запроса передаем JSON с токеном
```json
{
  "token": "%auth_token%"
}
```
В ответ получаем:
```
{
  "%Category1%": {
      "items": {
          %item1_id%: {
            "name": "%item1_name%",
            "price": "%item1_price%",
          },
          %item2_id%: { ... }
      },
      "categories": {
        "%category%" : {...}
      }
  }
}
```
  item_id - id предмета (передаем при создании заказа)
### 3. Сохранение заказа
POST запрос
```
/create_order
```
В теле запроса передаем JSON:
```
{
  "token": "user auth token",
  "items": [1, 4, 5 ...]
}
```
В items передаем список ID покупаемых предметов

## Админка
Админка доступна по URL 
```
/admin
```
Администратор может:
  1. Создавать/удалять/редактировать пользователей, давать права оператора(кассира)/администратора
  2. Создавать/удалять/редактировать рестораны
  3. Создавать/удалять/редактировать города к которым привязаны рестораны
  4. Создавать/удалять/редактировать продаваемые предметы
  5. Изменять статус заказа и просмотривать весь заказ
  6. Создавать/удалять/редактировать категории меню. Категории представляют дерево, в котором можно перемещать узлы (катеории)