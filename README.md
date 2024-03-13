---

# Запуск Flask приложения на виртуальном хостинге

## Введение
Данный гайд описывает процесс настройки и запуска Flask приложения на виртуальном хостинге. Пример будет основан на хостинге beget.ru, но большинство шагов будет аналогично для других хостингов.

## Подготовка
1. Подключитесь к своему аккаунту на хостинге через SSH:
   ```bash
   ssh username@your_domain
   ```
   В Windows можно использовать терминал, Putty или MobaXterm.

2. Если на вашем хостинге установлен Docker, перейдите в контейнер:
   ```bash
   ssh localhost -p222
   ```

## Установка Flask
1. Обновите pip:
   ```bash
   python3 -m pip install --upgrade pip
   ```

2. Установите Flask:
   ```bash
   python3 -m pip install flask
   ```

## Создание необходимых файлов и директорий
1. Создайте структуру директорий и файлов:
   ```
   .
   └── flask
       ├── HelloFlask
       │   └── __init__.py
       ├── .htaccess
       ├── passenger_wsgi.py
       └── tmp
   ```
   Используйте команды:
   ```bash
   mkdir HelloFlask tmp
   touch .htaccess
   touch passenger_wsgi.py
   ```

2. Настройте файл `.htaccess`:
   - Включите Passenger:
     ```
     PassengerEnabled On
     ```
   - Укажите путь к интерпретатору Python:
     ```
     PassengerPython /home/a/username/.local/bin/python3
     ```

3. Настройте `passenger_wsgi.py`:
   ```python
   import sys, os

   sys.path.append('/home/a/username/your_domain/HelloFlask')
   sys.path.append('/home/a/username/.local/bin/flask')

   from HelloFlask import app as application
   ```

4. Создайте `HelloFlask/__init__.py` с базовым Flask приложением:
   ```python
   from flask import Flask
   app = Flask(__name__)

   @app.route('/')
   def hello_world():
       return 'Hello Flask!'

   if __name__ == '__main__':
       app.run()
   ```

5. Вернитесь в корневую директорию и создайте символическую ссылку `public` на `public_html`:
   ```bash
   ln -s public_html public
   ```

6. Для перезагрузки приложения используйте:
   ```bash
   touch tmp/restart.txt
   ```

## Дополнительно: Flask API для сайта на PHP
Если у вас есть сайт на PHP и нужно добавить REST API на Flask:

1. Создайте директорию `flaskapi` на одном уровне с `public_html`.

2. Настройте `.htaccess` в `flaskapi` для работы с Flask.

3. Создайте структуру Flask приложения внутри `flaskapi`.

4. Настройте роутинг в Apache Virtual Host для директории `flaskapi`. Это может потребовать обращения в техподдержку хостинга.

## Заключение
После выполнения всех шагов ваше Flask приложение должно быть доступно по адресу вашего сайта. Для внесения изменений в код приложения не забывайте перезагружать его, используя `touch tmp/restart.txt`.

---

Убедитесь, что вы заменяете `username`, `your_domain`, и другие плейсхолдеры на актуальные значения для вашего хостинга и приложения.
