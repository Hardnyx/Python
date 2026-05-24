# `mongoengine`

## Propósito

`mongoengine` es una librería externa para trabajar con MongoDB mediante un ODM.

ODM significa `Object-Document Mapper`.

Permite representar documentos de MongoDB como clases de Python, definir campos, validar datos, consultar colecciones mediante una API orientada a objetos y estructurar modelos documentales sin trabajar directamente con diccionarios en cada operación.

## Naturaleza de la librería

`mongoengine` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install mongoengine
```

Importaciones frecuentes:

```python
from mongoengine import Document
from mongoengine import StringField
from mongoengine import IntField
from mongoengine import FloatField
from mongoengine import BooleanField
from mongoengine import connect
```

`mongoengine` trabaja sobre MongoDB.

No se usa para conectarse a:

```text
PostgreSQL
MySQL
SQLite
SQL Server
Oracle Database
Redis
```

## Relación con MongoDB

MongoDB es una base de datos documental.

Trabaja con:

```text
bases de datos
colecciones
documentos
campos
índices
consultas documentales
```

`mongoengine` permite mapear documentos de MongoDB a clases de Python.

Concepto aproximado:

```text
colección de MongoDB -> clase Document
documento            -> instancia de la clase
campo del documento  -> Field de MongoEngine
```

## Relación con PyMongo

PyMongo es el driver oficial de bajo nivel para MongoDB en Python.

MongoEngine es una capa de mayor nivel orientada a modelos.

Comparación conceptual:

```text
pymongo      -> trabaja directamente con clientes, bases, colecciones y diccionarios
mongoengine  -> trabaja con clases, campos, validación y objetos
```

Ejemplo con PyMongo:

```python
collection.insert_one({
    "name": "Laptop",
    "price": 3500
})
```

Ejemplo con MongoEngine:

```python
product = Product(
    name="Laptop",
    price=3500
)

product.save()
```

## Cuándo usar MongoEngine

Conviene usar `mongoengine` cuando se necesita:

```text
trabajar con MongoDB usando clases
definir esquemas documentales
validar documentos antes de guardarlos
usar una API parecida a un ORM
modelar documentos principales
modelar documentos embebidos
usar referencias entre documentos
organizar mejor una aplicación con MongoDB
evitar manipular diccionarios en toda la aplicación
```

## Cuándo no usar MongoEngine

No siempre conviene usar `mongoengine` cuando se necesita:

```text
control directo y completo de PyMongo
operaciones muy específicas de MongoDB
máximo rendimiento con mínimo overhead
aplicaciones asíncronas puras
trabajar con bases SQL
usar Redis
hacer consultas analíticas tabulares
```

Para control directo puede corresponder:

```text
pymongo
pymongo async
```

Para aplicaciones asíncronas con ODM puede evaluarse:

```text
beanie
odmantic
```

Para bases relacionales pueden corresponder:

```text
sqlalchemy
sqlmodel
peewee
tortoise-orm
```

## Instalación

Instalación básica:

```bash
python -m pip install mongoengine
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
mongoengine==0.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import mongoengine

print(mongoengine.__version__)
```

## Conexión básica

```python
from mongoengine import connect

connect(
    db="app",
    host="localhost",
    port=27017
)
```

Con URI:

```python
connect(
    host="mongodb://localhost:27017/app"
)
```

## Conexión con usuario y contraseña

```python
from mongoengine import connect

connect(
    db="app",
    host="localhost",
    port=27017,
    username="user",
    password="password",
    authentication_source="admin"
)
```

## Conexión con MongoDB Atlas

```python
from mongoengine import connect

connect(
    host="mongodb+srv://user:password@cluster.mongodb.net/app"
)
```

En proyectos reales, la URI debe venir de variables de entorno.

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
connect(
    host="mongodb+srv://user:password@cluster.mongodb.net/app"
)
```

Más conveniente:

```python
import os

from mongoengine import connect

connect(
    host=os.getenv("MONGO_URI")
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
MONGO_URI=mongodb://localhost:27017/app
```

Código:

```python
import os

from dotenv import load_dotenv
from mongoengine import connect

load_dotenv()

connect(
    host=os.getenv("MONGO_URI")
)
```

## `Document`

`Document` es la clase base para definir documentos principales.

```python
from mongoengine import Document
from mongoengine import StringField
from mongoengine import FloatField
from mongoengine import IntField
from mongoengine import BooleanField


class Product(Document):
    name = StringField(required=True)
    price = FloatField(required=True)
    stock = IntField(default=0)
    is_active = BooleanField(default=True)
```

La clase `Product` representa una colección de MongoDB.

Cada instancia de `Product` representa un documento.

## Guardar documento

```python
product = Product(
    name="Laptop",
    price=3500,
    stock=5
)

product.save()
```

## Definir colección

MongoEngine puede inferir el nombre de la colección, pero también puede declararse explícitamente.

```python
class Product(Document):
    name = StringField(required=True)

    meta = {
        "collection": "products"
    }
```

## Campos frecuentes

```python
from mongoengine import StringField
from mongoengine import IntField
from mongoengine import FloatField
from mongoengine import DecimalField
from mongoengine import BooleanField
from mongoengine import DateTimeField
from mongoengine import ListField
from mongoengine import DictField
from mongoengine import EmbeddedDocumentField
from mongoengine import ReferenceField
```

Ejemplo:

```python
class Product(Document):
    sku = StringField(required=True, unique=True)
    name = StringField(required=True, max_length=100)
    price = DecimalField(required=True, precision=2)
    stock = IntField(default=0, min_value=0)
    is_active = BooleanField(default=True)
```

## Campos obligatorios

```python
name = StringField(required=True)
```

Si el campo no se informa, la validación puede fallar al guardar.

## Valores por defecto

```python
stock = IntField(default=0)
is_active = BooleanField(default=True)
```

## Campos únicos

```python
sku = StringField(required=True, unique=True)
```

Esto indica que el valor debe ser único en la colección.

## Longitud máxima

```python
name = StringField(
    required=True,
    max_length=100
)
```

## Valores mínimos y máximos

```python
stock = IntField(
    default=0,
    min_value=0
)
```

## Choices

```python
status = StringField(
    choices=[
        "active",
        "inactive",
        "archived"
    ],
    default="active"
)
```

## Fechas

```python
from datetime import datetime, timezone
from mongoengine import DateTimeField

created_at = DateTimeField(
    default=lambda: datetime.now(timezone.utc)
)
```

## Documento básico

```python
from mongoengine import BooleanField
from mongoengine import DecimalField
from mongoengine import Document
from mongoengine import IntField
from mongoengine import StringField


class Product(Document):
    sku = StringField(required=True, unique=True)
    name = StringField(required=True, max_length=100)
    price = DecimalField(required=True, precision=2)
    stock = IntField(default=0, min_value=0)
    is_active = BooleanField(default=True)

    meta = {
        "collection": "products"
    }
```

## Insertar documentos

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500,
    stock=5
)

product.save()
```

## Consultar todos los documentos

```python
products = Product.objects()

for product in products:
    print(product.name)
```

## `objects`

`objects` permite construir consultas sobre la colección asociada al documento.

```python
Product.objects()
```

Consulta filtrada:

```python
Product.objects(is_active=True)
```

## Consultar un documento

```python
product = Product.objects(sku="LAP-001").first()

if product is not None:
    print(product.name)
```

## `first()`

Devuelve el primer documento que coincide o `None`.

```python
product = Product.objects(sku="LAP-001").first()
```

## `get()`

Devuelve un documento, pero genera excepción si no encuentra o si encuentra más de uno.

```python
product = Product.objects.get(sku="LAP-001")
```

Uso con manejo de error:

```python
from mongoengine import DoesNotExist

try:
    product = Product.objects.get(sku="LAP-001")
except DoesNotExist:
    product = None
```

## Filtros simples

Igualdad:

```python
Product.objects(sku="LAP-001")
```

Mayor que:

```python
Product.objects(price__gt=100)
```

Mayor o igual que:

```python
Product.objects(price__gte=100)
```

Menor que:

```python
Product.objects(price__lt=5000)
```

Menor o igual que:

```python
Product.objects(price__lte=5000)
```

Distinto:

```python
Product.objects(name__ne="Laptop")
```

## Filtros con texto

Contiene:

```python
Product.objects(name__contains="Lap")
```

Contiene sin distinguir mayúsculas:

```python
Product.objects(name__icontains="lap")
```

Empieza con:

```python
Product.objects(name__startswith="Lap")
```

## Filtro con lista

```python
Product.objects(sku__in=[
    "LAP-001",
    "MOU-001"
])
```

## Ordenar resultados

Ascendente:

```python
products = Product.objects.order_by("name")
```

Descendente:

```python
products = Product.objects.order_by("-price")
```

## Limitar resultados

```python
products = Product.objects.limit(10)
```

## Saltar resultados

```python
products = Product.objects.skip(10).limit(10)
```

## Contar documentos

```python
count = Product.objects(is_active=True).count()

print(count)
```

## Actualizar documento

```python
product = Product.objects(sku="LAP-001").first()

if product is not None:
    product.price = 3600
    product.stock = 4
    product.save()
```

## Actualización directa

```python
Product.objects(sku="LAP-001").update_one(
    set__price=3600,
    set__stock=4
)
```

## Actualización masiva

```python
Product.objects(is_active=False).update(
    set__stock=0
)
```

## Incrementar campo

```python
Product.objects(sku="LAP-001").update_one(
    inc__stock=1
)
```

## Eliminar documento

```python
product = Product.objects(sku="LAP-001").first()

if product is not None:
    product.delete()
```

## Eliminación masiva

```python
Product.objects(is_active=False).delete()
```

## Eliminación lógica

En aplicaciones reales, muchas veces conviene marcar documentos como inactivos.

```python
Product.objects(sku="LAP-001").update_one(
    set__is_active=False
)
```

Consulta de activos:

```python
Product.objects(is_active=True)
```

## Validación

MongoEngine valida campos antes de guardar.

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500,
    stock=5
)

product.validate()
```

`save()` también ejecuta validación por defecto.

```python
product.save()
```

## Error de validación

```python
from mongoengine import ValidationError

try:
    product = Product(
        sku="",
        name="",
        price=-10
    )

    product.save()
except ValidationError as error:
    print(error)
```

## Validación personalizada

```python
from mongoengine import ValidationError


def validate_positive_price(value):
    if value <= 0:
        raise ValidationError("El precio debe ser mayor que cero")


class Product(Document):
    price = FloatField(
        required=True,
        validation=validate_positive_price
    )
```

## Método `clean()`

`clean()` permite aplicar lógica de validación o normalización antes de guardar.

```python
class Product(Document):
    sku = StringField(required=True)
    name = StringField(required=True)

    def clean(self):
        self.sku = self.sku.upper().strip()
        self.name = self.name.strip()
```

## Documentos embebidos

MongoDB permite documentos dentro de documentos.

En MongoEngine se usa `EmbeddedDocument`.

```python
from mongoengine import EmbeddedDocument
from mongoengine import EmbeddedDocumentField
from mongoengine import StringField


class Address(EmbeddedDocument):
    city = StringField(required=True)
    country = StringField(required=True)


class Client(Document):
    name = StringField(required=True)
    address = EmbeddedDocumentField(Address)
```

Uso:

```python
client = Client(
    name="Ana",
    address=Address(
        city="Lima",
        country="Perú"
    )
)

client.save()
```

## Lista de documentos embebidos

```python
from mongoengine import EmbeddedDocument
from mongoengine import EmbeddedDocumentListField
from mongoengine import StringField


class Phone(EmbeddedDocument):
    label = StringField(required=True)
    number = StringField(required=True)


class Client(Document):
    name = StringField(required=True)
    phones = EmbeddedDocumentListField(Phone)
```

Uso:

```python
client = Client(
    name="Ana",
    phones=[
        Phone(label="mobile", number="999999999"),
        Phone(label="home", number="1111111")
    ]
)

client.save()
```

## Referencias

MongoEngine permite referenciar otros documentos.

```python
from mongoengine import ReferenceField


class Category(Document):
    name = StringField(required=True, unique=True)


class Product(Document):
    name = StringField(required=True)
    category = ReferenceField(Category)
```

Uso:

```python
category = Category(name="Tecnología").save()

product = Product(
    name="Laptop",
    category=category
)

product.save()
```

## Acceder a referencia

```python
product = Product.objects(name="Laptop").first()

if product is not None:
    print(product.category.name)
```

## `ReferenceField` frente a documento embebido

## Documento embebido

Conviene cuando:

```text
el dato pertenece claramente al documento principal
se consulta junto con el documento principal
no necesita vida independiente
no se reutiliza en muchas partes
```

## Referencia

Conviene cuando:

```text
el documento relacionado tiene vida propia
se consulta por separado
se reutiliza entre varios documentos
puede crecer o cambiar independientemente
```

## Campos de lista

Lista de valores simples:

```python
from mongoengine import ListField
from mongoengine import StringField

tags = ListField(StringField())
```

Uso:

```python
product = Product(
    name="Laptop",
    tags=["tecnologia", "oferta"]
)
```

## Consultar listas

```python
Product.objects(tags="oferta")
```

## Agregar elemento a lista

```python
Product.objects(sku="LAP-001").update_one(
    push__tags="oferta"
)
```

## Eliminar elemento de lista

```python
Product.objects(sku="LAP-001").update_one(
    pull__tags="oferta"
)
```

## DictField

`DictField` permite guardar estructuras flexibles.

```python
from mongoengine import DictField

metadata = DictField()
```

Uso:

```python
product = Product(
    name="Laptop",
    metadata={
        "brand": "Marca",
        "warranty_months": 12
    }
)
```

Debe usarse con criterio, porque reduce el control del esquema.

## DynamicDocument

`DynamicDocument` permite documentos con campos no declarados.

```python
from mongoengine import DynamicDocument
from mongoengine import StringField


class Event(DynamicDocument):
    event_type = StringField(required=True)
```

Uso:

```python
event = Event(
    event_type="click",
    page="/home",
    user_agent="browser"
)

event.save()
```

## `Document` frente a `DynamicDocument`

## `Document`

Conviene cuando se quiere esquema claro.

```text
campos definidos
validación más controlada
estructura más predecible
```

## `DynamicDocument`

Conviene cuando se necesitan campos variables.

```text
eventos flexibles
payloads semiestructurados
prototipos
datos cambiantes
```

Debe usarse con cuidado para no perder consistencia.

## Índices

Los índices pueden declararse en `meta`.

```python
class Product(Document):
    sku = StringField(required=True, unique=True)
    name = StringField(required=True)
    is_active = BooleanField(default=True)

    meta = {
        "collection": "products",
        "indexes": [
            "sku",
            "name",
            "is_active"
        ]
    }
```

Índice compuesto:

```python
meta = {
    "indexes": [
        ("is_active", "name")
    ]
}
```

Índice descendente:

```python
meta = {
    "indexes": [
        "-price"
    ]
}
```

## Índice único compuesto

```python
meta = {
    "indexes": [
        {
            "fields": [
                "category",
                "sku"
            ],
            "unique": True
        }
    ]
}
```

## Crear índices

MongoEngine puede crear índices definidos en los modelos.

```python
Product.ensure_indexes()
```

En aplicaciones reales, los índices deben revisarse como parte del diseño de base de datos.

## Herencia

MongoEngine permite herencia documental.

```python
class Animal(Document):
    name = StringField(required=True)

    meta = {
        "allow_inheritance": True
    }


class Dog(Animal):
    breed = StringField()
```

Debe usarse con cuidado porque puede complicar consultas y estructura de documentos.

## Consultas con `Q`

`Q` permite construir condiciones más complejas.

```python
from mongoengine import Q

products = Product.objects(
    Q(name__icontains="lap") |
    Q(sku="LAP-001")
)
```

Condición `AND`:

```python
products = Product.objects(
    Q(is_active=True) &
    Q(price__gte=100)
)
```

## Proyección de campos

Para cargar solo algunos campos:

```python
products = Product.objects.only(
    "sku",
    "name",
    "price"
)
```

Para excluir campos:

```python
products = Product.objects.exclude(
    "metadata"
)
```

## Convertir documento a diccionario

MongoEngine no siempre devuelve directamente un diccionario limpio para API.

Una opción simple:

```python
def product_to_dict(product):
    return {
        "id": str(product.id),
        "sku": product.sku,
        "name": product.name,
        "price": float(product.price),
        "stock": product.stock,
        "is_active": product.is_active
    }
```

Uso:

```python
product = Product.objects(sku="LAP-001").first()

if product is not None:
    data = product_to_dict(product)
```

## Convertir varios documentos

```python
products = Product.objects(is_active=True)

data = [
    product_to_dict(product)
    for product in products
]
```

## `to_json()`

MongoEngine permite convertir documentos a JSON.

```python
product = Product.objects(sku="LAP-001").first()

if product is not None:
    json_data = product.to_json()
```

Puede ser útil, pero en APIs suele convenir controlar explícitamente la forma de salida.

## Manejo de errores

Importaciones frecuentes:

```python
from mongoengine import DoesNotExist
from mongoengine import MultipleObjectsReturned
from mongoengine import NotUniqueError
from mongoengine import ValidationError
from mongoengine import OperationError
```

## Documento no encontrado

```python
from mongoengine import DoesNotExist

try:
    product = Product.objects.get(sku="LAP-001")
except DoesNotExist:
    product = None
```

## Más de un documento encontrado

```python
from mongoengine import MultipleObjectsReturned

try:
    product = Product.objects.get(name="Laptop")
except MultipleObjectsReturned:
    print("Existe más de un producto con ese nombre")
```

## Error de unicidad

```python
from mongoengine import NotUniqueError

try:
    Product(
        sku="LAP-001",
        name="Laptop",
        price=3500
    ).save()
except NotUniqueError:
    print("El SKU ya existe")
```

## Error de validación

```python
from mongoengine import ValidationError

try:
    product.validate()
except ValidationError as error:
    print(error)
```

## Uso con Flask

```python
import os

from flask import Flask
from mongoengine import connect

app = Flask(__name__)

connect(
    host=os.getenv("MONGO_URI", "mongodb://localhost:27017/app")
)


@app.route("/products")
def read_products():
    products = Product.objects(is_active=True).order_by("name")

    return [
        product_to_dict(product)
        for product in products
    ]
```

## Uso con FastAPI

MongoEngine es síncrono. Puede usarse en FastAPI, pero las operaciones bloquean el hilo donde se ejecutan.

Ejemplo simple:

```python
import os
from contextlib import asynccontextmanager

from fastapi import FastAPI, HTTPException
from mongoengine import connect
from mongoengine import disconnect


@asynccontextmanager
async def lifespan(app: FastAPI):
    connect(
        host=os.getenv("MONGO_URI", "mongodb://localhost:27017/app")
    )

    yield

    disconnect()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
def read_products():
    products = Product.objects(is_active=True).order_by("name")

    return [
        product_to_dict(product)
        for product in products
    ]


@app.get("/products/{sku}")
def read_product(sku: str):
    product = Product.objects(sku=sku, is_active=True).first()

    if product is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return product_to_dict(product)
```

Para aplicaciones asíncronas puras con MongoDB, puede evaluarse PyMongo async, Beanie u ODMantic.

## Uso con Pydantic

MongoEngine valida persistencia, pero en APIs suele convenir validar entrada con Pydantic.

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    sku: str = Field(min_length=1)
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

Uso:

```python
def create_product(product_create: ProductCreate):
    product = Product(
        sku=product_create.sku,
        name=product_create.name,
        price=product_create.price,
        stock=product_create.stock
    )

    product.save()

    return product
```

## Uso con pandas

MongoEngine puede alimentar un DataFrame.

```python
import pandas as pd

products = Product.objects(is_active=True)

rows = [
    product_to_dict(product)
    for product in products
]

df = pd.DataFrame(rows)

print(df.head())
```

Para análisis grandes, conviene consultar solo campos necesarios y revisar si PyMongo directo o agregaciones de MongoDB serían más apropiadas.

## Agregaciones

MongoEngine permite usar agregaciones de MongoDB mediante `aggregate()`.

```python
pipeline = [
    {
        "$match": {
            "is_active": True
        }
    },
    {
        "$group": {
            "_id": "$category",
            "count": {
                "$sum": 1
            }
        }
    }
]

results = Product.objects.aggregate(pipeline)

for row in results:
    print(row)
```

Para agregaciones complejas, muchas veces PyMongo directo puede ser más claro.

## Acceso a colección PyMongo

En algunos casos puede ser necesario acceder a la colección subyacente.

```python
collection = Product._get_collection()

result = collection.find_one({
    "sku": "LAP-001"
})
```

Debe usarse con cuidado, porque se sale parcialmente de la abstracción de MongoEngine.

## Organización recomendada

```text
app/
├─ database.py
├─ models.py
├─ schemas.py
├─ repositories.py
├─ services.py
└─ main.py
```

## `database.py`

```python
import os

from dotenv import load_dotenv
from mongoengine import connect
from mongoengine import disconnect

load_dotenv()


def connect_database():
    connect(
        host=os.getenv("MONGO_URI", "mongodb://localhost:27017/app")
    )


def disconnect_database():
    disconnect()
```

## `models.py`

```python
from mongoengine import BooleanField
from mongoengine import DecimalField
from mongoengine import Document
from mongoengine import IntField
from mongoengine import StringField


class Product(Document):
    sku = StringField(required=True, unique=True)
    name = StringField(required=True, max_length=100)
    price = DecimalField(required=True, precision=2)
    stock = IntField(default=0, min_value=0)
    is_active = BooleanField(default=True)

    meta = {
        "collection": "products",
        "indexes": [
            "sku",
            ("is_active", "name")
        ]
    }
```

## `schemas.py`

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    sku: str = Field(min_length=1)
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)


class ProductUpdate(BaseModel):
    name: str | None = None
    price: float | None = None
    stock: int | None = None
    is_active: bool | None = None
```

## `repositories.py`

```python
from .models import Product


def product_to_dict(product):
    return {
        "id": str(product.id),
        "sku": product.sku,
        "name": product.name,
        "price": float(product.price),
        "stock": product.stock,
        "is_active": product.is_active
    }


def list_products():
    products = Product.objects(
        is_active=True
    ).order_by("name")

    return [
        product_to_dict(product)
        for product in products
    ]


def get_product(sku):
    product = Product.objects(
        sku=sku,
        is_active=True
    ).first()

    if product is None:
        return None

    return product_to_dict(product)


def create_product(product_create):
    product = Product(
        sku=product_create.sku,
        name=product_create.name,
        price=product_create.price,
        stock=product_create.stock
    )

    product.save()

    return product_to_dict(product)


def update_stock(sku, stock):
    updated = Product.objects(
        sku=sku
    ).update_one(
        set__stock=stock
    )

    return updated


def deactivate_product(sku):
    updated = Product.objects(
        sku=sku
    ).update_one(
        set__is_active=False
    )

    return updated
```

## Separación por responsabilidades

```text
database.py      -> conexión y desconexión
models.py        -> documentos MongoEngine
schemas.py       -> validación de entrada
repositories.py  -> consultas y persistencia
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Migraciones y cambios de esquema

MongoEngine no gestiona migraciones como una base relacional con Alembic.

En MongoDB, los documentos pueden tener estructuras distintas, pero en proyectos reales conviene controlar cambios.

Estrategias comunes:

```text
scripts de migración propios
versionado de documentos
campos opcionales temporales
procesos de backfill
validaciones progresivas
```

Ejemplo conceptual:

```python
for product in Product.objects(category__exists=False):
    product.category = "general"
    product.save()
```

## Errores comunes

## Instalar el paquete y tratar de importar otro nombre

Instalación correcta:

```bash
python -m pip install mongoengine
```

Importación correcta:

```python
import mongoengine
```

o:

```python
from mongoengine import Document
```

No se importa como:

```python
import mongo_engine
```

## No conectar antes de consultar

Debe ejecutarse `connect()` antes de usar modelos.

```python
connect(host="mongodb://localhost:27017/app")
```

## Usar MongoEngine esperando una API asíncrona

MongoEngine se usa normalmente de forma síncrona.

Para aplicaciones `asyncio`, evaluar si corresponde usar una alternativa asíncrona.

## Usar `get()` cuando puede no existir

Problemático:

```python
product = Product.objects.get(sku="LAP-001")
```

si no se maneja `DoesNotExist`.

Más seguro:

```python
product = Product.objects(sku="LAP-001").first()
```

## No manejar `NotUniqueError`

Si hay campos únicos, las inserciones duplicadas pueden fallar.

```python
except NotUniqueError:
    ...
```

## Abusar de `DictField`

`DictField` es flexible, pero puede debilitar el esquema si se usa para todo.

## Usar referencias como si fueran joins relacionales

MongoDB no se debe modelar exactamente como una base SQL.

Si se necesitan muchas relaciones y joins, conviene revisar el diseño.

## No crear índices

Consultas frecuentes sin índices pueden ser lentas.

Declarar índices en `meta` ayuda a documentar el diseño.

## Devolver documentos MongoEngine directamente en APIs

Problemático:

```python
return product
```

Mejor convertir:

```python
return product_to_dict(product)
```

## No convertir `ObjectId` a texto

Para APIs JSON:

```python
"id": str(product.id)
```

## Buenas prácticas

## Definir modelos claros

```python
class Product(Document):
    ...
```

## Usar campos obligatorios donde corresponda

```python
required=True
```

## Usar índices para consultas frecuentes

```python
meta = {
    "indexes": [...]
}
```

## Usar documentos embebidos cuando el dato pertenece al documento principal

```python
EmbeddedDocument
EmbeddedDocumentField
```

## Usar referencias cuando el documento tiene vida propia

```python
ReferenceField
```

## Validar entrada con Pydantic en APIs

```python
ProductCreate
```

## Convertir salidas a diccionarios

```python
product_to_dict(product)
```

## Manejar errores específicos

```python
ValidationError
NotUniqueError
DoesNotExist
```

## Usar variables de entorno

```text
MONGO_URI
```

## No tratar MongoDB como SQL

El diseño debe basarse en documentos, consultas esperadas y patrones de acceso.

## Ejemplo integrado

```python
import os
from decimal import Decimal

from dotenv import load_dotenv
from mongoengine import BooleanField
from mongoengine import DecimalField
from mongoengine import Document
from mongoengine import IntField
from mongoengine import NotUniqueError
from mongoengine import StringField
from mongoengine import ValidationError
from mongoengine import connect
from mongoengine import disconnect

load_dotenv()


class Product(Document):
    sku = StringField(required=True, unique=True)
    name = StringField(required=True, max_length=100)
    price = DecimalField(required=True, precision=2)
    stock = IntField(default=0, min_value=0)
    is_active = BooleanField(default=True)

    meta = {
        "collection": "products",
        "indexes": [
            "sku",
            ("is_active", "name")
        ]
    }

    def clean(self):
        self.sku = self.sku.upper().strip()
        self.name = self.name.strip()


def connect_database():
    connect(
        host=os.getenv("MONGO_URI", "mongodb://localhost:27017/app")
    )


def product_to_dict(product):
    return {
        "id": str(product.id),
        "sku": product.sku,
        "name": product.name,
        "price": float(product.price),
        "stock": product.stock,
        "is_active": product.is_active
    }


def create_product(sku, name, price, stock=0):
    product = Product(
        sku=sku,
        name=name,
        price=Decimal(str(price)),
        stock=stock
    )

    try:
        product.save()
    except NotUniqueError:
        raise ValueError("El SKU ya existe")
    except ValidationError as error:
        raise ValueError(str(error))

    return product_to_dict(product)


def list_products():
    products = Product.objects(
        is_active=True
    ).order_by("name")

    return [
        product_to_dict(product)
        for product in products
    ]


def get_product(sku):
    product = Product.objects(
        sku=sku,
        is_active=True
    ).first()

    if product is None:
        return None

    return product_to_dict(product)


def update_stock(sku, stock):
    updated = Product.objects(
        sku=sku
    ).update_one(
        set__stock=stock
    )

    return updated


def deactivate_product(sku):
    updated = Product.objects(
        sku=sku
    ).update_one(
        set__is_active=False
    )

    return updated


def main():
    connect_database()

    try:
        Product.ensure_indexes()

        try:
            create_product(
                sku="LAP-001",
                name="Laptop",
                price=3500,
                stock=5
            )

            create_product(
                sku="MOU-001",
                name="Mouse",
                price=80,
                stock=20
            )
        except ValueError:
            pass

        products = list_products()

        for product in products:
            print(
                product["sku"],
                product["name"],
                product["price"],
                product["stock"]
            )

        update_stock(
            sku="LAP-001",
            stock=4
        )

        product = get_product("LAP-001")

        print(product)

    finally:
        disconnect()


if __name__ == "__main__":
    main()
```

## Ejemplo con documentos embebidos y referencias

```python
from mongoengine import Document
from mongoengine import EmbeddedDocument
from mongoengine import EmbeddedDocumentField
from mongoengine import ListField
from mongoengine import ReferenceField
from mongoengine import StringField


class Address(EmbeddedDocument):
    city = StringField(required=True)
    country = StringField(required=True)


class Category(Document):
    name = StringField(required=True, unique=True)


class Client(Document):
    name = StringField(required=True)
    address = EmbeddedDocumentField(Address)


class Product(Document):
    name = StringField(required=True)
    category = ReferenceField(Category)
    tags = ListField(StringField())


category = Category(name="Tecnología").save()

product = Product(
    name="Laptop",
    category=category,
    tags=[
        "computadora",
        "oficina"
    ]
)

product.save()

client = Client(
    name="Ana",
    address=Address(
        city="Lima",
        country="Perú"
    )
)

client.save()
```

## Relación con otras librerías

`mongoengine` se relaciona especialmente con:

```text
mongodb
pymongo
flask
fastapi
pydantic
pandas
python-dotenv
pytest
```

## Relación con PyMongo

PyMongo ofrece acceso directo a MongoDB.

MongoEngine ofrece una capa ODM con clases, campos y validación.

## Relación con Flask

MongoEngine se usa con frecuencia en aplicaciones Flask cuando se desea una capa de modelos sencilla sobre MongoDB.

## Relación con FastAPI

Puede usarse en FastAPI, pero al ser síncrono debe evaluarse el impacto en aplicaciones asíncronas.

## Relación con Pydantic

Pydantic puede validar entrada y salida en APIs, mientras MongoEngine modela persistencia en MongoDB.

## Relación con pandas

MongoEngine puede alimentar DataFrames convirtiendo documentos a diccionarios.

## Relación con Beanie y ODMantic

Beanie y ODMantic son alternativas orientadas a MongoDB con un enfoque más alineado con Pydantic y aplicaciones asíncronas.

## Orden didáctico interno

```text
1. Propósito de mongoengine
2. Instalación
3. Relación con MongoDB y PyMongo
4. Conexión
5. Document
6. Campos
7. Validación
8. Guardar documentos
9. Consultar con objects
10. Filtros
11. Actualizaciones
12. Eliminación y eliminación lógica
13. Documentos embebidos
14. Referencias
15. ListField y DictField
16. DynamicDocument
17. Índices
18. Q objects
19. Conversión a diccionarios y JSON
20. Uso con Flask, FastAPI, Pydantic y pandas
21. Organización recomendada
22. Migraciones y cambios de esquema
23. Errores comunes
24. Buenas prácticas
```