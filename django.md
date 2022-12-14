`mkdir django-app` - создаем новую папку
и далее создаем виртуальную среду
`python 3 -m venv .venv` - точка обозначает, что это скрытая директория

В джанго-проекте может быть десятки приложений, они называются applications

`mv исзодный файл/папка переимнованный фйл/папка` - вот так можно перименовать: mv - move
`mv файл/директория файла нужная директория файла` - `mv test_project ../` - перенос в другую директория, пример: перенос в директорию выше



заходим в папку .venv (или не заходим) `cd .venv`
`source .venv/bin/activate` или `source bin/activate` - активируем виртуальную среду

После активации виртуальной среды мы можем писать в питон-консоли.
пример
import sys
sys.prefix

`exit()` - выход из консоли питона

`deactivate` - выход из виртуального окружения

`rm файл` - удаление файла или папки, но лучше так не делать, тк удаляет навсегда

`pip(3) freeze` - находясь в виртуальной среде, мы можем проверить установленные пакеты через pip или pip3

`pip install нужный пакет` - можно доустановить необходимые пакеты

например, pip install django

`стрелочка вверх` - дублируем последнюю команду

СОЗДАНИЕ ДЖАНГО-ПРОЕКТА
`django-admin startproject имя_проекта`

важно находиться в виртуальной среде и нужной нам директории (django_test)

там появится папка, в ней py-файл и папка с таким же названием
там будет файл __init__.py - этот файл превращает ее из обычной папки в модуль, который мы можем импортировать в питоне, как обычную библиотеку или файл

`nano` - команда, открывающая текстовый редактор
`ctrl+x` - выход из редактора (именно ctrl а не cmd!)

Обычно мы редактируем файлы urls.py, settings.py

`nano t tab / t tab / u tab` - `nano test_app/test_app/urls.py` - открываем в нано юрлс
URLS.py - это центральный файл, который в зависимости от запросов выполняет разные юрл

в списке urlpatterns мы пока что увидим только админ. Это админка, которая создается по умолчанию.

СОЗДАНИЕ ПРИЛОЖЕНИЯ
`django-admin startspp название_приложения`

в папке posts:
views.py - это обработчики как во фласке
models.py - пишем классы (товар, пользователь и тд)

Когда создали проект, можно использовать не `django-admin` , а `python manage.py + команда`,
например `python manage.py runserver` - для запуска сервера

Как только запускаем сервер, появляется база данных db.sqlite3

В <b>settings</b> указан путь к этой бд в разделе database

<b>Миграции</b>
Нужны для того, чтобы накатывать и откатывать изменения в бд, механизм для отслеживания изменений.
`python manage.py migrate` - применяем миграцию

<b>НАСТРОЙКА АДМИН-ПАНЕЛИ</b>
1. `адрес/admin` - адрес для попадания в админ-панель
2. создание суперпользователя:
`python manage.py createsuperuser` - придумываем логин-пароль, почту указывать необязательно

`python manage.py shell` - это своя собственная джанго-консоль

СОЗДАНИЕ ПРИЛОЖЕНИЯ ПРОДОЛЖЕНИЕ
`python manage.py startapp my_app`
появляется каталог в корне проекта
если в папке есть файл <b>__init__.py</b> - это значит, что питон воспринимает папку как пакет

<b>APPS</b> - это не приложениЯ, а скоращенное название от APPlication settingS
Там хранится файл конфигурации приложения, он называется так же, как и наше приложение my_app

3. В <b>settings.py</b> -> <b>installed apps</b> прописываем
`my_app.apps.MyAppConfig`

4. создание `urls.py`
у каждого приложения должен быть свой urls.py, поэтому создаем его вручную в папке myapp
добавляем include в эту строчку:
`from django.urls import include, path`

Две кавычки в path urlpatterns = роуту во Фласке "/"

5. в urlpatterns (основном) в файле urls.py дописываем следующее:
`path('', include("my_app.urls"))`

6. в urls myapp
пишем
`from django.urls import path`
`from .views import index`
`urlpatterns = [
                path('', index)
                ]
                `
 Сначала указываем путь, только потом пишем функцию
 
 7. добавляем логику во views.py
views.py:

`from django.http import Httpresponse`

и пишем функцию:
`def index(request):
   return HttpResponse("welcome!")
   `
<b>СОЗДАНИЕ МОДЕЛИ</b>

1. заходим в models.py
2. модель - это класс без init и тд, аналогично sqlalchemy во Flask

`Class Post(models.Model):
  text = models.TextField()`
нужно отнаследоваться от класса models.Model

3. Готовим к миграции

в консоли пишем:
`python manage.py makemigrations my_app`
лучше указывать название приложения, т.к. приложений может быть очень много

4. регистрируем модель в my_app/admin.py - чтобы был доступ к модели из админки в браузере
пишем
`from .models import Post`
а потом
`admin.site.register(Post)

5. Преобразовываем модель (в models.py)

`Class Post(models.Model):
  title = models.CharField(max_length=20, default="no name")
  text = models.TextField()
  и тут же прописываем вот эту штуку:
  def __str__(self):
    return self.title`
метод __str__ - будет возвращать то что как бы на print

6. и не забываем сделать миграцию
`python manage.py makemigrations my_app`

7. Теперь правим вид страницы. Идем в my_app/views.py

прописываем 
`from django.views.generic import ListView, TemplateView`

`from .models import Post`

и пишем вьюху:

class HomePageView(ListView):
    model = Post
    template_name = "home.html"
    
8. Идем в my_app/urls.py

импортируем наш homepageview:

`from .views import HomePageBiew`

и добавляем путь в URLPATTERNS

`path('', HomePageView.as_view(), name="home")`

имя должно совпадать!!!! с названием html страницы

9. и теперь создаем шаблон templates. создаем папку templates и там home.html
10. 

