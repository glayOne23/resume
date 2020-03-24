# 2. Serializations
- Serializer mekanismenya mirip form
- in shell :
    ```python
    >>> from blog.api.serializers import BlogSerializer
    >>> from blog.models import Blog
    >>> b = BlogSerializer(data = {"title":"Website UI", "content":"Isi Website UI" })
    >>> b.is_valid()
    True
    >>> b.errors
    {}
    >>> b.validated_data
    OrderedDict([('title', 'Website UI'), ('content', 'Isi Website UI')])
    >>> b.save()
    <Blog: Website UI>
    >>> b.data
    {'id': 3, 'title': 'Website UI', 'content': 'Isi Website UI', 'author': None}
    # bisa gini jika dah login, tapi ini kan belum
    >>> q.save(author = request.user)
    # so caranya gini
    >>> from blog.models import Author
    >>> a = Author.objects.get(id=1)
    >>> a.name
    'jajang'
    >>> b1 = Blog.objects.get(id=3)
    >>> b1.title
    'Website UI'
    # buat update, harus kasih instancenya in this case is b1
    >>> b_s = BlogSerializer(b1, data={"author":a.id})
    # di django ada 2 request buat update : PUT dan PATCH, PUT buat update semua PATCH buat update sebagian, defaultnya itu PUT makanya error minta semua diisi
    >>> b_s.is_valid()
    False
    >>> b_s.errors
    {'title': [ErrorDetail(string='This field is required.', code='required')], 'content': [ErrorDetail(string='This field is required.', code='required')]}
    # Nah disini dikasih partial=True nunjukin klo request yang mau dipakai itu PATCH
    >>> b_s = BlogSerializer(b1, data={"author":a.id}, partial=True)
    >>> b_s.is_valid()
    True
    >>> b_s.save()
    <Blog: Website UI>
    >>> b_s.data
    {'id': 3, 'title': 'Website UI', 'content': 'Isi Website UI', 'author': 1}
    # menjadikan hasil dalam format json
    >>> blogs = Blog.objects.all()
    >>> all_serialized = BlogSerializer(blogs, many=True)
    >>> all_serialized.data
    [OrderedDict([('id', 1), ('title', 'Website UGM'), ('content', 'Isi webiste UGM'), ('author', 1)]), OrderedDict([('id', 2), ('title', 'Website UNNES'), ('content', 'Isi website UNS'), ('author', 2)]), OrderedDict([('id', 3), ('title', 'Website UI'), ('content', 'Isi Website UI'), ('author', 1)])]
    >>> serialized = BlogSerializer(blogs[0])
    >>> serialized.data
    {'id': 1, 'title': 'Website UGM', 'content': 'Isi webiste UGM', 'author': 1}
    ```
- Points
    1. (data = () )
    2. .is_valid()
    3. .errors
    4. .validated_data --> data yang udah valid
    5. .save() --> bakal manggil method create()
    6. .data --> return data in dic format
    7. NamaSerializer(queryset, many=True) --> naruh semua dalam format json
    8. NamaSerializer(queryset[0]) --> naruh 1 dalam format json

# 3. API using Serializers and Functions Based Views
- Guna : biar lebih paham cara kerja dari awalnya
## Untuk GET, POST, PUT, PATCH, DELETE
1. di urls.py
    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('api/v1/authors/', include('author.api.urls')),
    ]
    ```
2. di author.api.urls
    ```python
    from django.urls import path, include
    from author.api.views import author, author_details


    urlpatterns = [
        path('', author), 
        path('<int:id>/', author_details)
    ]
    ```
3. di author.api.serializers
    ```python
    from rest_framework import serializers
    from blog.models import Author

    class AuthorSerializer(serializers.ModelSerializer):
        class Meta:
            model = Author
            fields = (
                '__all__'
            )   
    ```
4. di author.api.views
    ```python
    from author.api.serializers import AuthorSerializer
    from blog.models import Author
    from django.http import JsonResponse
    from rest_framework.parsers import JSONParser
    from django.views.decorators.csrf import csrf_exempt
    from django.http import HttpResponse

    @csrf_exempt
    def author(request):
        if request.method == "GET":
            queryset = Author.objects.all()
            serializer = AuthorSerializer(queryset, many=True)
            return JsonResponse(serializer.data, safe=False)

        # untuk POST kita harus masukinnya pake json format
        elif request.method == "POST":
            json_parser = JSONParser()
            data = json_parser.parse(request)
            serializer = AuthorSerializer(data=data)
            if serializer.is_valid():
                serializer.save()
                                # 201 untuk creating and object
                return JsonResponse(serializer.data, status=201)
            return JsonResponse(serializer.errors, status=400)

    @csrf_exempt
    def author_details(request, id):
        try:
            instance = Author.objects.get(id=id)    
        except Author.DoesNotExist as e :
            return JsonResponse(
                {"error" : "object not found."}, status=404
            )

        if request.method == "GET":
            serializer = AuthorSerializer(instance)
            return JsonResponse(serializer.data)

        elif request.method == "PUT":
            json_parser = JSONParser()
            data = json_parser.parse(request)
            serializer = AuthorSerializer(instance, data=data)
            if serializer.is_valid():
                serializer.save()
                return JsonResponse(serializer.data, status=200)
            return JsonResponse(serializer.errors, status=400)

        elif request.method == "DELETE":
            instance.delete()
            return HttpResponse(status=204)
    ```

# 4. REST API Using Class based View
- Beda JsonResponse ama Response :  syntax doang, tapi klo Responese ntar enak tampilan bisa bagus
1. step 1,3 tetep, step 2,4 beda
2. di author.api.urls :
    ```python
    from django.urls import path, include
    from author.api.views import (
        author, 
        author_details, 
        AuthorAPIView, 
        AuthorDetailView
        )
    urlpatterns = [
        # path('', author), 
        # path('<int:id>/', author_details)
        path('',AuthorAPIView.as_view()), 
        path('<int:id>/', AuthorDetailView.as_view())
    ]
    ```
4. di author.api.views
    ```python
    from author.api.serializers import AuthorSerializer
    from blog.models import Author
    from django.http import JsonResponse
    from rest_framework.parsers import JSONParser
    from django.views.decorators.csrf import csrf_exempt
    from django.http import HttpResponse
    from rest_framework.views import APIView
    from rest_framework.response import Response


    class AuthorAPIView(APIView):

        # this request object is modified by drf to satisfy requirment by drf
        def get(self, request):
            queryset = Author.objects.all()
            serializer = AuthorSerializer(queryset, many=True)
            return Response(serializer.data)
        
        def post(self, request):
            # print(request.data)
            # {'name': 'jajang', 'password': '123'}
            data = request.data
            serializer = AuthorSerializer(data=data)
            if serializer.is_valid():
                serializer.save()
                return Response(serializer.data, status=201)
            return Response(serializer.errors, status=400)

    class AuthorDetailView(APIView):
        def get_object(self, id):
            try:
                return Author.objects.get(id=id)    
            except Author.DoesNotExist as e :
                return Response(
                    {"error" : "object not found."}, status=404
                )
        
        def get(self, request, id=None):
            # print(id)
            # 2
            instance = self.get_object(id)
            serializer = AuthorSerializer(instance)
            return Response(serializer.data)

        def put(self, request, id=None):
            data = request.data
            instance = self.get_object(id)
            serializer = AuthorSerializer(instance, data=data)
            if serializer.is_valid():
                serializer.save()
                return Response(serializer.data, status=200)
            return Response(serializer.errors, status=400)

        def delete(self, request, id=None):
            instance = self.get_object(id)
            instance.delete()
            return HttpResponse(status=204)
    ```

# 5. Model Mixins and Generic API Views
1. di author.api.urls:
- defaultnya pakenya `pk` tapi klo pake `id` harus ngisi `lookup_field`
    ```python
    from django.urls import path, include
    from author.api.views import (
        author, 
        author_details, 
        AuthorAPIView, 
        AuthorDetailView,
        AuthorGenericAPIView
        )

    urlpatterns = [
        # path('', author), 
        # path('<int:id>/', author_details)
        
        # path('',AuthorAPIView.as_view()), 
        # path('<int:id>/', AuthorDetailView.as_view()),

        path('', AuthorGenericAPIView.as_view()),
        path('<int:id>/', AuthorGenericAPIView.as_view()),
    ]
    ```
2. di author.api.views:
    ```python
    from author.api.serializers import AuthorSerializer
    from blog.models import Author
    from django.http import JsonResponse
    from rest_framework.parsers import JSONParser
    from django.views.decorators.csrf import csrf_exempt
    from django.http import HttpResponse
    from rest_framework.views import APIView
    from rest_framework.response import Response
    from rest_framework import generics, mixins


    class AuthorGenericAPIView(generics.GenericAPIView,
                                mixins.ListModelMixin,
                                mixins.CreateModelMixin,
                                mixins.RetrieveModelMixin,
                                mixins.UpdateModelMixin,
                                mixins.DestroyModelMixin):
        
        serializer_class = AuthorSerializer
        queryset = Author.objects.all()
        lookup_field = 'id'

        def get(self, request, id=None):
            if id:
                return self.retrieve(request, id)
            else:
                return self.list(request)

        def post(self, request):
            return self.create(request)

        def put(self, request, id=None):
            return self.update(request, id)

        def delete(self, request, id=None):
            return self.destroy(request, id)
    ```
- Jika pengen ada perubahan ketika di .save(), misal pengen ada id user yang masuk otomatis ketika sesuatu diupload, pakenya `perform_create` / `perform_update`
    ```python
        def perform_create(self, serializer):
            # serializer.save(created_by=self.request.user)
            serializer.save(name="bakal selalu anto namanya")

        def put(self, request, id=None):
            return self.update(request, id)
        
        def perform_update(self, serializer):
            # serializer.save(created_by=self.request.user)
            serializer.save(name="ya tetep aja bakal selalu anto namanya")
    ```

# 6. Authentications and Permissons
## 1. Basic Authentication
1. di viewsnya import :
    ```python
    from rest_framework.authentication import SessionAuthentication, BasicAuthentication
    from rest_framework.permissions import isAuthenticated, isAdminUser
    ```
2. di dalam class yang mau dikasih authentikasi tambahin :
    ```python
    authentication_classes = [SessionAuthentication, BasicAuthentication]
    # jika pengen semua bisa akses asal dah login
    permission_classes = [isAuthenticated,]
    ```

# 7. Token Authentication using DRF
- make login, logout api , use it to create token
1. add in `INSTALLED_APPS`
    ```python
    INSTALLED_APPS = [
        'rest_framework.authtoken',
    ]
    ```
2. `python manage.py migrate`
3. in urls.py
    ```python
    from author.api.views import (LoginView, LogoutView)

    path('api/v1/auth/login/', LoginView.as_view()),
    path('api/v1/auth/logout/', LogoutView.as_view()),
    ```
PASS

# 9. ModelViewSet and Routers in DRF
1. di author.api.urls :
    ```python
    from django.urls import include
    from author.api.views import (
        AuthorViewSet
        )
    from rest_framework.routers import DefaultRouter

    router = DefaultRouter()
    router.register('', AuthorViewSet)

    urlpatterns = [
        path('', include(router.urls)),
    ]
    ```
2. di author.api.views:
    ```python
    from author.api.serializers import AuthorSerializer
    from blog.models import Author
    from django.http import JsonResponse
    from rest_framework.parsers import JSONParser
    from django.views.decorators.csrf import csrf_exempt
    from django.http import HttpResponse
    from rest_framework.views import APIView
    from rest_framework.response import Response
    from rest_framework import generics, mixins
    from rest_framework import viewsets


    class AuthorViewSet(viewsets.ModelViewSet):
        serializer_class = AuthorSerializer
        queryset = Author.objects.all()    
        lookup_field = 'id'
    ```
- Nah trus misal pengen gini, bikin url buat blog yang dibikin oleh author itu (exp : `/authors/1/blogs`), gimana caranya  ? 
1. bikin serializer buat blognya di author.api.serializers:
    ```python
    from rest_framework import serializers
    from blog.models import Author, Blog

    class AuthorSerializer(serializers.ModelSerializer):
        class Meta:
            model = Author
            fields = (
                '__all__'
            )   

    class BlogSerializer(serializers.ModelSerializer):
        class Meta:
            model = Blog
            fields = (
                '__all__'
            )   
            read_only_fields = ('author',)
    ```
2. di author.api.views nya:
    ```python
    from author.api.serializers import AuthorSerializer, BlogSerializer
    from blog.models import Author, Blog
    from django.http import JsonResponse
    from rest_framework.parsers import JSONParser
    from django.views.decorators.csrf import csrf_exempt
    from django.http import HttpResponse
    from rest_framework.views import APIView
    from rest_framework.response import Response
    from rest_framework import generics, mixins
    from rest_framework import viewsets
    from rest_framework.decorators import action


    class AuthorViewSet(viewsets.ModelViewSet):
        serializer_class = AuthorSerializer
        queryset = Author.objects.all()    
        lookup_field = 'id'

        # tambahin ini buat getnya authors/1/blogs/
        @action(detail=True, methods=["GET"])
        def blogs(self, request, id=None):
            author = self.get_object()
            blogs = Blog.objects.filter(author=author)
            serializer = BlogSerializer(blogs, many=True)
            return Response(serializer.data, status=200) 
        
        # buat klo pengen ada createnya authors/1/blog/
        @action(detail=True, methods=["POST"])
        def blog(self, request, id=None):
            author = self.get_object()
            data = request.data
            # nambahin id author di data
            data["author"] = author.id
            serializer = AuthorSerializer(data=data)
            if serializer.is_valid():
                serializer.save()
                return Response(serializer.data, status=201)
            return Response(serializer.errors, status=400)
    ```
# 9. Nested Serializer for Create and Update
- Jadi klo misal, pengen pake nested serializer, which means bisa liat isi tabel lain jika manggil tabel tertentu, misal di blog bisa langsung liat isi apa aja yang ada di tabel authornya, atau di author bisa liat blog apa saja yang udah dia tulis, gimana caranya?
    - Jika ada FK --> pake `depth`
    - Jika nggk ada tapi berhubungan :
        1. bikin di model property yang mau dihubungin
        2. olah dan masukin diserialzernya dia sendiri
        - Note : semua proses ini ada di serializers tapi serializer nggk bisa ada queryset di fieldnya
        - TAPI INI ADA RESIKONYA, SETIAP BIKIN AUTHOR, BLOG JGA HRS DIISI
1. Jika ada FK (blog ke author) : di serializersnya :
    ```python
    from rest_framework import serializers
    from blog.models import Author, Blog


    class BlogSerializer(serializers.ModelSerializer):
        class Meta:
            model = Blog
            fields = (
                '__all__'
            )   
            read_only_fields = ('author',)
            depth = 1
    ```
 2. Jika nggk ada FK tapi berhubungan (author ke blog): TAPI INI ADA RESIKONYA, SETIAP BIKIN AUTHOR, BLOG JGA HRS DIISI
    1. di modelnya:
        ```python
        class Author(models.Model):
        name = models.CharField(max_length=128)
        password = models.CharField(max_length=128)
        
        def __str__(self):
            return self.name

        # tambahin ini    
        @property
        def blogs(self):
            return self.blog_set.all()
        ```
    2. di serializernya :
        ```python
        from rest_framework import serializers
        from blog.models import Author, Blog


        class BlogSerializer(serializers.ModelSerializer):
            class Meta:
                model = Blog
                fields = (
                    '__all__'
                )   
                read_only_fields = ('author',)
                depth = 1

        # ini
        class AuthorSerializer(serializers.ModelSerializer):
            blogs = BlogSerializer(many=True)
            class Meta:
                model = Author
                fields = (
                    '__all__'
                    # 'blogs'
                )   
        ```
    - Gimana cara mengatasi resikonya? buat jadi `required=False`: biar bisa kosong :
        ```python
        class AuthorSerializer(serializers.ModelSerializer):
            blogs = BlogSerializer(many=True, required=False)
            class Meta:
                model = Author
                fields = (
                    '__all__'
                    # 'blogs'
        ```
    - Trs, klo pengen pake cara ini dan ada create ama post buat blognya gimana? 
        - ADA DI VIDEONYA TAPI MAGER AGAK NGGK KEPAKE JGA KAYANYA

# 10. PASS

# 11. PASS

# 12. Upload Image or File using DRF
- Steps :
1. buat dir media, di setting.py :
    ```python
    MEDIA_URL = '/media/'
    MEDIA_ROOT = 'media'
    ```
2. di model.py.Author : tambahin ini :
    ```python
    cv = models.FileField(upload_to='cv/', max_length=255, null=True, blank=True)
    ```
3. di author.api.views : tambahin PARSER di classnya
    ```python
    from author.api.serializers import AuthorSerializer, BlogSerializer
    from blog.models import Author, Blog
    from django.http import JsonResponse
    # ini
    from rest_framework.parsers import JSONParser, FormParser, MultiPartParser
    from django.views.decorators.csrf import csrf_exempt
    from django.http import HttpResponse
    from rest_framework.views import APIView
    from rest_framework.response import Response
    from rest_framework import generics, mixins
    from rest_framework import viewsets
    from rest_framework.decorators import action

    class AuthorViewSet(viewsets.ModelViewSet):
        serializer_class = AuthorSerializer
        queryset = Author.objects.all()    
        lookup_field = 'id'
        
        # ini
        parser_classes = (JSONParser, FormParser, MultiPartParser)
    ```
- tapi klo pake cara ini, file nggk kehapus meski di PUT, PATCH, maupun di DELETE

# 13. Enable API documentation using Django Rest Swagger
1. pip install django-rest-swagger
2. di setting.py
    ```python
    INSTALLED_APPS = [
        ...
        'rest_framework_swagger',
        ...
    ]

    REST_FRAMEWORK = { 
        ... 
        'DEFAULT_SCHEMA_CLASS': 'rest_framework.schemas.coreapi.AutoSchema',
        ... 
        }
    ```
3. di urls.py:
    ```python
    from django.contrib import admin
    from django.urls import path, include

    from rest_framework_swagger.views import get_swagger_view    

    scema_view = get_swagger_view("API Documentation")


    urlpatterns = [
        path('api/documentation/', scema_view), 
    ]
    ```
