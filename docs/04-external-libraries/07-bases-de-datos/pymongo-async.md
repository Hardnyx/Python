# `pymongo-async`

## Propósito

`pymongo-async` no es una librería separada, sino la API asíncrona incluida dentro de `pymongo` para trabajar con MongoDB desde Python usando `async` y `await`.

Se utiliza para conectar aplicaciones asíncronas con MongoDB, especialmente backends construidos con frameworks como FastAPI, Starlette o cualquier aplicación basada en `asyncio`.

Permite ejecutar operaciones sobre MongoDB sin bloquear el flujo asíncrono de la aplicación.

## Naturaleza de la API

La instalación se realiza con el paquete `pymongo`:

```bash
python -m pip install pymongo
```

La importación principal es:

```python
from pymongo import AsyncMongoClient
```

No se instala como:

```bash
python -m pip install pymongo-async
```

Tampoco se importa como:

```python
import pymongo_async
```

La API asíncrona pertenece al ecosistema oficial de PyMongo.

## Relación con `pymongo`

PyMongo tiene dos formas principales de uso:

```text
MongoClient      -> cliente síncrono
AsyncMongoClient -> cliente asíncrono
```

Cliente síncrono:

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
```

Cliente asíncrono:

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient("mongodb://localhost:27017/")
```

La diferencia principal es que las operaciones de red en la API asíncrona deben esperarse con `await`.

## Relación con Motor

Motor fue durante años el driver asíncrono habitual para MongoDB en Python.

Sin embargo, para proyectos nuevos conviene priorizar `AsyncMongoClient`, porque la API asíncrona de PyMongo es el reemplazo recomendado.

Comparación conceptual:

```text
Motor antiguo      -> from motor.motor_asyncio import AsyncIOMotorClient
PyMongo async nuevo -> from pymongo import AsyncMongoClient
```

## Cuándo usar `pymongo-async`

Conviene usar la API asíncrona de PyMongo cuando se necesita:

```text
conectar una aplicación async con MongoDB
usar FastAPI con endpoints async
ejecutar consultas sin bloquear el event loop
manejar muchas solicitudes concurrentes
trabajar con MongoDB Atlas
usar colecciones documentales desde código async
reemplazar Motor en proyectos nuevos
```

## Cuándo no usar `pymongo-async`

No siempre conviene usar esta API cuando se necesita:

```text
un script simple y síncrono
una aplicación pequeña sin concurrencia relevante
evitar async y await
trabajar con bases relacionales SQL
usar PostgreSQL, MySQL o SQL Server
usar Redis
```

Para código síncrono puede ser suficiente:

```python
from pymongo import MongoClient
```

Para bases relacionales suelen corresponder:

```text
sqlalchemy
psycopg
pymysql
pyodbc
oracledb
```

## Instalación

Instalación básica:

```bash
python -m pip install pymongo
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
pymongo==4.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import pymongo

print(pymongo.__version__)
```

## Conexión básica

```python
import asyncio

from pymongo import AsyncMongoClient


async def main():
    client = AsyncMongoClient("mongodb://localhost:27017/")

    database = client["app"]
    collection = database["products"]

    await collection.insert_one({
        "name": "Laptop",
        "price": 3500,
        "stock": 5
    })

    product = await collection.find_one({
        "name": "Laptop"
    })

    print(product)

    await client.close()


asyncio.run(main())
```

## `AsyncMongoClient`

`AsyncMongoClient` representa el cliente asíncrono de conexión con MongoDB.

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient("mongodb://localhost:27017/")
```

Desde el cliente se accede a bases de datos:

```python
database = client["app"]
```

Desde la base de datos se accede a colecciones:

```python
collection = database["products"]
```

## Conexión local

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient("mongodb://localhost:27017/")
```

También puede usarse:

```python
client = AsyncMongoClient("localhost", 27017)
```

## Conexión a MongoDB Atlas

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient(
    "mongodb+srv://user:password@cluster.mongodb.net/?retryWrites=true&w=majority"
)
```

En proyectos reales, la URI debe venir de variables de entorno.

## Variables de entorno

```python
import os

from pymongo import AsyncMongoClient

mongo_uri = os.getenv("MONGO_URI")

client = AsyncMongoClient(mongo_uri)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
MONGO_URI=mongodb://localhost:27017/
MONGO_DATABASE=app
```

Código:

```python
import os

from dotenv import load_dotenv
from pymongo import AsyncMongoClient

load_dotenv()

mongo_uri = os.getenv("MONGO_URI")
database_name = os.getenv("MONGO_DATABASE", "app")

client = AsyncMongoClient(mongo_uri)
database = client[database_name]
```

## Verificar conexión

```python
import asyncio

from pymongo import AsyncMongoClient
from pymongo.errors import ServerSelectionTimeoutError


async def main():
    client = AsyncMongoClient(
        "mongodb://localhost:27017/",
        serverSelectionTimeoutMS=5000
    )

    try:
        await client.admin.command("ping")
        print("Conexión correcta")
    except ServerSelectionTimeoutError:
        print("No se pudo conectar con MongoDB")
    finally:
        await client.close()


asyncio.run(main())
```

## Acceder a base de datos

```python
database = client["app"]
```

También puede usarse:

```python
database = client.app
```

La forma con corchetes suele ser más explícita.

## Acceder a colección

```python
collection = database["products"]
```

También puede usarse:

```python
collection = database.products
```

## Insertar un documento

```python
result = await collection.insert_one({
    "name": "Laptop",
    "price": 3500,
    "stock": 5
})

print(result.inserted_id)
```

## Insertar varios documentos

```python
documents = [
    {"name": "Laptop", "price": 3500, "stock": 5},
    {"name": "Mouse", "price": 80, "stock": 20},
    {"name": "Teclado", "price": 150, "stock": 10}
]

result = await collection.insert_many(documents)

print(result.inserted_ids)
```

## Consultar un documento

```python
product = await collection.find_one({
    "name": "Laptop"
})

print(product)
```

Si no encuentra coincidencias, devuelve `None`.

## Consultar por `_id`

```python
from bson import ObjectId

product = await collection.find_one({
    "_id": ObjectId("64f1a2b3c4d5e6f789012345")
})

print(product)
```

Cuando el `_id` es de tipo `ObjectId`, el texto debe convertirse antes de consultar.

## Consultar varios documentos

`find()` no se espera con `await` directamente. Devuelve un cursor asíncrono.

```python
cursor = collection.find({
    "is_active": True
})

async for product in cursor:
    print(product)
```

## Convertir cursor a lista

```python
cursor = collection.find({
    "is_active": True
})

products = await cursor.to_list(length=100)

print(products)
```

Para traer todos los documentos del cursor:

```python
products = await cursor.to_list(length=None)
```

Debe usarse con cuidado si el resultado puede ser grande.

## Filtros simples

Igualdad:

```python
cursor = collection.find({
    "name": "Laptop"
})
```

Mayor que:

```python
cursor = collection.find({
    "price": {"$gt": 100}
})
```

Mayor o igual que:

```python
cursor = collection.find({
    "price": {"$gte": 100}
})
```

Menor que:

```python
cursor = collection.find({
    "price": {"$lt": 1000}
})
```

Distinto:

```python
cursor = collection.find({
    "name": {"$ne": "Laptop"}
})
```

## Filtro con `$in`

```python
cursor = collection.find({
    "category": {
        "$in": ["Tecnología", "Oficina"]
    }
})
```

## Filtro con `$or`

```python
cursor = collection.find({
    "$or": [
        {"category": "Tecnología"},
        {"price": {"$lt": 100}}
    ]
})
```

## Proyección

Incluir campos:

```python
cursor = collection.find(
    {"is_active": True},
    {"name": 1, "price": 1}
)
```

Excluir `_id`:

```python
cursor = collection.find(
    {"is_active": True},
    {"_id": 0, "name": 1, "price": 1}
)
```

## Ordenar resultados

```python
from pymongo import ASCENDING, DESCENDING

cursor = collection.find().sort("name", ASCENDING)
```

Descendente:

```python
cursor = collection.find().sort("price", DESCENDING)
```

## Limitar resultados

```python
cursor = collection.find().limit(10)
```

## Saltar resultados

```python
cursor = collection.find().skip(10).limit(10)
```

## Contar documentos

```python
count = await collection.count_documents({
    "is_active": True
})

print(count)
```

## Actualizar un documento

```python
result = await collection.update_one(
    {"name": "Laptop"},
    {
        "$set": {
            "price": 3600,
            "stock": 4
        }
    }
)

print(result.modified_count)
```

## Actualizar varios documentos

```python
result = await collection.update_many(
    {"category": "Tecnología"},
    {
        "$set": {
            "is_active": True
        }
    }
)

print(result.modified_count)
```

## Operadores de actualización

## `$set`

```python
await collection.update_one(
    {"name": "Laptop"},
    {"$set": {"price": 3600}}
)
```

## `$inc`

```python
await collection.update_one(
    {"name": "Laptop"},
    {"$inc": {"stock": 1}}
)
```

## `$unset`

```python
await collection.update_one(
    {"name": "Laptop"},
    {"$unset": {"temporary_field": ""}}
)
```

## `$push`

```python
await collection.update_one(
    {"name": "Laptop"},
    {"$push": {"tags": "oferta"}}
)
```

## `$pull`

```python
await collection.update_one(
    {"name": "Laptop"},
    {"$pull": {"tags": "oferta"}}
)
```

## Upsert

`upsert=True` inserta el documento si no existe coincidencia.

```python
await collection.update_one(
    {"sku": "LAP-001"},
    {
        "$set": {
            "name": "Laptop",
            "price": 3500,
            "stock": 5
        }
    },
    upsert=True
)
```

## Eliminar un documento

```python
result = await collection.delete_one({
    "name": "Laptop"
})

print(result.deleted_count)
```

## Eliminar varios documentos

```python
result = await collection.delete_many({
    "is_active": False
})

print(result.deleted_count)
```

## Eliminación lógica

```python
await collection.update_one(
    {"name": "Laptop"},
    {"$set": {"is_active": False}}
)
```

Esto conserva el documento y evita perder información histórica.

## Índices

Crear índice simple:

```python
await collection.create_index("name")
```

Crear índice único:

```python
await collection.create_index(
    "sku",
    unique=True
)
```

Índice compuesto:

```python
from pymongo import ASCENDING, DESCENDING

await collection.create_index([
    ("category", ASCENDING),
    ("price", DESCENDING)
])
```

## Listar índices

```python
async for index in collection.list_indexes():
    print(index)
```

## Eliminar índice

```python
await collection.drop_index("name_1")
```

El nombre exacto puede revisarse con `list_indexes()`.

## Aggregation pipeline

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
            "total_amount": {
                "$sum": "$price"
            },
            "count": {
                "$sum": 1
            }
        }
    },
    {
        "$sort": {
            "total_amount": -1
        }
    }
]

cursor = collection.aggregate(pipeline)

async for row in cursor:
    print(row)
```

## Etapas frecuentes de agregación

```text
$match
$group
$sort
$project
$limit
$skip
$lookup
$unwind
```

## `$match`

```python
{"$match": {"is_active": True}}
```

## `$group`

```python
{
    "$group": {
        "_id": "$category",
        "count": {"$sum": 1}
    }
}
```

## `$project`

```python
{
    "$project": {
        "_id": 0,
        "name": 1,
        "price": 1
    }
}
```

## `$lookup`

```python
{
    "$lookup": {
        "from": "categories",
        "localField": "category_id",
        "foreignField": "_id",
        "as": "category"
    }
}
```

Si se necesitan relaciones complejas con mucha frecuencia, conviene evaluar si una base relacional sería más adecuada.

## Trabajar con `ObjectId`

```python
from bson import ObjectId

value = "64f1a2b3c4d5e6f789012345"

if ObjectId.is_valid(value):
    object_id = ObjectId(value)
```

## Serializar documentos para JSON

Los documentos de MongoDB pueden incluir tipos que no son serializables directamente como JSON, como `ObjectId`.

```python
def serialize_document(document):
    if document is None:
        return None

    document["_id"] = str(document["_id"])

    return document
```

Uso:

```python
product = await collection.find_one({
    "name": "Laptop"
})

return serialize_document(product)
```

## Serializar varios documentos

```python
def serialize_document(document):
    document["_id"] = str(document["_id"])
    return document


async def serialize_cursor(cursor):
    documents = []

    async for document in cursor:
        documents.append(serialize_document(document))

    return documents
```

## Trabajar con fechas

```python
from datetime import datetime, timezone

await collection.insert_one({
    "name": "Reporte",
    "created_at": datetime.now(timezone.utc)
})
```

Conviene guardar fechas con un criterio consistente, especialmente en aplicaciones distribuidas.

## Trabajar con `Decimal128`

```python
from bson.decimal128 import Decimal128
from decimal import Decimal

await collection.insert_one({
    "name": "Laptop",
    "price": Decimal128(Decimal("3500.00"))
})
```

Al leer:

```python
product = await collection.find_one({
    "name": "Laptop"
})

price = product["price"].to_decimal()
```

## Manejo de errores

```python
from pymongo.errors import PyMongoError

try:
    await collection.insert_one({
        "name": "Laptop",
        "price": 3500
    })
except PyMongoError as error:
    print("Error de MongoDB:", error)
```

## Errores frecuentes

```text
PyMongoError
ConnectionFailure
ServerSelectionTimeoutError
DuplicateKeyError
OperationFailure
ConfigurationError
```

## Error de conexión

```python
from pymongo.errors import ServerSelectionTimeoutError

try:
    await client.admin.command("ping")
except ServerSelectionTimeoutError:
    print("No se pudo conectar con MongoDB")
```

## Error de clave duplicada

```python
from pymongo.errors import DuplicateKeyError

try:
    await collection.insert_one({
        "sku": "LAP-001",
        "name": "Laptop"
    })
except DuplicateKeyError:
    print("El SKU ya existe")
```

Para que ocurra este control, debe existir un índice único sobre `sku`.

## Cerrar cliente

```python
await client.close()
```

En aplicaciones web, normalmente el cliente se crea al iniciar la aplicación y se cierra al terminar.

## Uso con FastAPI

```python
import os
from contextlib import asynccontextmanager

from bson import ObjectId
from fastapi import FastAPI, HTTPException
from pymongo import AsyncMongoClient


def serialize_document(document):
    if document is None:
        return None

    document["_id"] = str(document["_id"])

    return document


@asynccontextmanager
async def lifespan(app: FastAPI):
    mongo_uri = os.getenv("MONGO_URI", "mongodb://localhost:27017/")
    database_name = os.getenv("MONGO_DATABASE", "app")

    app.state.mongo_client = AsyncMongoClient(mongo_uri)
    app.state.database = app.state.mongo_client[database_name]

    yield

    await app.state.mongo_client.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    collection = app.state.database["products"]

    cursor = collection.find(
        {"is_active": True},
        {"name": 1, "price": 1, "stock": 1, "is_active": 1}
    ).sort("name", 1)

    products = []

    async for document in cursor:
        products.append(serialize_document(document))

    return products


@app.get("/products/{product_id}")
async def read_product(product_id: str):
    if not ObjectId.is_valid(product_id):
        raise HTTPException(
            status_code=400,
            detail="ID inválido"
        )

    collection = app.state.database["products"]

    document = await collection.find_one({
        "_id": ObjectId(product_id),
        "is_active": True
    })

    if document is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return serialize_document(document)
```

## Uso con Pydantic

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

Uso:

```python
product_create = ProductCreate(
    name="Laptop",
    price=3500,
    stock=5
)

result = await collection.insert_one(
    product_create.model_dump()
)
```

## Uso con pandas

```python
import pandas as pd

cursor = collection.find({
    "is_active": True
})

documents = await cursor.to_list(length=None)

for document in documents:
    document["_id"] = str(document["_id"])

df = pd.DataFrame(documents)

print(df.head())
```

Para resultados grandes, conviene no cargar todo sin filtrar.

## Transacciones

MongoDB permite transacciones en despliegues compatibles.

```python
async with await client.start_session() as session:
    async with session.start_transaction():
        await collection.insert_one(
            {"name": "Laptop", "price": 3500},
            session=session
        )

        await collection.update_one(
            {"name": "Mouse"},
            {"$inc": {"stock": -1}},
            session=session
        )
```

Para operaciones simples sobre un solo documento, normalmente no se requiere una transacción explícita.

## Bulk writes

```python
from pymongo import InsertOne, UpdateOne

operations = [
    InsertOne({
        "name": "Laptop",
        "price": 3500
    }),
    UpdateOne(
        {"name": "Mouse"},
        {"$set": {"price": 80}},
        upsert=True
    )
]

result = await collection.bulk_write(operations)

print(result.inserted_count)
print(result.modified_count)
```

## Estructura recomendada

```text
app/
├─ database.py
├─ repositories.py
├─ schemas.py
└─ main.py
```

## `database.py`

```python
import os

from dotenv import load_dotenv
from pymongo import AsyncMongoClient

load_dotenv()

mongo_uri = os.getenv("MONGO_URI", "mongodb://localhost:27017/")
database_name = os.getenv("MONGO_DATABASE", "app")

client = AsyncMongoClient(mongo_uri)
database = client[database_name]
```

## `schemas.py`

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

## `repositories.py`

```python
from bson import ObjectId

from .database import database

collection = database["products"]


def serialize_document(document):
    if document is None:
        return None

    document["_id"] = str(document["_id"])

    return document


async def list_products():
    cursor = (
        collection
        .find({"is_active": True})
        .sort("name", 1)
    )

    products = []

    async for document in cursor:
        products.append(serialize_document(document))

    return products


async def get_product(product_id):
    if not ObjectId.is_valid(product_id):
        return None

    document = await collection.find_one({
        "_id": ObjectId(product_id),
        "is_active": True
    })

    return serialize_document(document)


async def create_product(product_create):
    data = product_create.model_dump()
    data["is_active"] = True

    result = await collection.insert_one(data)

    return str(result.inserted_id)


async def update_stock(product_id, stock):
    if not ObjectId.is_valid(product_id):
        return 0

    result = await collection.update_one(
        {"_id": ObjectId(product_id)},
        {"$set": {"stock": stock}}
    )

    return result.modified_count


async def deactivate_product(product_id):
    if not ObjectId.is_valid(product_id):
        return 0

    result = await collection.update_one(
        {"_id": ObjectId(product_id)},
        {"$set": {"is_active": False}}
    )

    return result.modified_count
```

## Separación por responsabilidades

```text
database.py      -> cliente y base de datos
schemas.py       -> validación de entrada
repositories.py  -> operaciones de MongoDB
services.py      -> reglas de negocio
main.py          -> aplicación o endpoints
```

## Migración desde Motor

Antes:

```python
from motor.motor_asyncio import AsyncIOMotorClient

client = AsyncIOMotorClient("mongodb://localhost:27017/")
```

Ahora:

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient("mongodb://localhost:27017/")
```

En términos generales, las operaciones de red siguen usando `await`.

```python
document = await collection.find_one({"name": "Laptop"})
```

Para cursores:

```python
cursor = collection.find({})

async for document in cursor:
    print(document)
```

## Diferencia con PyMongo síncrono

PyMongo síncrono:

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
document = collection.find_one({"name": "Laptop"})
```

PyMongo asíncrono:

```python
from pymongo import AsyncMongoClient

client = AsyncMongoClient("mongodb://localhost:27017/")
document = await collection.find_one({"name": "Laptop"})
```

## Errores comunes

## Instalar un paquete inexistente

Problemático:

```bash
python -m pip install pymongo-async
```

Correcto:

```bash
python -m pip install pymongo
```

## Importar mal el cliente

Problemático:

```python
from pymongo_async import AsyncMongoClient
```

Correcto:

```python
from pymongo import AsyncMongoClient
```

## Olvidar `await`

Problemático:

```python
document = collection.find_one({"name": "Laptop"})
```

Correcto:

```python
document = await collection.find_one({"name": "Laptop"})
```

## Usar `await` sobre `find()`

Problemático:

```python
cursor = await collection.find({})
```

Correcto:

```python
cursor = collection.find({})
```

Luego se itera:

```python
async for document in cursor:
    ...
```

## No convertir `ObjectId` a texto al devolver JSON

Problemático:

```python
return document
```

si contiene `_id` como `ObjectId`.

Mejor:

```python
document["_id"] = str(document["_id"])
```

## Consultar `_id` como texto

Problemático:

```python
await collection.find_one({"_id": product_id})
```

Correcto:

```python
await collection.find_one({"_id": ObjectId(product_id)})
```

## No validar `ObjectId`

Problemático:

```python
ObjectId(product_id)
```

si el valor no tiene formato válido.

Mejor:

```python
if ObjectId.is_valid(product_id):
    ...
```

## Crear cliente en cada endpoint

Menos recomendable:

```python
@app.get("/products")
async def read_products():
    client = AsyncMongoClient(...)
```

Mejor crear el cliente al iniciar la aplicación y reutilizarlo.

## No cerrar el cliente

En aplicaciones con ciclo de vida definido, cerrar al finalizar:

```python
await client.close()
```

## Convertir cursores grandes a lista sin control

Problemático:

```python
documents = await cursor.to_list(length=None)
```

si el resultado puede ser muy grande.

Mejor filtrar, limitar o iterar:

```python
async for document in cursor:
    ...
```

## No crear índices

Consultas frecuentes sin índices pueden volverse lentas.

```python
await collection.create_index("sku", unique=True)
```

## Buenas prácticas

## Usar `AsyncMongoClient` para aplicaciones async

```python
from pymongo import AsyncMongoClient
```

## Crear el cliente una vez por aplicación

```python
app.state.mongo_client = AsyncMongoClient(mongo_uri)
```

## Usar variables de entorno

```text
MONGO_URI
MONGO_DATABASE
```

## Usar `await` solo en operaciones asíncronas

```python
await collection.find_one(...)
await collection.insert_one(...)
```

## Iterar cursores con `async for`

```python
async for document in collection.find({}):
    ...
```

## Serializar `ObjectId`

```python
document["_id"] = str(document["_id"])
```

## Validar datos con Pydantic

```python
ProductCreate
```

## Crear índices para búsquedas frecuentes

```python
await collection.create_index("sku", unique=True)
```

## Separar acceso a datos

```text
repositories.py
```

## Evitar Motor en proyectos nuevos

Para código nuevo, priorizar la API asíncrona de PyMongo.

## Ejemplo integrado

```python
import asyncio
import os

from bson import ObjectId
from dotenv import load_dotenv
from pymongo import ASCENDING, AsyncMongoClient
from pymongo.errors import DuplicateKeyError, PyMongoError

load_dotenv()


def serialize_document(document):
    if document is None:
        return None

    document["_id"] = str(document["_id"])

    return document


async def create_client():
    mongo_uri = os.getenv("MONGO_URI", "mongodb://localhost:27017/")

    return AsyncMongoClient(mongo_uri)


async def create_indexes(collection):
    await collection.create_index(
        [("sku", ASCENDING)],
        unique=True
    )


async def create_product(collection, sku, name, price, stock=0):
    document = {
        "sku": sku,
        "name": name,
        "price": price,
        "stock": stock,
        "is_active": True
    }

    try:
        result = await collection.insert_one(document)
    except DuplicateKeyError:
        raise ValueError("El SKU ya existe")

    return str(result.inserted_id)


async def list_products(collection):
    cursor = (
        collection
        .find(
            {"is_active": True},
            {"sku": 1, "name": 1, "price": 1, "stock": 1, "is_active": 1}
        )
        .sort("name", ASCENDING)
    )

    products = []

    async for document in cursor:
        products.append(serialize_document(document))

    return products


async def get_product(collection, product_id):
    if not ObjectId.is_valid(product_id):
        return None

    document = await collection.find_one({
        "_id": ObjectId(product_id),
        "is_active": True
    })

    return serialize_document(document)


async def update_stock(collection, product_id, stock):
    if not ObjectId.is_valid(product_id):
        return 0

    result = await collection.update_one(
        {"_id": ObjectId(product_id)},
        {"$set": {"stock": stock}}
    )

    return result.modified_count


async def deactivate_product(collection, product_id):
    if not ObjectId.is_valid(product_id):
        return 0

    result = await collection.update_one(
        {"_id": ObjectId(product_id)},
        {"$set": {"is_active": False}}
    )

    return result.modified_count


async def main():
    client = await create_client()

    database_name = os.getenv("MONGO_DATABASE", "app")
    database = client[database_name]
    products = database["products"]

    try:
        await client.admin.command("ping")

        await create_indexes(products)

        try:
            await create_product(products, "LAP-001", "Laptop", 3500, stock=5)
            await create_product(products, "MOU-001", "Mouse", 80, stock=20)
        except ValueError:
            pass

        product_list = await list_products(products)

        for product in product_list:
            print(
                product["_id"],
                product["sku"],
                product["name"],
                product["price"],
                product["stock"]
            )

    except PyMongoError as error:
        print("Error de MongoDB:", error)
    finally:
        await client.close()


asyncio.run(main())
```

## Relación con otras librerías

`pymongo-async` se relaciona especialmente con:

```text
pymongo
mongodb
bson
fastapi
starlette
pydantic
pandas
python-dotenv
pytest
pytest-asyncio
```

## Relación con MongoDB

PyMongo Async permite trabajar con MongoDB mediante documentos, colecciones, filtros, agregaciones, índices y operaciones CRUD.

## Relación con BSON

MongoDB almacena documentos en BSON.

Tipos frecuentes:

```python
from bson import ObjectId
from bson.decimal128 import Decimal128
```

## Relación con FastAPI

La API asíncrona de PyMongo encaja con endpoints `async`.

```python
@app.get("/products")
async def read_products():
    ...
```

## Relación con Pydantic

Pydantic puede validar datos antes de insertarlos en MongoDB.

```python
product_create.model_dump()
```

## Relación con pandas

Los documentos pueden convertirse a `DataFrame` después de ser consultados.

```python
df = pd.DataFrame(documents)
```

## Relación con Motor

Motor queda como librería existente para proyectos heredados, pero para documentación nueva conviene priorizar `AsyncMongoClient`.

## Orden didáctico interno

```text
1. Propósito de pymongo-async
2. Instalación mediante pymongo
3. AsyncMongoClient
4. Conexión local y Atlas
5. Bases de datos y colecciones
6. insert_one() e insert_many()
7. find_one() y find()
8. AsyncCursor, async for y to_list()
9. Filtros, proyecciones, ordenamiento y límites
10. update_one(), update_many() y operadores de actualización
11. delete_one(), delete_many() y eliminación lógica
12. Índices
13. Aggregation pipeline
14. ObjectId y serialización JSON
15. Manejo de errores
16. Uso con FastAPI, Pydantic y pandas
17. Migración desde Motor
18. Errores comunes
19. Buenas prácticas
```