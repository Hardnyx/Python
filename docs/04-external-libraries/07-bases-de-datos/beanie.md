# `beanie`

## Propósito

`beanie` es una librería externa para trabajar con MongoDB mediante un ODM asíncrono.

ODM significa `Object-Document Mapper`.

Permite representar documentos de MongoDB como clases de Python basadas en Pydantic, validar datos, ejecutar consultas asíncronas, insertar documentos, actualizar información, eliminar registros, definir índices, trabajar con relaciones y organizar el acceso a MongoDB con una estructura más declarativa.

## Naturaleza de la librería

`beanie` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install beanie
```

Importaciones frecuentes:

```python
from beanie import Document
from beanie import Indexed
from beanie import init_beanie
```

También se usa junto con el cliente asíncrono de PyMongo:

```python
from pymongo import AsyncMongoClient
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

`beanie` permite mapear documentos de MongoDB a clases de Python.

Relación conceptual:

```text
colección de MongoDB -> clase Document
documento            -> instancia de Document
campo del documento  -> atributo anotado con tipos de Python
validación           -> Pydantic
```

## Relación con Pydantic

Beanie se apoya en Pydantic para definir modelos y validar datos.

Ejemplo:

```python
from beanie import Document


class Product(Document):
    sku: str
    name: str
    price: float
    stock: int = 0
    is_active: bool = True
```

La clase `Product` funciona como modelo Pydantic y como documento persistente en MongoDB.

## Relación con PyMongo async

Beanie usa un cliente asíncrono de MongoDB para conectarse a la base.

Ejemplo:

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient("mongodb://localhost:27017")
```

Luego se inicializa Beanie:

```python
await init_beanie(
    database=client.app,
    document_models=[Product]
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

## Beanie

Trabaja con modelos declarativos.

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500
)

await product.insert()
```

## Regla práctica

Para control directo sobre MongoDB:

```text
pymongo
```

Para modelos, validación y consultas orientadas a objetos en código asíncrono:

```text
beanie
```

## Cuándo usar Beanie

Conviene usar `beanie` cuando se necesita:

```text
trabajar con MongoDB en aplicaciones async
usar modelos basados en Pydantic
integrar MongoDB con FastAPI
validar documentos antes de guardarlos
definir esquemas documentales
usar consultas con sintaxis Python
trabajar con índices
usar documentos embebidos
usar relaciones entre documentos
organizar la capa de persistencia
```

## Cuándo no usar Beanie

No siempre conviene usar `beanie` cuando se necesita:

```text
un script simple con MongoDB
control directo de bajo nivel
operaciones muy específicas de PyMongo
evitar abstracciones ODM
trabajar con bases SQL
usar Redis
hacer análisis tabular local
usar una aplicación completamente síncrona
```

Para código síncrono puede corresponder:

```text
pymongo
mongoengine
bunnet
```

Para control directo asíncrono puede corresponder:

```text
pymongo async
```

Para bases relacionales pueden corresponder:

```text
sqlalchemy
sqlmodel
tortoise-orm
peewee
```

## Instalación

Instalación básica:

```bash
python -m pip install beanie
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
beanie==1.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import beanie

print(beanie.__version__)
```

## Modelo básico

```python
from beanie import Document


class Product(Document):
    sku: str
    name: str
    price: float
    stock: int = 0
    is_active: bool = True
```

## `Document`

`Document` es la clase base para definir documentos persistentes.

```python
from beanie import Document
```

Ejemplo:

```python
class Product(Document):
    ...
```

Cada clase que hereda de `Document` representa una colección de MongoDB.

## Campos

Los campos se definen con anotaciones de tipo de Python.

```python
class Product(Document):
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
modelos Pydantic
ObjectId compatible con Beanie
```

## Campos obligatorios

Un campo sin valor por defecto es obligatorio.

```python
class Product(Document):
    sku: str
    name: str
    price: float
```

## Campos opcionales

```python
from typing import Optional

from beanie import Document


class Product(Document):
    description: Optional[str] = None
```

En Python moderno también puede usarse:

```python
class Product(Document):
    description: str | None = None
```

## Valores por defecto

```python
class Product(Document):
    stock: int = 0
    is_active: bool = True
```

## Validación con Pydantic

Beanie usa Pydantic para validar datos.

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500,
    stock=5
)
```

Si el tipo o la regla no coinciden, Pydantic puede generar un error de validación.

## Uso de `Field`

```python
from pydantic import Field
from beanie import Document


class Product(Document):
    sku: str = Field(min_length=1)
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True
```

## Definir colección

Puede configurarse el nombre de la colección con `Settings`.

```python
from beanie import Document


class Product(Document):
    sku: str
    name: str

    class Settings:
        name = "products"
```

## Inicialización

Antes de usar los modelos, debe inicializarse Beanie.

```python
from beanie import init_beanie
from pymongo import AsyncMongoClient


async def init_database():
    client = AsyncMongoClient("mongodb://localhost:27017")

    await init_beanie(
        database=client.app,
        document_models=[Product]
    )
```

## `init_beanie()`

`init_beanie()` registra los modelos documentales y los asocia con una base de datos MongoDB.

```python
await init_beanie(
    database=client.app,
    document_models=[
        Product
    ]
)
```

Si hay varios modelos:

```python
await init_beanie(
    database=client.app,
    document_models=[
        Product,
        Client,
        Order
    ]
)
```

## Conexión local

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient(
    "mongodb://localhost:27017"
)
```

Uso con Beanie:

```python
await init_beanie(
    database=client.app,
    document_models=[Product]
)
```

## Conexión con MongoDB Atlas

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient(
    "mongodb+srv://user:password@cluster.mongodb.net/app"
)
```

En proyectos reales, la URI debe venir de variables de entorno.

## Variables de entorno

No conviene escribir credenciales directamente en el código.

```python
import os

from pymongo import AsyncMongoClient

client = AsyncMongoClient(
    os.getenv("MONGO_URI")
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
from pymongo import AsyncMongoClient

load_dotenv()

client = AsyncMongoClient(
    os.getenv("MONGO_URI")
)

database = client[os.getenv("MONGO_DATABASE", "app")]
```

Inicialización:

```python
await init_beanie(
    database=database,
    document_models=[Product]
)
```

## Insertar documento

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    price=3500,
    stock=5
)

await product.insert()
```

## Guardar documento

```python
product = Product(
    sku="MOU-001",
    name="Mouse",
    price=80,
    stock=20
)

await product.save()
```

## Insertar varios documentos

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

await Product.insert_many(products)
```

## Consultar todos los documentos

```python
products = await Product.find_all().to_list()
```

Uso:

```python
for product in products:
    print(product.name)
```

## Consultar con filtro

```python
products = await Product.find(
    Product.is_active == True
).to_list()
```

## Consultar un documento

```python
product = await Product.find_one(
    Product.sku == "LAP-001"
)
```

Si no encuentra coincidencias, devuelve `None`.

## Consultar por ID

```python
product = await Product.get(product_id)
```

`product_id` debe tener un formato compatible con el identificador usado por Beanie y MongoDB.

## Filtros simples

Igualdad:

```python
Product.find(Product.sku == "LAP-001")
```

Mayor que:

```python
Product.find(Product.price > 100)
```

Mayor o igual que:

```python
Product.find(Product.price >= 100)
```

Menor que:

```python
Product.find(Product.price < 5000)
```

Menor o igual que:

```python
Product.find(Product.price <= 5000)
```

Distinto:

```python
Product.find(Product.name != "Laptop")
```

## Varias condiciones

```python
products = await Product.find(
    Product.is_active == True,
    Product.price >= 100
).to_list()
```

También puede usarse lógica más explícita según la consulta necesaria.

## Ordenar resultados

```python
products = await Product.find(
    Product.is_active == True
).sort(Product.name).to_list()
```

Orden descendente:

```python
products = await Product.find(
    Product.is_active == True
).sort(-Product.price).to_list()
```

## Limitar resultados

```python
products = await Product.find_all().limit(10).to_list()
```

## Saltar resultados

```python
products = await Product.find_all().skip(10).limit(10).to_list()
```

## Contar documentos

```python
count = await Product.find(
    Product.is_active == True
).count()

print(count)
```

## Actualizar documento cargado

```python
product = await Product.find_one(
    Product.sku == "LAP-001"
)

if product is not None:
    product.price = 3600
    product.stock = 4

    await product.save()
```

## Actualización parcial con `set`

```python
product = await Product.find_one(
    Product.sku == "LAP-001"
)

if product is not None:
    await product.set({
        Product.price: 3600,
        Product.stock: 4
    })
```

## Actualización directa

```python
await Product.find_one(
    Product.sku == "LAP-001"
).set({
    Product.price: 3600,
    Product.stock: 4
})
```

Debe usarse solo si se sabe que el documento existe o si se maneja el caso `None`.

## Actualizar varios documentos

```python
await Product.find(
    Product.is_active == False
).set({
    Product.stock: 0
})
```

## Incrementar campo

```python
product = await Product.find_one(
    Product.sku == "LAP-001"
)

if product is not None:
    await product.inc({
        Product.stock: 1
    })
```

## Eliminar documento

```python
product = await Product.find_one(
    Product.sku == "LAP-001"
)

if product is not None:
    await product.delete()
```

## Eliminar varios documentos

```python
await Product.find(
    Product.is_active == False
).delete()
```

## Eliminación lógica

En aplicaciones reales, muchas veces conviene marcar documentos como inactivos.

```python
product = await Product.find_one(
    Product.sku == "LAP-001"
)

if product is not None:
    await product.set({
        Product.is_active: False
    })
```

Consulta de activos:

```python
products = await Product.find(
    Product.is_active == True
).to_list()
```

## Índices

Beanie permite definir índices.

Una forma simple es usar `Indexed`.

```python
from beanie import Document
from beanie import Indexed


class Product(Document):
    sku: Indexed(str, unique=True)
    name: str
    price: float
```

También puede definirse en `Settings`.

```python
class Product(Document):
    sku: str
    name: str
    is_active: bool = True

    class Settings:
        name = "products"
        indexes = [
            "sku",
            "name",
            [
                ("is_active", 1),
                ("name", 1)
            ]
        ]
```

## Índice único

```python
from beanie import Indexed


class Product(Document):
    sku: Indexed(str, unique=True)
```

Este índice ayuda a evitar documentos duplicados por `sku`.

## Documentos embebidos

Como Beanie usa Pydantic, puede insertar modelos Pydantic dentro de documentos.

```python
from pydantic import BaseModel
from beanie import Document


class Category(BaseModel):
    name: str
    description: str | None = None


class Product(Document):
    sku: str
    name: str
    category: Category
```

Uso:

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    category=Category(
        name="Tecnología",
        description="Productos tecnológicos"
    )
)

await product.insert()
```

## Listas

```python
from beanie import Document


class Product(Document):
    sku: str
    name: str
    tags: list[str] = []
```

Uso:

```python
product = Product(
    sku="LAP-001",
    name="Laptop",
    tags=[
        "computadora",
        "oficina"
    ]
)

await product.insert()
```

## Consultar listas

```python
products = await Product.find(
    Product.tags == "oficina"
).to_list()
```

La consulta exacta puede depender de cómo se modelen las listas y operadores usados.

## Diccionarios

```python
class Product(Document):
    sku: str
    name: str
    metadata: dict = {}
```

Debe usarse con criterio, porque reduce el control del esquema.

## Relaciones con `Link`

Beanie permite modelar relaciones entre documentos.

```python
from beanie import Document
from beanie import Link


class Category(Document):
    name: str


class Product(Document):
    name: str
    category: Link[Category]
```

Uso:

```python
category = Category(name="Tecnología")

await category.insert()

product = Product(
    name="Laptop",
    category=category
)

await product.insert()
```

## Cargar relaciones

```python
product = await Product.find_one(
    Product.name == "Laptop",
    fetch_links=True
)
```

Debe usarse con cuidado para no cargar más datos de los necesarios.

## Documento embebido frente a relación

## Documento embebido

Conviene cuando:

```text
el dato pertenece al documento principal
se consulta siempre junto al documento principal
no tiene vida independiente
no se reutiliza demasiado
```

## Relación con `Link`

Conviene cuando:

```text
el documento relacionado tiene vida propia
se consulta por separado
se reutiliza entre varios documentos
puede crecer o cambiar independientemente
```

## Agregaciones

Beanie permite ejecutar agregaciones de MongoDB.

```python
pipeline = [
    {
        "$match": {
            "is_active": True
        }
    },
    {
        "$group": {
            "_id": "$category.name",
            "count": {
                "$sum": 1
            }
        }
    }
]

results = await Product.aggregate(pipeline).to_list()
```

Para agregaciones muy complejas, a veces PyMongo directo puede ser más claro.

## Proyecciones

Las proyecciones permiten devolver solo parte del documento o usar un modelo de salida.

Ejemplo conceptual:

```python
from pydantic import BaseModel


class ProductSummary(BaseModel):
    sku: str
    name: str
    price: float
```

Consulta:

```python
products = await Product.find(
    Product.is_active == True
).project(ProductSummary).to_list()
```

## Conversión a diccionario

Como los documentos son modelos Pydantic, puede usarse `model_dump()`.

```python
product = await Product.find_one(
    Product.sku == "LAP-001"
)

if product is not None:
    data = product.model_dump()
```

Para APIs, suele convenir controlar explícitamente el formato de salida.

## Conversión para JSON

```python
product = await Product.find_one(
    Product.sku == "LAP-001"
)

if product is not None:
    data = product.model_dump(mode="json")
```

## Función de serialización

```python
def product_to_dict(product):
    return product.model_dump(
        mode="json"
    )
```

Uso:

```python
products = await Product.find(
    Product.is_active == True
).to_list()

data = [
    product_to_dict(product)
    for product in products
]
```

## Manejo de errores

Beanie puede generar errores de validación de Pydantic y errores provenientes de MongoDB.

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

    await product.insert()
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

    await product.insert()
except DuplicateKeyError:
    print("El SKU ya existe")
```

Para que este error ocurra de forma confiable, debe existir un índice único.

## Uso con FastAPI

Beanie encaja bien con FastAPI porque ambos trabajan cómodamente con `async` y Pydantic.

```python
import os
from contextlib import asynccontextmanager

from beanie import Document, Indexed, init_beanie
from fastapi import FastAPI, HTTPException
from pydantic import Field
from pymongo import AsyncMongoClient


class Product(Document):
    sku: Indexed(str, unique=True)
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True

    class Settings:
        name = "products"


@asynccontextmanager
async def lifespan(app: FastAPI):
    client = AsyncMongoClient(
        os.getenv("MONGO_URI", "mongodb://localhost:27017")
    )

    app.state.mongo_client = client

    await init_beanie(
        database=client[os.getenv("MONGO_DATABASE", "app")],
        document_models=[
            Product
        ]
    )

    yield

    await client.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    products = await Product.find(
        Product.is_active == True
    ).sort(Product.name).to_list()

    return [
        product.model_dump(mode="json")
        for product in products
    ]


@app.get("/products/{sku}")
async def read_product(sku: str):
    product = await Product.find_one(
        Product.sku == sku,
        Product.is_active == True
    )

    if product is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return product.model_dump(mode="json")
```

## Uso con modelos de entrada

Aunque `Document` valida datos, en APIs suele convenir separar entrada y persistencia.

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
@app.post("/products")
async def create_product(product_create: ProductCreate):
    product = Product(
        **product_create.model_dump()
    )

    await product.insert()

    return product.model_dump(mode="json")
```

## Uso con pandas

Beanie puede alimentar un DataFrame.

```python
import pandas as pd

products = await Product.find(
    Product.is_active == True
).to_list()

rows = [
    product.model_dump(mode="json")
    for product in products
]

df = pd.DataFrame(rows)

print(df.head())
```

Para análisis grandes, conviene usar agregaciones de MongoDB o extraer solo los campos necesarios.

## Migraciones

Beanie incluye soporte para migraciones.

En proyectos reales, las migraciones pueden usarse para:

```text
cambiar estructura de documentos
renombrar campos
crear valores por defecto
ajustar tipos
migrar documentos antiguos
mantener evolución controlada del esquema
```

Ejemplo conceptual:

```text
migrations/
├─ 2026_01_01_add_is_active_to_products.py
├─ 2026_01_10_rename_product_code_to_sku.py
└─ 2026_02_01_backfill_categories.py
```

## Cambios de esquema en MongoDB

MongoDB permite documentos con estructuras distintas, pero en aplicaciones reales conviene controlar la evolución.

Estrategias comunes:

```text
migraciones de datos
campos opcionales temporales
versionado de documentos
backfills
validaciones progresivas
```

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

from beanie import init_beanie
from dotenv import load_dotenv
from pymongo import AsyncMongoClient

from .models import Product

load_dotenv()


async def init_database():
    client = AsyncMongoClient(
        os.getenv("MONGO_URI", "mongodb://localhost:27017")
    )

    database = client[
        os.getenv("MONGO_DATABASE", "app")
    ]

    await init_beanie(
        database=database,
        document_models=[
            Product
        ]
    )

    return client
```

## `models.py`

```python
from beanie import Document
from beanie import Indexed
from pydantic import Field


class Product(Document):
    sku: Indexed(str, unique=True)
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True

    class Settings:
        name = "products"
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
    return product.model_dump(
        mode="json"
    )


async def list_products():
    products = await Product.find(
        Product.is_active == True
    ).sort(Product.name).to_list()

    return [
        product_to_dict(product)
        for product in products
    ]


async def get_product(sku):
    product = await Product.find_one(
        Product.sku == sku,
        Product.is_active == True
    )

    if product is None:
        return None

    return product_to_dict(product)


async def create_product(product_create):
    product = Product(
        **product_create.model_dump()
    )

    await product.insert()

    return product_to_dict(product)


async def update_stock(sku, stock):
    product = await Product.find_one(
        Product.sku == sku
    )

    if product is None:
        return None

    await product.set({
        Product.stock: stock
    })

    return product_to_dict(product)


async def deactivate_product(sku):
    product = await Product.find_one(
        Product.sku == sku
    )

    if product is None:
        return None

    await product.set({
        Product.is_active: False
    })

    return product_to_dict(product)
```

## Separación por responsabilidades

```text
database.py      -> cliente MongoDB e inicialización de Beanie
models.py        -> documentos Beanie
schemas.py       -> modelos Pydantic de entrada y salida
repositories.py  -> consultas y persistencia
services.py      -> reglas de negocio
main.py          -> aplicación
```

## Errores comunes

## Instalar el paquete y tratar de importar otro nombre

Instalación correcta:

```bash
python -m pip install beanie
```

Importación correcta:

```python
from beanie import Document
```

Incorrecto:

```python
import beanie_odm
```

## No ejecutar `init_beanie()`

Definir modelos no basta.

Debe ejecutarse:

```python
await init_beanie(...)
```

antes de consultar o guardar documentos.

## Olvidar `await`

Problemático:

```python
product.insert()
```

Correcto:

```python
await product.insert()
```

## Crear el cliente MongoDB en cada operación

Menos recomendable:

```python
async def create_product():
    client = AsyncMongoClient(...)
```

Mejor crear el cliente al iniciar la aplicación y reutilizarlo.

## No cerrar el cliente

Al finalizar la aplicación:

```python
await client.close()
```

## No separar modelos de entrada y documentos

En APIs, conviene separar:

```text
ProductCreate -> entrada
Product       -> persistencia
ProductRead   -> salida opcional
```

## No crear índices

Si se consulta frecuentemente por `sku`, `name` o `is_active`, conviene definir índices.

## Usar `dict` para todo

`dict` es flexible, pero puede debilitar la validación si reemplaza campos bien definidos.

## Abusar de relaciones

MongoDB no debe modelarse como una base relacional tradicional.

Si una relación se consulta siempre con el documento principal, puede convenir documento embebido.

## Devolver datos sin modo JSON

Para APIs puede convenir:

```python
model_dump(mode="json")
```

## Construir consultas dinámicas sin validar campos

Si el usuario puede elegir campos para ordenar o filtrar, deben validarse contra listas permitidas.

## Buenas prácticas

## Usar modelos claros

```python
class Product(Document):
    ...
```

## Usar Pydantic `Field`

```python
price: float = Field(gt=0)
```

## Inicializar Beanie al iniciar la app

```python
await init_beanie(...)
```

## Reutilizar el cliente de MongoDB

No crear un cliente por consulta.

## Usar índices para consultas frecuentes

```python
sku: Indexed(str, unique=True)
```

## Separar entrada, persistencia y salida

```text
schemas.py
models.py
repositories.py
```

## Convertir salidas a JSON

```python
product.model_dump(mode="json")
```

## Manejar errores de MongoDB

```python
DuplicateKeyError
PyMongoError
```

## Usar documentos embebidos cuando corresponda

```python
BaseModel dentro de Document
```

## Usar relaciones solo cuando tengan sentido

```python
Link[Category]
```

## Diseñar MongoDB según patrones de consulta

No copiar automáticamente el diseño de una base SQL.

## Ejemplo integrado

```python
import asyncio
import os

from beanie import Document
from beanie import Indexed
from beanie import init_beanie
from dotenv import load_dotenv
from pydantic import Field
from pymongo import AsyncMongoClient
from pymongo.errors import DuplicateKeyError
from pymongo.errors import PyMongoError

load_dotenv()


class Product(Document):
    sku: Indexed(str, unique=True)
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
    is_active: bool = True

    class Settings:
        name = "products"


def product_to_dict(product):
    return product.model_dump(
        mode="json"
    )


async def init_database():
    client = AsyncMongoClient(
        os.getenv("MONGO_URI", "mongodb://localhost:27017")
    )

    await init_beanie(
        database=client[
            os.getenv("MONGO_DATABASE", "app")
        ],
        document_models=[
            Product
        ]
    )

    return client


async def create_product(sku, name, price, stock=0):
    product = Product(
        sku=sku,
        name=name,
        price=price,
        stock=stock
    )

    await product.insert()

    return product_to_dict(product)


async def list_products():
    products = await Product.find(
        Product.is_active == True
    ).sort(Product.name).to_list()

    return [
        product_to_dict(product)
        for product in products
    ]


async def get_product(sku):
    product = await Product.find_one(
        Product.sku == sku,
        Product.is_active == True
    )

    if product is None:
        return None

    return product_to_dict(product)


async def update_stock(sku, stock):
    product = await Product.find_one(
        Product.sku == sku
    )

    if product is None:
        return None

    await product.set({
        Product.stock: stock
    })

    return product_to_dict(product)


async def deactivate_product(sku):
    product = await Product.find_one(
        Product.sku == sku
    )

    if product is None:
        return None

    await product.set({
        Product.is_active: False
    })

    return product_to_dict(product)


async def main():
    client = await init_database()

    try:
        try:
            await create_product(
                sku="LAP-001",
                name="Laptop",
                price=3500,
                stock=5
            )

            await create_product(
                sku="MOU-001",
                name="Mouse",
                price=80,
                stock=20
            )
        except DuplicateKeyError:
            pass

        products = await list_products()

        for product in products:
            print(
                product["sku"],
                product["name"],
                product["price"],
                product["stock"]
            )

        updated_product = await update_stock(
            sku="LAP-001",
            stock=4
        )

        print(updated_product)

    except PyMongoError as error:
        print("Error de MongoDB:", error)
    finally:
        await client.close()


if __name__ == "__main__":
    asyncio.run(main())
```

## Ejemplo con documentos embebidos y relaciones

```python
from beanie import Document
from beanie import Link
from pydantic import BaseModel


class Address(BaseModel):
    city: str
    country: str


class Category(Document):
    name: str

    class Settings:
        name = "categories"


class Client(Document):
    name: str
    address: Address

    class Settings:
        name = "clients"


class Product(Document):
    name: str
    category: Link[Category]
    tags: list[str] = []

    class Settings:
        name = "products"


category = Category(
    name="Tecnología"
)

await category.insert()

product = Product(
    name="Laptop",
    category=category,
    tags=[
        "computadora",
        "oficina"
    ]
)

await product.insert()

client = Client(
    name="Ana",
    address=Address(
        city="Lima",
        country="Perú"
    )
)

await client.insert()
```

## Relación con otras librerías

`beanie` se relaciona especialmente con:

```text
mongodb
pymongo
pydantic
fastapi
python-dotenv
pandas
pytest
pytest-asyncio
```

## Relación con FastAPI

Beanie encaja naturalmente con FastAPI porque ambos usan patrones compatibles con Pydantic y `async`.

## Relación con Pydantic

Beanie usa modelos basados en Pydantic para validar y serializar documentos.

## Relación con PyMongo

PyMongo proporciona el cliente y la comunicación de bajo nivel con MongoDB.

Beanie ofrece una capa ODM sobre ese acceso.

## Relación con MongoEngine

MongoEngine es un ODM síncrono para MongoDB.

Beanie es una opción orientada a aplicaciones asíncronas.

## Relación con ODMantic

ODMantic también es un ODM asíncrono basado en Pydantic.

La elección entre Beanie y ODMantic depende del estilo de API, características requeridas y preferencias del proyecto.

## Orden didáctico interno

```text
1. Propósito de beanie
2. Instalación
3. Relación con MongoDB, Pydantic y PyMongo async
4. Document
5. Campos y Field
6. Settings
7. init_beanie()
8. Insertar documentos
9. Consultar con find(), find_one() y get()
10. Filtros, ordenamiento y paginación
11. Actualizaciones
12. Eliminación y eliminación lógica
13. Índices
14. Documentos embebidos
15. Relaciones con Link
16. Agregaciones
17. Proyecciones
18. Conversión a diccionarios y JSON
19. Uso con FastAPI y pandas
20. Migraciones
21. Organización recomendada
22. Errores comunes
23. Buenas prácticas
```