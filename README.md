elib
====
### Команды для локального развертывания

#### Создание окружения

    git clone https://github.com/Riliam/elib.git
    cd elib
    virtualenv venv
    . venv/bin/activate
    pip install -r requirements.txt

#### Создание и наполнение базы данных

    sqlite3 db.sqlite3 < create_db.sql
    sqlite3 db.sqlite3 < populate_db.sql
Или

    python create_schema_and_populate_db.py

#### Запуск сервера

    python app.py

### Развернутое приложение на heroku.com
http://floating-beach-6335.herokuapp.com/

### Пароль и логин
admin/admin

### Заметки
1. Получилось ajax-приложение, можно сказать, одностраничное. У меня не было опыта работы с jQuery и Ajax, было интересно попробовать. Многие задачи решил плохо и некрасиво. Наверное, на jQuery одностраничные приложения не делаются или делаются совсем по-другому, потому что у меня было много боли.
2. Недостаточно проработана система оповещания пользователя о событиях. Есть базовые:
    - неверный логин;
    - попытка доступа к функции, доступной только авторизованным пользователям;
    - сообщения ошибок во время проверки книг и авторов при добавлении/редактировании.
Можно добавить сообщения о добавлении/изменении/удалении объектов. Есть ошибка: если попытаться добавить автора к книге с пустым названием, приложение не ругнется, а просто не добавит книгу.
3. Реализовано добавление автора книги, но не книги автора. Во время добавления или редактирования книги можно выбрать из уже добавленных авторов, а можно добавить одного или нескольких новых.
4. Проектирование взаимодейсвтия.
    - 3 группы элементов в основном рабочем окне: надписи(черные, большие), функции редактирования содержимого (синие, мелкий шрифт) и объекты, связанные с другими объектами, например, авторы определенной книги (серые). Эти группы четко различаются и создают смысл;
    - основной процесс работы построен вокруг книги. Есть два способа создания новой записи: первый - создать авторов книги, а потом при создании книги выбрать их в списке, второй - создать книгу и добавить авторов, которые автоматически станут авторами книги;
    - возможно, следовало более явно указать смысл связанных объектов, потому что обозначения для книг авторов и авторов книг слишком похожи. Можно было сделать, например, так:
        `Code complete Автор: Steve McConnell или Mark Lutz. Книги: "Learning Python", "Python Pocket Book"`
    Но тогда возникает задачи множественного числа и разделения запятыми. Поэтому я оставил простой вариант
    - есть возможность переключения просмотра только книг или только авторов.
5. Поиск на основе Ajax - неудачный выбор. При работе на Heroku слишком большой пинг. На локальной машине работает быстро, поэтому я не подумал об этом.
6. Передача id не в url, а в скрытом поле формы - даже не знаю, зачем я это сделал. Просто захотелось попробовать.
7. Валидация данных перед сохранением в базу - самая базовая: проверка на наличие названия книги или имени автора и на ограничения по длинне. Проверок на наличие авторов книги или книг автора нету, чтобы не усложнять взаимодействие.
8. Аутентификация и авторизация - на основе функций работы с хэшами паролей в werkzeug и сниппета login_required в документации к Flask.
9. Была проблема на Heroku отправки на сервер данных из браузера, которые туда еще не поступили. Не придумал ничего лучше, чем добавить в код на сервере, кода данные должны поступить цикл ожидания этих данных с интервалом и максимальным временем ожидания.
9. Использовал официальную документацию Flask, SQLAlchemy, Jinja2, jQuery, WTForms, stackoverflow и туториалы по развертыванию на heroku.
