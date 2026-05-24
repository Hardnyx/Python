# `sqlalchemy`

## Propósito

`sqlalchemy` es una librería externa para trabajar con bases de datos SQL desde Python. Se utiliza para conectarse a bases de datos relacionales, ejecutar consultas, definir tablas, mapear clases de Python a tablas, manejar sesiones, controlar transacciones y construir aplicaciones que necesitan persistencia de datos.

Puede usarse de dos formas principales:

```text
SQLAlchemy Core -> construcción explícita de SQL con objetos Python
SQLAlchemy ORM  -> mapeo de clases Python a tablas de base de datos
```

## Naturaleza de la librería

`sqlalchemy` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install sqlalchemy
```

La importación depende del componente usado.

Importaciones frecuentes:

```python
from sqlalchemy import create_engine
from sqlalchemy import select
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import Session
from sqlalchemy.orm import mapped_column
```

Para bases de datos distintas de SQLite, normalmente también debe instalarse el driver correspondiente.

Ejemplos:

```bash
python -m pip install psycopg
python -m pip install pymysql
```

## Idea central

La idea principal de SQLAlchemy es servir como capa de comunicación entre Python y una base de datos relacional.

Flujo típico con ORM:

```text
clase Python -> modelo ORM -> tabla SQL
objeto Python -> fila de tabla
atributo -> columna
Session -> unidad de trabajo y transacción
```

Ejemplo conceptual:

```python
product = Product(name="Laptop", price=3500)
session.add(product)
session.commit()
```

Ese objeto Python se convierte en un registro persistido en la base de datos.

## Cuándo usar SQLAlchemy

Conviene usar SQLAlchemy cuando se necesita:

- conectarse a bases de datos SQL
- ejecutar consultas desde Python
- modelar tablas como clases
- manejar relaciones entre tablas
- controlar transacciones
- construir aplicaciones con persistencia
- separar lógica de base de datos del resto del código
- trabajar con SQLite, PostgreSQL, MySQL, MariaDB, SQL Server u otras bases soportadas
- integrar bases de datos con FastAPI, Flask, scripts o procesos de datos

## Cuándo no usar SQLAlchemy

No siempre conviene usar SQLAlchemy si solo se necesita:

- leer un CSV
- transformar datos tabulares en memoria
- trabajar con archivos Excel
- usar una base NoSQL
- ejecutar una consulta SQL puntual sin capa adicional
- hacer análisis exploratorio rápido

Para datos tabulares en archivos, suele bastar con:

```text
pandas
polars
```

Para consultas muy simples sobre SQLite, puede bastar con `sqlite3`.

Para proyectos Django, normalmente se usa el ORM propio de Django.

## Instalación

Instalación básica:

```bash
python -m pip install sqlalchemy
```

Verificación:

```python
import sqlalchemy

print(sqlalchemy.__version__)
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
SQLAlchemy==2.x.x
```

La versión exacta puede variar según el entorno.

## Drivers de base de datos

SQLAlchemy no siempre se conecta solo a la base de datos. En muchos casos necesita un driver.

## SQLite

SQLite usa soporte incluido en Python mediante `sqlite3`.

Normalmente no requiere instalación adicional.

```python
engine = create_engine("sqlite:///app.db")
```

## PostgreSQL

Driver frecuente:

```bash
python -m pip install psycopg
```

URL de conexión conceptual:

```python
engine = create_engine(
    "postgresql+psycopg://user:password@localhost:5432/database"
)
```

## MySQL o MariaDB

Driver frecuente:

```bash
python -m pip install pymysql
```

URL de conexión conceptual:

```python
engine = create_engine(
    "mysql+pymysql://user:password@localhost:3306/database"
)
```

## SQL Server

Driver frecuente:

```bash
python -m pip install pyodbc
```

URL de conexión conceptual:

```python
engine = create_engine(
    "mssql+pyodbc://user:password@server/database?driver=ODBC+Driver+18+for+SQL+Server"
)
```

## Engine

## Propósito

El `Engine` representa el punto principal de conexión entre SQLAlchemy y la base de datos.

Se crea con `create_engine()`.

```python
from sqlalchemy import create_engine

engine = create_engine("sqlite:///app.db")
```

El engine administra elementos como:

```text
dialecto SQL
driver de conexión
pool de conexiones
comunicación con la base de datos
```

## Crear engine para SQLite en archivo

```python
from sqlalchemy import create_engine

engine = create_engine("sqlite:///app.db")
```

Esto crea o usa un archivo llamado:

```text
app.db
```

## Crear engine para SQLite en memoria

```python
from sqlalchemy import create_engine

engine = create_engine("sqlite:///:memory:")
```

Este tipo de base existe solo durante la ejecución del programa.

## Mostrar SQL generado

```python
engine = create_engine(
    "sqlite:///app.db",
    echo=True
)
```

`echo=True` muestra las sentencias SQL ejecutadas. Es útil para aprendizaje y depuración.

## SQLAlchemy Core vs ORM

## SQLAlchemy Core

Core permite definir tablas y construir consultas SQL usando objetos Python.

Ejemplo conceptual:

```python
from sqlalchemy import Table, Column, Integer, String, MetaData

metadata = MetaData()

products = Table(
    "products",
    metadata,
    Column("id", Integer, primary_key=True),
    Column("name", String),
)
```

## SQLAlchemy ORM

ORM permite definir clases Python que representan tablas.

Ejemplo conceptual:

```python
class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
```

## Regla práctica

Para aplicaciones con objetos de dominio, relaciones y lógica de negocio, suele usarse el ORM.

Para construcción explícita de consultas, migraciones manuales o capas de datos más cercanas a SQL, Core puede ser más directo.

En la práctica, ambos enfoques pueden convivir.

## Declarative Base

La base declarativa permite definir modelos ORM como clases.

```python
from sqlalchemy.orm import DeclarativeBase


class Base(DeclarativeBase):
    pass
```

Todas las clases ORM heredan de `Base`.

```python
class Product(Base):
    ...
```

## Modelo básico

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column


class Base(DeclarativeBase):
    pass


class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    price: Mapped[float]
    stock: Mapped[int] = mapped_column(default=0)
```

## `__tablename__`

Define el nombre de la tabla en la base de datos.

```python
__tablename__ = "products"
```

## `Mapped`

`Mapped` se usa como anotación de tipo para atributos persistidos por el ORM.

```python
name: Mapped[str]
price: Mapped[float]
```

## `mapped_column()`

`mapped_column()` permite configurar detalles de una columna.

```python
id: Mapped[int] = mapped_column(primary_key=True)
```

También puede indicar longitud, nulabilidad, valores únicos o valores por defecto.

```python
name: Mapped[str] = mapped_column(String(100), unique=True)
```

## Tipos frecuentes

Importaciones:

```python
from sqlalchemy import Boolean
from sqlalchemy import Date
from sqlalchemy import DateTime
from sqlalchemy import Float
from sqlalchemy import ForeignKey
from sqlalchemy import Integer
from sqlalchemy import Numeric
from sqlalchemy import String
from sqlalchemy import Text
```

Ejemplos:

```python
name: Mapped[str] = mapped_column(String(100))
description: Mapped[str | None] = mapped_column(Text, nullable=True)
price: Mapped[float] = mapped_column(Float)
is_active: Mapped[bool] = mapped_column(Boolean, default=True)
```

## Clave primaria

Toda tabla ORM normalmente necesita una clave primaria.

```python
id: Mapped[int] = mapped_column(primary_key=True)
```

La clave primaria identifica de forma única cada registro.

## Columnas obligatorias y opcionales

Columna obligatoria:

```python
name: Mapped[str]
```

Columna opcional:

```python
description: Mapped[str | None]
```

También puede indicarse explícitamente:

```python
description: Mapped[str | None] = mapped_column(nullable=True)
```

## Crear tablas

Después de definir los modelos, se pueden crear las tablas con:

```python
Base.metadata.create_all(engine)
```

Ejemplo completo:

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column


class Base(DeclarativeBase):
    pass


class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    price: Mapped[float]
    stock: Mapped[int] = mapped_column(default=0)


engine = create_engine("sqlite:///app.db", echo=True)

Base.metadata.create_all(engine)
```

## Importante sobre `create_all()`

`create_all()` crea tablas que no existen.

No es un sistema completo de migraciones.

Para proyectos reales donde el esquema cambia en el tiempo, suele usarse:

```text
alembic
```

## Session

## Propósito

`Session` representa una unidad de trabajo con la base de datos.

Permite:

- agregar objetos
- consultar objetos
- modificar objetos
- eliminar objetos
- confirmar cambios con `commit()`
- revertir cambios con `rollback()`
- manejar transacciones

Ejemplo:

```python
from sqlalchemy.orm import Session

with Session(engine) as session:
    ...
```

## Crear registro

```python
from sqlalchemy.orm import Session

product = Product(
    name="Laptop",
    price=3500,
    stock=5
)

with Session(engine) as session:
    session.add(product)
    session.commit()
```

## Crear varios registros

```python
products = [
    Product(name="Laptop", price=3500, stock=5),
    Product(name="Mouse", price=80, stock=20),
]

with Session(engine) as session:
    session.add_all(products)
    session.commit()
```

## `commit()`

`commit()` confirma la transacción.

```python
session.commit()
```

Sin `commit()`, los cambios pueden no quedar guardados.

## `rollback()`

`rollback()` revierte cambios pendientes si ocurre un error.

```python
try:
    session.add(product)
    session.commit()
except Exception:
    session.rollback()
    raise
```

## `flush()`

`flush()` envía cambios pendientes a la base de datos sin confirmar la transacción.

Puede ser útil cuando se necesita obtener un `id` generado antes del `commit()`.

```python
with Session(engine) as session:
    product = Product(name="Laptop", price=3500)
    session.add(product)
    session.flush()

    print(product.id)

    session.commit()
```

## Consultar registros

SQLAlchemy 2.x usa `select()` de forma central.

```python
from sqlalchemy import select
from sqlalchemy.orm import Session

with Session(engine) as session:
    statement = select(Product)

    products = session.scalars(statement).all()

    print(products)
```

## Consultar un registro por id

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    print(product)
```

`session.get()` devuelve `None` si no encuentra el registro.

## Filtrar registros

```python
from sqlalchemy import select

with Session(engine) as session:
    statement = select(Product).where(Product.price >= 100)

    products = session.scalars(statement).all()

    print(products)
```

## Obtener primer resultado

```python
with Session(engine) as session:
    statement = select(Product).where(Product.name == "Laptop")

    product = session.scalars(statement).first()

    print(product)
```

## Obtener un único resultado

```python
with Session(engine) as session:
    statement = select(Product).where(Product.name == "Laptop")

    product = session.scalars(statement).one()
```

`one()` espera exactamente un resultado.

Si hay cero o más de uno, genera error.

## `one_or_none()`

```python
product = session.scalars(statement).one_or_none()
```

Devuelve un objeto o `None`, pero genera error si hay más de un resultado.

## Ordenar resultados

```python
statement = select(Product).order_by(Product.name)
```

Orden descendente:

```python
statement = select(Product).order_by(Product.price.desc())
```

## Limitar resultados

```python
statement = select(Product).limit(10)
```

Con desplazamiento:

```python
statement = select(Product).offset(10).limit(10)
```

## Actualizar registros

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    if product is not None:
        product.price = 3600
        session.commit()
```

El ORM detecta cambios en el objeto y los envía a la base de datos al confirmar.

## Eliminar registros

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    if product is not None:
        session.delete(product)
        session.commit()
```

## Transacción con `session.begin()`

```python
from sqlalchemy.orm import Session

with Session(engine) as session:
    with session.begin():
        session.add(Product(name="Laptop", price=3500))
        session.add(Product(name="Mouse", price=80))
```

Si ocurre un error dentro del bloque, la transacción se revierte.

## Relaciones

SQLAlchemy ORM permite representar relaciones entre tablas.

## Uno a muchos

Ejemplo:

```text
Category -> Product
una categoría tiene muchos productos
un producto pertenece a una categoría
```

Código:

```python
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship


class Category(Base):
    __tablename__ = "categories"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]

    products: Mapped[list["Product"]] = relationship(
        back_populates="category"
    )


class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    price: Mapped[float]
    category_id: Mapped[int] = mapped_column(
        ForeignKey("categories.id")
    )

    category: Mapped["Category"] = relationship(
        back_populates="products"
    )
```

## `ForeignKey`

`ForeignKey` define la relación a nivel de columna.

```python
category_id: Mapped[int] = mapped_column(
    ForeignKey("categories.id")
)
```

## `relationship()`

`relationship()` define la relación a nivel de objetos Python.

```python
category: Mapped["Category"] = relationship(
    back_populates="products"
)
```

## `back_populates`

`back_populates` conecta ambos lados de una relación.

```python
products: Mapped[list["Product"]] = relationship(
    back_populates="category"
)
```

y:

```python
category: Mapped["Category"] = relationship(
    back_populates="products"
)
```

## Crear objetos relacionados

```python
with Session(engine) as session:
    category = Category(name="Tecnología")

    product = Product(
        name="Laptop",
        price=3500,
        category=category
    )

    session.add(product)
    session.commit()
```

## Consultar con relación

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    if product is not None:
        print(product.name)
        print(product.category.name)
```

## Carga perezosa

Por defecto, ciertas relaciones pueden cargarse cuando se accede a ellas.

```python
product.category
```

Esto puede emitir una consulta adicional.

En consultas con muchas filas, debe cuidarse para evitar demasiadas consultas.

## `selectinload`

Para cargar relaciones de forma más eficiente:

```python
from sqlalchemy.orm import selectinload

statement = (
    select(Product)
    .options(selectinload(Product.category))
)
```

## `joinedload`

También puede cargarse con join:

```python
from sqlalchemy.orm import joinedload

statement = (
    select(Product)
    .options(joinedload(Product.category))
)
```

## Consultas con join

```python
statement = (
    select(Product)
    .join(Product.category)
    .where(Category.name == "Tecnología")
)
```

Ejecutar:

```python
with Session(engine) as session:
    products = session.scalars(statement).all()
```

## Consultas con columnas específicas

```python
statement = select(Product.name, Product.price)

with Session(engine) as session:
    rows = session.execute(statement).all()

    for name, price in rows:
        print(name, price)
```

Cuando se seleccionan columnas, suele usarse `session.execute()`.

Cuando se seleccionan entidades ORM, suele usarse `session.scalars()`.

## Agregaciones

```python
from sqlalchemy import func

statement = select(
    func.count(Product.id)
)

with Session(engine) as session:
    count = session.scalar(statement)

    print(count)
```

## Suma y promedio

```python
statement = select(
    func.sum(Product.stock),
    func.avg(Product.price)
)
```

## Agrupar

```python
statement = (
    select(Category.name, func.count(Product.id))
    .join(Product)
    .group_by(Category.name)
)
```

## SQL textual

SQLAlchemy permite ejecutar SQL textual con `text()`.

```python
from sqlalchemy import text

with engine.connect() as connection:
    result = connection.execute(
        text("SELECT 1")
    )

    print(result.scalar())
```

Consulta con parámetros:

```python
statement = text(
    "SELECT * FROM products WHERE price >= :minimum_price"
)

with engine.connect() as connection:
    result = connection.execute(
        statement,
        {"minimum_price": 100}
    )

    rows = result.all()
```

Usar parámetros evita concatenar valores directamente dentro del SQL.

## Core básico

SQLAlchemy Core permite trabajar sin clases ORM.

```python
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import MetaData
from sqlalchemy import String
from sqlalchemy import Table

metadata = MetaData()

products = Table(
    "products",
    metadata,
    Column("id", Integer, primary_key=True),
    Column("name", String(100)),
)
```

Crear tablas:

```python
metadata.create_all(engine)
```

Insertar:

```python
with engine.begin() as connection:
    connection.execute(
        products.insert(),
        [
            {"name": "Laptop"},
            {"name": "Mouse"},
        ]
    )
```

Consultar:

```python
statement = select(products)

with engine.connect() as connection:
    rows = connection.execute(statement).all()

    print(rows)
```

## Conexiones

## `engine.connect()`

Permite abrir una conexión explícita.

```python
with engine.connect() as connection:
    result = connection.execute(text("SELECT 1"))
    print(result.scalar())
```

## `engine.begin()`

Abre conexión con transacción.

```python
with engine.begin() as connection:
    connection.execute(text("INSERT INTO logs(message) VALUES ('ok')"))
```

Si ocurre un error, la transacción se revierte.

## URLs de conexión

## SQLite

Archivo local:

```python
"sqlite:///app.db"
```

Base en memoria:

```python
"sqlite:///:memory:"
```

## PostgreSQL

```python
"postgresql+psycopg://user:password@localhost:5432/database"
```

## MySQL

```python
"mysql+pymysql://user:password@localhost:3306/database"
```

## SQL Server

```python
"mssql+pyodbc://user:password@server/database?driver=ODBC+Driver+18+for+SQL+Server"
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
engine = create_engine(
    "postgresql+psycopg://user:password@localhost:5432/database"
)
```

Más conveniente:

```python
import os

from sqlalchemy import create_engine

database_url = os.getenv("DATABASE_URL")

engine = create_engine(database_url)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
DATABASE_URL=sqlite:///app.db
```

Código:

```python
import os

from dotenv import load_dotenv
from sqlalchemy import create_engine

load_dotenv()

database_url = os.getenv("DATABASE_URL")

engine = create_engine(database_url)
```

## Integración con pandas

Pandas puede leer datos usando una conexión o engine SQLAlchemy.

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("sqlite:///app.db")

df = pd.read_sql(
    "SELECT * FROM products",
    engine
)

print(df.head())
```

También puede escribir datos:

```python
df.to_sql(
    "products_export",
    engine,
    if_exists="replace",
    index=False
)
```

## Integración con FastAPI

SQLAlchemy suele usarse como capa de datos en FastAPI.

Patrón conceptual:

```text
FastAPI endpoint -> service -> SQLAlchemy Session -> database
```

Ejemplo mínimo:

```python
from fastapi import Depends, FastAPI
from sqlalchemy.orm import Session

app = FastAPI()


def get_session():
    with Session(engine) as session:
        yield session


@app.get("/products")
def read_products(session: Session = Depends(get_session)):
    statement = select(Product)
    products = session.scalars(statement).all()

    return products
```

En una aplicación real, se suelen usar modelos de respuesta y esquemas Pydantic.

## Integración con Flask

SQLAlchemy puede usarse directamente con Flask o mediante extensiones como Flask-SQLAlchemy.

Uso directo conceptual:

```python
from flask import Flask
from sqlalchemy import select
from sqlalchemy.orm import Session

app = Flask(__name__)


@app.route("/products")
def read_products():
    with Session(engine) as session:
        products = session.scalars(select(Product)).all()

    return [
        {
            "id": product.id,
            "name": product.name,
            "price": product.price,
        }
        for product in products
    ]
```

## Migraciones con Alembic

SQLAlchemy no gestiona automáticamente migraciones complejas de esquema.

Para migraciones suele usarse `alembic`.

Instalación:

```bash
python -m pip install alembic
```

Inicializar:

```bash
alembic init migrations
```

Crear migración:

```bash
alembic revision --autogenerate -m "create products table"
```

Aplicar migraciones:

```bash
alembic upgrade head
```

## `create_all()` vs Alembic

## `create_all()`

Útil para:

```text
prototipos
pruebas
scripts pequeños
aprendizaje
```

## Alembic

Útil para:

```text
proyectos reales
control de versiones del esquema
cambios incrementales
migraciones reproducibles
despliegue ordenado
```

## Organización recomendada

Estructura simple:

```text
app/
├─ database.py
├─ models.py
├─ repositories.py
└─ main.py
```

## `database.py`

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import Session

engine = create_engine("sqlite:///app.db")


def get_session():
    with Session(engine) as session:
        yield session
```

## `models.py`

```python
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column


class Base(DeclarativeBase):
    pass


class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    price: Mapped[float]
    stock: Mapped[int] = mapped_column(default=0)
```

## `repositories.py`

```python
from sqlalchemy import select
from sqlalchemy.orm import Session

from .models import Product


def list_products(session: Session):
    statement = select(Product).order_by(Product.name)

    return session.scalars(statement).all()


def get_product(session: Session, product_id: int):
    return session.get(Product, product_id)


def create_product(session: Session, name: str, price: float, stock: int = 0):
    product = Product(
        name=name,
        price=price,
        stock=stock
    )

    session.add(product)
    session.commit()
    session.refresh(product)

    return product
```

## Patrón repository

El patrón repository separa el acceso a datos del resto de la aplicación.

```text
router / view -> service -> repository -> database
```

Esto evita que consultas SQLAlchemy queden mezcladas con lógica HTTP o lógica de presentación.

## Sesión por solicitud

En aplicaciones web, suele usarse una sesión por solicitud.

Flujo conceptual:

```text
inicia request
se crea Session
se ejecuta endpoint
commit o rollback
se cierra Session
termina request
```

Esto evita sesiones globales compartidas indebidamente.

## Sesiones globales

No conviene usar una única sesión global para toda la aplicación.

Problemático:

```python
session = Session(engine)
```

y reutilizarla en muchas solicitudes o procesos.

Más seguro:

```python
with Session(engine) as session:
    ...
```

o usar una dependencia por solicitud en frameworks web.

## Manejo de errores

```python
from sqlalchemy.exc import SQLAlchemyError

try:
    with Session(engine) as session:
        session.add(product)
        session.commit()
except SQLAlchemyError as error:
    print("Error de base de datos:", error)
```

En aplicaciones reales, normalmente se registra el error y se devuelve una respuesta controlada.

## Excepciones frecuentes

```python
IntegrityError
NoResultFound
MultipleResultsFound
SQLAlchemyError
OperationalError
ProgrammingError
```

Importación:

```python
from sqlalchemy.exc import IntegrityError
from sqlalchemy.exc import SQLAlchemyError
```

## Restricciones de unicidad

Modelo:

```python
from sqlalchemy import String

class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    sku: Mapped[str] = mapped_column(String(50), unique=True)
    name: Mapped[str]
```

Manejo:

```python
from sqlalchemy.exc import IntegrityError

try:
    session.add(product)
    session.commit()
except IntegrityError:
    session.rollback()
    raise ValueError("El SKU ya existe")
```

## Índices

```python
name: Mapped[str] = mapped_column(index=True)
```

Un índice puede mejorar búsquedas frecuentes, pero también tiene costo de mantenimiento en escritura.

## Constraints

SQLAlchemy permite definir restricciones.

```python
from sqlalchemy import CheckConstraint


class Product(Base):
    __tablename__ = "products"

    __table_args__ = (
        CheckConstraint("price > 0", name="check_price_positive"),
    )

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    price: Mapped[float]
```

## Representación de objetos

Es útil definir `__repr__` para depuración.

```python
def __repr__(self):
    return f"Product(id={self.id!r}, name={self.name!r})"
```

Ejemplo completo:

```python
class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    price: Mapped[float]

    def __repr__(self):
        return f"Product(id={self.id!r}, name={self.name!r})"
```

## Actualizar datos de un objeto después de commit

Puede usarse `session.refresh()`.

```python
session.add(product)
session.commit()
session.refresh(product)

print(product.id)
```

Esto es útil para obtener valores generados por la base de datos.

## Async SQLAlchemy

SQLAlchemy también tiene soporte asíncrono.

Importaciones frecuentes:

```python
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy.ext.asyncio import create_async_engine
```

Ejemplo conceptual:

```python
from sqlalchemy.ext.asyncio import create_async_engine

engine = create_async_engine(
    "sqlite+aiosqlite:///app.db"
)
```

Para SQLite asíncrono suele requerirse:

```bash
python -m pip install aiosqlite
```

El uso asíncrono requiere patrones distintos, como `async with` y `await`.

Para aprendizaje inicial, suele ser mejor dominar primero el uso síncrono.

## Errores comunes

## Olvidar `commit()`

Problemático:

```python
session.add(product)
```

sin:

```python
session.commit()
```

El objeto puede no quedar persistido.

## Usar una sesión global

Problemático:

```python
session = Session(engine)
```

reutilizada en todo el programa.

Mejor:

```python
with Session(engine) as session:
    ...
```

## No hacer `rollback()` después de un error

Después de ciertos errores, la sesión puede quedar en estado inválido hasta llamar a:

```python
session.rollback()
```

## Usar `create_all()` como migración de producción

`create_all()` no reemplaza un sistema de migraciones.

Para cambios de esquema controlados, usar Alembic.

## Guardar credenciales en el código

Problemático:

```python
DATABASE_URL = "postgresql+psycopg://user:password@localhost/db"
```

Mejor:

```python
DATABASE_URL = os.getenv("DATABASE_URL")
```

## Concatenar SQL con valores del usuario

Problemático:

```python
statement = text(f"SELECT * FROM products WHERE name = '{name}'")
```

Mejor:

```python
statement = text("SELECT * FROM products WHERE name = :name")
connection.execute(statement, {"name": name})
```

## Confundir `execute()` y `scalars()`

Para entidades ORM:

```python
products = session.scalars(select(Product)).all()
```

Para filas o columnas múltiples:

```python
rows = session.execute(select(Product.name, Product.price)).all()
```

## No cerrar conexiones o sesiones

Usar context managers ayuda:

```python
with Session(engine) as session:
    ...
```

## No definir clave primaria

Cada clase ORM mapeada necesita una clave primaria.

```python
id: Mapped[int] = mapped_column(primary_key=True)
```

## Generar demasiadas consultas por relaciones

Acceder a relaciones en bucles puede generar muchas consultas.

Conviene evaluar:

```python
selectinload()
joinedload()
```

## Buenas prácticas

## Usar SQLAlchemy 2.x style

```python
statement = select(Product)
products = session.scalars(statement).all()
```

## Definir una base declarativa común

```python
class Base(DeclarativeBase):
    pass
```

## Usar `Mapped` y `mapped_column()`

```python
id: Mapped[int] = mapped_column(primary_key=True)
```

## Manejar sesiones con context manager

```python
with Session(engine) as session:
    ...
```

## Confirmar o revertir transacciones explícitamente

```python
session.commit()
session.rollback()
```

## Separar modelos y acceso a datos

```text
models.py
repositories.py
services.py
```

## Usar variables de entorno para credenciales

```python
os.getenv("DATABASE_URL")
```

## Usar Alembic para migraciones

```bash
alembic revision --autogenerate -m "message"
alembic upgrade head
```

## Usar parámetros en SQL textual

```python
text("SELECT * FROM products WHERE id = :id")
```

## Revisar SQL generado en desarrollo

```python
create_engine("sqlite:///app.db", echo=True)
```

## Ejemplo integrado

```python
from pathlib import Path

from sqlalchemy import create_engine
from sqlalchemy import select
from sqlalchemy.exc import IntegrityError
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import Session
from sqlalchemy.orm import mapped_column


class Base(DeclarativeBase):
    pass


class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(unique=True)
    price: Mapped[float]
    stock: Mapped[int] = mapped_column(default=0)
    is_active: Mapped[bool] = mapped_column(default=True)

    def __repr__(self):
        return f"Product(id={self.id!r}, name={self.name!r})"


def create_database(engine):
    Base.metadata.create_all(engine)


def create_product(session, name, price, stock=0):
    product = Product(
        name=name,
        price=price,
        stock=stock
    )

    session.add(product)

    try:
        session.commit()
    except IntegrityError:
        session.rollback()
        raise ValueError("El producto ya existe")

    session.refresh(product)

    return product


def list_products(session):
    statement = (
        select(Product)
        .where(Product.is_active.is_(True))
        .order_by(Product.name)
    )

    return session.scalars(statement).all()


def update_stock(session, product_id, stock):
    product = session.get(Product, product_id)

    if product is None:
        return None

    product.stock = stock
    session.commit()
    session.refresh(product)

    return product


database_path = Path("app.db")
engine = create_engine(
    f"sqlite:///{database_path}",
    echo=False
)

create_database(engine)

with Session(engine) as session:
    if not list_products(session):
        create_product(session, "Laptop", 3500, stock=5)
        create_product(session, "Mouse", 80, stock=20)

    products = list_products(session)

    for product in products:
        print(product.id, product.name, product.price, product.stock)
```

## Relación con otras librerías

`sqlalchemy` se relaciona especialmente con:

- `alembic`, para migraciones de base de datos
- `psycopg`, para PostgreSQL
- `pymysql`, para MySQL o MariaDB
- `pyodbc`, para SQL Server
- `pandas`, para leer o escribir datos tabulares desde SQL
- `fastapi`, para construir APIs con persistencia
- `flask`, para aplicaciones web con base de datos
- `pydantic`, para validación de datos alrededor de APIs
- `python-dotenv`, para cargar variables de entorno
- `pytest`, para pruebas
- `sqlite3`, como base incluida en Python para SQLite

## Orden didáctico interno

```text
1. Propósito de sqlalchemy
2. Instalación y drivers
3. Engine
4. SQLAlchemy Core vs ORM
5. DeclarativeBase
6. Modelos con Mapped y mapped_column()
7. Crear tablas
8. Session
9. Crear, consultar, actualizar y eliminar registros
10. Transacciones
11. Relaciones
12. Consultas con select(), where(), order_by(), limit() y join()
13. Agregaciones
14. SQL textual
15. Integración con pandas, FastAPI y Flask
16. Migraciones con Alembic
17. Organización de proyecto
18. Errores comunes
19. Buenas prácticas
```