# `peewee`

## Propósito

`peewee` es una librería externa para trabajar con bases de datos SQL mediante un ORM ligero.

Permite definir tablas como clases de Python, representar filas como objetos, ejecutar consultas usando una sintaxis orientada a objetos y trabajar con bases de datos relacionales sin escribir SQL manual en cada operación.

Se utiliza especialmente en proyectos pequeños o medianos donde se necesita un ORM más simple que SQLAlchemy o Django ORM.

## Naturaleza de la librería

`peewee` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install peewee
```

La importación habitual es:

```python
from peewee import Model
from peewee import CharField
from peewee import IntegerField
from peewee import FloatField
from peewee import BooleanField
from peewee import SqliteDatabase
```

## Idea central

La idea principal de `peewee` es definir modelos como clases.

Ejemplo:

```python
from peewee import CharField, FloatField, IntegerField, Model, SqliteDatabase

database = SqliteDatabase("app.db")


class Product(Model):
    name = CharField()
    price = FloatField()
    stock = IntegerField(default=0)

    class Meta:
        database = database
```

La clase `Product` representa una tabla.

Cada instancia de `Product` representa una fila.

Cada atributo de campo representa una columna.

## Cuándo usar Peewee

Conviene usar `peewee` cuando se necesita:

```text
un ORM ligero
modelos simples
consultas SQL expresadas con Python
trabajar con SQLite
trabajar con PostgreSQL
trabajar con MySQL o MariaDB
crear aplicaciones pequeñas o medianas
evitar la complejidad de SQLAlchemy en casos simples
usar un ORM fuera de Django
```

## Cuándo no usar Peewee

No siempre conviene usar `peewee` cuando se necesita:

```text
control avanzado de SQLAlchemy
modelos relacionales muy complejos
ecosistema completo de Django
migraciones sofisticadas desde el inicio
arquitecturas empresariales grandes
soporte muy amplio de dialectos
bases no relacionales como MongoDB
caché como Redis
```

Para proyectos complejos, `SQLAlchemy` puede ser más flexible.

Para aplicaciones Django, normalmente se usa el ORM de Django.

Para modelos integrados con FastAPI y Pydantic, puede evaluarse `sqlmodel`.

## Instalación

Instalación básica:

```bash
python -m pip install peewee
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
peewee==3.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import peewee

print(peewee.__version__)
```

## Conexión con SQLite

```python
from peewee import SqliteDatabase

database = SqliteDatabase("app.db")
```

Base en memoria:

```python
database = SqliteDatabase(":memory:")
```

SQLite es la opción más simple para empezar con Peewee.

## Conexión con PostgreSQL

Para PostgreSQL se necesita un driver compatible, como `psycopg`.

Instalación conceptual:

```bash
python -m pip install "psycopg[binary]"
```

Conexión:

```python
from peewee import PostgresqlDatabase

database = PostgresqlDatabase(
    "app",
    user="postgres",
    password="secret",
    host="localhost",
    port=5432
)
```

## Conexión con MySQL o MariaDB

Para MySQL o MariaDB suele usarse `pymysql`.

Instalación:

```bash
python -m pip install pymysql
```

Conexión:

```python
from peewee import MySQLDatabase

database = MySQLDatabase(
    "app",
    user="root",
    password="secret",
    host="localhost",
    port=3306
)
```

## Modelo base

Es común definir un modelo base para no repetir la base de datos en cada modelo.

```python
from peewee import Model, SqliteDatabase

database = SqliteDatabase("app.db")


class BaseModel(Model):
    class Meta:
        database = database
```

Luego los modelos heredan de `BaseModel`.

```python
class Product(BaseModel):
    ...
```

## Modelo básico

```python
from peewee import BooleanField, CharField, FloatField, IntegerField


class Product(BaseModel):
    name = CharField(unique=True)
    price = FloatField()
    stock = IntegerField(default=0)
    is_active = BooleanField(default=True)
```

## Campos frecuentes

```python
from peewee import AutoField
from peewee import BooleanField
from peewee import CharField
from peewee import DateField
from peewee import DateTimeField
from peewee import DecimalField
from peewee import FloatField
from peewee import ForeignKeyField
from peewee import IntegerField
from peewee import TextField
```

Ejemplos:

```python
id = AutoField()
name = CharField(max_length=100)
description = TextField(null=True)
price = DecimalField(max_digits=10, decimal_places=2)
stock = IntegerField(default=0)
is_active = BooleanField(default=True)
```

## Clave primaria

Peewee puede crear una clave primaria automáticamente si no se define una.

También puede declararse explícitamente:

```python
from peewee import AutoField


class Product(BaseModel):
    id = AutoField()
    name = CharField()
```

## Campos opcionales

```python
description = TextField(null=True)
```

`null=True` permite guardar `NULL` en la base de datos.

## Valores por defecto

```python
stock = IntegerField(default=0)
is_active = BooleanField(default=True)
```

## Crear tablas

```python
database.connect()
database.create_tables([Product])
database.close()
```

Patrón con context manager:

```python
with database:
    database.create_tables([Product])
```

## Crear varias tablas

```python
with database:
    database.create_tables([
        Product,
        Category
    ])
```

## Insertar registros

## `create()`

```python
product = Product.create(
    name="Laptop",
    price=3500,
    stock=5
)

print(product.id)
```

`create()` inserta el registro y devuelve la instancia creada.

## Crear instancia y guardar

```python
product = Product(
    name="Mouse",
    price=80,
    stock=20
)

product.save()
```

## Consultar registros

## Seleccionar todos

```python
products = Product.select()

for product in products:
    print(product.name, product.price)
```

## Filtrar registros

```python
products = Product.select().where(
    Product.price >= 100
)

for product in products:
    print(product.name)
```

## Obtener un registro

```python
product = Product.get(Product.name == "Laptop")

print(product.price)
```

Si no existe, se genera una excepción.

## `get_or_none()`

```python
product = Product.get_or_none(
    Product.name == "Laptop"
)

if product is not None:
    print(product.price)
```

Este patrón evita manejar una excepción cuando el registro puede no existir.

## Consultar por id

```python
product = Product.get_by_id(1)
```

Alternativa segura:

```python
product = Product.get_or_none(Product.id == 1)
```

## Ordenar resultados

```python
products = Product.select().order_by(Product.name)
```

Orden descendente:

```python
products = Product.select().order_by(Product.price.desc())
```

## Limitar resultados

```python
products = Product.select().limit(10)
```

Con desplazamiento:

```python
products = Product.select().offset(10).limit(10)
```

## Contar registros

```python
count = Product.select().where(
    Product.is_active == True
).count()

print(count)
```

## Actualizar registros

## Actualizar una instancia

```python
product = Product.get(Product.name == "Laptop")

product.price = 3600
product.stock = 4

product.save()
```

## Actualización masiva

```python
query = (
    Product
    .update(stock=0)
    .where(Product.is_active == False)
)

rows_updated = query.execute()

print(rows_updated)
```

## Eliminar registros

## Eliminar una instancia

```python
product = Product.get(Product.name == "Laptop")

product.delete_instance()
```

## Eliminación masiva

```python
rows_deleted = (
    Product
    .delete()
    .where(Product.is_active == False)
    .execute()
)

print(rows_deleted)
```

## Eliminación lógica

En aplicaciones reales suele ser preferible marcar registros como inactivos.

```python
product = Product.get_by_id(1)

product.is_active = False
product.save()
```

Consulta de registros activos:

```python
products = Product.select().where(
    Product.is_active == True
)
```

## Relaciones

Peewee permite relaciones mediante `ForeignKeyField`.

Ejemplo conceptual:

```text
Category -> Product
una categoría tiene muchos productos
un producto pertenece a una categoría
```

## ForeignKeyField

```python
from peewee import ForeignKeyField


class Category(BaseModel):
    name = CharField(unique=True)


class Product(BaseModel):
    name = CharField()
    price = FloatField()
    category = ForeignKeyField(
        Category,
        backref="products"
    )
```

## Crear objetos relacionados

```python
category = Category.create(name="Tecnología")

product = Product.create(
    name="Laptop",
    price=3500,
    category=category
)
```

## Acceder a la relación

```python
product = Product.get(Product.name == "Laptop")

print(product.category.name)
```

## Acceder desde el lado inverso

```python
category = Category.get(Category.name == "Tecnología")

for product in category.products:
    print(product.name)
```

`backref="products"` permite acceder a los productos desde la categoría.

## Joins

```python
query = (
    Product
    .select(Product, Category)
    .join(Category)
    .where(Category.name == "Tecnología")
)

for product in query:
    print(product.name, product.category.name)
```

## Consultas con varias condiciones

Condición `AND`:

```python
products = Product.select().where(
    (Product.price >= 100) &
    (Product.is_active == True)
)
```

Condición `OR`:

```python
products = Product.select().where(
    (Product.name == "Laptop") |
    (Product.name == "Mouse")
)
```

Los paréntesis son importantes por la precedencia de operadores.

## Búsqueda de texto

```python
products = Product.select().where(
    Product.name.contains("lap")
)
```

También pueden usarse otros métodos según el tipo de campo y base de datos.

## Agregaciones

Peewee expone funciones SQL mediante `fn`.

```python
from peewee import fn

count = Product.select(fn.COUNT(Product.id)).scalar()

print(count)
```

## Suma

```python
total_stock = Product.select(
    fn.SUM(Product.stock)
).scalar()

print(total_stock)
```

## Agrupar

```python
query = (
    Product
    .select(Category.name, fn.COUNT(Product.id).alias("product_count"))
    .join(Category)
    .group_by(Category.name)
)

for row in query.dicts():
    print(row)
```

## `dicts()`

`dicts()` devuelve filas como diccionarios.

```python
query = Product.select(
    Product.id,
    Product.name,
    Product.price
).dicts()

for row in query:
    print(row["name"])
```

## `tuples()`

`tuples()` devuelve filas como tuplas.

```python
query = Product.select(
    Product.name,
    Product.price
).tuples()

for name, price in query:
    print(name, price)
```

## `namedtuples()`

```python
query = Product.select(
    Product.name,
    Product.price
).namedtuples()

for row in query:
    print(row.name, row.price)
```

## Transacciones

Peewee permite usar transacciones con `atomic()`.

```python
with database.atomic():
    Product.create(
        name="Laptop",
        price=3500,
        stock=5
    )

    Product.create(
        name="Mouse",
        price=80,
        stock=20
    )
```

Si ocurre un error dentro del bloque, la transacción se revierte.

## Rollback explícito

```python
try:
    with database.atomic():
        Product.create(name="Laptop", price=3500)
        Product.create(name="Laptop", price=3600)
except Exception:
    print("No se pudo completar la transacción")
```

Si `name` es único, el segundo registro puede fallar y se revierte el bloque.

## Conexión y cierre

## Conectar

```python
database.connect()
```

## Cerrar

```python
database.close()
```

## Verificar si está cerrada

```python
if database.is_closed():
    database.connect()
```

## Context manager

```python
with database:
    database.create_tables([Product])
```

## `connection_context()`

```python
with database.connection_context():
    products = Product.select()

    for product in products:
        print(product.name)
```

Este patrón abre y cierra la conexión alrededor del bloque.

## Uso con SQLite

```python
from peewee import SqliteDatabase

database = SqliteDatabase(
    "app.db",
    pragmas={
        "journal_mode": "wal",
        "foreign_keys": 1
    }
)
```

`foreign_keys=1` permite activar restricciones de claves foráneas en SQLite.

## Uso con PostgreSQL

```python
from peewee import PostgresqlDatabase

database = PostgresqlDatabase(
    "app",
    user="postgres",
    password="secret",
    host="localhost",
    port=5432
)
```

## Uso con MySQL

```python
from peewee import MySQLDatabase

database = MySQLDatabase(
    "app",
    user="root",
    password="secret",
    host="localhost",
    port=3306
)
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

```python
import os

from peewee import PostgresqlDatabase

database = PostgresqlDatabase(
    os.getenv("DB_NAME"),
    user=os.getenv("DB_USER"),
    password=os.getenv("DB_PASSWORD"),
    host=os.getenv("DB_HOST"),
    port=int(os.getenv("DB_PORT", "5432"))
)
```

## Uso con `python-dotenv`

```python
import os

from dotenv import load_dotenv
from peewee import PostgresqlDatabase

load_dotenv()

database = PostgresqlDatabase(
    os.getenv("DB_NAME"),
    user=os.getenv("DB_USER"),
    password=os.getenv("DB_PASSWORD"),
    host=os.getenv("DB_HOST", "localhost"),
    port=int(os.getenv("DB_PORT", "5432"))
)
```

## Migraciones

Peewee incluye herramientas de migración en `playhouse.migrate`.

Ejemplo conceptual:

```python
from playhouse.migrate import SqliteMigrator, migrate

migrator = SqliteMigrator(database)

migrate(
    migrator.add_column(
        "product",
        "description",
        TextField(null=True)
    )
)
```

Para proyectos muy simples puede bastar con scripts controlados.

Para proyectos más grandes, conviene evaluar cuidadosamente la estrategia de migraciones.

## Extensiones `playhouse`

Peewee incluye extensiones bajo `playhouse`.

Algunas áreas comunes:

```text
migraciones
campos extendidos
integración con Flask
atajos de consulta
herramientas para SQLite
```

No es necesario aprender todo `playhouse` al inicio.

Primero conviene dominar:

```text
Model
fields
select()
where()
create()
save()
delete_instance()
ForeignKeyField
atomic()
```

## Uso con FastAPI

Ejemplo conceptual:

```python
from fastapi import FastAPI, HTTPException
from peewee import BooleanField, CharField, FloatField, IntegerField, Model, SqliteDatabase

database = SqliteDatabase("app.db")


class BaseModel(Model):
    class Meta:
        database = database


class Product(BaseModel):
    name = CharField(unique=True)
    price = FloatField()
    stock = IntegerField(default=0)
    is_active = BooleanField(default=True)


app = FastAPI()


@app.on_event("startup")
def startup():
    database.connect()
    database.create_tables([Product])


@app.on_event("shutdown")
def shutdown():
    if not database.is_closed():
        database.close()


@app.get("/products")
def read_products():
    products = Product.select().where(Product.is_active == True)

    return [
        {
            "id": product.id,
            "name": product.name,
            "price": product.price,
            "stock": product.stock
        }
        for product in products
    ]


@app.get("/products/{product_id}")
def read_product(product_id: int):
    product = Product.get_or_none(Product.id == product_id)

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

## Uso con Flask

```python
from flask import Flask
from peewee import BooleanField, CharField, FloatField, IntegerField, Model, SqliteDatabase

database = SqliteDatabase("app.db")


class BaseModel(Model):
    class Meta:
        database = database


class Product(BaseModel):
    name = CharField(unique=True)
    price = FloatField()
    stock = IntegerField(default=0)
    is_active = BooleanField(default=True)


app = Flask(__name__)


@app.before_request
def before_request():
    if database.is_closed():
        database.connect()


@app.teardown_request
def teardown_request(exception):
    if not database.is_closed():
        database.close()


@app.route("/products")
def read_products():
    products = Product.select().where(Product.is_active == True)

    return [
        {
            "id": product.id,
            "name": product.name,
            "price": product.price,
            "stock": product.stock
        }
        for product in products
    ]
```

## Uso con pandas

Peewee no reemplaza a pandas, pero puede alimentar DataFrames.

```python
import pandas as pd

query = Product.select(
    Product.id,
    Product.name,
    Product.price,
    Product.stock
).dicts()

df = pd.DataFrame(list(query))

print(df.head())
```

También puede usarse SQL directo desde la conexión, pero para análisis intensivo puede convenir `pandas`, `duckdb` o SQLAlchemy según el caso.

## Organización recomendada

```text
app/
├─ database.py
├─ models.py
├─ repositories.py
├─ services.py
└─ main.py
```

## `database.py`

```python
from peewee import SqliteDatabase

database = SqliteDatabase(
    "app.db",
    pragmas={
        "foreign_keys": 1
    }
)
```

## `models.py`

```python
from peewee import BooleanField, CharField, FloatField, IntegerField, Model

from .database import database


class BaseModel(Model):
    class Meta:
        database = database


class Product(BaseModel):
    name = CharField(unique=True)
    price = FloatField()
    stock = IntegerField(default=0)
    is_active = BooleanField(default=True)
```

## `repositories.py`

```python
from .models import Product


def list_products():
    return list(
        Product
        .select()
        .where(Product.is_active == True)
        .order_by(Product.name)
    )


def get_product(product_id):
    return Product.get_or_none(Product.id == product_id)


def create_product(name, price, stock=0):
    return Product.create(
        name=name,
        price=price,
        stock=stock
    )


def update_stock(product, stock):
    product.stock = stock
    product.save()

    return product


def deactivate_product(product):
    product.is_active = False
    product.save()

    return product
```

## Separación por responsabilidades

```text
database.py      -> conexión a base de datos
models.py        -> modelos ORM
repositories.py  -> consultas y operaciones CRUD
services.py      -> reglas de negocio
main.py          -> punto de entrada o aplicación
```

## Errores comunes

## No asociar el modelo con una base de datos

Problemático:

```python
class Product(Model):
    name = CharField()
```

sin `Meta.database`.

Correcto:

```python
class Product(Model):
    name = CharField()

    class Meta:
        database = database
```

## Olvidar crear las tablas

Definir modelos no crea automáticamente las tablas.

Debe ejecutarse:

```python
database.create_tables([Product])
```

## No cerrar conexiones

En scripts pequeños puede no notarse, pero en aplicaciones conviene controlar apertura y cierre.

```python
database.close()
```

## Usar `get()` cuando el registro puede no existir

`get()` lanza excepción si no encuentra datos.

Más seguro:

```python
Product.get_or_none(Product.id == product_id)
```

## Olvidar paréntesis en condiciones compuestas

Problemático:

```python
Product.select().where(Product.price > 100 & Product.is_active == True)
```

Correcto:

```python
Product.select().where(
    (Product.price > 100) &
    (Product.is_active == True)
)
```

## Usar Peewee esperando toda la flexibilidad de SQLAlchemy

Peewee es simple y expresivo, pero SQLAlchemy ofrece mayor amplitud para casos avanzados.

## No usar transacciones en operaciones relacionadas

Si varias operaciones deben completarse juntas, conviene usar:

```python
with database.atomic():
    ...
```

## Guardar credenciales en el código

Problemático:

```python
password="secret"
```

Mejor:

```python
password=os.getenv("DB_PASSWORD")
```

## No activar foreign keys en SQLite

Cuando se usan relaciones en SQLite, conviene configurar:

```python
pragmas={"foreign_keys": 1}
```

## Buenas prácticas

## Definir un `BaseModel`

```python
class BaseModel(Model):
    class Meta:
        database = database
```

## Usar nombres claros de modelos y campos

```python
Product
Category
name
price
stock
is_active
```

## Usar `get_or_none()` cuando el registro puede faltar

```python
Product.get_or_none(Product.id == product_id)
```

## Usar `atomic()` para transacciones

```python
with database.atomic():
    ...
```

## Separar modelos de consultas

```text
models.py
repositories.py
```

## Usar variables de entorno

```text
DB_NAME
DB_USER
DB_PASSWORD
DB_HOST
DB_PORT
```

## Activar foreign keys en SQLite cuando corresponda

```python
SqliteDatabase("app.db", pragmas={"foreign_keys": 1})
```

## Usar migraciones con criterio

Para cambios de esquema, evaluar `playhouse.migrate` o una estrategia controlada.

## Ejemplo integrado

```python
import os
from decimal import Decimal

from dotenv import load_dotenv
from peewee import BooleanField
from peewee import CharField
from peewee import DecimalField
from peewee import IntegerField
from peewee import Model
from peewee import SqliteDatabase
from peewee import IntegrityError

load_dotenv()

database_path = os.getenv("DATABASE_PATH", "app.db")

database = SqliteDatabase(
    database_path,
    pragmas={
        "foreign_keys": 1
    }
)


class BaseModel(Model):
    class Meta:
        database = database


class Product(BaseModel):
    name = CharField(unique=True)
    price = DecimalField(max_digits=10, decimal_places=2)
    stock = IntegerField(default=0)
    is_active = BooleanField(default=True)


def create_tables():
    with database:
        database.create_tables([Product])


def create_product(name, price, stock=0):
    try:
        return Product.create(
            name=name,
            price=Decimal(str(price)),
            stock=stock
        )
    except IntegrityError:
        raise ValueError("El producto ya existe")


def list_products():
    return list(
        Product
        .select()
        .where(Product.is_active == True)
        .order_by(Product.name)
    )


def get_product(product_id):
    return Product.get_or_none(Product.id == product_id)


def update_stock(product_id, stock):
    product = get_product(product_id)

    if product is None:
        return None

    product.stock = stock
    product.save()

    return product


def deactivate_product(product_id):
    product = get_product(product_id)

    if product is None:
        return None

    product.is_active = False
    product.save()

    return product


create_tables()

with database.connection_context():
    try:
        create_product("Laptop", 3500, stock=5)
        create_product("Mouse", 80, stock=20)
    except ValueError:
        pass

    products = list_products()

    for product in products:
        print(
            product.id,
            product.name,
            product.price,
            product.stock
        )
```

## Relación con otras librerías

`peewee` se relaciona especialmente con:

```text
sqlite
postgresql
mysql
mariadb
psycopg
pymysql
fastapi
flask
pandas
python-dotenv
pytest
```

## Relación con SQLite

Peewee funciona muy bien con SQLite para aplicaciones pequeñas, prototipos y herramientas locales.

## Relación con PostgreSQL

Peewee puede conectarse a PostgreSQL mediante drivers como `psycopg`.

## Relación con MySQL y MariaDB

Peewee puede conectarse a MySQL o MariaDB mediante drivers como `pymysql`.

## Relación con FastAPI y Flask

Peewee puede usarse como capa ORM en aplicaciones web ligeras.

En aplicaciones medianas o grandes conviene separar:

```text
database
models
repositories
services
routes
```

## Relación con SQLAlchemy

Peewee y SQLAlchemy son ORMs, pero SQLAlchemy es más amplio y flexible.

Peewee es más pequeño y directo.

## Orden didáctico interno

```text
1. Propósito de peewee
2. Instalación
3. Conexión con SQLite, PostgreSQL y MySQL
4. Modelo base
5. Modelos y campos
6. Crear tablas
7. Insertar registros
8. Consultar con select(), where() y get_or_none()
9. Actualizar y eliminar registros
10. Relaciones con ForeignKeyField
11. Joins
12. Agregaciones con fn
13. Transacciones con atomic()
14. Manejo de conexiones
15. Migraciones con playhouse.migrate
16. Uso con FastAPI, Flask y pandas
17. Organización recomendada
18. Errores comunes
19. Buenas prácticas
```