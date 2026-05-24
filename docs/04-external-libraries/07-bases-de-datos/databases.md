# `databases`

## Propósito

`databases` es una librería externa para trabajar con bases de datos SQL desde código asíncrono en Python.

Permite conectarse a bases relacionales, ejecutar consultas SQL, usar expresiones de SQLAlchemy Core, manejar conexiones asíncronas y conectar aplicaciones web asíncronas con bases como PostgreSQL, MySQL o SQLite.

No es un ORM completo. Su propósito principal es ofrecer una capa asíncrona simple para ejecutar consultas contra bases SQL.

## Naturaleza de la librería

`databases` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install databases
```

Importación principal:

```python
from databases import Database
```

También suele usarse junto con SQLAlchemy Core:

```python
import sqlalchemy
from databases import Database
```

## Idea central

La idea principal de `databases` es crear un objeto `Database`, conectarlo de forma asíncrona y ejecutar consultas usando `await`.

Flujo típico:

```text
Python async
-> Database
-> SQLAlchemy Core o SQL textual
-> base de datos SQL
```

Ejemplo conceptual:

```python
from databases import Database

database = Database("sqlite+aiosqlite:///app.db")

await database.connect()

rows = await database.fetch_all("SELECT 1 AS value")

await database.disconnect()
```

## Cuándo usar `databases`

Conviene usar `databases` cuando se necesita:

```text
conectar una aplicación async con una base SQL
usar FastAPI o Starlette con base de datos
ejecutar consultas SQL sin bloquear el event loop
usar SQLAlchemy Core sin usar SQLAlchemy ORM
trabajar con PostgreSQL, MySQL o SQLite
mantener una capa de acceso a datos simple
usar queries explícitas
evitar un ORM completo
```

## Cuándo no usar `databases`

No siempre conviene usar `databases` cuando se necesita:

```text
un ORM completo
modelos con relaciones complejas
mapeo automático de tablas a clases
migraciones integradas
consultas ORM avanzadas
trabajar con MongoDB
trabajar con Redis
trabajar solo con archivos CSV o Excel
```

Para ORM completo puede corresponder:

```text
sqlalchemy
sqlmodel
peewee
tortoise-orm
django ORM
```

Para MongoDB:

```text
pymongo
pymongo async
```

Para Redis:

```text
redis
```

## Instalación

Instalación básica:

```bash
python -m pip install databases
```

Para SQLite asíncrono:

```bash
python -m pip install "databases[aiosqlite]"
```

Para PostgreSQL con `asyncpg`:

```bash
python -m pip install "databases[asyncpg]"
```

Para MySQL con driver asíncrono:

```bash
python -m pip install "databases[aiomysql]"
```

También puede instalarse el driver por separado si se prefiere.

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de líneas:

```text
databases==0.x.x
asyncpg==0.x.x
aiosqlite==0.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import databases

print(databases.__version__)
```

## Relación con SQLAlchemy Core

`databases` suele usarse con SQLAlchemy Core.

SQLAlchemy Core permite definir tablas y construir consultas.

`databases` ejecuta esas consultas de forma asíncrona.

Relación conceptual:

```text
SQLAlchemy Core -> define tablas y queries
databases       -> ejecuta queries async
driver async    -> comunica con la base
```

Ejemplo:

```python
import sqlalchemy
from databases import Database

metadata = sqlalchemy.MetaData()

products = sqlalchemy.Table(
    "products",
    metadata,
    sqlalchemy.Column("id", sqlalchemy.Integer, primary_key=True),
    sqlalchemy.Column("name", sqlalchemy.String),
    sqlalchemy.Column("price", sqlalchemy.Numeric),
)

database = Database("sqlite+aiosqlite:///app.db")
```

## Diferencia frente a SQLAlchemy ORM

## SQLAlchemy ORM

Mapea tablas a clases.

```python
class Product(Base):
    ...
```

## `databases`

Ejecuta consultas asíncronas.

```python
rows = await database.fetch_all(query)
```

No crea por sí mismo un modelo ORM completo.

## Regla práctica

```text
Consultas async con SQLAlchemy Core -> databases
ORM completo síncrono o async        -> SQLAlchemy
API con modelos Pydantic y SQL       -> SQLModel
```

## URLs de conexión

## SQLite

```python
database = Database("sqlite+aiosqlite:///app.db")
```

## PostgreSQL

```python
database = Database(
    "postgresql+asyncpg://user:password@localhost:5432/app"
)
```

## MySQL

```python
database = Database(
    "mysql+aiomysql://user:password@localhost:3306/app"
)
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

```python
import os

from databases import Database

database_url = os.getenv("DATABASE_URL")

if database_url is None:
    raise RuntimeError("DATABASE_URL no está configurada")

database = Database(database_url)
```

## Uso con `python-dotenv`

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
DATABASE_URL=sqlite+aiosqlite:///app.db
```

Código:

```python
import os

from databases import Database
from dotenv import load_dotenv

load_dotenv()

database_url = os.getenv("DATABASE_URL", "sqlite+aiosqlite:///app.db")

database = Database(database_url)
```

## Crear objeto `Database`

```python
from databases import Database

database = Database("sqlite+aiosqlite:///app.db")
```

Este objeto representa la conexión lógica con la base.

Antes de ejecutar consultas, debe conectarse.

## Conectar

```python
await database.connect()
```

## Desconectar

```python
await database.disconnect()
```

En aplicaciones web, la conexión suele abrirse al iniciar la aplicación y cerrarse al finalizar.

## Script mínimo

```python
import asyncio

from databases import Database


async def main():
    database = Database("sqlite+aiosqlite:///app.db")

    await database.connect()

    try:
        value = await database.fetch_val("SELECT 1")
        print(value)
    finally:
        await database.disconnect()


asyncio.run(main())
```

## Crear tablas

`databases` no es una herramienta de migraciones.

Para crear tablas en ejemplos pequeños puede usarse SQLAlchemy Core.

```python
import sqlalchemy

metadata = sqlalchemy.MetaData()

products = sqlalchemy.Table(
    "products",
    metadata,
    sqlalchemy.Column("id", sqlalchemy.Integer, primary_key=True),
    sqlalchemy.Column("name", sqlalchemy.String(length=100), nullable=False),
    sqlalchemy.Column("price", sqlalchemy.Numeric(10, 2), nullable=False),
    sqlalchemy.Column("stock", sqlalchemy.Integer, nullable=False, default=0),
)
```

Para crear físicamente las tablas puede usarse un engine síncrono de SQLAlchemy en prototipos:

```python
from sqlalchemy import create_engine

engine = create_engine("sqlite:///app.db")

metadata.create_all(engine)
```

En proyectos reales, conviene usar migraciones con Alembic.

## Definir tabla con SQLAlchemy Core

```python
import sqlalchemy

metadata = sqlalchemy.MetaData()

products = sqlalchemy.Table(
    "products",
    metadata,
    sqlalchemy.Column("id", sqlalchemy.Integer, primary_key=True),
    sqlalchemy.Column("name", sqlalchemy.String(length=100), nullable=False, unique=True),
    sqlalchemy.Column("price", sqlalchemy.Numeric(10, 2), nullable=False),
    sqlalchemy.Column("stock", sqlalchemy.Integer, nullable=False, server_default="0"),
    sqlalchemy.Column("is_active", sqlalchemy.Boolean, nullable=False, server_default="1"),
)
```

## Insertar datos

Con SQLAlchemy Core:

```python
query = products.insert().values(
    name="Laptop",
    price=3500,
    stock=5,
    is_active=True
)

product_id = await database.execute(query)
```

`execute()` devuelve el valor generado cuando el backend lo permite.

## Insertar varios registros

```python
query = products.insert()

values = [
    {
        "name": "Laptop",
        "price": 3500,
        "stock": 5,
        "is_active": True,
    },
    {
        "name": "Mouse",
        "price": 80,
        "stock": 20,
        "is_active": True,
    },
]

await database.execute_many(
    query=query,
    values=values
)
```

## Consultar todos los registros

```python
query = products.select()

rows = await database.fetch_all(query)

for row in rows:
    print(row["name"])
```

## Consultar un registro

```python
query = products.select().where(
    products.c.id == 1
)

row = await database.fetch_one(query)

print(row)
```

`fetch_one()` devuelve una fila o `None`.

## Consultar un valor

```python
query = sqlalchemy.select(
    sqlalchemy.func.count(products.c.id)
)

count = await database.fetch_val(query)

print(count)
```

## `fetch_all()`

Devuelve una lista de filas.

```python
rows = await database.fetch_all(query)
```

Uso típico:

```python
for row in rows:
    print(row["name"])
```

## `fetch_one()`

Devuelve una fila o `None`.

```python
row = await database.fetch_one(query)
```

## `fetch_val()`

Devuelve un valor único.

```python
value = await database.fetch_val(query)
```

Es útil para conteos, sumas, IDs o consultas que devuelven una sola celda.

## `execute()`

Ejecuta una sentencia y devuelve un resultado asociado, como un ID generado si corresponde.

```python
product_id = await database.execute(query)
```

## `execute_many()`

Ejecuta una sentencia para varios registros.

```python
await database.execute_many(
    query=query,
    values=values
)
```

## Acceso a columnas

Las filas pueden accederse por nombre.

```python
row = await database.fetch_one(query)

if row is not None:
    print(row["name"])
    print(row["price"])
```

También puede convertirse a diccionario:

```python
data = dict(row)
```

## Filtrar registros

```python
query = products.select().where(
    products.c.price >= 100
)

rows = await database.fetch_all(query)
```

## Varias condiciones

```python
query = products.select().where(
    sqlalchemy.and_(
        products.c.price >= 100,
        products.c.is_active == True
    )
)
```

También puede usarse el operador `&`:

```python
query = products.select().where(
    (products.c.price >= 100) &
    (products.c.is_active == True)
)
```

## Ordenar resultados

```python
query = products.select().order_by(
    products.c.name
)
```

Orden descendente:

```python
query = products.select().order_by(
    products.c.price.desc()
)
```

## Limitar resultados

```python
query = products.select().limit(10)
```

Con desplazamiento:

```python
query = products.select().offset(10).limit(10)
```

## Actualizar datos

```python
query = (
    products
    .update()
    .where(products.c.id == 1)
    .values(price=3600, stock=4)
)

await database.execute(query)
```

## Eliminar datos

```python
query = (
    products
    .delete()
    .where(products.c.id == 1)
)

await database.execute(query)
```

## Eliminación lógica

En aplicaciones reales, muchas veces se prefiere marcar registros como inactivos.

```python
query = (
    products
    .update()
    .where(products.c.id == 1)
    .values(is_active=False)
)

await database.execute(query)
```

Consulta de activos:

```python
query = products.select().where(
    products.c.is_active == True
)
```

## SQL textual

También puede ejecutarse SQL textual.

```python
rows = await database.fetch_all("""
    SELECT id, name, price, stock
    FROM products
    ORDER BY name
""")
```

Con parámetros:

```python
row = await database.fetch_one(
    """
    SELECT id, name, price, stock
    FROM products
    WHERE id = :product_id
    """,
    values={
        "product_id": 1
    }
)
```

## Parámetros SQL

Cuando se usa SQL textual con `databases`, conviene usar parámetros nombrados.

```python
await database.fetch_one(
    """
    SELECT *
    FROM products
    WHERE name = :name
    """,
    values={
        "name": "Laptop"
    }
)
```

No conviene concatenar valores dentro de la consulta.

## Evitar concatenar SQL

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"

row = await database.fetch_one(query)
```

Más seguro:

```python
row = await database.fetch_one(
    """
    SELECT *
    FROM products
    WHERE name = :name
    """,
    values={
        "name": name
    }
)
```

## Transacciones

`databases` permite usar transacciones.

```python
async with database.transaction():
    await database.execute(
        products.insert().values(
            name="Laptop",
            price=3500,
            stock=5
        )
    )

    await database.execute(
        products.insert().values(
            name="Mouse",
            price=80,
            stock=20
        )
    )
```

Si ocurre un error dentro del bloque, la transacción se revierte.

Si el bloque termina correctamente, se confirma.

## Transacción manual

```python
transaction = await database.transaction()

try:
    await database.execute(...)
except Exception:
    await transaction.rollback()
    raise
else:
    await transaction.commit()
```

El patrón con `async with database.transaction()` suele ser más claro.

## Consultas con joins

```python
categories = sqlalchemy.Table(
    "categories",
    metadata,
    sqlalchemy.Column("id", sqlalchemy.Integer, primary_key=True),
    sqlalchemy.Column("name", sqlalchemy.String(length=100), nullable=False),
)

query = (
    sqlalchemy
    .select(
        products.c.id,
        products.c.name,
        products.c.price,
        categories.c.name.label("category")
    )
    .select_from(
        products.join(
            categories,
            products.c.category_id == categories.c.id
        )
    )
)

rows = await database.fetch_all(query)
```

## Agregaciones

```python
query = sqlalchemy.select(
    sqlalchemy.func.count(products.c.id)
)

count = await database.fetch_val(query)
```

## Agrupar

```python
query = (
    sqlalchemy
    .select(
        products.c.is_active,
        sqlalchemy.func.count(products.c.id).label("count")
    )
    .group_by(products.c.is_active)
)

rows = await database.fetch_all(query)
```

## Uso con FastAPI

`databases` encaja bien con el ciclo de vida de FastAPI.

```python
import os
from contextlib import asynccontextmanager

from databases import Database
from fastapi import FastAPI, HTTPException

database = Database(
    os.getenv("DATABASE_URL", "sqlite+aiosqlite:///app.db")
)


@asynccontextmanager
async def lifespan(app: FastAPI):
    await database.connect()
    yield
    await database.disconnect()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    rows = await database.fetch_all("""
        SELECT id, name, price, stock
        FROM products
        WHERE is_active = 1
        ORDER BY name
    """)

    return [
        dict(row)
        for row in rows
    ]


@app.get("/products/{product_id}")
async def read_product(product_id: int):
    row = await database.fetch_one(
        """
        SELECT id, name, price, stock
        FROM products
        WHERE id = :product_id
        """,
        values={
            "product_id": product_id
        }
    )

    if row is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return dict(row)
```

## Uso con SQLAlchemy Core en FastAPI

```python
import os
from contextlib import asynccontextmanager

import sqlalchemy
from databases import Database
from fastapi import FastAPI, HTTPException

metadata = sqlalchemy.MetaData()

products = sqlalchemy.Table(
    "products",
    metadata,
    sqlalchemy.Column("id", sqlalchemy.Integer, primary_key=True),
    sqlalchemy.Column("name", sqlalchemy.String(length=100), nullable=False),
    sqlalchemy.Column("price", sqlalchemy.Numeric(10, 2), nullable=False),
    sqlalchemy.Column("stock", sqlalchemy.Integer, nullable=False, server_default="0"),
    sqlalchemy.Column("is_active", sqlalchemy.Boolean, nullable=False, server_default="1"),
)

database = Database(
    os.getenv("DATABASE_URL", "sqlite+aiosqlite:///app.db")
)


@asynccontextmanager
async def lifespan(app: FastAPI):
    await database.connect()
    yield
    await database.disconnect()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    query = (
        products
        .select()
        .where(products.c.is_active == True)
        .order_by(products.c.name)
    )

    rows = await database.fetch_all(query)

    return [
        dict(row)
        for row in rows
    ]
```

## Uso con Starlette

```python
from databases import Database
from starlette.applications import Starlette
from starlette.responses import JSONResponse
from starlette.routing import Route

database = Database("sqlite+aiosqlite:///app.db")


async def read_products(request):
    rows = await database.fetch_all("""
        SELECT id, name, price
        FROM products
    """)

    return JSONResponse([
        dict(row)
        for row in rows
    ])


async def startup():
    await database.connect()


async def shutdown():
    await database.disconnect()


app = Starlette(
    routes=[
        Route("/products", read_products)
    ],
    on_startup=[startup],
    on_shutdown=[shutdown],
)
```

## Uso con Pydantic

`databases` no valida datos por sí mismo.

En APIs, suele combinarse con Pydantic.

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

Uso:

```python
async def create_product(product: ProductCreate):
    query = products.insert().values(
        name=product.name,
        price=product.price,
        stock=product.stock,
        is_active=True
    )

    product_id = await database.execute(query)

    return {
        "id": product_id,
        **product.model_dump()
    }
```

## Uso con pandas

`databases` es asíncrono y no es la opción principal para análisis tabular.

Pero puede alimentar un DataFrame.

```python
import pandas as pd

rows = await database.fetch_all("""
    SELECT id, name, price, stock
    FROM products
""")

df = pd.DataFrame([
    dict(row)
    for row in rows
])

print(df.head())
```

Para análisis local intensivo puede convenir:

```text
pandas
polars
duckdb
```

## Migraciones

`databases` no es una herramienta de migraciones.

Para gestionar cambios de esquema suele usarse:

```text
alembic
```

Flujo conceptual:

```text
SQLAlchemy Core define tablas
Alembic gestiona migraciones
databases ejecuta queries async
```

En prototipos puede usarse:

```python
metadata.create_all(engine)
```

En proyectos reales, conviene usar migraciones versionadas.

## Organización recomendada

```text
app/
├─ database.py
├─ tables.py
├─ schemas.py
├─ repositories.py
├─ services.py
└─ main.py
```

## `database.py`

```python
import os

from databases import Database
from dotenv import load_dotenv

load_dotenv()

database_url = os.getenv("DATABASE_URL", "sqlite+aiosqlite:///app.db")

database = Database(database_url)
```

## `tables.py`

```python
import sqlalchemy

metadata = sqlalchemy.MetaData()

products = sqlalchemy.Table(
    "products",
    metadata,
    sqlalchemy.Column("id", sqlalchemy.Integer, primary_key=True),
    sqlalchemy.Column("name", sqlalchemy.String(length=100), nullable=False, unique=True),
    sqlalchemy.Column("price", sqlalchemy.Numeric(10, 2), nullable=False),
    sqlalchemy.Column("stock", sqlalchemy.Integer, nullable=False, server_default="0"),
    sqlalchemy.Column("is_active", sqlalchemy.Boolean, nullable=False, server_default="1"),
)
```

## `schemas.py`

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
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
from .database import database
from .tables import products


async def list_products():
    query = (
        products
        .select()
        .where(products.c.is_active == True)
        .order_by(products.c.name)
    )

    rows = await database.fetch_all(query)

    return [
        dict(row)
        for row in rows
    ]


async def get_product(product_id: int):
    query = products.select().where(
        products.c.id == product_id
    )

    row = await database.fetch_one(query)

    if row is None:
        return None

    return dict(row)


async def create_product(product_create):
    query = products.insert().values(
        name=product_create.name,
        price=product_create.price,
        stock=product_create.stock,
        is_active=True
    )

    return await database.execute(query)


async def deactivate_product(product_id: int):
    query = (
        products
        .update()
        .where(products.c.id == product_id)
        .values(is_active=False)
    )

    await database.execute(query)
```

## Separación por responsabilidades

```text
database.py      -> objeto Database y URL de conexión
tables.py        -> tablas de SQLAlchemy Core
schemas.py       -> modelos Pydantic
repositories.py  -> operaciones de base de datos
services.py      -> reglas de negocio
main.py          -> aplicación
```

## Errores comunes

## No instalar el driver asíncrono

Problemático:

```bash
python -m pip install databases
```

y luego usar:

```python
sqlite+aiosqlite:///app.db
```

sin tener `aiosqlite`.

Solución:

```bash
python -m pip install "databases[aiosqlite]"
```

## No conectar antes de consultar

Problemático:

```python
rows = await database.fetch_all("SELECT 1")
```

sin haber ejecutado:

```python
await database.connect()
```

## No desconectar al finalizar

Debe cerrarse la conexión:

```python
await database.disconnect()
```

## Olvidar `await`

Problemático:

```python
rows = database.fetch_all(query)
```

Correcto:

```python
rows = await database.fetch_all(query)
```

## Confundir `databases` con un ORM

`databases` no define modelos ORM completos.

Para ORM puede usarse:

```text
SQLAlchemy ORM
SQLModel
Tortoise ORM
Peewee
```

## Concatenar valores en SQL textual

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"
```

Correcto:

```python
await database.fetch_one(
    "SELECT * FROM products WHERE name = :name",
    values={"name": name}
)
```

## Usar `metadata.create_all()` como migración de producción

`create_all()` no reemplaza migraciones.

Para producción, usar Alembic.

## Convertir resultados grandes a lista sin control

```python
rows = await database.fetch_all("SELECT * FROM large_table")
```

puede consumir mucha memoria.

Conviene paginar:

```sql
LIMIT :limit OFFSET :offset
```

## Guardar credenciales en el código

Problemático:

```python
Database("postgresql+asyncpg://user:password@localhost/app")
```

Mejor:

```python
Database(os.getenv("DATABASE_URL"))
```

## Buenas prácticas

## Usar variables de entorno

```text
DATABASE_URL
```

## Conectar y desconectar en el ciclo de vida de la app

```python
await database.connect()
await database.disconnect()
```

## Usar SQLAlchemy Core para queries estructuradas

```python
products.select().where(...)
```

## Usar parámetros en SQL textual

```python
:name
```

## Separar tablas, schemas y repositories

```text
tables.py
schemas.py
repositories.py
```

## Usar Alembic para migraciones

```bash
alembic revision --autogenerate -m "create products table"
alembic upgrade head
```

## Usar transacciones en operaciones relacionadas

```python
async with database.transaction():
    ...
```

## Paginar resultados grandes

```sql
LIMIT :limit OFFSET :offset
```

## No usarlo como reemplazo automático de un ORM

`databases` es una capa de ejecución async, no un sistema completo de modelado ORM.

## Ejemplo integrado

```python
import asyncio
import os
from decimal import Decimal

import sqlalchemy
from databases import Database
from dotenv import load_dotenv
from sqlalchemy import create_engine

load_dotenv()

sync_database_url = os.getenv("SYNC_DATABASE_URL", "sqlite:///app.db")
async_database_url = os.getenv("DATABASE_URL", "sqlite+aiosqlite:///app.db")

metadata = sqlalchemy.MetaData()

products = sqlalchemy.Table(
    "products",
    metadata,
    sqlalchemy.Column("id", sqlalchemy.Integer, primary_key=True),
    sqlalchemy.Column("name", sqlalchemy.String(length=100), nullable=False, unique=True),
    sqlalchemy.Column("price", sqlalchemy.Numeric(10, 2), nullable=False),
    sqlalchemy.Column("stock", sqlalchemy.Integer, nullable=False, server_default="0"),
    sqlalchemy.Column("is_active", sqlalchemy.Boolean, nullable=False, server_default="1"),
)

database = Database(async_database_url)


def create_tables():
    engine = create_engine(sync_database_url)
    metadata.create_all(engine)


async def create_product(name, price, stock=0):
    query = products.insert().values(
        name=name,
        price=Decimal(str(price)),
        stock=stock,
        is_active=True
    )

    return await database.execute(query)


async def list_products():
    query = (
        products
        .select()
        .where(products.c.is_active == True)
        .order_by(products.c.name)
    )

    rows = await database.fetch_all(query)

    return [
        dict(row)
        for row in rows
    ]


async def get_product(product_id):
    query = products.select().where(
        products.c.id == product_id
    )

    row = await database.fetch_one(query)

    if row is None:
        return None

    return dict(row)


async def update_stock(product_id, stock):
    query = (
        products
        .update()
        .where(products.c.id == product_id)
        .values(stock=stock)
    )

    await database.execute(query)


async def deactivate_product(product_id):
    query = (
        products
        .update()
        .where(products.c.id == product_id)
        .values(is_active=False)
    )

    await database.execute(query)


async def main():
    create_tables()

    await database.connect()

    try:
        try:
            await create_product("Laptop", 3500, stock=5)
            await create_product("Mouse", 80, stock=20)
        except Exception:
            pass

        product_list = await list_products()

        for product in product_list:
            print(
                product["id"],
                product["name"],
                product["price"],
                product["stock"]
            )

    finally:
        await database.disconnect()


asyncio.run(main())
```

## Relación con otras librerías

`databases` se relaciona especialmente con:

```text
sqlalchemy
asyncpg
aiosqlite
aiomysql
fastapi
starlette
pydantic
alembic
python-dotenv
pytest
pytest-asyncio
```

## Relación con SQLAlchemy

SQLAlchemy Core define tablas y consultas.

`databases` ejecuta esas consultas de forma asíncrona.

## Relación con FastAPI

FastAPI puede usar `databases` en endpoints `async`.

La conexión debe abrirse al iniciar la aplicación y cerrarse al finalizar.

## Relación con Alembic

Alembic puede gestionar migraciones cuando se usan tablas SQLAlchemy Core.

`databases` no reemplaza a Alembic.

## Relación con asyncpg y aiosqlite

`databases` usa drivers asíncronos por debajo.

Ejemplos:

```text
PostgreSQL -> asyncpg
SQLite     -> aiosqlite
MySQL      -> aiomysql
```

## Orden didáctico interno

```text
1. Propósito de databases
2. Instalación
3. Relación con SQLAlchemy Core
4. Database
5. URLs de conexión
6. connect() y disconnect()
7. fetch_all(), fetch_one(), fetch_val()
8. execute() y execute_many()
9. Consultas con SQLAlchemy Core
10. SQL textual con parámetros
11. INSERT, UPDATE y DELETE
12. Transacciones
13. Uso con FastAPI y Starlette
14. Uso con Pydantic
15. Migraciones con Alembic
16. Organización recomendada
17. Errores comunes
18. Buenas prácticas
```