# `django`

## Propósito

`django` es una librería externa para construir aplicaciones web completas en Python. Se utiliza para desarrollar sitios web, paneles administrativos, sistemas internos, aplicaciones con usuarios, formularios, bases de datos, vistas HTML, APIs y proyectos web estructurados.

A diferencia de frameworks más ligeros como `flask`, Django incluye muchas piezas integradas desde el inicio:

```text
rutas
vistas
modelos
ORM
migraciones
plantillas
formularios
autenticación
sesiones
panel de administración
archivos estáticos
seguridad web básica
testing
```

Por eso suele describirse como un framework de enfoque completo.

## Naturaleza de la librería

`django` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install django
```

La importación depende del componente usado.

Ejemplos frecuentes:

```python
from django.db import models
from django.shortcuts import render
from django.http import HttpResponse
from django.urls import path
from django.contrib import admin
```

Django también incluye herramientas de línea de comandos, como:

```bash
django-admin
python manage.py
```

## Idea central

La idea principal de Django es organizar una aplicación web en piezas separadas:

```text
URL -> view -> model / service -> template / response
```

En una aplicación HTML tradicional:

```text
usuario solicita una URL
Django encuentra una ruta
Django ejecuta una vista
la vista consulta modelos o prepara datos
Django renderiza una plantilla
el navegador recibe HTML
```

En una API simple:

```text
cliente solicita una URL
Django encuentra una ruta
Django ejecuta una vista
la vista devuelve JSON
```

## Cuándo usar Django

Conviene usar Django cuando se necesita:

- construir una aplicación web completa
- trabajar con base de datos relacional
- crear modelos de datos
- usar un panel administrativo rápidamente
- manejar usuarios, permisos y sesiones
- crear formularios
- renderizar HTML con plantillas
- estructurar proyectos medianos o grandes
- aprovechar un ORM integrado
- aplicar migraciones de base de datos
- construir sistemas internos o de gestión

## Cuándo no usar Django

Django puede ser excesivo si solo se necesita:

- un script simple
- una API pequeña de pocos endpoints
- una automatización local
- un microservicio mínimo
- una aplicación sin base de datos
- un prototipo extremadamente pequeño

En esos casos puede bastar con:

```text
flask
fastapi
```

Para APIs modernas con validación automática fuerte mediante type hints, `fastapi` suele ser más directo.

Para sitios web completos con usuarios, modelos, panel administrativo y estructura integrada, Django suele ser una opción muy sólida.

## Instalación

Instalación básica:

```bash
python -m pip install django
```

Verificación:

```bash
python -m django --version
```

También puede verificarse desde Python:

```python
import django

print(django.get_version())
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
Django==6.x.x
```

La versión exacta puede variar según el entorno.

## Crear un proyecto

Para crear un proyecto nuevo se usa:

```bash
django-admin startproject config .
```

Esto crea una estructura similar a:

```text
project/
├─ manage.py
└─ config/
   ├─ __init__.py
   ├─ asgi.py
   ├─ settings.py
   ├─ urls.py
   └─ wsgi.py
```

El punto final indica que el proyecto se crea en el directorio actual.

```bash
django-admin startproject config .
```

Sin el punto, Django crea una carpeta adicional.

```bash
django-admin startproject config
```

## Archivos principales del proyecto

## `manage.py`

Archivo de utilidad para ejecutar comandos del proyecto.

Ejemplos:

```bash
python manage.py runserver
python manage.py migrate
python manage.py makemigrations
python manage.py createsuperuser
```

## `settings.py`

Contiene la configuración general del proyecto.

Incluye elementos como:

```text
INSTALLED_APPS
MIDDLEWARE
ROOT_URLCONF
TEMPLATES
DATABASES
LANGUAGE_CODE
TIME_ZONE
STATIC_URL
```

## `urls.py`

Define las rutas principales del proyecto.

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path("admin/", admin.site.urls),
]
```

## `asgi.py`

Punto de entrada para servidores ASGI.

Se usa para despliegues asíncronos o servidores compatibles con ASGI.

## `wsgi.py`

Punto de entrada para servidores WSGI.

Se usa en despliegues tradicionales con servidores compatibles con WSGI.

## Ejecutar el servidor de desarrollo

```bash
python manage.py runserver
```

Por defecto, suele quedar disponible en:

```text
http://127.0.0.1:8000/
```

También puede indicarse un puerto:

```bash
python manage.py runserver 8080
```

O una dirección:

```bash
python manage.py runserver 0.0.0.0:8000
```

El servidor de desarrollo no debe usarse como servidor final de producción.

## Proyecto y aplicación

Django distingue entre:

```text
proyecto
aplicación
```

## Proyecto

El proyecto contiene la configuración global.

Ejemplo:

```text
config/
```

Incluye:

```text
settings.py
urls.py
asgi.py
wsgi.py
```

## Aplicación

Una aplicación contiene una parte funcional del sistema.

Ejemplos:

```text
products
clients
orders
accounts
reports
```

Una aplicación puede incluir:

```text
models.py
views.py
urls.py
admin.py
forms.py
tests.py
templates/
static/
```

## Crear una aplicación

```bash
python manage.py startapp products
```

Estructura generada:

```text
products/
├─ __init__.py
├─ admin.py
├─ apps.py
├─ migrations/
│  └─ __init__.py
├─ models.py
├─ tests.py
└─ views.py
```

## Registrar una aplicación

Para que Django reconozca la aplicación, se agrega en `INSTALLED_APPS`.

Archivo:

```text
config/settings.py
```

Ejemplo:

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "products",
]
```

También puede usarse la clase de configuración:

```python
INSTALLED_APPS = [
    ...,
    "products.apps.ProductsConfig",
]
```

## Primer view

Archivo:

```text
products/views.py
```

Código:

```python
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hola desde Django")
```

Una view recibe un objeto `request` y devuelve una respuesta HTTP.

## Primer URL de aplicación

Crear archivo:

```text
products/urls.py
```

Código:

```python
from django.urls import path

from . import views

urlpatterns = [
    path("", views.index, name="index"),
]
```

## Conectar URLs de aplicación al proyecto

Archivo:

```text
config/urls.py
```

Código:

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("admin/", admin.site.urls),
    path("products/", include("products.urls")),
]
```

Con esta configuración, la ruta queda disponible en:

```text
/products/
```

## URL dispatcher

Django usa un sistema de URLconf para mapear rutas a vistas.

Ejemplo:

```python
from django.urls import path

from . import views

urlpatterns = [
    path("products/", views.product_list, name="product_list"),
]
```

Cada entrada de `urlpatterns` conecta:

```text
ruta
view
nombre de ruta
```

## Parámetros de ruta

```python
urlpatterns = [
    path("products/<int:product_id>/", views.product_detail, name="product_detail"),
]
```

View:

```python
def product_detail(request, product_id):
    return HttpResponse(f"Producto: {product_id}")
```

Tipos frecuentes:

```text
str
int
slug
uuid
path
```

## Nombre de rutas

El parámetro `name` permite referenciar rutas sin escribir la URL manualmente.

```python
path("products/", views.product_list, name="product_list")
```

En una plantilla:

```html
<a href="{% url 'product_list' %}">Productos</a>
```

## Views

Una view es una función o clase que recibe una solicitud y devuelve una respuesta.

## Function-based view

```python
from django.http import HttpResponse


def product_list(request):
    return HttpResponse("Lista de productos")
```

## View que devuelve JSON

```python
from django.http import JsonResponse


def product_list(request):
    data = [
        {"id": 1, "name": "Laptop"},
        {"id": 2, "name": "Mouse"},
    ]

    return JsonResponse({"products": data})
```

## View con método HTTP

```python
from django.http import JsonResponse


def product_create(request):
    if request.method != "POST":
        return JsonResponse(
            {"error": "Método no permitido"},
            status=405
        )

    return JsonResponse(
        {"message": "Producto creado"},
        status=201
    )
```

## Renderizar HTML

Para devolver HTML se usa normalmente `render()`.

```python
from django.shortcuts import render


def product_list(request):
    products = [
        {"name": "Laptop", "price": 3500},
        {"name": "Mouse", "price": 80},
    ]

    return render(
        request,
        "products/product_list.html",
        {"products": products}
    )
```

## Templates

Las plantillas permiten generar HTML dinámico.

Estructura recomendada dentro de una app:

```text
products/
└─ templates/
   └─ products/
      └─ product_list.html
```

Archivo:

```text
products/templates/products/product_list.html
```

Contenido:

```html
<!doctype html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <title>Productos</title>
</head>
<body>
    <h1>Productos</h1>

    <ul>
    {% for product in products %}
        <li>{{ product.name }} - {{ product.price }}</li>
    {% endfor %}
    </ul>
</body>
</html>
```

## Variables en templates

```html
{{ product.name }}
{{ product.price }}
```

## Condicionales en templates

```html
{% if products %}
    <p>Hay productos registrados.</p>
{% else %}
    <p>No hay productos.</p>
{% endif %}
```

## Bucles en templates

```html
{% for product in products %}
    <p>{{ product.name }}</p>
{% endfor %}
```

## Herencia de templates

Una plantilla base permite reutilizar estructura HTML.

Archivo:

```text
templates/base.html
```

Contenido:

```html
<!doctype html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <title>{% block title %}Mi sitio{% endblock %}</title>
</head>
<body>
    <header>
        <h1>Mi aplicación</h1>
    </header>

    <main>
        {% block content %}
        {% endblock %}
    </main>
</body>
</html>
```

Plantilla hija:

```html
{% extends "base.html" %}

{% block title %}Productos{% endblock %}

{% block content %}
    <h2>Lista de productos</h2>
{% endblock %}
```

## Configurar carpeta global de templates

En `settings.py` puede configurarse una carpeta global:

```python
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [
            BASE_DIR / "templates",
        ],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
]
```

## Modelos

Los modelos representan datos del sistema y se definen como clases de Python.

Archivo:

```text
products/models.py
```

Ejemplo:

```python
from django.db import models


class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField(default=0)
    is_active = models.BooleanField(default=True)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.name
```

Cada atributo representa un campo de base de datos.

## Campos frecuentes

```python
models.CharField(max_length=100)
models.TextField()
models.IntegerField()
models.PositiveIntegerField()
models.DecimalField(max_digits=10, decimal_places=2)
models.FloatField()
models.BooleanField(default=True)
models.DateField()
models.DateTimeField()
models.EmailField()
models.URLField()
models.FileField(upload_to="files/")
models.ImageField(upload_to="images/")
```

## `__str__`

El método `__str__` define cómo se representa el objeto como texto.

```python
def __str__(self):
    return self.name
```

Esto es útil en el admin, shell y depuración.

## Relaciones entre modelos

## ForeignKey

Relación muchos a uno.

```python
from django.db import models


class Category(models.Model):
    name = models.CharField(max_length=100)


class Product(models.Model):
    category = models.ForeignKey(
        Category,
        on_delete=models.CASCADE,
        related_name="products"
    )

    name = models.CharField(max_length=100)
```

## OneToOneField

Relación uno a uno.

```python
class Profile(models.Model):
    user = models.OneToOneField(
        "auth.User",
        on_delete=models.CASCADE
    )
```

## ManyToManyField

Relación muchos a muchos.

```python
class Product(models.Model):
    tags = models.ManyToManyField("Tag", related_name="products")
```

## `on_delete`

En una relación `ForeignKey`, `on_delete` indica qué ocurre cuando se elimina el objeto relacionado.

Opciones frecuentes:

```python
models.CASCADE
models.PROTECT
models.SET_NULL
models.SET_DEFAULT
models.DO_NOTHING
```

Ejemplo:

```python
category = models.ForeignKey(
    Category,
    on_delete=models.PROTECT
)
```

## Migraciones

Las migraciones registran cambios en modelos y los aplican a la base de datos.

Crear migraciones:

```bash
python manage.py makemigrations
```

Aplicar migraciones:

```bash
python manage.py migrate
```

Flujo típico:

```text
editar models.py
python manage.py makemigrations
python manage.py migrate
```

## Ver migraciones

```bash
python manage.py showmigrations
```

## Ver SQL de una migración

```bash
python manage.py sqlmigrate products 0001
```

## Base de datos

Django usa SQLite por defecto en proyectos nuevos.

Configuración típica en `settings.py`:

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    }
}
```

Para producción suelen usarse bases de datos como:

```text
PostgreSQL
MySQL
MariaDB
Oracle
SQLite
```

La elección depende del proyecto, entorno y necesidades de despliegue.

## ORM

El ORM de Django permite consultar la base de datos usando Python.

## Crear registros

```python
from products.models import Product

product = Product.objects.create(
    name="Laptop",
    price=3500,
    stock=5
)
```

## Consultar todos los registros

```python
products = Product.objects.all()
```

## Filtrar registros

```python
products = Product.objects.filter(is_active=True)
```

## Obtener un registro

```python
product = Product.objects.get(id=1)
```

Si no existe, se genera una excepción `DoesNotExist`.

## Usar `get_object_or_404`

En views, suele usarse:

```python
from django.shortcuts import get_object_or_404

from .models import Product


def product_detail(request, product_id):
    product = get_object_or_404(Product, id=product_id)

    return render(
        request,
        "products/product_detail.html",
        {"product": product}
    )
```

## Ordenar registros

```python
products = Product.objects.order_by("name")
```

Orden descendente:

```python
products = Product.objects.order_by("-price")
```

## Filtros frecuentes

```python
Product.objects.filter(name__icontains="lap")
Product.objects.filter(price__gte=100)
Product.objects.filter(price__lte=1000)
Product.objects.filter(created_at__year=2026)
Product.objects.filter(is_active=True)
```

## Actualizar registros

```python
product = Product.objects.get(id=1)
product.price = 3600
product.save()
```

Actualización masiva:

```python
Product.objects.filter(is_active=False).update(stock=0)
```

## Eliminar registros

```python
product = Product.objects.get(id=1)
product.delete()
```

Eliminación masiva:

```python
Product.objects.filter(is_active=False).delete()
```

## QuerySets

Un `QuerySet` representa una consulta a la base de datos.

```python
products = Product.objects.filter(is_active=True)
```

Puede encadenarse:

```python
products = (
    Product.objects
    .filter(is_active=True)
    .order_by("name")
)
```

En muchos casos, un `QuerySet` no se ejecuta hasta que se evalúa.

## Agregaciones

```python
from django.db.models import Count, Sum, Avg

summary = Product.objects.aggregate(
    total_stock=Sum("stock"),
    average_price=Avg("price"),
    product_count=Count("id")
)
```

## Anotaciones

```python
from django.db.models import Count

categories = Category.objects.annotate(
    product_count=Count("products")
)
```

## Admin

Django incluye un panel administrativo.

Ruta por defecto:

```text
/admin/
```

Para usarlo, primero se aplican migraciones:

```bash
python manage.py migrate
```

Luego se crea un superusuario:

```bash
python manage.py createsuperuser
```

## Registrar modelos en el admin

Archivo:

```text
products/admin.py
```

Código:

```python
from django.contrib import admin

from .models import Product


admin.site.register(Product)
```

## Personalizar el admin

```python
from django.contrib import admin

from .models import Product


@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):
    list_display = ("id", "name", "price", "stock", "is_active")
    list_filter = ("is_active",)
    search_fields = ("name",)
    ordering = ("name",)
```

Opciones frecuentes:

```text
list_display
list_filter
search_fields
ordering
readonly_fields
date_hierarchy
fieldsets
```

## Formularios

Django tiene un sistema de formularios.

## Form simple

```python
from django import forms


class ProductSearchForm(forms.Form):
    q = forms.CharField(required=False, max_length=100)
```

Uso en view:

```python
from django.shortcuts import render

from .forms import ProductSearchForm
from .models import Product


def product_search(request):
    form = ProductSearchForm(request.GET)
    products = Product.objects.all()

    if form.is_valid():
        q = form.cleaned_data["q"]

        if q:
            products = products.filter(name__icontains=q)

    return render(
        request,
        "products/product_search.html",
        {
            "form": form,
            "products": products,
        }
    )
```

## ModelForm

`ModelForm` permite crear formularios basados en modelos.

```python
from django import forms

from .models import Product


class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = ["name", "price", "stock", "is_active"]
```

View para crear:

```python
from django.shortcuts import redirect, render

from .forms import ProductForm


def product_create(request):
    if request.method == "POST":
        form = ProductForm(request.POST)

        if form.is_valid():
            form.save()
            return redirect("product_list")
    else:
        form = ProductForm()

    return render(
        request,
        "products/product_form.html",
        {"form": form}
    )
```

Template:

```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Guardar</button>
</form>
```

## CSRF

Django incluye protección CSRF para formularios.

En formularios HTML con método `POST`, debe agregarse:

```html
{% csrf_token %}
```

Ejemplo:

```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Enviar</button>
</form>
```

## Archivos estáticos

Los archivos estáticos incluyen:

```text
CSS
JavaScript
imágenes
fuentes
```

Estructura dentro de una app:

```text
products/
└─ static/
   └─ products/
      └─ styles.css
```

En template:

```html
{% load static %}

<link rel="stylesheet" href="{% static 'products/styles.css' %}">
```

## Configuración básica de estáticos

En `settings.py`:

```python
STATIC_URL = "static/"
```

Para producción suele configurarse también:

```python
STATIC_ROOT = BASE_DIR / "staticfiles"
```

Comando para recolectar estáticos:

```bash
python manage.py collectstatic
```

## Archivos subidos por usuarios

Configuración frecuente:

```python
MEDIA_URL = "media/"
MEDIA_ROOT = BASE_DIR / "media"
```

En `urls.py` durante desarrollo:

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
]

if settings.DEBUG:
    urlpatterns += static(
        settings.MEDIA_URL,
        document_root=settings.MEDIA_ROOT
    )
```

## Autenticación

Django incluye sistema de autenticación y autorización.

Componentes frecuentes:

```text
User
Group
Permission
login
logout
password hashing
sessions
decorators
mixins
```

## Proteger una view con login

```python
from django.contrib.auth.decorators import login_required


@login_required
def dashboard(request):
    return render(request, "dashboard.html")
```

## Usuario actual

```python
def profile(request):
    user = request.user

    return render(
        request,
        "profile.html",
        {"user": user}
    )
```

## Login y logout

Django incluye views genéricas para autenticación.

En `urls.py`:

```python
from django.urls import include, path

urlpatterns = [
    path("accounts/", include("django.contrib.auth.urls")),
]
```

Esto habilita rutas como:

```text
/accounts/login/
/accounts/logout/
/accounts/password_change/
/accounts/password_reset/
```

## Class-based views

Django permite usar vistas basadas en clases.

## TemplateView

```python
from django.views.generic import TemplateView


class HomeView(TemplateView):
    template_name = "home.html"
```

URL:

```python
from django.urls import path

from .views import HomeView

urlpatterns = [
    path("", HomeView.as_view(), name="home"),
]
```

## ListView

```python
from django.views.generic import ListView

from .models import Product


class ProductListView(ListView):
    model = Product
    template_name = "products/product_list.html"
    context_object_name = "products"
```

## DetailView

```python
from django.views.generic import DetailView

from .models import Product


class ProductDetailView(DetailView):
    model = Product
    template_name = "products/product_detail.html"
    context_object_name = "product"
```

## CreateView

```python
from django.urls import reverse_lazy
from django.views.generic import CreateView

from .models import Product


class ProductCreateView(CreateView):
    model = Product
    fields = ["name", "price", "stock", "is_active"]
    template_name = "products/product_form.html"
    success_url = reverse_lazy("product_list")
```

## Function-based views vs class-based views

## Function-based views

Ventajas:

```text
simples
explícitas
fáciles de leer al inicio
útiles para lógica personalizada
```

## Class-based views

Ventajas:

```text
reutilización
menos código repetido
buenas para CRUD estándar
integración con genéricos
```

Regla práctica:

```text
funciones -> para aprender y lógica directa
clases    -> para CRUD repetitivo y patrones reutilizables
```

## Middleware

Un middleware procesa solicitudes y respuestas de forma transversal.

Django ya incluye middleware para:

```text
seguridad
sesiones
mensajes
CSRF
autenticación
clickjacking
```

Configuración en `settings.py`:

```python
MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",
    "django.contrib.sessions.middleware.SessionMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.csrf.CsrfViewMiddleware",
    "django.contrib.auth.middleware.AuthenticationMiddleware",
    "django.contrib.messages.middleware.MessageMiddleware",
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
]
```

## Settings

`settings.py` concentra la configuración del proyecto.

Elementos importantes:

```python
SECRET_KEY
DEBUG
ALLOWED_HOSTS
INSTALLED_APPS
MIDDLEWARE
ROOT_URLCONF
TEMPLATES
DATABASES
LANGUAGE_CODE
TIME_ZONE
STATIC_URL
```

## `DEBUG`

```python
DEBUG = True
```

En producción debe ser:

```python
DEBUG = False
```

## `ALLOWED_HOSTS`

```python
ALLOWED_HOSTS = ["example.com", "www.example.com"]
```

En desarrollo puede estar vacío o ajustado al entorno.

## `SECRET_KEY`

No debe escribirse directamente en código en producción.

Mejor usar variables de entorno.

```python
import os

SECRET_KEY = os.getenv("DJANGO_SECRET_KEY")
```

## Variables de entorno

Puede usarse `python-dotenv`.

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
DJANGO_SECRET_KEY=clave-local
DJANGO_DEBUG=True
DATABASE_URL=sqlite:///db.sqlite3
```

Carga simple:

```python
import os

from dotenv import load_dotenv

load_dotenv()

SECRET_KEY = os.getenv("DJANGO_SECRET_KEY")
DEBUG = os.getenv("DJANGO_DEBUG") == "True"
```

## Shell de Django

Django incluye una shell con el contexto del proyecto.

```bash
python manage.py shell
```

Ejemplo:

```python
from products.models import Product

Product.objects.create(
    name="Laptop",
    price=3500,
    stock=5
)

Product.objects.all()
```

## Comandos frecuentes

## Crear proyecto

```bash
django-admin startproject config .
```

## Crear app

```bash
python manage.py startapp products
```

## Ejecutar servidor

```bash
python manage.py runserver
```

## Crear migraciones

```bash
python manage.py makemigrations
```

## Aplicar migraciones

```bash
python manage.py migrate
```

## Crear superusuario

```bash
python manage.py createsuperuser
```

## Abrir shell

```bash
python manage.py shell
```

## Ejecutar tests

```bash
python manage.py test
```

## Recolectar estáticos

```bash
python manage.py collectstatic
```

## Testing

Django incluye herramientas para pruebas.

Archivo:

```text
products/tests.py
```

Ejemplo:

```python
from django.test import TestCase

from .models import Product


class ProductModelTests(TestCase):
    def test_product_string_representation(self):
        product = Product.objects.create(
            name="Laptop",
            price=3500,
            stock=5
        )

        self.assertEqual(str(product), "Laptop")
```

## Test de view

```python
from django.test import TestCase
from django.urls import reverse


class ProductViewTests(TestCase):
    def test_product_list_status_code(self):
        response = self.client.get(reverse("product_list"))

        self.assertEqual(response.status_code, 200)
```

## Ejecutar pruebas

```bash
python manage.py test
```

## Estructura recomendada básica

```text
project/
├─ manage.py
├─ config/
│  ├─ __init__.py
│  ├─ asgi.py
│  ├─ settings.py
│  ├─ urls.py
│  └─ wsgi.py
├─ products/
│  ├─ __init__.py
│  ├─ admin.py
│  ├─ apps.py
│  ├─ forms.py
│  ├─ models.py
│  ├─ urls.py
│  ├─ views.py
│  ├─ tests.py
│  ├─ migrations/
│  ├─ templates/
│  │  └─ products/
│  └─ static/
│     └─ products/
└─ templates/
   └─ base.html
```

## Separación por responsabilidades

```text
models.py   -> estructura de datos
views.py    -> lógica HTTP
urls.py     -> rutas
forms.py    -> formularios
admin.py    -> configuración del admin
templates/  -> HTML
static/     -> CSS, JS e imágenes
tests.py    -> pruebas
```

## API JSON simple con Django

Aunque Django suele usarse para aplicaciones completas, también puede devolver JSON sin instalar librerías adicionales.

View:

```python
from django.http import JsonResponse

from .models import Product


def product_list_api(request):
    products = Product.objects.filter(is_active=True)

    data = [
        {
            "id": product.id,
            "name": product.name,
            "price": float(product.price),
            "stock": product.stock,
        }
        for product in products
    ]

    return JsonResponse({"products": data})
```

URL:

```python
from django.urls import path

from . import views

urlpatterns = [
    path("api/products/", views.product_list_api, name="product_list_api"),
]
```

Para APIs grandes, suele usarse `djangorestframework`.

## Relación con Django REST Framework

Django REST Framework, normalmente abreviado como DRF, es una librería externa construida sobre Django para crear APIs REST.

Django por sí solo puede devolver JSON, pero DRF agrega herramientas como:

```text
serializers
viewsets
routers
permissions
authentication classes
pagination
Browsable API
```

Regla práctica:

```text
Django simple -> JSON puntual
Django + DRF  -> API REST estructurada
```

## Comparación con Flask

## Django

- framework completo
- incluye ORM
- incluye migraciones
- incluye admin
- incluye autenticación
- estructura más definida
- adecuado para aplicaciones completas con base de datos

## Flask

- framework ligero
- estructura más flexible
- muchas piezas se agregan mediante extensiones
- adecuado para proyectos pequeños, APIs simples o aplicaciones a medida

## Regla práctica

Si se necesita una aplicación web completa con base de datos, usuarios y administración, Django suele ahorrar mucho trabajo.

Si se necesita máxima libertad desde cero, Flask puede ser más liviano.

## Comparación con FastAPI

## Django

- fuerte para aplicaciones completas
- incluye ORM, admin y plantillas
- funciona muy bien para sistemas web tradicionales
- puede crear APIs, especialmente con DRF

## FastAPI

- fuerte para APIs modernas
- usa type hints y Pydantic
- genera documentación OpenAPI automáticamente
- muy cómodo para servicios JSON

## Regla práctica

Para aplicaciones web completas, Django es una opción fuerte.

Para APIs JSON modernas y tipadas, FastAPI suele ser más directo.

## Casos de uso frecuentes

## Sistema administrativo interno

Django es muy útil cuando se necesita gestionar datos con modelos y panel admin.

## Aplicación CRUD

Ejemplo:

```text
crear productos
editar productos
listar productos
eliminar productos
buscar productos
```

## Sitio con usuarios

Django incluye autenticación, sesiones y permisos.

## Sistema con base de datos relacional

El ORM y las migraciones facilitan trabajar con tablas y relaciones.

## Panel de reportes

Puede combinarse con:

```text
pandas
plotly
matplotlib
openpyxl
```

para generar reportes o visualizaciones.

## Errores comunes

## No registrar la app en `INSTALLED_APPS`

Si se crea una app pero no se registra, Django puede no detectar modelos, templates o configuración asociada.

```python
INSTALLED_APPS = [
    ...,
    "products",
]
```

## Crear modelo y olvidar migraciones

Después de cambiar `models.py`, debe ejecutarse:

```bash
python manage.py makemigrations
python manage.py migrate
```

## Confundir proyecto con app

El proyecto contiene configuración global.

La app contiene funcionalidad específica.

```text
config -> proyecto
products -> app
```

## No conectar URLs de la app

Crear `products/urls.py` no basta. También debe incluirse en `config/urls.py`.

```python
path("products/", include("products.urls"))
```

## Usar rutas hardcodeadas en templates

Menos recomendable:

```html
<a href="/products/">Productos</a>
```

Más mantenible:

```html
<a href="{% url 'product_list' %}">Productos</a>
```

## No usar `{% csrf_token %}` en formularios POST

Problemático:

```html
<form method="post">
    {{ form.as_p }}
    <button type="submit">Guardar</button>
</form>
```

Correcto:

```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Guardar</button>
</form>
```

## Dejar `DEBUG=True` en producción

En producción debe usarse:

```python
DEBUG = False
```

## No configurar `ALLOWED_HOSTS`

En producción debe configurarse con los dominios permitidos.

```python
ALLOWED_HOSTS = ["example.com"]
```

## Escribir `SECRET_KEY` directamente en producción

Menos seguro:

```python
SECRET_KEY = "clave-real"
```

Mejor:

```python
SECRET_KEY = os.getenv("DJANGO_SECRET_KEY")
```

## Usar SQLite sin evaluar el contexto

SQLite es útil para desarrollo y proyectos pequeños, pero para aplicaciones multiusuario o producción puede ser más adecuado PostgreSQL u otra base relacional.

## Consultas ineficientes

Acceder a relaciones dentro de bucles puede causar muchas consultas.

Conviene conocer:

```python
select_related()
prefetch_related()
```

## Buenas prácticas

## Separar proyecto y apps

```text
config/
products/
clients/
orders/
```

## Usar nombres claros para apps

```text
products
clients
orders
reports
accounts
```

## Definir `__str__` en modelos

```python
def __str__(self):
    return self.name
```

## Crear migraciones después de cambiar modelos

```bash
python manage.py makemigrations
python manage.py migrate
```

## Usar `get_object_or_404` en views

```python
product = get_object_or_404(Product, id=product_id)
```

## Usar nombres de rutas

```python
path("products/", views.product_list, name="product_list")
```

## Usar herencia de templates

```html
{% extends "base.html" %}
```

## Usar variables de entorno

```python
os.getenv("DJANGO_SECRET_KEY")
```

## Personalizar el admin

```python
list_display = ("id", "name", "price")
```

## Escribir pruebas

```bash
python manage.py test
```

## Mantener lógica compleja fuera de views

Para proyectos medianos, conviene separar:

```text
views.py     -> entrada HTTP
services.py  -> lógica de negocio
models.py    -> datos
forms.py     -> validación de formularios
```

## Ejemplo integrado

Estructura:

```text
project/
├─ manage.py
├─ config/
│  ├─ settings.py
│  └─ urls.py
└─ products/
   ├─ admin.py
   ├─ models.py
   ├─ urls.py
   ├─ views.py
   └─ templates/
      └─ products/
         ├─ product_list.html
         └─ product_detail.html
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

Admin:

```python
from django.contrib import admin

from .models import Product


@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):
    list_display = ("id", "name", "price", "stock", "is_active")
    list_filter = ("is_active",)
    search_fields = ("name",)
```

Views:

```python
from django.shortcuts import get_object_or_404, render

from .models import Product


def product_list(request):
    products = (
        Product.objects
        .filter(is_active=True)
        .order_by("name")
    )

    return render(
        request,
        "products/product_list.html",
        {"products": products}
    )


def product_detail(request, product_id):
    product = get_object_or_404(
        Product,
        id=product_id,
        is_active=True
    )

    return render(
        request,
        "products/product_detail.html",
        {"product": product}
    )
```

URLs de app:

```python
from django.urls import path

from . import views

urlpatterns = [
    path("", views.product_list, name="product_list"),
    path("<int:product_id>/", views.product_detail, name="product_detail"),
]
```

URLs del proyecto:

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("admin/", admin.site.urls),
    path("products/", include("products.urls")),
]
```

Template de lista:

```html
{% extends "base.html" %}

{% block title %}Productos{% endblock %}

{% block content %}
    <h2>Productos</h2>

    <ul>
    {% for product in products %}
        <li>
            <a href="{% url 'product_detail' product.id %}">
                {{ product.name }}
            </a>
            - {{ product.price }}
        </li>
    {% empty %}
        <li>No hay productos disponibles.</li>
    {% endfor %}
    </ul>
{% endblock %}
```

Template de detalle:

```html
{% extends "base.html" %}

{% block title %}{{ product.name }}{% endblock %}

{% block content %}
    <h2>{{ product.name }}</h2>

    <p>Precio: {{ product.price }}</p>
    <p>Stock: {{ product.stock }}</p>

    <a href="{% url 'product_list' %}">Volver</a>
{% endblock %}
```

Comandos:

```bash
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

## Relación con otras librerías

`django` se relaciona especialmente con:

- `djangorestframework`, para construir APIs REST sobre Django
- `psycopg`, para conectar con PostgreSQL
- `mysqlclient`, para conectar con MySQL o MariaDB
- `python-dotenv`, para cargar variables de entorno
- `pillow`, para trabajar con campos de imagen
- `whitenoise`, para servir archivos estáticos en ciertos despliegues
- `gunicorn`, como servidor WSGI en producción
- `uvicorn`, cuando se trabaja con ASGI
- `pytest` y `pytest-django`, para pruebas
- `pandas`, cuando se procesan datos dentro de reportes o vistas
- `openpyxl`, cuando se generan archivos Excel desde vistas

## Orden didáctico interno

```text
1. Propósito de django
2. Instalación
3. Proyecto y aplicación
4. manage.py y comandos principales
5. settings.py
6. urls.py y URL dispatcher
7. Views
8. Templates
9. Modelos
10. Migraciones
11. ORM y QuerySets
12. Admin
13. Formularios
14. Archivos estáticos y media
15. Autenticación
16. Class-based views
17. Middleware
18. Testing
19. Estructura de proyecto
20. Comparación con Flask y FastAPI
21. Errores comunes
22. Buenas prácticas
```