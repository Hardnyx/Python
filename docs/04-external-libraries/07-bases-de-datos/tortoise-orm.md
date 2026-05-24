# `tortoise-orm`

## Propósito

`tortoise-orm` es una librería externa para trabajar con bases de datos SQL mediante un ORM asíncrono.

Permite definir tablas como clases de Python, representar registros como objetos, consultar datos con una API de alto nivel y ejecutar operaciones de base de datos usando `async` y `await`.

Está inspirada en el ORM de Django, pero diseñada para aplicaciones asíncronas.

## Naturaleza de la librería

`tortoise-orm` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install tortoise-orm
```

Importaciones frecuentes:

```python
from tortoise import Tortoise
from tortoise import fields
from tortoise.models import Model
```

También puede integrarse con frameworks asíncronos como FastAPI.

## Idea central

La idea principal de `tortoise-orm` es definir modelos como clases y ejecutar operaciones contra la base de datos de forma asíncrona.

Ejemplo básico:

```python
from tortoise import fields
from tortoise.models import Model


class Product(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100)
    price = fields.DecimalField(max_digits=10, decimal_places=2)
    stock = fields.IntField(default=0)
    is_active = fields.BooleanField(default=True)

    def __str__(self):
        return self.name
```

La clase `Product` representa una tabla.

Cada instancia de `Product` representa un registro.

Cada campo representa una columna.

## Cuándo usar Tortoise ORM

Conviene usar `tortoise-orm` cuando se necesita:

```text
un ORM asíncrono
trabajar con FastAPI o Starlette
usar async y await para consultas SQL
definir modelos relacionales como clases
usar una API parecida a Django ORM
trabajar con PostgreSQL, SQLite, MySQL u otras bases compatibles
mantener consultas de base de datos no bloqueantes
```

## Cuándo no usar Tortoise ORM

No siempre conviene usar `tortoise-orm` cuando se necesita:

```text
un script simple y síncrono
usar Django ORM
control muy avanzado de SQLAlchemy
consultas SQL altamente personalizadas
bases no relacionales como MongoDB
caché o almacenamiento temporal como Redis
```

Para scripts síncronos puede bastar con:

```text
sqlite3
psycopg
pymysql
pyodbc
```

Para ORM avanzado puede corresponder:

```text
sqlalchemy
```

Para aplicaciones Django, normalmente se usa el ORM propio de Django.

## Instalación

Instalación básica:

```bash
python -m pip install tortoise-orm
```

Para PostgreSQL, puede instalarse con soporte para `asyncpg`:

```bash
python -m pip install "tortoise-orm[asyncpg]"
```

Para MySQL o MariaDB, puede instalarse con soporte correspondiente según el driver utilizado:

```bash
python -m pip install "tortoise-orm[asyncmy]"
```

Para SQLite, Tortoise ORM puede trabajar con `aiosqlite`.

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
tortoise-orm==1.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import tortoise

print(tortoise.__version__)
```

## Modelo básico

```python
from tortoise import fields
from tortoise.models import Model


class Product(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100)
    price = fields.DecimalField(max_digits=10, decimal_places=2)
    stock = fields.IntField(default=0)
    is_active = fields.BooleanField(default=True)
```

## Clase `Model`

Todos los modelos heredan de `Model`.

```python
from tortoise.models import Model
```

Ejemplo:

```python
class Product(Model):
    ...
```

## Campos

Los campos se importan desde `tortoise.fields`.

```python
from tortoise import fields
```

Campos frecuentes:

```python
fields.IntField()
fields.CharField()
fields.TextField()
fields.DecimalField()
fields.FloatField()
fields.BooleanField()
fields.DateField()
fields.DatetimeField()
fields.ForeignKeyField()
fields.ManyToManyField()
```

## Clave primaria

```python
id = fields.IntField(pk=True)
```

`pk=True` indica que el campo es la clave primaria.

## Texto corto

```python
name = fields.CharField(max_length=100)
```

## Texto largo

```python
description = fields.TextField(null=True)
```

## Número decimal

```python
price = fields.DecimalField(max_digits=10, decimal_places=2)
```

## Booleano

```python
is_active = fields.BooleanField(default=True)
```

## Fechas

```python
created_at = fields.DatetimeField(auto_now_add=True)
updated_at = fields.DatetimeField(auto_now=True)
```

## Configuración de conexión

Antes de usar los modelos, Tortoise debe inicializarse.

```python
from tortoise import Tortoise


async def init_database():
    await Tortoise.init(
        db_url="sqlite://app.db",
        modules={
            "models": ["app.models"]
        }
    )

    await Tortoise.generate_schemas()
```

## `Tortoise.init()`

Inicializa la conexión y registra los módulos donde están los modelos.

```python
await Tortoise.init(
    db_url="sqlite://app.db",
    modules={
        "models": ["app.models"]
    }
)
```

## `db_url`

Define la conexión con la base de datos.

SQLite:

```python
"sqlite://app.db"
```

PostgreSQL:

```python
"postgres://user:password@localhost:5432/app"
```

MySQL:

```python
"mysql://user:password@localhost:3306/app"
```

## `modules`

Indica dónde buscar modelos.

```python
modules={
    "models": ["app.models"]
}
```

Si los modelos están en varios archivos:

```python
modules={
    "models": [
        "app.products.models",
        "app.clients.models"
    ]
}
```

## Crear esquemas

```python
await Tortoise.generate_schemas()
```

Este método crea las tablas según los modelos.

Debe usarse con cuidado en proyectos reales.

Para producción, conviene usar migraciones.

## Cerrar conexiones

```python
await Tortoise.close_connections()
```

Es importante cerrar conexiones al finalizar una aplicación o script.

## Script mínimo

```python
import asyncio

from tortoise import Tortoise, fields
from tortoise.models import Model


class Product(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100)
    price = fields.DecimalField(max_digits=10, decimal_places=2)
    stock = fields.IntField(default=0)


async def main():
    await Tortoise.init(
        db_url="sqlite://app.db",
        modules={
            "models": ["__main__"]
        }
    )

    await Tortoise.generate_schemas()

    product = await Product.create(
        name="Laptop",
        price=3500,
        stock=5
    )

    print(product.id, product.name)

    await Tortoise.close_connections()


asyncio.run(main())
```

## Crear registros

```python
product = await Product.create(
    name="Laptop",
    price=3500,
    stock=5
)
```

`create()` inserta el registro y devuelve el objeto creado.

## Crear instancia y guardar

```python
product = Product(
    name="Mouse",
    price=80,
    stock=20
)

await product.save()
```

## Consultar todos los registros

```python
products = await Product.all()
```

Uso:

```python
for product in products:
    print(product.name)
```

## Filtrar registros

```python
products = await Product.filter(
    is_active=True
)
```

Filtro por comparación:

```python
products = await Product.filter(
    price__gte=100
)
```

## Obtener un registro

```python
product = await Product.get(id=1)
```

Si no existe, se genera una excepción.

## `get_or_none()`

```python
product = await Product.get_or_none(id=1)

if product is not None:
    print(product.name)
```

Este patrón es útil cuando el registro puede no existir.

## Primer resultado

```python
product = await Product.filter(
    is_active=True
).first()
```

## Ordenar resultados

```python
products = await Product.all().order_by("name")
```

Orden descendente:

```python
products = await Product.all().order_by("-price")
```

## Limitar resultados

```python
products = await Product.all().limit(10)
```

Con desplazamiento:

```python
products = await Product.all().offset(10).limit(10)
```

## Contar registros

```python
count = await Product.filter(
    is_active=True
).count()

print(count)
```

## Actualizar un registro

```python
product = await Product.get_or_none(id=1)

if product is not None:
    product.price = 3600
    product.stock = 4

    await product.save()
```

## Actualización masiva

```python
updated_count = await Product.filter(
    is_active=False
).update(
    stock=0
)

print(updated_count)
```

## Eliminar registro

```python
product = await Product.get_or_none(id=1)

if product is not None:
    await product.delete()
```

## Eliminación masiva

```python
deleted_count = await Product.filter(
    is_active=False
).delete()

print(deleted_count)
```

## Eliminación lógica

En aplicaciones reales, muchas veces conviene marcar registros como inactivos.

```python
product = await Product.get_or_none(id=1)

if product is not None:
    product.is_active = False
    await product.save()
```

Consulta de activos:

```python
products = await Product.filter(is_active=True)
```

## Filtros frecuentes

Mayor o igual:

```python
Product.filter(price__gte=100)
```

Menor o igual:

```python
Product.filter(price__lte=1000)
```

Contiene texto:

```python
Product.filter(name__icontains="lap")
```

Está en lista:

```python
Product.filter(id__in=[1, 2, 3])
```

No nulo:

```python
Product.filter(description__not_isnull=True)
```

## Relaciones

Tortoise ORM permite definir relaciones entre modelos.

## Uno a muchos

Ejemplo conceptual:

```text
Category -> Product
una categoría tiene muchos productos
un producto pertenece a una categoría
```

Código:

```python
from tortoise import fields
from tortoise.models import Model


class Category(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100, unique=True)

    products: fields.ReverseRelation["Product"]


class Product(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100)
    price = fields.DecimalField(max_digits=10, decimal_places=2)

    category = fields.ForeignKeyField(
        "models.Category",
        related_name="products"
    )
```

## `ForeignKeyField`

```python
category = fields.ForeignKeyField(
    "models.Category",
    related_name="products"
)
```

Define una relación muchos a uno.

## Crear objetos relacionados

```python
category = await Category.create(
    name="Tecnología"
)

product = await Product.create(
    name="Laptop",
    price=3500,
    category=category
)
```

## Consultar relación

```python
product = await Product.get(id=1).prefetch_related("category")

print(product.category.name)
```

## Relación inversa

```python
category = await Category.get(name="Tecnología")

products = await category.products.all()
```

## `prefetch_related()`

Permite cargar relaciones para evitar consultas adicionales repetidas.

```python
products = await Product.all().prefetch_related("category")
```

Uso:

```python
for product in products:
    print(product.name, product.category.name)
```

## Muchos a muchos

```python
class Tag(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=50, unique=True)


class Product(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100)

    tags = fields.ManyToManyField(
        "models.Tag",
        related_name="products"
    )
```

Agregar relación:

```python
product = await Product.get(id=1)
tag = await Tag.get(name="oferta")

await product.tags.add(tag)
```

Consultar:

```python
tags = await product.tags.all()
```

## Valores como diccionarios

```python
products = await Product.all().values(
    "id",
    "name",
    "price",
    "stock"
)
```

Esto devuelve estructuras tipo diccionario.

## Valores como lista

```python
names = await Product.all().values_list(
    "name",
    flat=True
)
```

## Consultas con campos relacionados

```python
products = await Product.filter(
    category__name="Tecnología"
)
```

## Agregaciones

```python
from tortoise.functions import Count

result = await Product.all().annotate(
    total=Count("id")
).values("total")
```

Para agregaciones más complejas, conviene revisar la API disponible y, si el caso supera al ORM, considerar SQL directo o SQLAlchemy.

## Transacciones

Tortoise ORM permite usar transacciones.

```python
from tortoise.transactions import in_transaction


async with in_transaction():
    await Product.create(
        name="Laptop",
        price=3500,
        stock=5
    )

    await Product.create(
        name="Mouse",
        price=80,
        stock=20
    )
```

Si ocurre un error dentro del bloque, la transacción se revierte.

## Transacción con conexión

```python
from tortoise.transactions import in_transaction


async with in_transaction() as connection:
    await Product.create(
        name="Laptop",
        price=3500,
        using_db=connection
    )
```

## Uso con FastAPI

Tortoise ORM puede integrarse con FastAPI mediante el ciclo de vida de la aplicación.

```python
import os
from contextlib import asynccontextmanager

from fastapi import FastAPI, HTTPException
from tortoise import Tortoise, fields
from tortoise.models import Model


class Product(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100)
    price = fields.DecimalField(max_digits=10, decimal_places=2)
    stock = fields.IntField(default=0)
    is_active = fields.BooleanField(default=True)


@asynccontextmanager
async def lifespan(app: FastAPI):
    await Tortoise.init(
        db_url=os.getenv("DATABASE_URL", "sqlite://app.db"),
        modules={
            "models": ["__main__"]
        }
    )

    await Tortoise.generate_schemas()

    yield

    await Tortoise.close_connections()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    return await Product.filter(is_active=True).values(
        "id",
        "name",
        "price",
        "stock"
    )


@app.get("/products/{product_id}")
async def read_product(product_id: int):
    product = await Product.get_or_none(
        id=product_id,
        is_active=True
    )

    if product is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return {
        "id": product.id,
        "name": product.name,
        "price": product.price,
        "stock": product.stock
    }
```

## Uso con Pydantic

Tortoise ORM puede generar modelos Pydantic desde modelos ORM.

```python
from tortoise.contrib.pydantic import pydantic_model_creator

ProductRead = pydantic_model_creator(
    Product,
    name="ProductRead"
)
```

Uso:

```python
product = await Product.get(id=1)

return await ProductRead.from_tortoise_orm(product)
```

Para listas:

```python
products = Product.filter(is_active=True)

return await ProductRead.from_queryset(products)
```

## Uso con variables de entorno

```python
import os

database_url = os.getenv("DATABASE_URL", "sqlite://app.db")
```

Configuración:

```python
await Tortoise.init(
    db_url=database_url,
    modules={
        "models": ["app.models"]
    }
)
```

## Configuración con diccionario

También puede configurarse con un diccionario.

```python
TORTOISE_ORM = {
    "connections": {
        "default": "sqlite://app.db"
    },
    "apps": {
        "models": {
            "models": ["app.models"],
            "default_connection": "default",
        }
    },
}
```

Inicialización:

```python
await Tortoise.init(config=TORTOISE_ORM)
```

## Migraciones

Tortoise ORM cuenta con sistema de migraciones propio en versiones recientes.

Flujo conceptual:

```text
definir modelos
configurar migraciones
crear migración
aplicar migración
versionar archivos de migración
```

Comandos frecuentes:

```bash
tortoise init
tortoise migrate
tortoise upgrade
```

La configuración de migraciones debe apuntar al módulo de modelos y a la carpeta donde se guardarán los archivos de migración.

Ejemplo conceptual:

```python
TORTOISE_ORM = {
    "connections": {
        "default": "sqlite://app.db",
    },
    "apps": {
        "models": {
            "models": ["app.models"],
            "default_connection": "default",
            "migrations": "app.migrations",
        },
    },
}
```

## Relación con Aerich

Aerich fue una herramienta común de migraciones para Tortoise ORM.

En documentación nueva, conviene priorizar el sistema de migraciones propio de Tortoise ORM.

Aerich puede aparecer en proyectos existentes o material antiguo.

## `generate_schemas()` vs migraciones

## `generate_schemas()`

Útil para:

```text
aprendizaje
prototipos
tests simples
scripts pequeños
```

## Migraciones

Útiles para:

```text
proyectos reales
bases persistentes
producción
trabajo en equipo
control histórico del esquema
```

En proyectos reales, no conviene depender de `generate_schemas()` como mecanismo principal de evolución del esquema.

## Uso con PostgreSQL

Instalación:

```bash
python -m pip install "tortoise-orm[asyncpg]"
```

Configuración:

```python
await Tortoise.init(
    db_url="postgres://user:password@localhost:5432/app",
    modules={
        "models": ["app.models"]
    }
)
```

## Uso con SQLite

```python
await Tortoise.init(
    db_url="sqlite://app.db",
    modules={
        "models": ["app.models"]
    }
)
```

## Uso con MySQL o MariaDB

```python
await Tortoise.init(
    db_url="mysql://user:password@localhost:3306/app",
    modules={
        "models": ["app.models"]
    }
)
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

from tortoise import Tortoise

TORTOISE_ORM = {
    "connections": {
        "default": os.getenv("DATABASE_URL", "sqlite://app.db")
    },
    "apps": {
        "models": {
            "models": ["app.models"],
            "default_connection": "default",
            "migrations": "app.migrations",
        }
    },
}


async def init_database():
    await Tortoise.init(config=TORTOISE_ORM)


async def close_database():
    await Tortoise.close_connections()
```

## `models.py`

```python
from tortoise import fields
from tortoise.models import Model


class Product(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100, unique=True)
    price = fields.DecimalField(max_digits=10, decimal_places=2)
    stock = fields.IntField(default=0)
    is_active = fields.BooleanField(default=True)

    def __str__(self):
        return self.name
```

## `schemas.py`

```python
from tortoise.contrib.pydantic import pydantic_model_creator

from .models import Product

ProductRead = pydantic_model_creator(
    Product,
    name="ProductRead"
)

ProductCreate = pydantic_model_creator(
    Product,
    name="ProductCreate",
    exclude_readonly=True
)
```

## `repositories.py`

```python
from .models import Product


async def list_products():
    return await Product.filter(
        is_active=True
    ).order_by("name")


async def get_product(product_id: int):
    return await Product.get_or_none(
        id=product_id,
        is_active=True
    )


async def create_product(name, price, stock=0):
    return await Product.create(
        name=name,
        price=price,
        stock=stock
    )


async def update_stock(product: Product, stock: int):
    product.stock = stock

    await product.save()

    return product


async def deactivate_product(product: Product):
    product.is_active = False

    await product.save()

    return product
```

## Separación por responsabilidades

```text
database.py      -> configuración e inicialización de Tortoise
models.py        -> modelos ORM
schemas.py       -> modelos Pydantic
repositories.py  -> consultas y operaciones de base de datos
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Errores comunes

## Olvidar `await`

Problemático:

```python
products = Product.all()
```

Correcto:

```python
products = await Product.all()
```

## No inicializar Tortoise

Definir modelos no basta.

Debe ejecutarse:

```python
await Tortoise.init(...)
```

antes de usar consultas.

## No cerrar conexiones

Al finalizar una aplicación o script:

```python
await Tortoise.close_connections()
```

## Usar `generate_schemas()` como migración de producción

`generate_schemas()` puede servir para empezar, pero no debe reemplazar migraciones en proyectos reales.

## Configurar mal el módulo de modelos

Problemático:

```python
modules={
    "models": ["models"]
}
```

si los modelos están realmente en:

```text
app.models
```

Correcto:

```python
modules={
    "models": ["app.models"]
}
```

## Usar `get()` cuando el registro puede no existir

`get()` genera excepción si no encuentra el registro.

Más seguro:

```python
await Product.get_or_none(id=product_id)
```

## Devolver objetos ORM directamente sin serializar

En APIs, conviene devolver diccionarios o modelos Pydantic.

```python
await ProductRead.from_tortoise_orm(product)
```

o:

```python
await Product.filter(...).values(...)
```

## No usar transacciones en operaciones relacionadas

Si varias operaciones deben completarse juntas:

```python
async with in_transaction():
    ...
```

## Mezclar lógica HTTP con consultas

Conviene separar endpoints, servicios y repositorios.

## Buenas prácticas

## Usar `async` y `await` de forma consistente

```python
product = await Product.create(...)
```

## Inicializar la base al iniciar la aplicación

```python
await Tortoise.init(...)
```

## Cerrar conexiones al finalizar

```python
await Tortoise.close_connections()
```

## Separar modelos, repositorios y servicios

```text
models.py
repositories.py
services.py
```

## Usar `get_or_none()` cuando el registro puede faltar

```python
product = await Product.get_or_none(id=product_id)
```

## Usar migraciones en proyectos reales

```bash
tortoise migrate
tortoise upgrade
```

## Usar `values()` o Pydantic para respuestas API

```python
await Product.all().values("id", "name")
```

## Usar variables de entorno

```text
DATABASE_URL
```

## Usar transacciones para operaciones relacionadas

```python
async with in_transaction():
    ...
```

## Ejemplo integrado

```python
import asyncio
import os
from decimal import Decimal

from dotenv import load_dotenv
from tortoise import Tortoise, fields
from tortoise.exceptions import IntegrityError
from tortoise.models import Model

load_dotenv()


class Product(Model):
    id = fields.IntField(pk=True)
    name = fields.CharField(max_length=100, unique=True)
    price = fields.DecimalField(max_digits=10, decimal_places=2)
    stock = fields.IntField(default=0)
    is_active = fields.BooleanField(default=True)

    def __str__(self):
        return self.name


async def init_database():
    await Tortoise.init(
        db_url=os.getenv("DATABASE_URL", "sqlite://app.db"),
        modules={
            "models": ["__main__"]
        }
    )

    await Tortoise.generate_schemas()


async def create_product(name, price, stock=0):
    try:
        return await Product.create(
            name=name,
            price=Decimal(str(price)),
            stock=stock
        )
    except IntegrityError:
        raise ValueError("El producto ya existe")


async def list_products():
    return await Product.filter(
        is_active=True
    ).order_by("name")


async def get_product(product_id):
    return await Product.get_or_none(
        id=product_id,
        is_active=True
    )


async def update_stock(product_id, stock):
    product = await get_product(product_id)

    if product is None:
        return None

    product.stock = stock

    await product.save()

    return product


async def deactivate_product(product_id):
    product = await get_product(product_id)

    if product is None:
        return None

    product.is_active = False

    await product.save()

    return product


async def main():
    await init_database()

    try:
        try:
            await create_product("Laptop", 3500, stock=5)
            await create_product("Mouse", 80, stock=20)
        except ValueError:
            pass

        products = await list_products()

        for product in products:
            print(
                product.id,
                product.name,
                product.price,
                product.stock
            )

    finally:
        await Tortoise.close_connections()


asyncio.run(main())
```

## Relación con otras librerías

`tortoise-orm` se relaciona especialmente con:

```text
asyncio
fastapi
starlette
pydantic
asyncpg
aiosqlite
asyncmy
python-dotenv
pytest
pytest-asyncio
```

## Relación con FastAPI

Tortoise ORM encaja con endpoints `async`.

```python
@app.get("/products")
async def read_products():
    ...
```

## Relación con Pydantic

Puede generar modelos Pydantic desde modelos ORM.

```python
pydantic_model_creator(Product)
```

## Relación con asyncpg

Para PostgreSQL, Tortoise ORM puede usar `asyncpg` como driver asíncrono.

## Relación con SQLite

Para SQLite, Tortoise ORM puede usar conexiones asíncronas mediante `aiosqlite`.

## Relación con Django ORM

Tortoise ORM está inspirado en Django ORM, pero no es el ORM de Django.

Su diferencia central es que está diseñado para uso asíncrono independiente.

## Relación con SQLAlchemy

SQLAlchemy es más amplio y flexible para casos avanzados.

Tortoise ORM puede ser más directo si se quiere una API asíncrona familiar y menos extensa.

## Orden didáctico interno

```text
1. Propósito de tortoise-orm
2. Instalación
3. Relación con asyncio
4. Modelos con Model
5. Campos con fields
6. Tortoise.init()
7. generate_schemas()
8. Crear registros
9. Consultar con all(), filter(), get() y get_or_none()
10. Actualizar y eliminar registros
11. Relaciones
12. values() y values_list()
13. Transacciones
14. Uso con FastAPI
15. Uso con Pydantic
16. Migraciones
17. Organización recomendada
18. Errores comunes
19. Buenas prácticas
```