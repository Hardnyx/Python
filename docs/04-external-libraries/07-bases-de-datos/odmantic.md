# `odmantic`

## Propósito

`odmantic` es una librería externa para trabajar con MongoDB mediante un ODM.

ODM significa `Object-Document Mapper`.

Permite representar documentos de MongoDB como clases de Python, validar datos con Pydantic, ejecutar consultas usando operadores de Python, guardar documentos, consultar colecciones, actualizar instancias, eliminar registros y organizar el acceso a MongoDB con una capa de modelos.

## Naturaleza de la librería

`odmantic` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install odmantic
```

Importaciones frecuentes:

```python
from odmantic import AIOEngine
from odmantic import EmbeddedModel
from odmantic import Field
from odmantic import Index
from odmantic import Model
from odmantic import Reference
from odmantic import SyncEngine
```

`odmantic` trabaja con MongoDB.

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

`odmantic` permite mapear documentos de MongoDB a clases de Python.

Relación conceptual:

```text
colección de MongoDB -> clase Model
documento            -> instancia de Model
campo del documento  -> atributo tipado de Python
validación           -> Pydantic
```

## Relación con Pydantic

ODMantic usa Pydantic para definir, validar y serializar modelos.

Ejemplo:

```python
from odmantic import Field, Model


class Product(Model):
    sku: str = Field(unique=True)
    name: str
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True
```

La clase `Product` funciona como modelo de datos y como documento persistente.

## Relación con PyMongo y Motor

ODMantic no reemplaza completamente a PyMongo o Motor.

Más bien, ofrece una capa de modelos sobre clientes de MongoDB.

En modo asíncrono puede usarse con `AIOEngine`.

```python
from odmantic import AIOEngine

engine = AIOEngine()
```

En modo síncrono puede usarse con `SyncEngine`.

```python
from odmantic import SyncEngine

engine = SyncEngine()
```

También puede recibir clientes personalizados.

```python
from motor.motor_asyncio import AsyncIOMotorClient
from odmantic import AIOEngine

client = AsyncIOMotorClient("mongodb://localhost:27017")

engine = AIOEngine(
    client=client,
    database="app"
)
```

Modo síncrono:

```python
from pymongo import MongoClient
from odmantic import SyncEngine

client = MongoClient("mongodb://localhost:27017")

engine = SyncEngine(
    client=client,
    database="app"
)
```

## Diferencia frente a PyMongo

## PyMongo

Trabaja directamente con bases, colecciones y diccionarios.

```python
await collection.insert_one({
    "sku": "LAP-001",
    "name": "Laptop",
    "price": 3500
})
```

## ODMantic

Trabaja con modelos declarativos.

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500
)

await engine.save(product)
```

## Regla práctica

Para control directo sobre MongoDB:

```text
pymongo
```

Para modelos tipados, validación y consultas orientadas a objetos:

```text
odmantic
```

## Cuándo usar ODMantic

Conviene usar `odmantic` cuando se necesita:

```text
trabajar con MongoDB usando clases
validar documentos con Pydantic
usar type hints de Python
crear modelos documentales claros
consultar MongoDB con operadores de Python
integrar MongoDB con FastAPI
usar modo síncrono o asíncrono
definir índices desde modelos
trabajar con documentos embebidos
usar referencias entre documentos
organizar una capa de persistencia
```

## Cuándo no usar ODMantic

No siempre conviene usar `odmantic` cuando se necesita:

```text
control directo de bajo nivel
operaciones muy específicas de MongoDB
evitar cualquier abstracción ODM
trabajar con bases SQL
usar Redis
hacer análisis tabular local
usar migraciones de esquema integradas
usar un ORM relacional
```

Para control directo puede corresponder:

```text
pymongo
pymongo async
```

Para otros ODM de MongoDB puede evaluarse:

```text
beanie
mongoengine
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
python -m pip install odmantic
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
odmantic==1.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import odmantic

print(odmantic.__version__)
```

## Modelo básico

```python
from odmantic import Field, Model


class Product(Model):
    sku: str = Field(unique=True)
    name: str
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True
```

## `Model`

`Model` es la clase base para definir documentos persistentes.

```python
from odmantic import Model
```

Ejemplo:

```python
class Product(Model):
    ...
```

Cada clase que hereda de `Model` representa una colección de MongoDB.

## Campos

Los campos se definen con anotaciones de tipo de Python.

```python
class Product(Model):
    sku: str
    name: str
    price: float
    stock: int = 0
    is_active: bool = True
```

Campos frecuentes:

```text
str
int
float
bool
datetime
list
dict
EmbeddedModel
Model referenciado
```

## Campos obligatorios

Un campo sin valor por defecto es obligatorio.

```python
class Product(Model):
    sku: str
    name: str
    price: float
```

## Campos opcionales

```python
class Product(Model):
    description: str | None = None
```

## Valores por defecto

```python
class Product(Model):
    stock: int = 0
    is_active: bool = True
```

## Uso de `Field`

`Field` permite configurar validaciones y propiedades del campo.

```python
from odmantic import Field


class Product(Model):
    sku: str = Field(unique=True)
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

## Validaciones frecuentes

```python
name: str = Field(min_length=1, max_length=100)
price: float = Field(gt=0)
stock: int = Field(default=0, ge=0)
```

## Campo único

```python
sku: str = Field(unique=True)
```

Esto crea una restricción lógica mediante índice único cuando la base se configura con los índices del modelo.

## Campo indexado

```python
name: str = Field(index=True)
```

Esto declara que el campo debe tener índice.

## Nombre de clave personalizado

Puede definirse el nombre real del campo en MongoDB.

```python
class Product(Model):
    sku: str = Field(key_name="product_sku")
```

En Python se usa:

```python
product.sku
```

En MongoDB se guarda con la clave:

```text
product_sku
```

## Nombre de colección

Por defecto, ODMantic deriva el nombre de colección desde el nombre de la clase.

Ejemplo conceptual:

```text
Product      -> product
CapitalCity  -> capital_city
ProductModel -> product
```

Puede personalizarse con `model_config`.

```python
class Product(Model):
    sku: str
    name: str

    model_config = {
        "collection": "products"
    }
```

## `model_config`

`model_config` permite configurar opciones del modelo.

Ejemplo:

```python
class Product(Model):
    sku: str = Field(unique=True)
    name: str
    stock: int = 0

    model_config = {
        "collection": "products"
    }
```

También puede usarse para índices.

```python
class Product(Model):
    sku: str = Field(unique=True)
    name: str
    stock: int

    model_config = {
        "collection": "products",
        "indexes": lambda: [
            Index(Product.name, Product.stock)
        ]
    }
```

## AIOEngine

`AIOEngine` permite trabajar con MongoDB de forma asíncrona.

```python
from odmantic import AIOEngine

engine = AIOEngine()
```

Por defecto intenta conectarse a MongoDB local.

```text
localhost:27017
```

## Crear `AIOEngine` con cliente personalizado

```python
from motor.motor_asyncio import AsyncIOMotorClient
from odmantic import AIOEngine

client = AsyncIOMotorClient(
    "mongodb://localhost:27017"
)

engine = AIOEngine(
    client=client,
    database="app"
)
```

## SyncEngine

`SyncEngine` permite trabajar con MongoDB de forma síncrona.

```python
from odmantic import SyncEngine

engine = SyncEngine()
```

Con cliente personalizado:

```python
from pymongo import MongoClient
from odmantic import SyncEngine

client = MongoClient(
    "mongodb://localhost:27017"
)

engine = SyncEngine(
    client=client,
    database="app"
)
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

```python
import os

from motor.motor_asyncio import AsyncIOMotorClient
from odmantic import AIOEngine

client = AsyncIOMotorClient(
    os.getenv("MONGO_URI", "mongodb://localhost:27017")
)

engine = AIOEngine(
    client=client,
    database=os.getenv("MONGO_DATABASE", "app")
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
MONGO_URI=mongodb://localhost:27017
MONGO_DATABASE=app
```

Código:

```python
import os

from dotenv import load_dotenv
from motor.motor_asyncio import AsyncIOMotorClient
from odmantic import AIOEngine

load_dotenv()

client = AsyncIOMotorClient(
    os.getenv("MONGO_URI", "mongodb://localhost:27017")
)

engine = AIOEngine(
    client=client,
    database=os.getenv("MONGO_DATABASE", "app")
)
```

## Crear documento

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500,
    stock=5
)
```

En este punto, el objeto existe solo en memoria.

## Guardar documento

Modo asíncrono:

```python
await engine.save(product)
```

Modo síncrono:

```python
engine.save(product)
```

`save()` funciona como operación de guardado. Si el documento ya existe con el mismo identificador, se actualiza.

## Guardar varios documentos

Modo asíncrono:

```python
products = [
    Product(
        sku="LAP-001",
        name="Laptop",
        price=3500,
        stock=5
    ),
    Product(
        sku="MOU-001",
        name="Mouse",
        price=80,
        stock=20
    )
]

await engine.save_all(products)
```

Modo síncrono:

```python
engine.save_all(products)
```

## Consultar todos los documentos

Modo asíncrono:

```python
products = await engine.find(Product)
```

Modo síncrono:

```python
products = engine.find(Product)
```

Uso:

```python
for product in products:
    print(product.name)
```

## Consultar con filtro

```python
products = await engine.find(
    Product,
    Product.is_active == True
)
```

## Consultar un documento

```python
product = await engine.find_one(
    Product,
    Product.sku == "LAP-001"
)

if product is not None:
    print(product.name)
```

Si no encuentra coincidencias, devuelve `None`.

## Consultar por ID

```python
product = await engine.find_one(
    Product,
    Product.id == product_id
)
```

## Filtros simples

Igualdad:

```python
Product.sku == "LAP-001"
```

Mayor que:

```python
Product.price > 100
```

Mayor o igual que:

```python
Product.price >= 100
```

Menor que:

```python
Product.price < 5000
```

Menor o igual que:

```python
Product.price <= 5000
```

Distinto:

```python
Product.name != "Laptop"
```

## Varias condiciones

Las condiciones pueden pasarse como argumentos separados.

```python
products = await engine.find(
    Product,
    Product.is_active == True,
    Product.price >= 100
)
```

ODMantic las combina como una condición `AND`.

## Operadores lógicos

ODMantic permite construir condiciones con helpers de consulta.

```python
from odmantic.query import and_
from odmantic.query import or_


products = await engine.find(
    Product,
    or_(
        Product.sku == "LAP-001",
        Product.sku == "MOU-001"
    )
)
```

Condición `AND` explícita:

```python
products = await engine.find(
    Product,
    and_(
        Product.is_active == True,
        Product.price >= 100
    )
)
```

## Ordenar resultados

Ascendente:

```python
products = await engine.find(
    Product,
    sort=Product.name
)
```

Descendente:

```python
products = await engine.find(
    Product,
    sort=-Product.price
)
```

También puede usarse:

```python
from odmantic.query import asc
from odmantic.query import desc

products = await engine.find(
    Product,
    sort=asc(Product.name)
)

products = await engine.find(
    Product,
    sort=desc(Product.price)
)
```

## Limitar resultados

```python
products = await engine.find(
    Product,
    limit=10
)
```

## Saltar resultados

```python
products = await engine.find(
    Product,
    skip=10,
    limit=10
)
```

## Contar documentos

```python
count = await engine.count(
    Product,
    Product.is_active == True
)

print(count)
```

## Actualizar documento

Una forma común de actualizar es cargar el documento, modificarlo y volver a guardarlo.

```python
product = await engine.find_one(
    Product,
    Product.sku == "LAP-001"
)

if product is not None:
    product.price = 3600
    product.stock = 4

    await engine.save(product)
```

## Actualización con `model_update()`

Para aplicar un conjunto de cambios, puede usarse `model_update()`.

```python
from pydantic import BaseModel


class ProductPatch(BaseModel):
    price: float
    stock: int


product = await engine.find_one(
    Product,
    Product.sku == "LAP-001"
)

if product is not None:
    patch = ProductPatch(
        price=3600,
        stock=4
    )

    product.model_update(patch)

    await engine.save(product)
```

También puede aplicarse un diccionario.

```python
product.model_update({
    "price": 3600,
    "stock": 4
})
```

## Eliminar documento

```python
product = await engine.find_one(
    Product,
    Product.sku == "LAP-001"
)

if product is not None:
    await engine.delete(product)
```

## Eliminar por filtro

```python
deleted_count = await engine.remove(
    Product,
    Product.is_active == False
)

print(deleted_count)
```

## Eliminar solo uno

```python
deleted_count = await engine.remove(
    Product,
    Product.sku == "LAP-001",
    just_one=True
)
```

## Eliminación lógica

En aplicaciones reales, muchas veces conviene marcar documentos como inactivos.

```python
product = await engine.find_one(
    Product,
    Product.sku == "LAP-001"
)

if product is not None:
    product.is_active = False

    await engine.save(product)
```

Consulta de activos:

```python
products = await engine.find(
    Product,
    Product.is_active == True
)
```

## Índices

ODMantic permite definir índices desde los campos o desde `model_config`.

Índice simple:

```python
class Product(Model):
    sku: str = Field(unique=True)
    name: str = Field(index=True)
    price: float
```

Índice compuesto:

```python
class Product(Model):
    name: str
    stock: int
    category: str

    model_config = {
        "indexes": lambda: [
            Index(Product.name, Product.stock)
        ]
    }
```

## Crear índices en la base

Para crear índices declarados en los modelos debe configurarse la base.

Modo asíncrono:

```python
await engine.configure_database([
    Product
])
```

Modo síncrono:

```python
engine.configure_database([
    Product
])
```

## Índice único compuesto

```python
class Product(Model):
    sku: str
    category: str
    name: str

    model_config = {
        "indexes": lambda: [
            Index(Product.category, Product.sku, unique=True)
        ]
    }
```

## Índices avanzados con PyMongo

Para índices más avanzados puede usarse `pymongo.IndexModel`.

```python
import pymongo

from odmantic import Model


class Post(Model):
    title: str
    content: str

    model_config = {
        "indexes": lambda: [
            pymongo.IndexModel([
                (+Post.title, pymongo.TEXT),
                (+Post.content, pymongo.TEXT)
            ])
        ]
    }
```

## Documentos embebidos

Los documentos embebidos se definen con `EmbeddedModel`.

```python
from odmantic import EmbeddedModel
from odmantic import Model


class Address(EmbeddedModel):
    city: str
    country: str


class Client(Model):
    name: str
    address: Address
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

await engine.save(client)
```

## Lista de documentos embebidos

```python
from odmantic import EmbeddedModel
from odmantic import Model


class Phone(EmbeddedModel):
    label: str
    number: str


class Client(Model):
    name: str
    phones: list[Phone]
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

await engine.save(client)
```

## Consultar por documento embebido

Puede consultarse por campos embebidos.

```python
client = await engine.find_one(
    Client,
    Client.address.city == "Lima"
)
```

## Referencias

ODMantic permite referenciar otros modelos.

```python
from odmantic import Model
from odmantic import Reference


class Category(Model):
    name: str


class Product(Model):
    name: str
    category: Category = Reference()
```

Uso:

```python
category = Category(name="Tecnología")

product = Product(
    name="Laptop",
    category=category
)

await engine.save(product)
```

Cuando se guarda el documento principal, ODMantic puede persistir también el documento referenciado.

## Documento embebido frente a referencia

## Documento embebido

Conviene cuando:

```text
el dato pertenece al documento principal
se consulta siempre junto al documento principal
no tiene vida independiente
no se reutiliza demasiado
```

## Referencia

Conviene cuando:

```text
el documento relacionado tiene vida propia
se consulta por separado
se reutiliza entre varios documentos
puede crecer o cambiar independientemente
```

## Sesiones

ODMantic permite usar sesiones mediante el engine.

Modo asíncrono:

```python
async with engine.session() as session:
    product = Product(
        sku="LAP-001",
        name="Laptop",
        price=3500
    )

    await session.save(product)
```

Modo síncrono:

```python
with engine.session() as session:
    product = Product(
        sku="LAP-001",
        name="Laptop",
        price=3500
    )

    session.save(product)
```

Las sesiones pueden ayudar a mantener consistencia entre operaciones relacionadas.

## Transacciones

Puede usarse transacción desde una sesión cuando el entorno de MongoDB lo soporta.

```python
async with engine.session() as session:
    async with session.transaction():
        product = Product(
            sku="LAP-001",
            name="Laptop",
            price=3500
        )

        await session.save(product)
```

MongoDB requiere ciertas condiciones para transacciones, como replica set o clúster compatible.

## Acceso a colección cruda

ODMantic permite acceder a la colección subyacente.

```python
collection = engine.get_collection(Product)
```

En modo asíncrono devuelve una colección compatible con Motor.

En modo síncrono devuelve una colección compatible con PyMongo.

Esto puede ser útil para:

```text
agregaciones avanzadas
operaciones específicas de MongoDB
consultas no cubiertas por el ODM
diagnóstico
```

## Consulta cruda

```python
collection = engine.get_collection(Product)

documents = await collection.find({
    "price": {
        "$gte": 100
    }
}).to_list(length=None)
```

Luego puede validarse un documento crudo como modelo.

```python
products = [
    Product.model_validate_doc(document)
    for document in documents
]
```

## Documento crudo desde modelo

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500
)

document = product.model_dump_doc()

print(document)
```

## Agregaciones

Para agregaciones complejas puede usarse la colección cruda.

```python
collection = engine.get_collection(Product)

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

results = await collection.aggregate(
    pipeline
).to_list(length=None)
```

## Conversión a diccionario

Como ODMantic usa Pydantic, puede usarse `model_dump()`.

```python
product = await engine.find_one(
    Product,
    Product.sku == "LAP-001"
)

if product is not None:
    data = product.model_dump()
```

Para salidas JSON, conviene controlar explícitamente el identificador.

```python
def product_to_dict(product):
    return {
        "id": str(product.id),
        "sku": product.sku,
        "name": product.name,
        "price": product.price,
        "stock": product.stock,
        "is_active": product.is_active
    }
```

## Uso con FastAPI

ODMantic puede usarse con FastAPI, especialmente con `AIOEngine`.

```python
import os
from contextlib import asynccontextmanager

from fastapi import FastAPI, HTTPException
from motor.motor_asyncio import AsyncIOMotorClient
from odmantic import AIOEngine, Field, Model
from pydantic import BaseModel


class Product(Model):
    sku: str = Field(unique=True)
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True

    model_config = {
        "collection": "products"
    }


class ProductCreate(BaseModel):
    sku: str
    name: str
    price: float
    stock: int = 0


def product_to_dict(product):
    return {
        "id": str(product.id),
        "sku": product.sku,
        "name": product.name,
        "price": product.price,
        "stock": product.stock,
        "is_active": product.is_active
    }


@asynccontextmanager
async def lifespan(app: FastAPI):
    client = AsyncIOMotorClient(
        os.getenv("MONGO_URI", "mongodb://localhost:27017")
    )

    app.state.mongo_client = client
    app.state.engine = AIOEngine(
        client=client,
        database=os.getenv("MONGO_DATABASE", "app")
    )

    await app.state.engine.configure_database([
        Product
    ])

    yield

    client.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    products = await app.state.engine.find(
        Product,
        Product.is_active == True,
        sort=Product.name
    )

    return [
        product_to_dict(product)
        for product in products
    ]


@app.get("/products/{sku}")
async def read_product(sku: str):
    product = await app.state.engine.find_one(
        Product,
        Product.sku == sku,
        Product.is_active == True
    )

    if product is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return product_to_dict(product)


@app.post("/products")
async def create_product(product_create: ProductCreate):
    product = Product(
        **product_create.model_dump()
    )

    await app.state.engine.save(product)

    return product_to_dict(product)
```

## Uso con modelos Pydantic de entrada

Aunque `Model` valida datos, en APIs suele convenir separar entrada y persistencia.

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    sku: str = Field(min_length=1)
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

Uso:

```python
product = Product(
    **product_create.model_dump()
)
```

## Uso síncrono

ODMantic también permite uso síncrono.

```python
from odmantic import Field, Model, SyncEngine


class Product(Model):
    sku: str = Field(unique=True)
    name: str
    price: float
    stock: int = 0


engine = SyncEngine()

product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500,
    stock=5
)

engine.save(product)

products = engine.find(Product)

for product in products:
    print(product.name)
```

## Uso con pandas

ODMantic puede alimentar un DataFrame.

```python
import pandas as pd

products = await engine.find(
    Product,
    Product.is_active == True
)

rows = [
    product_to_dict(product)
    for product in products
]

df = pd.DataFrame(rows)

print(df.head())
```

Para análisis grandes, conviene consultar solo los campos necesarios o usar agregaciones de MongoDB.

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
from motor.motor_asyncio import AsyncIOMotorClient
from odmantic import AIOEngine

from .models import Product

load_dotenv()


def create_mongo_client():
    return AsyncIOMotorClient(
        os.getenv("MONGO_URI", "mongodb://localhost:27017")
    )


def create_engine(client):
    return AIOEngine(
        client=client,
        database=os.getenv("MONGO_DATABASE", "app")
    )


async def configure_database(engine):
    await engine.configure_database([
        Product
    ])
```

## `models.py`

```python
from odmantic import Field
from odmantic import Index
from odmantic import Model


class Product(Model):
    sku: str = Field(unique=True)
    name: str = Field(min_length=1, max_length=100, index=True)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True

    model_config = {
        "collection": "products",
        "indexes": lambda: [
            Index(Product.is_active, Product.name)
        ]
    }
```

## `schemas.py`

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    sku: str = Field(min_length=1)
    name: str = Field(min_length=1, max_length=100)
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
        "price": product.price,
        "stock": product.stock,
        "is_active": product.is_active
    }


async def list_products(engine):
    products = await engine.find(
        Product,
        Product.is_active == True,
        sort=Product.name
    )

    return [
        product_to_dict(product)
        for product in products
    ]


async def get_product(engine, sku):
    product = await engine.find_one(
        Product,
        Product.sku == sku,
        Product.is_active == True
    )

    if product is None:
        return None

    return product_to_dict(product)


async def create_product(engine, product_create):
    product = Product(
        **product_create.model_dump()
    )

    await engine.save(product)

    return product_to_dict(product)


async def update_stock(engine, sku, stock):
    product = await engine.find_one(
        Product,
        Product.sku == sku
    )

    if product is None:
        return None

    product.stock = stock

    await engine.save(product)

    return product_to_dict(product)


async def deactivate_product(engine, sku):
    product = await engine.find_one(
        Product,
        Product.sku == sku
    )

    if product is None:
        return None

    product.is_active = False

    await engine.save(product)

    return product_to_dict(product)
```

## Separación por responsabilidades

```text
database.py      -> cliente MongoDB, engine y configuración de índices
models.py        -> modelos ODMantic
schemas.py       -> modelos Pydantic de entrada y salida
repositories.py  -> consultas y persistencia
services.py      -> reglas de negocio
main.py          -> aplicación
```

## Migraciones y cambios de esquema

ODMantic no es principalmente una herramienta de migraciones.

MongoDB permite documentos con estructuras distintas, pero en aplicaciones reales conviene controlar cambios.

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
products = await engine.find(Product)

for product in products:
    if product.is_active is None:
        product.is_active = True
        await engine.save(product)
```

## Manejo de errores

ODMantic puede generar errores de validación de Pydantic y errores provenientes de MongoDB.

Importaciones frecuentes:

```python
from pydantic import ValidationError
from pymongo.errors import DuplicateKeyError
from pymongo.errors import PyMongoError
```

## Error de validación

```python
from pydantic import ValidationError

try:
    product = Product(
        sku="",
        name="",
        price=-10
    )
except ValidationError as error:
    print(error)
```

## Error de clave duplicada

```python
from pymongo.errors import DuplicateKeyError

try:
    product = Product(
        sku="LAP-001",
        name="Laptop",
        price=3500
    )

    await engine.save(product)
except DuplicateKeyError:
    print("El SKU ya existe")
```

Para que este error ocurra de forma confiable, debe existir un índice único creado en la base.

## Error general de MongoDB

```python
from pymongo.errors import PyMongoError

try:
    products = await engine.find(Product)
except PyMongoError as error:
    print("Error de MongoDB:", error)
```

## Errores comunes

## Instalar el paquete y tratar de importar otro nombre

Instalación correcta:

```bash
python -m pip install odmantic
```

Importación correcta:

```python
from odmantic import Model
```

Incorrecto:

```python
import odmantic_odm
```

## No crear el engine

Definir modelos no basta.

Debe crearse un engine:

```python
engine = AIOEngine()
```

o:

```python
engine = SyncEngine()
```

## Olvidar `await` en modo asíncrono

Problemático:

```python
engine.save(product)
```

Correcto:

```python
await engine.save(product)
```

## No configurar índices

Declarar índices en el modelo no siempre basta para que existan físicamente en MongoDB.

Debe ejecutarse:

```python
await engine.configure_database([Product])
```

## Usar `save()` sin entender su comportamiento

`save()` puede comportarse como upsert.

Si una instancia ya existe, puede actualizarse.

## No separar modelos de entrada y modelos de persistencia

En APIs, conviene separar:

```text
ProductCreate -> entrada
Product       -> persistencia
ProductRead   -> salida opcional
```

## Usar MongoDB como si fuera SQL

MongoDB no debe diseñarse copiando automáticamente tablas relacionales.

El diseño debe depender de documentos, consultas esperadas y patrones de acceso.

## Abusar de referencias

Si un dato se consulta siempre junto al documento principal, puede convenir un documento embebido.

## Devolver modelos directamente sin controlar JSON

Para APIs, conviene convertir explícitamente.

```python
product_to_dict(product)
```

## No convertir `ObjectId` a texto

Para respuestas JSON:

```python
"id": str(product.id)
```

## Construir consultas dinámicas sin validar campos

Si el usuario puede elegir campos para ordenar o filtrar, deben validarse contra una lista permitida.

## Buenas prácticas

## Usar modelos claros

```python
class Product(Model):
    ...
```

## Usar `Field` para validaciones

```python
price: float = Field(gt=0)
```

## Crear índices para consultas frecuentes

```python
name: str = Field(index=True)
```

## Ejecutar `configure_database()`

```python
await engine.configure_database([Product])
```

## Reutilizar el engine

No crear un engine nuevo por cada consulta.

## Separar entrada, persistencia y salida

```text
schemas.py
models.py
repositories.py
```

## Convertir salidas a estructuras JSON controladas

```python
product_to_dict(product)
```

## Usar documentos embebidos cuando corresponda

```python
class Address(EmbeddedModel):
    ...
```

## Usar referencias solo cuando tengan sentido

```python
category: Category = Reference()
```

## Usar acceso crudo solo cuando sea necesario

```python
engine.get_collection(Product)
```

## Diseñar MongoDB según patrones de consulta

No copiar automáticamente el diseño de una base SQL.

## Ejemplo integrado asíncrono

```python
import asyncio
import os

from dotenv import load_dotenv
from motor.motor_asyncio import AsyncIOMotorClient
from odmantic import AIOEngine
from odmantic import Field
from odmantic import Index
from odmantic import Model
from pydantic import ValidationError
from pymongo.errors import DuplicateKeyError
from pymongo.errors import PyMongoError

load_dotenv()


class Product(Model):
    sku: str = Field(unique=True)
    name: str = Field(min_length=1, max_length=100, index=True)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True

    model_config = {
        "collection": "products",
        "indexes": lambda: [
            Index(Product.is_active, Product.name)
        ]
    }


def create_engine():
    client = AsyncIOMotorClient(
        os.getenv("MONGO_URI", "mongodb://localhost:27017")
    )

    engine = AIOEngine(
        client=client,
        database=os.getenv("MONGO_DATABASE", "app")
    )

    return client, engine


def product_to_dict(product):
    return {
        "id": str(product.id),
        "sku": product.sku,
        "name": product.name,
        "price": product.price,
        "stock": product.stock,
        "is_active": product.is_active
    }


async def create_product(engine, sku, name, price, stock=0):
    product = Product(
        sku=sku,
        name=name,
        price=price,
        stock=stock
    )

    await engine.save(product)

    return product_to_dict(product)


async def list_products(engine):
    products = await engine.find(
        Product,
        Product.is_active == True,
        sort=Product.name
    )

    return [
        product_to_dict(product)
        for product in products
    ]


async def get_product(engine, sku):
    product = await engine.find_one(
        Product,
        Product.sku == sku,
        Product.is_active == True
    )

    if product is None:
        return None

    return product_to_dict(product)


async def update_stock(engine, sku, stock):
    product = await engine.find_one(
        Product,
        Product.sku == sku
    )

    if product is None:
        return None

    product.stock = stock

    await engine.save(product)

    return product_to_dict(product)


async def deactivate_product(engine, sku):
    product = await engine.find_one(
        Product,
        Product.sku == sku
    )

    if product is None:
        return None

    product.is_active = False

    await engine.save(product)

    return product_to_dict(product)


async def main():
    client, engine = create_engine()

    try:
        await engine.configure_database([
            Product
        ])

        try:
            await create_product(
                engine,
                sku="LAP-001",
                name="Laptop",
                price=3500,
                stock=5
            )

            await create_product(
                engine,
                sku="MOU-001",
                name="Mouse",
                price=80,
                stock=20
            )
        except DuplicateKeyError:
            pass

        products = await list_products(engine)

        for product in products:
            print(
                product["sku"],
                product["name"],
                product["price"],
                product["stock"]
            )

        updated_product = await update_stock(
            engine,
            sku="LAP-001",
            stock=4
        )

        print(updated_product)

    except ValidationError as error:
        print("Error de validación:", error)
    except PyMongoError as error:
        print("Error de MongoDB:", error)
    finally:
        client.close()


if __name__ == "__main__":
    asyncio.run(main())
```

## Ejemplo integrado síncrono

```python
import os

from dotenv import load_dotenv
from odmantic import Field
from odmantic import Model
from odmantic import SyncEngine
from pymongo import MongoClient

load_dotenv()


class Product(Model):
    sku: str = Field(unique=True)
    name: str
    price: float
    stock: int = 0
    is_active: bool = True

    model_config = {
        "collection": "products"
    }


def create_engine():
    client = MongoClient(
        os.getenv("MONGO_URI", "mongodb://localhost:27017")
    )

    engine = SyncEngine(
        client=client,
        database=os.getenv("MONGO_DATABASE", "app")
    )

    return client, engine


def main():
    client, engine = create_engine()

    try:
        engine.configure_database([
            Product
        ])

        product = Product(
            sku="LAP-001",
            name="Laptop",
            price=3500,
            stock=5
        )

        engine.save(product)

        products = engine.find(
            Product,
            Product.is_active == True,
            sort=Product.name
        )

        for product in products:
            print(
                product.sku,
                product.name,
                product.price,
                product.stock
            )

    finally:
        client.close()


if __name__ == "__main__":
    main()
```

## Ejemplo con documentos embebidos y referencias

```python
from odmantic import EmbeddedModel
from odmantic import Model
from odmantic import Reference


class Address(EmbeddedModel):
    city: str
    country: str


class Category(Model):
    name: str


class Client(Model):
    name: str
    address: Address


class Product(Model):
    name: str
    category: Category = Reference()
    tags: list[str] = []


category = Category(
    name="Tecnología"
)

product = Product(
    name="Laptop",
    category=category,
    tags=[
        "computadora",
        "oficina"
    ]
)

client = Client(
    name="Ana",
    address=Address(
        city="Lima",
        country="Perú"
    )
)

await engine.save(product)
await engine.save(client)
```

## Relación con otras librerías

`odmantic` se relaciona especialmente con:

```text
mongodb
pymongo
motor
pydantic
fastapi
python-dotenv
pandas
pytest
pytest-asyncio
```

## Relación con FastAPI

ODMantic encaja con FastAPI porque permite modelos basados en Pydantic y uso asíncrono mediante `AIOEngine`.

## Relación con Pydantic

ODMantic usa Pydantic para validación, serialización y definición de modelos.

## Relación con Motor

En modo asíncrono, ODMantic puede trabajar con clientes de Motor.

## Relación con PyMongo

En modo síncrono, ODMantic puede trabajar con clientes de PyMongo.

También puede accederse a la colección subyacente cuando se requieren operaciones crudas.

## Relación con MongoEngine

MongoEngine es otro ODM para MongoDB, más tradicional y síncrono.

ODMantic ofrece una API basada en type hints y Pydantic, con soporte síncrono y asíncrono.

## Relación con Beanie

Beanie también es un ODM asíncrono basado en Pydantic.

La elección entre ODMantic y Beanie depende del estilo de API, características requeridas, migraciones, integración y preferencias del proyecto.

## Orden didáctico interno

```text
1. Propósito de odmantic
2. Instalación
3. Relación con MongoDB, Pydantic, PyMongo y Motor
4. Model
5. Field
6. model_config
7. AIOEngine y SyncEngine
8. save() y save_all()
9. find(), find_one() y count()
10. Filtros con operadores de Python
11. Ordenamiento, limit y skip
12. Actualización mediante modificación de instancia
13. model_update()
14. delete() y remove()
15. Índices y configure_database()
16. EmbeddedModel
17. Reference
18. Sesiones y transacciones
19. Acceso a colección cruda
20. Conversión a diccionarios
21. Uso con FastAPI, pandas y Pydantic
22. Organización recomendada
23. Errores comunes
24. Buenas prácticas
```