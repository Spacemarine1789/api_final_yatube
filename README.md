# api_final
### Описание

    В данном проекте реализоавно API на основе Django REST framework. Данный програмный 
интерфейс позволяет выполнять взаимодействие с БД проекта Yatube. API позволяет выполнять 
действия создания, чтения, редактирования и удаления (CRUD) записей в БД для моделей 
Post, Comments, Follow. Так же интерфейс позволяет выполнять GET запросы для модели Group. 
Читать записи из БД для моделей Post, Group, Comment могут могут анонимные пользователи. 
В во всех остальных случаях требуется аутентификация,которая производится с помошью JWT-токена.

### Установка:

1) Запустите терминал и откройте в нем папку, в которую хотите клонировать проект.
2) Клонируйте репозиторий и перейдите в него в командной строке:

```
https://github.com/Spacemarine1789/api_final_yatube.git
```

```
cd api_final_yatube
```

3) Cоздате и активируйте виртуальное окружение:

```
python3 -m venv env
```

```
source env/bin/activate
```

Если вы пользователь **Windows**:

```
source env/Scripts/activate
```

```
python3 -m pip install --upgrade pip
```

4) Установите зависимости из файла **requirements.txt**:

```
pip install -r requirements.txt
```

5) Выполните миграции:

```
python3 manage.py migrate
```

6) Запустите проект:

```
python3 manage.py runserver
```

обратите внимание что запускать проект и выполнять миграции необходимо из дериктории, 
в которой расположен файл **manage.py**.

### Примеры запросов.

1) Получение **JWT-токена**.
Получение токена происходит с помошью **POST** запроса по адресу:

```
/api/v1/jwt/create/

```
При этом в теле запросы вы заполняете логин и пароль:

```
{
  "username": "string",
  "password": "string"
}
```
В результате вы получаете 2 токена:
```
{
  "refresh": "string",
  "access": "string"
}
```
Токен **access** затем передаете в заголоке запросов для аутентификации. 
Токен **refresh** служит для обновления **access** токена. Для обновления необходимо
отправить **POST** запрос по адресу: 
```
/api/v1/jwt/refresh/
```
В теле запроса необходимо указать **refresh** Токен:
```
{
  "refresh": "string"
}
```
В результате вы получите новый **access** токен. 

2) Примеры запросов для модели **Post**:

Запросы для **записей** выполняются по адресам:
```
/api/v1/posts/
```
Для получения списка записей и добавлния новой записи (запросы **GET** и **POST**)
```
/api/v1/posts/{id}/
```
Для получения отдельной записи, ее редактирования и удаления (запросы **GET**, 
**PUT**, **PATCH**, **DELITE**)

При **GET** запросе  для получения списка записей интерфейс выдает следующий ответ:
```
{
  "count": 123,
  "next": "http://api.example.org/accounts/?offset=400&limit=100",
  "previous": "http://api.example.org/accounts/?offset=200&limit=100",
  "results": [
    {
      "id": 0,
      "author": "string",
      "text": "string",
      "pub_date": "2021-10-14T20:41:29.648Z",
      "image": "string",
      "group": 0
    }
  ]
}
```
Обратите внимание при получении списка групп настроена **LimitOffsetPagination**, 
поэтому вы помжете настраивать количество выдаваемых записей с помошью дополнительных 
параметров *offset* и *limit*.

При **GET** запросе к одной записи интерфейс выдает следующий ответ:
```
{
"id": 0,
"author": "string",
"text": "string",
"pub_date": "2019-08-24T14:15:22Z",
"image": "string",
"group": 0
}
```

При **POST** запросе на создание записи в теле запроса указываются параметры *text*
записи, *image* и id группы *group*. При этом только параметр *text* является обязательным:
```
{
"text": "string",
"image": "string",
"group": 0
}
```
При этом в интерфейс выдает следующий ответ:
```
{
"id": 0,
"author": "string",
"text": "string",
"pub_date": "2019-08-24T14:15:22Z",
"image": "string",
"group": 0
}
```

При **PUT** и **PATCH** запросе на изменение записи в теле запроса указываются 
те поля записи которые вы хотите изменить:
```
{
"text": "string",
"image": "string",
"group": 0
}
```
При этом в интерфейс выдает следующий ответ:
```
{
"id": 0,
"author": "string",
"text": "string",
"pub_date": "2019-08-24T14:15:22Z",
"image": "string",
"group": 0
}
```

При **DELITE** запросе в теле ничего не передается. 

3) Примеры запросов для модели **Comment**:

Запросы для **коментариев** выполняются по адресам:
```
/api/v1/posts/{post_id}/comments/
```
Для получения списка коментариев к определнной *записи* и добавлния 
новового коментария (запросы **GET** и **POST**)
```
/api/v1/posts/{post_id}/comments/{id}/
```
Для получения отдельного коментария, его редактирования и удаления (запросы **GET**, 
**PUT**, **PATCH**, **DELITE**)

При **GET** запросе  для получения списка коментариев интерфейс выдает следующий ответ:
```
[
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
]
```
При **GET** запросе к определенному коментарию интерфейс выдает следующий ответ:
```
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
```

При **POST** запросе на создание коментария в теле запроса указывается параметр *text*:
```
{
"text": "string"
}
```
При этом в интерфейс выдает следующий ответ:
```
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
```

При **PUT** и **PATCH** запросе на изменение коментария в теле запроса указываются 
новое значение параметра *text*:
```
{
"text": "string"
}
```
При этом в интерфейс выдает следующий ответ:
```
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
```

При **DELITE** запросе в теле ничего не передается. 

4) Примеры запросов для модели **Group**:

Запросы для **групп** могут быть только **GET**. Они 
выполняются по адресам для получения списка групп:
```
/api/v1/groups/
```
Или для получения определенной группы:
```
/api/v1/groups/{id}/
```
Ответ на **GET** запрос для получения списка групп:
```
[
  {
    "id": 0,
    "title": "string",
    "slug": "string",
    "description": "string"
  }
]
```
Ответ на **GET** запрос для получения отдельной группы:
```
{
  "id": 0,
  "title": "string",
  "slug": "string",
  "description": "string"
}
```

5) Примеры запросов для модели **Follow**:

Запросы для **подписок** могут выполнять только аутентифицированые пользователи.
Интерфейс позволяет выполнять только **GET** и **POST** запросы. Они 
выполняются по адресу:
```
/api/v1/follow/
```
При **GET** запросе интерфейс возвращает список тех, на кого подписан пользователь,
отправивший запрос:
```
[
  {
    "user": "string",
    "following": "string"
  }
]
```
При **POST** запросе интерфейс в теле запроса указывается *username* пользователя, 
на которого хочет подписатся текущий пользователь. 
```
{
"following": "string"
}
```
В результате в ответе получаем:
```
{
  "user": "string",
  "following": "string"
}
```

Здесь приведены примеры только успшпешных запросов и ответов к ним. Подробности об этом 
API, а так же примеры неудачных запросов можно найти в ReDoc документации по адресу:
```
/redoc/
```
