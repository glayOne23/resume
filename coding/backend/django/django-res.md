# Backend Vers
# 1. Begining Phase 1
1. mkdir project_name
2. cd project_name
3. mkdir frontend && mkdir backend
4. cd back-end
5. virtualenv env -p python3
6. cara 2 :  python3 -m venv env 
7. activate it with : source env/bin/activate
8. pip install Django==2.2.5
9. pip install djangorestframework && pip install markdown && pip install django-filter
10. mkdir src && cd src
11. django-admin startproject project_name .
# 2. Beginning Phase 2
1. Add 'rest_framework' to your INSTALLED_APPS setting.
    ```python
    INSTALLED_APPS = [
        ...
        'rest_framework',
    ]
    ```
2. If you're intending to use the browsable API you'll probably also want to add REST framework's login and logout views. Add the following to your root urls.py file :
    ```python
    ...
    from django.urls import path, include

    urlpatterns = [
        ...
        path('api-auth/', include('rest_framework.urls'))
    ]
    ```
3. Any global settings for a REST framework API are kept in a single configuration dictionary named REST_FRAMEWORK. Start off by adding the following to your settings.py module:
    ```python
    REST_FRAMEWORK = {
        # Use Django's standard `django.contrib.auth` permissions,
        # or allow read-only access for unauthenticated users.
        'DEFAULT_PERMISSION_CLASSES': [
            'rest_framework.permissions.DjangoModelPermissionsOrAnonReadOnly'
        ]
    }
    ```
# 3. Begining Phase 3
1. python manage.py startapp articles
2. add in INSTALLED_APPS :
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'articles'
]
```
3. in articles/models
```python
class Article(models.Model) :
    title = models.CharField(max_length=120)
    content = models.TextField()

    def __str__(self):
        return self.title
```
4. python manage.py makemigrations && python manage.py migrate
5. in articles/admin.py : `admin.site.register(Article)`
6. python manage.py createsuperuser
# 4. Serializer,  Generic Views and Url
-  Serializer is basically just a way converting JSON data into a model
- Steps :
1. make file in `articles/api/__init__.py`
2. make file in `articles/api/serializers.py`
---
- Serializer part
3. in serializers.py :
    ```python
    from rest_framework import serializers

    from articles.models import Article

    class ArticleSerializer(serializers.ModelSerializer):
        class Meta:
            model = Article
            fields = ('title','content' )
    ```
---
- Generic part
4. in articles/api/views.py :
    ```python
    from rest_framework.generics import ListAPIView, RetrieveAPIView

    from articles.models import Article

    from .serializers import ArticleSerializer

    class ArticleListView(ListAPIView):
        queryset = Article.objects.all()
        serializer_class = ArticleSerializer

    class ArticleDetailView(RetrieveAPIView):
        queryset = Article.objects.all()
        serializer_class = ArticleSerializer
    ```
---
- Url part
5. in articles/api/urls.py :
    ```python
    from django.urls import path

    from .views import ArticleListView, ArticleDetailView

    urlpatterns = [
        path('', ArticleListView.as_view()),
        path('<pk>', ArticleDetailView.as_view())
    ]
    ```
6. import articles/api/urls.py to django_res/urls.py:
    ```python
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api-auth/', include('rest_framework.urls')),
        path('api/', include('articles.api.urls')),
    ]
    ```
# 5. Generic Views and Url for Create Update Delete
- Perlu diingat disini masih pake post, jadi manggilnya pake `<pk>/update/` atau `<pk>/delete/`, kan klo method form nya pake put atau destroy nggk perlu pakai itu
1. in articles/api/view.py :
```python
from rest_framework.generics import (
    ListAPIView, 
    RetrieveAPIView,
    CreateAPIView,
    UpdateAPIView,
    DestroyAPIView
    )

from articles.models import Article
from .serializers import ArticleSerializer

class ArticleListView(ListAPIView):
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer

class ArticleDetailView(RetrieveAPIView):
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer

class ArticleCreateView(CreateAPIView):
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer

class ArticleUpdateView(UpdateAPIView):
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer

class ArticleDeleteView(DestroyAPIView):
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer
```
2. in articles/api/urls.py
```python
from django.urls import path

from .views import (
    ArticleListView, 
    ArticleDetailView,
    ArticleCreateView,
    ArticleUpdateView,
    ArticleDeleteView,
    )

urlpatterns = [
    path('', ArticleListView.as_view()),
    path('create/', ArticleCreateView.as_view()),
    path('<pk>', ArticleDetailView.as_view()),
    path('<pk>/update/', ArticleUpdateView.as_view()),
    path('<pk>/delete/', ArticleDeleteView.as_view())
]
```
# 6. ViewSets (automatically bikin CRUD)
1. in articles/api/views.py
    ```python
    from rest_framework import viewsets

    from articles.models import Article
    from .serializers import ArticleSerializer

    class ArticleViewSet(viewsets.ModelViewSet):
        """
        A viewset for viewing and editing article instances.
        """
        serializer_class = ArticleSerializer
        queryset = Article.objects.all()
    ```
2. in articles/api/urls.py
    ```python
    from articles.api.views import ArticleViewSet
    from rest_framework.routers import DefaultRouter

    router = DefaultRouter()
    router.register(r'', ArticleViewSet, basename='article')
    urlpatterns = router.urls
    ```
# 7. Rest AUTH Django
- installation and configuration steps :
1. pip install django-rest-auth
2. Add rest_auth app to INSTALLED_APPS in your django settings.py:
    ```python
    INSTALLED_APPS = (
        ...,
        'rest_framework',
        'rest_framework.authtoken',
        ...,
        'rest_auth'
    )
    ```
3. in django_res/urls.py, add this :
    ```python
    urlpatterns = [
        ...,
        path('rest-auth/', include('rest_auth.urls'))
    ]
    ```
4. python manage.py migrate
---
- add registration 
5. If you want to enable standard registration process you will need to install django-allauth by using :  `pip install django-rest-auth[with_social]`
6. Add django.contrib.sites, allauth, allauth.account and rest_auth.registration apps to INSTALLED_APPS in your django settings.py :
7. Add SITE_ID = 1 to your django settings.py :
    ```python
    INSTALLED_APPS = (
        ...,
        'django.contrib.sites',
        'allauth',
        'allauth.account',
        'rest_auth.registration',
    )

    SITE_ID = 1
    ```
8. Add rest_auth.registration urls:
    ```python
    urlpatterns = [
        ...,
        path('rest-auth/', include('rest_auth.urls')),
        path('rest-auth/registration/', include('rest_auth.registration.urls'))
    ]
    ```
---
- add social  (look documentation for further)
9. in setting.py
    ```python
    INSTALLED_APPS = (
        ...,
        'allauth.socialaccount',

    )
    ```
---
10 . `python manage.py migrate`

ERROR WITH CONNECTION REFUSED

# 8. Connecting Django to XAMPP in Ubuntu
- sudo apt install libmysqlclient-dev
- sudo apt install python3-dev
- pip install mysqlclient
- kode di setting.py :
    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'django_connect_trial',
            'USER': 'root',
            'PASSWORD': '',
            'HOST': '127.0.0.1',
            'PORT': '3306',
            'OPTIONS': {
                'init_command': "SET sql_mode='STRICT_TRANS_TABLES'"
            }    
        }
    }
    ```
# 9. Token Django
1. in settings.py :
    ```python
    REST_FRAMEWORK = {
        'DEFAULT_AUTHENTICATION_CLASSES': [
            'rest_framework.authetication.TokenAuthentication'
        ]
    }
    ```
2. in settings.py is `INSTALLED_APPS` add :
    ```python
    INSTALLED_APPS = [
        ...
        'rest_framework.authtoken',
    ]
    ```
3. python manage.py migrate
- Biar bisa pas login mucul token :
4. di urls.py pusat :
    ```python
    ...
    from rest_framework.authtoken.views import obtain_auth_token
    ...
    path('api/token/', obtain_auth_token)
    ```