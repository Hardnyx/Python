# `django-rest-framework`

## Propósito

`django-rest-framework`, conocido comúnmente como DRF, es una librería externa para construir APIs web sobre Django.

Se utiliza para crear APIs REST con serializers, vistas, permisos, autenticación, paginación, filtros, validación de datos, endpoints CRUD, documentación navegable y respuestas estructuradas en formatos como JSON.

DRF no reemplaza a Django. Se construye sobre Django y aprovecha su sistema de modelos, URLs, vistas, autenticación, permisos, settings y base de datos.

## Naturaleza de la librería

`django-rest-framework` no forma parte de la biblioteca estándar de Python.

Tampoco forma parte del núcleo de Django.

Debe instalarse antes de usarse:

```bash
python -m pip install djangorestframework
```

Luego debe registrarse en `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...,
    "rest_framework",
]
```

Importaciones frecuentes:

```python
from rest_framework import serializers
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework.views import APIView
from rest_framework import generics
from rest_framework import viewsets
from rest_framework.routers import DefaultRouter
```

## Idea central

La idea principal de DRF es convertir objetos de Django en datos aptos para una API.

Flujo típico:

```text
request HTTP
-> view / viewset
-> serializer
-> model / queryset
-> response JSON
```

Ejemplo conceptual:

```text
Product model
-> ProductSerializer
-> ProductViewSet
-> Router
-> /api/products/
```

## Cuándo usar Django REST Framework

Conviene usar DRF cuando se necesita:

- construir una API REST sobre Django
- exponer modelos de Django como endpoints
- validar datos de entrada
- serializar objetos a JSON
- deserializar JSON hacia modelos
- crear endpoints CRUD
- aplicar permisos
- usar autenticación
- paginar resultados
- filtrar búsquedas
- usar routers automáticos
- aprovechar la browsable API
- crear APIs estructuradas y mantenibles

## Cuándo no usar Django REST Framework

DRF puede ser excesivo si solo se necesita devolver un JSON puntual.

Para una respuesta JSON simple, Django puede bastar:

```python
from django.http import JsonResponse


def status_view(request):
    return JsonResponse({"status": "ok"})
```

Si se está construyendo una API independiente sin necesidad del ecosistema Django, `fastapi` puede ser una alternativa más directa.

Si ya existe un proyecto Django con modelos, usuarios, permisos y base de datos, DRF suele encajar muy bien.

## Instalación

Instalación básica:

```bash
python -m pip install djangorestframework
```

Instalación con soporte para filtros:

```bash
python -m pip install djangorestframework django-filter
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
djangorestframework==3.x.x
```

La versión exacta puede variar según el entorno.

## Registro en Django

Archivo:

```text
config/settings.py
```

Configuración:

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "rest_framework",
]
```

Si se usa `django-filter`:

```python
INSTALLED_APPS = [
    ...,
    "rest_framework",
    "django_filters",
]
```

## Primer modelo de ejemplo

Archivo:

```text
products/models.py
```

Código:

```python
from django.db import models


class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField(default=0)
    is_active = models.BooleanField(default=True)

    def __str__(self):
        return self.name
```

Después de crear o modificar modelos:

```bash
python manage.py makemigrations
python manage.py migrate
```

## Serializers

## Propósito

Un serializer convierte objetos complejos, como modelos de Django, en datos simples que pueden representarse como JSON.

También hace el proceso inverso: recibe datos JSON, los valida y puede crear o actualizar objetos.

Flujo:

```text
model instance -> serializer -> JSON-compatible data
JSON input -> serializer -> validated data -> model instance
```

## Serializer básico

Archivo:

```text
products/serializers.py
```

Código:

```python
from rest_framework import serializers


class ProductSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    name = serializers.CharField(max_length=100)
    price = serializers.DecimalField(max_digits=10, decimal_places=2)
    stock = serializers.IntegerField(min_value=0)
    is_active = serializers.BooleanField(default=True)
```

Este serializer define manualmente los campos esperados.

## ModelSerializer

`ModelSerializer` crea campos a partir de un modelo de Django.

```python
from rest_framework import serializers

from .models import Product


class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = [
            "id",
            "name",
            "price",
            "stock",
            "is_active",
        ]
```

Este enfoque es el más común cuando la API está basada en modelos.

## Campos de solo lectura

```python
class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = ["id", "name", "price", "stock"]
        read_only_fields = ["id"]
```

## Incluir todos los campos

```python
class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = "__all__"
```

Aunque es posible usar `fields = "__all__"`, en APIs públicas o mantenibles suele ser mejor declarar campos explícitamente.

## Excluir campos

```python
class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        exclude = ["is_active"]
```

## Validación de campos

```python
from rest_framework import serializers

from .models import Product


class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = ["id", "name", "price", "stock"]

    def validate_price(self, value):
        if value <= 0:
            raise serializers.ValidationError(
                "El precio debe ser mayor que cero."
            )

        return value
```

## Validación del objeto completo

```python
class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = ["id", "name", "price", "stock"]

    def validate(self, attrs):
        price = attrs.get("price")
        stock = attrs.get("stock")

        if price is not None and stock == 0 and price > 10000:
            raise serializers.ValidationError(
                "Un producto sin stock no debería tener precio elevado."
            )

        return attrs
```

## Crear objetos con serializer

```python
data = {
    "name": "Laptop",
    "price": "3500.00",
    "stock": 5,
    "is_active": True,
}

serializer = ProductSerializer(data=data)

if serializer.is_valid():
    product = serializer.save()
```

## Errores de validación

```python
serializer = ProductSerializer(data=data)

if not serializer.is_valid():
    print(serializer.errors)
```

## Serializar una instancia

```python
product = Product.objects.get(id=1)

serializer = ProductSerializer(product)

print(serializer.data)
```

## Serializar múltiples objetos

```python
products = Product.objects.all()

serializer = ProductSerializer(products, many=True)

print(serializer.data)
```

## `many=True`

Debe usarse cuando se serializa un conjunto de objetos.

```python
serializer = ProductSerializer(products, many=True)
```

Sin `many=True`, DRF espera un solo objeto.

## Response

DRF usa `Response` para devolver respuestas API.

```python
from rest_framework.response import Response
```

Ejemplo:

```python
return Response({"status": "ok"})
```

A diferencia de `JsonResponse`, `Response` forma parte del sistema de DRF y trabaja con renderers, content negotiation y la browsable API.

## Status codes

DRF ofrece constantes de estado HTTP.

```python
from rest_framework import status
```

Ejemplo:

```python
return Response(
    {"message": "Producto creado"},
    status=status.HTTP_201_CREATED
)
```

Códigos frecuentes:

```python
status.HTTP_200_OK
status.HTTP_201_CREATED
status.HTTP_204_NO_CONTENT
status.HTTP_400_BAD_REQUEST
status.HTTP_401_UNAUTHORIZED
status.HTTP_403_FORBIDDEN
status.HTTP_404_NOT_FOUND
status.HTTP_500_INTERNAL_SERVER_ERROR
```

## Function-based views

DRF permite crear vistas basadas en funciones usando `@api_view`.

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response


@api_view(["GET"])
def product_list(request):
    return Response({"products": []})
```

## Endpoint GET con serializer

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response

from .models import Product
from .serializers import ProductSerializer


@api_view(["GET"])
def product_list(request):
    products = Product.objects.filter(is_active=True)
    serializer = ProductSerializer(products, many=True)

    return Response(serializer.data)
```

## Endpoint POST con serializer

```python
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response

from .serializers import ProductSerializer


@api_view(["POST"])
def product_create(request):
    serializer = ProductSerializer(data=request.data)

    if serializer.is_valid():
        serializer.save()

        return Response(
            serializer.data,
            status=status.HTTP_201_CREATED
        )

    return Response(
        serializer.errors,
        status=status.HTTP_400_BAD_REQUEST
    )
```

## Endpoint de detalle

```python
from django.shortcuts import get_object_or_404
from rest_framework.decorators import api_view
from rest_framework.response import Response

from .models import Product
from .serializers import ProductSerializer


@api_view(["GET"])
def product_detail(request, product_id):
    product = get_object_or_404(Product, id=product_id)

    serializer = ProductSerializer(product)

    return Response(serializer.data)
```

## URLs con function-based views

Archivo:

```text
products/urls.py
```

Código:

```python
from django.urls import path

from . import views

urlpatterns = [
    path("products/", views.product_list, name="product_list"),
    path("products/create/", views.product_create, name="product_create"),
    path("products/<int:product_id>/", views.product_detail, name="product_detail"),
]
```

## APIView

`APIView` permite escribir vistas basadas en clases con métodos como `get`, `post`, `put`, `patch` y `delete`.

```python
from rest_framework.views import APIView
from rest_framework.response import Response


class ProductListAPIView(APIView):
    def get(self, request):
        return Response({"products": []})
```

## APIView con GET y POST

```python
from rest_framework import status
from rest_framework.response import Response
from rest_framework.views import APIView

from .models import Product
from .serializers import ProductSerializer


class ProductListCreateAPIView(APIView):
    def get(self, request):
        products = Product.objects.filter(is_active=True)
        serializer = ProductSerializer(products, many=True)

        return Response(serializer.data)

    def post(self, request):
        serializer = ProductSerializer(data=request.data)

        if serializer.is_valid():
            serializer.save()

            return Response(
                serializer.data,
                status=status.HTTP_201_CREATED
            )

        return Response(
            serializer.errors,
            status=status.HTTP_400_BAD_REQUEST
        )
```

## URL con APIView

```python
from django.urls import path

from .views import ProductListCreateAPIView

urlpatterns = [
    path(
        "products/",
        ProductListCreateAPIView.as_view(),
        name="product_list_create"
    ),
]
```

## Generic views

DRF incluye vistas genéricas para patrones comunes.

Importación:

```python
from rest_framework import generics
```

## ListAPIView

```python
from rest_framework import generics

from .models import Product
from .serializers import ProductSerializer


class ProductListAPIView(generics.ListAPIView):
    queryset = Product.objects.filter(is_active=True)
    serializer_class = ProductSerializer
```

## RetrieveAPIView

```python
class ProductDetailAPIView(generics.RetrieveAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

## CreateAPIView

```python
class ProductCreateAPIView(generics.CreateAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

## UpdateAPIView

```python
class ProductUpdateAPIView(generics.UpdateAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

## DestroyAPIView

```python
class ProductDeleteAPIView(generics.DestroyAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

## ListCreateAPIView

```python
class ProductListCreateAPIView(generics.ListCreateAPIView):
    queryset = Product.objects.filter(is_active=True)
    serializer_class = ProductSerializer
```

## RetrieveUpdateDestroyAPIView

```python
class ProductDetailAPIView(generics.RetrieveUpdateDestroyAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

## `get_queryset()`

Para personalizar la consulta, puede sobrescribirse `get_queryset()`.

```python
class ProductListAPIView(generics.ListAPIView):
    serializer_class = ProductSerializer

    def get_queryset(self):
        queryset = Product.objects.filter(is_active=True)

        search = self.request.query_params.get("search")

        if search:
            queryset = queryset.filter(name__icontains=search)

        return queryset
```

## `perform_create()`

Para modificar el guardado en una vista de creación:

```python
class ProductCreateAPIView(generics.CreateAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer

    def perform_create(self, serializer):
        serializer.save(is_active=True)
```

## ViewSets

Un ViewSet agrupa acciones relacionadas en una sola clase.

En lugar de definir manualmente vistas para:

```text
listar
crear
obtener detalle
actualizar
eliminar
```

puede usarse un ViewSet.

## ModelViewSet

```python
from rest_framework import viewsets

from .models import Product
from .serializers import ProductSerializer


class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

`ModelViewSet` proporciona acciones CRUD estándar:

```text
list
create
retrieve
update
partial_update
destroy
```

## ReadOnlyModelViewSet

Si solo se necesita lectura:

```python
class ProductReadOnlyViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Product.objects.filter(is_active=True)
    serializer_class = ProductSerializer
```

Acciones principales:

```text
list
retrieve
```

## ViewSet con `get_queryset()`

```python
class ProductViewSet(viewsets.ModelViewSet):
    serializer_class = ProductSerializer

    def get_queryset(self):
        return Product.objects.filter(is_active=True)
```

## Acción personalizada

```python
from rest_framework.decorators import action
from rest_framework.response import Response


class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer

    @action(detail=False, methods=["get"])
    def active(self, request):
        products = Product.objects.filter(is_active=True)
        serializer = self.get_serializer(products, many=True)

        return Response(serializer.data)
```

Ruta generada conceptualmente:

```text
/products/active/
```

## Routers

Los routers generan URLs automáticamente para ViewSets.

```python
from rest_framework.routers import DefaultRouter

from .views import ProductViewSet

router = DefaultRouter()
router.register("products", ProductViewSet, basename="product")

urlpatterns = router.urls
```

Esto genera rutas para acciones CRUD comunes.

## URL principal con router

Archivo:

```text
products/urls.py
```

Código:

```python
from rest_framework.routers import DefaultRouter

from .views import ProductViewSet

router = DefaultRouter()
router.register("products", ProductViewSet, basename="product")

urlpatterns = router.urls
```

Archivo:

```text
config/urls.py
```

Código:

```python
from django.urls import include, path

urlpatterns = [
    path("api/", include("products.urls")),
]
```

Rutas resultantes:

```text
/api/products/
/api/products/{id}/
```

## DefaultRouter vs SimpleRouter

## DefaultRouter

Incluye una raíz navegable de API.

```python
router = DefaultRouter()
```

## SimpleRouter

Genera rutas, pero no incluye raíz de API.

```python
from rest_framework.routers import SimpleRouter

router = SimpleRouter()
```

## Browsable API

DRF incluye una interfaz navegable desde el navegador.

Permite:

- ver endpoints
- inspeccionar datos
- probar solicitudes
- enviar formularios
- revisar respuestas
- navegar una API durante desarrollo

Para habilitar login y logout en la browsable API:

```python
from django.urls import include, path

urlpatterns = [
    path("api-auth/", include("rest_framework.urls")),
]
```

Esto no reemplaza un sistema completo de autenticación para producción, pero es útil para desarrollo.

## Autenticación

DRF puede usar distintas estrategias de autenticación.

Configuración global:

```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework.authentication.SessionAuthentication",
        "rest_framework.authentication.BasicAuthentication",
    ],
}
```

## SessionAuthentication

Usa el sistema de sesiones de Django.

Es útil cuando la API se consume desde el mismo sitio web o desde la browsable API.

## BasicAuthentication

Usa autenticación básica HTTP.

Puede servir para pruebas, pero no debería usarse sin HTTPS en entornos reales.

## TokenAuthentication

DRF incluye autenticación por token si se registra la app correspondiente.

```python
INSTALLED_APPS = [
    ...,
    "rest_framework",
    "rest_framework.authtoken",
]
```

Migraciones:

```bash
python manage.py migrate
```

Configuración:

```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework.authentication.TokenAuthentication",
    ],
}
```

## Permisos

Los permisos controlan quién puede acceder a una vista.

Importación:

```python
from rest_framework.permissions import IsAuthenticated
```

## Permiso en una vista

```python
from rest_framework.permissions import IsAuthenticated
from rest_framework.views import APIView


class ProtectedAPIView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        return Response({"status": "authorized"})
```

## Permiso en ViewSet

```python
from rest_framework.permissions import IsAuthenticated
from rest_framework import viewsets


class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    permission_classes = [IsAuthenticated]
```

## Permisos frecuentes

```python
AllowAny
IsAuthenticated
IsAdminUser
IsAuthenticatedOrReadOnly
DjangoModelPermissions
DjangoObjectPermissions
```

## Permisos globales

```python
REST_FRAMEWORK = {
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.IsAuthenticated",
    ],
}
```

Con esta configuración, todas las vistas requieren autenticación salvo que se indique lo contrario.

## Permitir acceso público en una vista

```python
from rest_framework.permissions import AllowAny


class PublicAPIView(APIView):
    permission_classes = [AllowAny]

    def get(self, request):
        return Response({"status": "public"})
```

## Filtros

DRF puede filtrar resultados manualmente o mediante backends.

## Filtro manual

```python
class ProductListAPIView(generics.ListAPIView):
    serializer_class = ProductSerializer

    def get_queryset(self):
        queryset = Product.objects.all()

        search = self.request.query_params.get("search")

        if search:
            queryset = queryset.filter(name__icontains=search)

        return queryset
```

## Filtro con django-filter

Instalación:

```bash
python -m pip install django-filter
```

Registro:

```python
INSTALLED_APPS = [
    ...,
    "django_filters",
]
```

Configuración global:

```python
REST_FRAMEWORK = {
    "DEFAULT_FILTER_BACKENDS": [
        "django_filters.rest_framework.DjangoFilterBackend",
    ],
}
```

Uso en una vista:

```python
class ProductListAPIView(generics.ListAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filterset_fields = ["is_active", "stock"]
```

Ejemplo de consulta:

```text
/api/products/?is_active=true
```

## SearchFilter

```python
from rest_framework import filters


class ProductListAPIView(generics.ListAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filter_backends = [filters.SearchFilter]
    search_fields = ["name"]
```

Ejemplo:

```text
/api/products/?search=lap
```

## OrderingFilter

```python
from rest_framework import filters


class ProductListAPIView(generics.ListAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filter_backends = [filters.OrderingFilter]
    ordering_fields = ["name", "price", "stock"]
```

Ejemplo:

```text
/api/products/?ordering=price
/api/products/?ordering=-price
```

## Paginación

La paginación divide resultados largos en páginas.

Configuración global:

```python
REST_FRAMEWORK = {
    "DEFAULT_PAGINATION_CLASS": "rest_framework.pagination.PageNumberPagination",
    "PAGE_SIZE": 10,
}
```

Ejemplo de uso:

```text
/api/products/?page=2
```

## PageNumberPagination personalizada

```python
from rest_framework.pagination import PageNumberPagination


class StandardResultsSetPagination(PageNumberPagination):
    page_size = 10
    page_size_query_param = "page_size"
    max_page_size = 100
```

Uso:

```python
class ProductListAPIView(generics.ListAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    pagination_class = StandardResultsSetPagination
```

## Versionado

DRF puede manejar versionado de APIs, aunque no siempre es necesario al inicio.

Ejemplo conceptual de rutas:

```text
/api/v1/products/
/api/v2/products/
```

Una estrategia simple es separar las URLs por versión.

```python
urlpatterns = [
    path("api/v1/", include("products.urls")),
]
```

## Renderers

Los renderers definen cómo se representa la respuesta.

Configuración frecuente:

```python
REST_FRAMEWORK = {
    "DEFAULT_RENDERER_CLASSES": [
        "rest_framework.renderers.JSONRenderer",
        "rest_framework.renderers.BrowsableAPIRenderer",
    ],
}
```

Para APIs estrictamente JSON, puede dejarse solo:

```python
REST_FRAMEWORK = {
    "DEFAULT_RENDERER_CLASSES": [
        "rest_framework.renderers.JSONRenderer",
    ],
}
```

## Parsers

Los parsers definen cómo se interpreta el cuerpo de una solicitud.

Ejemplos:

```python
JSONParser
FormParser
MultiPartParser
```

Uso en una vista:

```python
from rest_framework.parsers import MultiPartParser


class UploadAPIView(APIView):
    parser_classes = [MultiPartParser]

    def post(self, request):
        file = request.FILES.get("file")

        return Response({"filename": file.name})
```

## Subida de archivos

```python
from rest_framework.parsers import MultiPartParser
from rest_framework.response import Response
from rest_framework.views import APIView


class FileUploadAPIView(APIView):
    parser_classes = [MultiPartParser]

    def post(self, request):
        uploaded_file = request.FILES.get("file")

        if uploaded_file is None:
            return Response(
                {"error": "Archivo requerido"},
                status=status.HTTP_400_BAD_REQUEST
            )

        return Response(
            {"filename": uploaded_file.name},
            status=status.HTTP_201_CREATED
        )
```

## Settings globales de DRF

DRF usa la clave `REST_FRAMEWORK` en `settings.py`.

Ejemplo:

```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework.authentication.SessionAuthentication",
    ],
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.IsAuthenticatedOrReadOnly",
    ],
    "DEFAULT_PAGINATION_CLASS": "rest_framework.pagination.PageNumberPagination",
    "PAGE_SIZE": 20,
}
```

## Testing

DRF incluye herramientas para probar APIs.

Importaciones frecuentes:

```python
from rest_framework.test import APITestCase
from rest_framework import status
```

## Test de listado

```python
from rest_framework import status
from rest_framework.test import APITestCase

from .models import Product


class ProductAPITests(APITestCase):
    def test_product_list(self):
        Product.objects.create(
            name="Laptop",
            price=3500,
            stock=5
        )

        response = self.client.get("/api/products/")

        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.assertEqual(len(response.data), 1)
```

## Test de creación

```python
class ProductAPITests(APITestCase):
    def test_product_create(self):
        data = {
            "name": "Laptop",
            "price": "3500.00",
            "stock": 5,
            "is_active": True,
        }

        response = self.client.post(
            "/api/products/",
            data,
            format="json"
        )

        self.assertEqual(response.status_code, status.HTTP_201_CREATED)
        self.assertEqual(Product.objects.count(), 1)
```

## Autenticación en tests

```python
from django.contrib.auth.models import User
from rest_framework.test import APITestCase


class ProtectedAPITests(APITestCase):
    def test_authenticated_request(self):
        user = User.objects.create_user(
            username="testuser",
            password="password123"
        )

        self.client.force_authenticate(user=user)

        response = self.client.get("/api/protected/")

        self.assertEqual(response.status_code, 200)
```

## Estructura recomendada

```text
products/
├─ __init__.py
├─ admin.py
├─ apps.py
├─ models.py
├─ serializers.py
├─ urls.py
├─ views.py
├─ tests.py
└─ migrations/
```

Para proyectos más grandes:

```text
products/
├─ api/
│  ├─ __init__.py
│  ├─ serializers.py
│  ├─ urls.py
│  └─ views.py
├─ models.py
├─ services.py
├─ admin.py
└─ tests/
```

## Separación por responsabilidades

```text
models.py       -> estructura de datos
serializers.py  -> validación y conversión de datos
views.py        -> endpoints API
urls.py         -> rutas
permissions.py  -> permisos personalizados
filters.py      -> filtros personalizados
services.py     -> lógica de negocio
tests.py        -> pruebas
```

## Comparación con Django puro

## Django puro

Puede devolver JSON con `JsonResponse`.

```python
return JsonResponse({"status": "ok"})
```

Es suficiente para endpoints simples.

## Django REST Framework

Agrega herramientas específicas para APIs:

```text
serializers
APIView
generic views
viewsets
routers
permissions
authentication
pagination
filtering
browsable API
testing API
```

Regla práctica:

```text
JSON puntual -> Django puro
API REST estructurada -> Django REST Framework
```

## Comparación con FastAPI

## DRF

- se integra profundamente con Django
- aprovecha modelos, ORM, admin y auth de Django
- es muy fuerte para APIs sobre proyectos Django existentes
- usa serializers para validación
- trabaja muy bien con bases de datos relacionales mediante ORM de Django

## FastAPI

- se enfoca en APIs modernas desde cero
- usa type hints y Pydantic de forma central
- genera documentación OpenAPI automáticamente
- suele ser más directo para microservicios JSON
- no depende de Django

Regla práctica:

```text
proyecto Django existente -> DRF
API independiente moderna -> FastAPI
```

## Errores comunes

## No registrar `rest_framework`

Problemático:

```python
INSTALLED_APPS = [
    ...,
]
```

Correcto:

```python
INSTALLED_APPS = [
    ...,
    "rest_framework",
]
```

## Olvidar `many=True`

Problemático:

```python
products = Product.objects.all()
serializer = ProductSerializer(products)
```

Correcto:

```python
serializer = ProductSerializer(products, many=True)
```

## Usar `request.POST` en lugar de `request.data`

En DRF, para datos de entrada de API se usa:

```python
request.data
```

No:

```python
request.POST
```

## Devolver `JsonResponse` dentro de vistas DRF

En vistas DRF, suele usarse:

```python
return Response(data)
```

No es obligatorio en todos los casos, pero mantiene consistencia con DRF.

## No validar `serializer.is_valid()`

Problemático:

```python
serializer = ProductSerializer(data=request.data)
serializer.save()
```

Correcto:

```python
serializer = ProductSerializer(data=request.data)

if serializer.is_valid():
    serializer.save()
```

También puede usarse:

```python
serializer.is_valid(raise_exception=True)
```

## Exponer todos los campos sin revisar

Problemático:

```python
fields = "__all__"
```

Puede exponer campos internos no deseados.

Más seguro:

```python
fields = ["id", "name", "price", "stock"]
```

## Usar ModelViewSet para todo sin pensar permisos

`ModelViewSet` crea operaciones completas de lectura y escritura.

Si solo se necesita lectura:

```python
ReadOnlyModelViewSet
```

Si se usa escritura, deben revisarse permisos, autenticación y validaciones.

## No configurar paginación

Un endpoint que devuelve miles de registros sin paginación puede ser lento y pesado.

## No controlar permisos globales

Si no se configuran permisos, algunos endpoints pueden quedar más abiertos de lo esperado.

## Escribir lógica compleja dentro del serializer o view

Conviene separar lógica de negocio en servicios cuando el proyecto crece.

## Buenas prácticas

## Definir serializers explícitos

```python
fields = ["id", "name", "price", "stock"]
```

## Usar `ModelSerializer` para modelos de Django

```python
class ProductSerializer(serializers.ModelSerializer):
    ...
```

## Usar generic views para CRUD simple

```python
generics.ListCreateAPIView
generics.RetrieveUpdateDestroyAPIView
```

## Usar ViewSets y routers para APIs CRUD consistentes

```python
router.register("products", ProductViewSet)
```

## Configurar permisos

```python
permission_classes = [IsAuthenticated]
```

## Configurar paginación

```python
PAGE_SIZE = 20
```

## Usar filtros y búsqueda cuando el endpoint liste datos

```python
filter_backends = [filters.SearchFilter, filters.OrderingFilter]
```

## Separar lógica de negocio

```text
serializer -> validación y representación
view       -> protocolo HTTP
service    -> reglas de negocio
```

## Escribir tests de API

```python
APITestCase
```

## Usar status codes de DRF

```python
status.HTTP_201_CREATED
```

## Ejemplo integrado

Estructura:

```text
products/
├─ models.py
├─ serializers.py
├─ views.py
├─ urls.py
└─ tests.py
```

Modelo:

```python
from django.db import models


class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField(default=0)
    is_active = models.BooleanField(default=True)

    def __str__(self):
        return self.name
```

Serializer:

```python
from rest_framework import serializers

from .models import Product


class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = [
            "id",
            "name",
            "price",
            "stock",
            "is_active",
        ]
        read_only_fields = ["id"]

    def validate_price(self, value):
        if value <= 0:
            raise serializers.ValidationError(
                "El precio debe ser mayor que cero."
            )

        return value
```

ViewSet:

```python
from rest_framework import filters, viewsets
from rest_framework.permissions import IsAuthenticatedOrReadOnly

from .models import Product
from .serializers import ProductSerializer


class ProductViewSet(viewsets.ModelViewSet):
    serializer_class = ProductSerializer
    permission_classes = [IsAuthenticatedOrReadOnly]
    filter_backends = [
        filters.SearchFilter,
        filters.OrderingFilter,
    ]
    search_fields = ["name"]
    ordering_fields = ["name", "price", "stock"]

    def get_queryset(self):
        return Product.objects.filter(is_active=True)
```

URLs de app:

```python
from rest_framework.routers import DefaultRouter

from .views import ProductViewSet

router = DefaultRouter()
router.register("products", ProductViewSet, basename="product")

urlpatterns = router.urls
```

URLs del proyecto:

```python
from django.urls import include, path

urlpatterns = [
    path("api/", include("products.urls")),
    path("api-auth/", include("rest_framework.urls")),
]
```

Settings:

```python
REST_FRAMEWORK = {
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.IsAuthenticatedOrReadOnly",
    ],
    "DEFAULT_PAGINATION_CLASS": "rest_framework.pagination.PageNumberPagination",
    "PAGE_SIZE": 20,
}
```

Rutas resultantes:

```text
GET     /api/products/
POST    /api/products/
GET     /api/products/{id}/
PUT     /api/products/{id}/
PATCH   /api/products/{id}/
DELETE  /api/products/{id}/
```

## Relación con otras librerías

`django-rest-framework` se relaciona especialmente con:

- `django`, porque se construye sobre su framework web, ORM y autenticación
- `django-filter`, para filtrado declarativo de endpoints
- `djangorestframework-simplejwt`, para autenticación con JWT
- `drf-spectacular`, para documentación OpenAPI avanzada
- `drf-yasg`, como alternativa para documentación Swagger/OpenAPI
- `pytest` y `pytest-django`, para pruebas
- `factory-boy`, para crear datos de prueba
- `pandas`, cuando la API expone o recibe datos tabulares procesados
- `openpyxl`, cuando se generan archivos Excel desde endpoints

## Orden didáctico interno

```text
1. Propósito de django-rest-framework
2. Instalación y registro en INSTALLED_APPS
3. Relación con Django
4. Serializers
5. ModelSerializer
6. Response y status codes
7. Function-based views con api_view
8. APIView
9. Generic views
10. ViewSets
11. Routers
12. Browsable API
13. Authentication
14. Permissions
15. Filtering, search y ordering
16. Pagination
17. Parsers y renderers
18. Testing
19. Estructura de proyecto
20. Comparación con Django puro y FastAPI
21. Errores comunes
22. Buenas prácticas
```