# Course Contents
### (0:00:00) 1 - Welcome
### (0:01:14) 2 - Installing to Get Started
### (0:05:02) 3 - Setup your Virtual Environment for Django
### (0:14:39) 4 - Create a Blank Django Project
### (0:18:54) 5 - Setup Your Code Text Editor
### (0:22:27) 6 - Settings
### (0:29:58) 7 - Built-In Components
### (0:33:57) 8 - Your First App Component
### (0:42:34) 9 - Create Product Objects in the Python Shell
### (0:46:18) 10 - New Model Fields
### (0:52:52) 11 - Change a Model
### (0:59:27) 12 - Default Homepage to Custom Homepage
### (1:04:48) 13 - URL Routing and Requests
### (1:10:23) 14 - Django Templates
### (1:16:50) 15 - Django Templating Engine Basics
### (1:24:00) 16 - Include Template Tag
### (1:26:49) 17 - Rendering Context in a Template
### (1:33:21) 18 - For Loop in a Template
### (1:37:01) 19 - Using Conditions in a Template
### (1:42:17) 20 - Template Tags and Filters
### (1:48:59) 21 - Render Data from the Database with a Model
### (1:59:55) 22 - How Django Templates Load with Apps
### (2:06:50) 23 - Django Model Forms
### (2:14:16) 24 - Raw HTML Form
### (2:25:33) 25 - Pure Django Form
### (2:35:30) 26 - Form Widgets
### (2:41:29) 27 - Form Validation Methods
### (2:48:59) 28 - Initial Values for Forms
### (2:51:42) 29 - Dynamic URL Routing
### (2:54:26) 30 - Handle DoesNotExist
### (2:56:24) 31 - Delete and Confirm
### (2:58:24) 32 - View of a List of Database Objects
### (3:00:00) 33 - Dynamic Linking of URLs
### (3:01:17) 34 - Django URLs Reverse
### (3:03:10) 35 - In App URLs and Namespacing
### (3:07:35) 36 - Class Based Views - ListView
### (3:10:45) 37 - Class Based Views - DetailView
### (3:15:38) 38 - Class Based Views - CreateView and UpdateView
### (3:21:23) 39 - Class Based Views - DeleteView
### (3:24:02) 40 - Function Based View to Class Based View
### (3:27:15) 41 - Raw Detail Class Based View
### (3:30:31) 42 - Raw List Class Based View
### (3:33:32) 43 - Raw Create Class Based View
### (3:26:03) 44 - Form Validation on a Post Method
### (3:37:58) 45 - Raw Update Class Based View
### (3:41:13) 46 - Raw Delete Class Based View
### (3:42:17) 47 - Custom Mixin for Class Based Views


---
# 1. Setting Environment
## 1. make virtual env in the folder u want put in it and install django 
1. virtualenv . -p python3
    - jika bikin folder baru virtualenv nama -p python3
2. activate it with : source bin/activate
3. pip install Django==2.2.5
4. to deactivate : deactivate
5. to see any things installed : pip freeze 
# 2. Create a blank project django
- django-admin --> kirain mirip php artisan ternyata bukan
## 1. make a new project
1. create new dir inside venv with name : src
2. django-admin startproject project_name . --> will make 1 dir and 1 file : manage.py and project_name that contains conviguration
3. to run server : python manage.py runserver
# 3. Setting
- all components stored in settings.py/ INSTALLED_APPS = [...here....]
- setting.py --> letak semua setting (db, components, middleware, url)
# 4. Built-In Components (apps that installed default, like admin, auth, etc)
- apps in django == components --> pieces of a bigger django project
## Langkah-langkah :
1. python manage.py migrate --> migrasi default migration yang ada ke db
`
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
`
    - admin page akan langsung ada
2. buat super user di db dgn setting bawaan :  `python manage.py createsuperuser`
# 5. Your First App Component
- membuat components : python manage.py startapp component_name [example: products or blogs]
    - akan membuat dir baru sesuai nama component, exp : product
## 1. Model
- Steps to make new table :
1. open product/model.py --> bwt isi db product
    ```python
    from django.db import models

    class Product(models.Model) :
        title        = models.CharField(max_length=120)
        description  = models.TextField(blank=True, null=True)
        price        = models.DecimalField(decimal_places=2, max_digits=1000000)  
    ```
2. python manage.py makemigrations --> untuk bwt migration dari model
3. python manage.py migrate
4. pake step 2 dan 3 setiap ada perubahan pada model
- jika ada tambahan coloumn, valuenya gimana jika udah ada data sebelumnya?bisa dibuat default value atau null
    ```python
    class Product(models.Model) :
        title        = models.CharField(max_length=120)
        description  = models.TextField(blank=True, null=True)
        price        = models.DecimalField(decimal_places=2, max_digits=1000000)  
        summary      = models.TextField(default='this is cool') 
    ```
- Menambahkan products ke admin bawaan:
1. di products/admin.py , tambahkan :
    ```python
    from .models import Product

    admin.site.register(Product)
    ```
## 2. App
1. di setting.py / INSTALLED_APPS[ ], tambahkan products,
# 6. Using Python Shell to Create Product Objects
1. python manage.py shell
2. in shell
```python
# importing Product
>>> from products.models import Product
# see all Product objects
>>> Product.objects.all()
# create new Object
>>> Product.objects.create(title="Sabun", description="blabla", price="2000", summary="blabala")
```
# 7. Custom Homepage
- Steps :
1. make new app and add it in setting.py: python manage.py startapp pages
2. in pages/view.py -->place that handle your various web pages (mirip controller gtu) :
    ```python
    from django.shortcuts import render
    from django.http import HttpResponse

    def home_view(*args, *kwargs):
        return HttpResponse("<h1>Halaman Home</h1>")    
    ```
3. in trydjango/urls.py --> place that handle all url :
    ```python
    from django.contrib import admin
    from django.urls import path

    from pages.views import home_view

    urlpatterns = [
        path('admin/', admin.site.urls),
        path("", home_view, name="home"),
    ]

    ```
# 8. Url Routing and Requests
- Guna *args dan **kwargs :
    - *args --> nangkap request
    - **kwargs --> nanngkap context
    ```python
    def home_view(*args, **kwargs):
    print(args)
    return HttpResponse("<h1>Halaman Home</h1>")


    # result
    # (<WSGIRequest: GET '/'>,) {}
    ```
- Membuat view seperti di Laravel :
1. di pages/views.py :
    ```python
    def home_view(request, *args, **kwargs):
        return render(request, "home.html", {})
    ```
2. buat dir baru di src/ dengan nama templates --> buat handle semua view
3. buat src/templates terdetek, di trydjango/settings.py/TEMPLATES =[{...}] :
    ```python
    'DIRS': [os.path.join(BASE_DIR, "templates")],
    ```
4. make src/templates/home.html untuk view home:
    ```html
    <h1>Home Page</h1>
    ```
# 9. Django Templating Engine Basics
1. make a root page, page that all pages are gonna borrow from --> src/templates/base.html :
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Try Django</title>
    </head>
    <body>
        <h1>This is navbar</h1>

        {% block content %}
            place to be replaced
        {% endblock%}
    </body>
    </html>
    ```
2. in home.html, extends base.html and use block :
    ```html
    {% extends 'base.html' %}

    {% block content %}
        {{request.user}}
        <h1>Home Pages</h1>
    {% endblock %}
    ```
# 10. Include Template Tag
1. make src/templates/navbar.html :
    ```html
    <nav>
        <ul>
            <li>Home</li>
            <li>Info</li>
        </ul>
    </nav>
    ```
2. in base.html, include navbar.html :
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Try Django</title>
    </head>
    <body>
        {% include 'navbar.html' %}

        {% block content %}
            hai
        {% endblock%}
    </body>
    </html>
    ```
# 11. Rendering Context in a Template
- Context tu kaya lempar tangkap data dri model -> controller -> view di laravel
1. di pages/views.py :
    ```python
    def home_view(request, *args, **kwargs):
        my_context = {
            "name" : "tangkap context",
            "my_list" : [123,456,789],
        }
        return render(request, "home.html", my_context)
    ```
2. di templates/home.html :
    ```html
    {% extends 'base.html' %}

    {% block content %}
        {{request.user}}
        <h1>Home Pages</h1>
        <p> {{ name }} </p>
        <p> {{my_list}} </p>
    {% endblock %}
    ```
# 12. For Loop in Template
- di home.html :
    ```html
    <ul>
        {% for my_sub_item in my_list %}
            <li> {{forloop.counter}}. {{my_sub_item}} </li>
            <!-- forloop.counter buat automatically add number 1,2,3,4 -->
        {% endfor%}
    </ul>
    ```
# 13. Using Conditions in a Template
- Best pratice nya emang di view(controller klo di laravel), tapi klo pengen condition di template caranya gini :
    ```html
    {% for my_sub_item in my_list %}
        {% if my_sub_item == 123 %}
            <li> {{my_sub_item |add:22}} </li>
        {% else %}    
            <li> {{forloop.counter}}. {{my_sub_item}} </li>
        {% endif %}
    {% endfor%}
    ```
# 14. Render Data from the Database with a Model
1. di product/view.py :
    ```python
    from django.shortcuts import render

    from .models import Product

    # Create your views here.
    def product_detail_view(request) :
        obj = Product.objects.get(id=1)
        context = {
            "obj" : obj
        } 
        return render(request, "products/detail.html", context)
    ```
2. buat templates/products/detail.html , isi dengan :
    ```html
    {% extends "base.html" %}

    {% block content %}

    <h2>{{obj.title}} </h2>
    <h2> 
        {% if description %}
            {{obj.description}} 
        {% else %}
            Tidak ada deskripsi
        {% endif %}
    </h2>
    <h2> {{obj.price}} </h2>
    <h2> {{obj.summary}} </h2>
    {% endblock %}
    ```
3. di trydjango/urls.py :
    ```python
    from django.contrib import admin
    from django.urls import path

    from pages.views import home_view, info_view
    from products.views import product_detail_view

    urlpatterns = [
        path('admin/', admin.site.urls),
        path("", home_view, name="home"),
        path("info/", info_view, name="info"),
        path("product/", product_detail_view)
    ]
    ```
# 15. How Django Templates Load with Apps
- Kan sebelumnya kita naro templates product di src/templates/products, nah sebenarnya itu cara buat ngeoveride, dan src/templates/ tu best practicenya buat layout inti aja, template untuk tiap app baiknya di taro di src/nama_app/templates/nama_app
# 16. Django Model Forms
cara django singkat 
# 17. Raw HTML Form
cara dari raw html
# 18. Pure Django Form
cara django panjang, bedanya cma di view nya dibanding pake Model Forms, panjangan ini tapi intinya sama
# 19. Form Widgets

# 20. Form Validation Methods

# 21. Initial Values for Forms

# 22. Dynamic URL Routing

# 23. Handle DoesNotExist

# 24. Delete and Confirm










