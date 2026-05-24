# `sqlmodel`

## Propósito

`sqlmodel` es una librería externa para trabajar con bases de datos SQL usando clases de Python.

Combina ideas de `SQLAlchemy` y `Pydantic` para permitir que una misma clase pueda servir como modelo de base de datos y como modelo de validación de datos.

Se utiliza especialmente en aplicaciones que necesitan:

```text
modelos de datos
tablas SQL
validación con type hints
integración con FastAPI
consultas mediante ORM
reducción de duplicación entre modelos de API y modelos de base de datos
```

## Naturaleza de la librería

`sqlmodel` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install sqlmodel
```

Importación principal:

```python
from sqlmodel import SQLModel
from sqlmodel import Field
from sqlmodel import Session
from sqlmodel import create_engine
from sqlmodel import select
```

`SQLModel` se apoya internamente en:

```text
Pydantic
SQLAlchemy
```

## Idea central

La idea principal de `sqlmodel` es definir modelos usando clases de Python y anotaciones de tipo.

Ejemplo:

```python
from sqlmodel import Field, SQLModel


class Product(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    price: float
    stock: int = 0
```

Esta clase representa una tabla SQL.

También permite crear objetos Python:

```python
product = Product(
    name="Laptop",
    price=3500,
    stock=5
)
```

## Cuándo usar `sqlmodel`

Conviene usar `sqlmodel` cuando se necesita:

```text
trabajar con bases SQL usando clases
integrar modelos con FastAPI
reducir duplicación entre modelos Pydantic y modelos SQLAlchemy
crear APIs con validación de datos
hacer CRUD simple o medio
usar SQLite, PostgreSQL, MySQL u otra base compatible con SQLAlchemy
aprender ORM con una sintaxis más compacta
```

## Cuándo no usar `sqlmodel`

No siempre conviene usar `sqlmodel` cuando se necesita:

```text
control completo y avanzado de SQLAlchemy
consultas SQL muy complejas
modelado relacional avanzado
bases no relacionales como MongoDB
caché o datos temporales como Redis
migraciones sin entender SQLAlchemy/Alembic
aplicaciones Django, que ya tienen su propio ORM
```

Para casos avanzados de ORM, `SQLAlchemy` directo puede ser más flexible.

Para Django, normalmente se usa el ORM de Django.

Para MongoDB se usa `pymongo`.

Para Redis se usa `redis`.

## Instalación

Instalación básica:

```bash
python -m pip install sqlmodel
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
sqlmodel==0.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import sqlmodel

print(sqlmodel.__version__)
```

## Modelo básico

```python
from sqlmodel import Field, SQLModel


class Product(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    price: float
    stock: int = 0
```

Partes principales:

```text
SQLModel      -> clase base
table=True    -> indica que el modelo representa una tabla
Field()       -> configura campos especiales
primary_key   -> define clave primaria
```

## `table=True`

Sin `table=True`, la clase funciona como modelo de datos, pero no como tabla.

Modelo de tabla:

```python
class Product(SQLModel, table=True):
    ...
```

Modelo solo para validación o transferencia:

```python
class ProductCreate(SQLModel):
    ...
```

## `Field`

`Field()` permite configurar un campo.

```python
id: int | None = Field(default=None, primary_key=True)
```

También puede usarse para restricciones o valores por defecto.

```python
name: str = Field(index=True)
price: float = Field(gt=0)
```

## Clave primaria

```python
id: int | None = Field(default=None, primary_key=True)
```

La clave primaria identifica de forma única cada registro.

Se usa `None` como valor inicial porque normalmente la base de datos genera el id al insertar.

## Crear engine

El engine representa la conexión con la base de datos.

```python
from sqlmodel import create_engine

engine = create_engine("sqlite:///app.db")
```

## Engine en memoria

```python
engine = create_engine("sqlite://")
```

Esta base existe solo durante la ejecución.

## Engine con archivo SQLite

```python
engine = create_engine("sqlite:///app.db")
```

Esto crea o abre el archivo:

```text
app.db
```

## Mostrar SQL generado

```python
engine = create_engine(
    "sqlite:///app.db",
    echo=True
)
```

`echo=True` muestra las sentencias SQL ejecutadas. Es útil para aprendizaje y depuración.

## Crear tablas

```python
from sqlmodel import SQLModel

SQLModel.metadata.create_all(engine)
```

Ejemplo completo:

```python
from sqlmodel import Field, SQLModel, create_engine


class Product(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    price: float
    stock: int = 0


engine = create_engine("sqlite:///app.db")

SQLModel.metadata.create_all(engine)
```

## Importante sobre `create_all()`

`create_all()` crea tablas que no existen.

No reemplaza un sistema de migraciones.

Para proyectos reales donde el esquema cambia en el tiempo, suele usarse:

```text
alembic
```

## Session

`Session` representa una unidad de trabajo con la base de datos.

Permite:

```text
insertar objetos
consultar registros
actualizar datos
eliminar objetos
confirmar transacciones
revertir cambios
```

Importación:

```python
from sqlmodel import Session
```

Uso:

```python
with Session(engine) as session:
    ...
```

## Insertar registro

```python
from sqlmodel import Session

product = Product(
    name="Laptop",
    price=3500,
    stock=5
)

with Session(engine) as session:
    session.add(product)
    session.commit()
    session.refresh(product)

print(product.id)
```

## `add()`

Agrega un objeto a la sesión.

```python
session.add(product)
```

## `commit()`

Confirma los cambios en la base de datos.

```python
session.commit()
```

## `refresh()`

Actualiza el objeto con valores generados por la base de datos.

```python
session.refresh(product)
```

Esto suele usarse para obtener el `id` generado después de insertar.

## Insertar varios registros

```python
products = [
    Product(name="Laptop", price=3500, stock=5),
    Product(name="Mouse", price=80, stock=20),
    Product(name="Teclado", price=150, stock=10),
]

with Session(engine) as session:
    session.add_all(products)
    session.commit()
```

## Consultar registros

Para consultar se usa `select()`.

```python
from sqlmodel import select

with Session(engine) as session:
    statement = select(Product)
    products = session.exec(statement).all()

for product in products:
    print(product.name)
```

## Consultar por id

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    print(product)
```

Si no encuentra el registro, devuelve `None`.

## Filtrar registros

```python
with Session(engine) as session:
    statement = select(Product).where(Product.price >= 100)
    products = session.exec(statement).all()
```

## Obtener el primer resultado

```python
with Session(engine) as session:
    statement = select(Product).where(Product.name == "Laptop")
    product = session.exec(statement).first()
```

## Obtener un único resultado

```python
with Session(engine) as session:
    statement = select(Product).where(Product.name == "Laptop")
    product = session.exec(statement).one()
```

`one()` espera exactamente un resultado.

Si no hay resultados o hay más de uno, genera error.

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

## Actualizar registro

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    if product is not None:
        product.price = 3600
        product.stock = 4

        session.add(product)
        session.commit()
        session.refresh(product)
```

El objeto se modifica en Python y luego se confirma en la base de datos.

## Eliminar registro

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    if product is not None:
        session.delete(product)
        session.commit()
```

## Eliminación lógica

En aplicaciones reales, muchas veces se prefiere marcar el registro como inactivo.

```python
from sqlmodel import Field, SQLModel


class Product(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    price: float
    stock: int = 0
    is_active: bool = True
```

Uso:

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    if product is not None:
        product.is_active = False
        session.add(product)
        session.commit()
```

## Modelos para API

Una ventaja de SQLModel es poder separar modelos de entrada, tabla y salida.

## Modelo base

```python
from sqlmodel import SQLModel


class ProductBase(SQLModel):
    name: str
    price: float
    stock: int = 0
```

## Modelo de tabla

```python
from sqlmodel import Field


class Product(ProductBase, table=True):
    id: int | None = Field(default=None, primary_key=True)
```

## Modelo de creación

```python
class ProductCreate(ProductBase):
    pass
```

## Modelo de lectura

```python
class ProductRead(ProductBase):
    id: int
```

## Modelo de actualización

```python
class ProductUpdate(SQLModel):
    name: str | None = None
    price: float | None = None
    stock: int | None = None
```

Este patrón evita exponer directamente todos los detalles internos de la tabla.

## Uso con FastAPI

SQLModel se usa con frecuencia junto a FastAPI.

Ejemplo conceptual:

```python
from fastapi import Depends, FastAPI, HTTPException
from sqlmodel import Field, Session, SQLModel, create_engine, select


class Product(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    price: float
    stock: int = 0


engine = create_engine("sqlite:///app.db")

app = FastAPI()


def create_db_and_tables():
    SQLModel.metadata.create_all(engine)


def get_session():
    with Session(engine) as session:
        yield session


@app.on_event("startup")
def on_startup():
    create_db_and_tables()


@app.get("/products")
def read_products(session: Session = Depends(get_session)):
    products = session.exec(select(Product)).all()

    return products


@app.post("/products")
def create_product(
    product: Product,
    session: Session = Depends(get_session)
):
    session.add(product)
    session.commit()
    session.refresh(product)

    return product
```

## Uso de modelos separados con FastAPI

```python
from fastapi import Depends, FastAPI, HTTPException
from sqlmodel import Field, Session, SQLModel, create_engine, select


class ProductBase(SQLModel):
    name: str
    price: float
    stock: int = 0


class Product(ProductBase, table=True):
    id: int | None = Field(default=None, primary_key=True)


class ProductCreate(ProductBase):
    pass


class ProductRead(ProductBase):
    id: int


engine = create_engine("sqlite:///app.db")

app = FastAPI()


def get_session():
    with Session(engine) as session:
        yield session


@app.post("/products", response_model=ProductRead)
def create_product(
    product_create: ProductCreate,
    session: Session = Depends(get_session)
):
    product = Product.model_validate(product_create)

    session.add(product)
    session.commit()
    session.refresh(product)

    return product
```

## Relaciones

SQLModel puede definir relaciones usando `Relationship`.

Ejemplo conceptual:

```text
Category -> Product
una categoría tiene muchos productos
un producto pertenece a una categoría
```

## Modelo con Foreign Key

```python
from sqlmodel import Field, Relationship, SQLModel


class Category(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str

    products: list["Product"] = Relationship(back_populates="category")


class Product(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    price: float

    category_id: int | None = Field(default=None, foreign_key="category.id")
    category: Category | None = Relationship(back_populates="products")
```

## `foreign_key`

Define una relación a nivel de base de datos.

```python
category_id: int | None = Field(default=None, foreign_key="category.id")
```

La cadena tiene esta forma:

```text
nombre_tabla.nombre_columna
```

## `Relationship`

Define una relación a nivel de objetos Python.

```python
category: Category | None = Relationship(back_populates="products")
```

## Crear objetos relacionados

```python
category = Category(name="Tecnología")

product = Product(
    name="Laptop",
    price=3500,
    category=category
)

with Session(engine) as session:
    session.add(product)
    session.commit()
    session.refresh(product)
```

## Consultar con relación

```python
with Session(engine) as session:
    product = session.get(Product, 1)

    if product is not None:
        print(product.name)
        print(product.category.name)
```

En relaciones y consultas complejas, sigue siendo importante entender SQLAlchemy.

## Restricciones y validación

Como SQLModel usa Pydantic, se pueden definir reglas en modelos no necesariamente de tabla.

Ejemplo:

```python
from sqlmodel import SQLModel


class ProductCreate(SQLModel):
    name: str
    price: float
    stock: int = 0
```

Para validación más explícita, pueden usarse herramientas de Pydantic según la versión del ecosistema utilizada.

## Índices

```python
name: str = Field(index=True)
```

Esto indica que se cree un índice sobre la columna.

## Valores únicos

SQLModel puede usar argumentos de SQLAlchemy cuando se necesita más control.

Para casos avanzados, puede ser más claro usar SQLAlchemy directamente o revisar cómo pasar restricciones mediante `sa_column`.

Ejemplo conceptual:

```python
from sqlalchemy import Column, String
from sqlmodel import Field, SQLModel


class Product(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    sku: str = Field(sa_column=Column(String, unique=True))
```

## Uso con PostgreSQL

SQLModel usa SQLAlchemy por debajo, por lo que puede conectarse a PostgreSQL mediante un driver compatible.

Ejemplo:

```python
engine = create_engine(
    "postgresql+psycopg://user:password@localhost:5432/app"
)
```

## Uso con MySQL

```python
engine = create_engine(
    "mysql+pymysql://user:password@localhost:3306/app"
)
```

## Uso con SQL Server

```python
engine = create_engine(
    "mssql+pyodbc://user:password@server/database?driver=ODBC+Driver+18+for+SQL+Server"
)
```

## Uso con SQLite

```python
engine = create_engine("sqlite:///app.db")
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

```python
import os

from sqlmodel import create_engine

database_url = os.getenv("DATABASE_URL")

if database_url is None:
    raise RuntimeError("DATABASE_URL no está configurada")

engine = create_engine(database_url)
```

## Uso con `python-dotenv`

```python
import os

from dotenv import load_dotenv
from sqlmodel import create_engine

load_dotenv()

database_url = os.getenv("DATABASE_URL", "sqlite:///app.db")

engine = create_engine(database_url)
```

## Migraciones con Alembic

SQLModel no reemplaza a Alembic.

Para proyectos reales, los cambios de esquema deben gestionarse con migraciones.

Flujo conceptual:

```text
SQLModel define modelos
SQLAlchemy expone metadata
Alembic genera y aplica migraciones
```

En `env.py` de Alembic se debe apuntar a:

```python
from app.models import SQLModel

target_metadata = SQLModel.metadata
```

También puede importarse el módulo donde están definidos todos los modelos para que el metadata los conozca.

## `create_all()` vs Alembic

## `create_all()`

Útil para:

```text
aprendizaje
prototipos
tests simples
scripts pequeños
```

## Alembic

Útil para:

```text
proyectos reales
control de versiones del esquema
bases persistentes
trabajo en equipo
producción
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

from dotenv import load_dotenv
from sqlmodel import Session, create_engine

load_dotenv()

database_url = os.getenv("DATABASE_URL", "sqlite:///app.db")

engine = create_engine(database_url)


def get_session():
    with Session(engine) as session:
        yield session
```

## `models.py`

```python
from sqlmodel import Field, SQLModel


class Product(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str = Field(index=True)
    price: float
    stock: int = 0
    is_active: bool = True
```

## `schemas.py`

```python
from sqlmodel import SQLModel


class ProductCreate(SQLModel):
    name: str
    price: float
    stock: int = 0


class ProductRead(SQLModel):
    id: int
    name: str
    price: float
    stock: int
    is_active: bool


class ProductUpdate(SQLModel):
    name: str | None = None
    price: float | None = None
    stock: int | None = None
    is_active: bool | None = None
```

## `repositories.py`

```python
from sqlmodel import Session, select

from .models import Product
from .schemas import ProductCreate, ProductUpdate


def list_products(session: Session):
    statement = (
        select(Product)
        .where(Product.is_active.is_(True))
        .order_by(Product.name)
    )

    return session.exec(statement).all()


def get_product(session: Session, product_id: int):
    return session.get(Product, product_id)


def create_product(session: Session, product_create: ProductCreate):
    product = Product.model_validate(product_create)

    session.add(product)
    session.commit()
    session.refresh(product)

    return product


def update_product(
    session: Session,
    product: Product,
    product_update: ProductUpdate
):
    update_data = product_update.model_dump(exclude_unset=True)

    for key, value in update_data.items():
        setattr(product, key, value)

    session.add(product)
    session.commit()
    session.refresh(product)

    return product
```

## Separación por responsabilidades

```text
models.py       -> tablas
schemas.py      -> modelos de entrada y salida
database.py     -> engine y session
repositories.py -> operaciones de base de datos
services.py     -> reglas de negocio
main.py         -> aplicación o punto de entrada
```

## Uso con tests

Para pruebas puede usarse SQLite en memoria o un archivo temporal.

```python
from sqlmodel import SQLModel, Session, create_engine


engine = create_engine("sqlite://")

SQLModel.metadata.create_all(engine)

with Session(engine) as session:
    product = Product(name="Laptop", price=3500)
    session.add(product)
    session.commit()
```

En pruebas más realistas, conviene crear una base de test separada.

## Consultas con columnas específicas

```python
statement = select(Product.name, Product.price)

with Session(engine) as session:
    rows = session.exec(statement).all()

    for name, price in rows:
        print(name, price)
```

## Agregaciones

SQLModel permite usar funciones de SQLAlchemy.

```python
from sqlalchemy import func
from sqlmodel import select

statement = select(func.count(Product.id))

with Session(engine) as session:
    count = session.exec(statement).one()

    print(count)
```

## Suma

```python
statement = select(func.sum(Product.stock))
```

## Promedio

```python
statement = select(func.avg(Product.price))
```

## Joins

```python
statement = (
    select(Product, Category)
    .join(Category)
    .where(Category.name == "Tecnología")
)
```

Para consultas relacionales complejas, resulta útil conocer SQLAlchemy.

## SQL textual

Cuando sea necesario ejecutar SQL directo, puede usarse SQLAlchemy.

```python
from sqlalchemy import text

with Session(engine) as session:
    result = session.exec(
        text("SELECT 1 AS value")
    ).all()

    print(result)
```

Para valores externos, deben usarse parámetros.

## Errores comunes

## Olvidar `table=True`

Problemático:

```python
class Product(SQLModel):
    ...
```

si se espera crear una tabla.

Correcto:

```python
class Product(SQLModel, table=True):
    ...
```

## Olvidar clave primaria

Una tabla ORM normalmente necesita clave primaria.

```python
id: int | None = Field(default=None, primary_key=True)
```

## No crear tablas

Definir el modelo no crea la tabla automáticamente.

Debe ejecutarse:

```python
SQLModel.metadata.create_all(engine)
```

o usarse Alembic.

## Confundir modelo de tabla con modelo de entrada

Usar directamente el modelo de tabla como entrada puede exponer campos que no deberían recibirse del cliente.

Mejor separar:

```text
Product
ProductCreate
ProductRead
ProductUpdate
```

## Olvidar `commit()`

```python
session.add(product)
```

sin:

```python
session.commit()
```

puede no persistir cambios.

## Olvidar `refresh()`

Después de insertar, el objeto puede necesitar:

```python
session.refresh(product)
```

para obtener valores generados por la base.

## Usar `create_all()` como migración de producción

`create_all()` no reemplaza migraciones.

Para producción, usar Alembic.

## No cerrar sesiones

Más seguro:

```python
with Session(engine) as session:
    ...
```

## Guardar credenciales en el código

Problemático:

```python
engine = create_engine("postgresql+psycopg://user:password@localhost/app")
```

Mejor:

```python
engine = create_engine(os.getenv("DATABASE_URL"))
```

## Creer que SQLModel elimina la necesidad de entender SQLAlchemy

SQLModel simplifica mucho, pero en consultas avanzadas, relaciones, índices, constraints y migraciones sigue siendo importante conocer SQLAlchemy.

## Buenas prácticas

## Separar modelos de tabla y modelos de API

```text
Product
ProductCreate
ProductRead
ProductUpdate
```

## Usar `Field()` para clave primaria e índices

```python
id: int | None = Field(default=None, primary_key=True)
name: str = Field(index=True)
```

## Usar sesiones con context manager

```python
with Session(engine) as session:
    ...
```

## Confirmar cambios con `commit()`

```python
session.commit()
```

## Refrescar objetos después de insertar

```python
session.refresh(product)
```

## Usar variables de entorno

```python
DATABASE_URL
```

## Usar Alembic para proyectos reales

```bash
alembic revision --autogenerate -m "create products table"
alembic upgrade head
```

## Mantener lógica de negocio fuera de los modelos

Separar:

```text
models
schemas
repositories
services
```

## Usar SQLAlchemy directo cuando el caso sea avanzado

SQLModel no impide usar herramientas de SQLAlchemy.

## Ejemplo integrado

```python
import os
from pathlib import Path

from dotenv import load_dotenv
from sqlmodel import Field, Session, SQLModel, create_engine, select

load_dotenv()


class ProductBase(SQLModel):
    name: str
    price: float
    stock: int = 0


class Product(ProductBase, table=True):
    id: int | None = Field(default=None, primary_key=True)
    is_active: bool = True


class ProductCreate(ProductBase):
    pass


class ProductUpdate(SQLModel):
    name: str | None = None
    price: float | None = None
    stock: int | None = None
    is_active: bool | None = None


database_url = os.getenv("DATABASE_URL", "sqlite:///app.db")

engine = create_engine(database_url)


def create_db_and_tables():
    SQLModel.metadata.create_all(engine)


def create_product(session: Session, product_create: ProductCreate):
    product = Product.model_validate(product_create)

    session.add(product)
    session.commit()
    session.refresh(product)

    return product


def list_products(session: Session):
    statement = (
        select(Product)
        .where(Product.is_active.is_(True))
        .order_by(Product.name)
    )

    return session.exec(statement).all()


def get_product(session: Session, product_id: int):
    return session.get(Product, product_id)


def update_product(
    session: Session,
    product: Product,
    product_update: ProductUpdate
):
    update_data = product_update.model_dump(exclude_unset=True)

    for key, value in update_data.items():
        setattr(product, key, value)

    session.add(product)
    session.commit()
    session.refresh(product)

    return product


def deactivate_product(session: Session, product: Product):
    product.is_active = False

    session.add(product)
    session.commit()
    session.refresh(product)

    return product


create_db_and_tables()

with Session(engine) as session:
    if not list_products(session):
        create_product(
            session,
            ProductCreate(
                name="Laptop",
                price=3500,
                stock=5
            )
        )

        create_product(
            session,
            ProductCreate(
                name="Mouse",
                price=80,
                stock=20
            )
        )

    products = list_products(session)

    for product in products:
        print(
            product.id,
            product.name,
            product.price,
            product.stock
        )
```

## Ejemplo integrado con FastAPI

```python
import os
from contextlib import asynccontextmanager

from dotenv import load_dotenv
from fastapi import Depends, FastAPI, HTTPException
from sqlmodel import Field, Session, SQLModel, create_engine, select

load_dotenv()


class ProductBase(SQLModel):
    name: str
    price: float
    stock: int = 0


class Product(ProductBase, table=True):
    id: int | None = Field(default=None, primary_key=True)
    is_active: bool = True


class ProductCreate(ProductBase):
    pass


class ProductRead(ProductBase):
    id: int
    is_active: bool


class ProductUpdate(SQLModel):
    name: str | None = None
    price: float | None = None
    stock: int | None = None
    is_active: bool | None = None


database_url = os.getenv("DATABASE_URL", "sqlite:///app.db")
engine = create_engine(database_url)


def create_db_and_tables():
    SQLModel.metadata.create_all(engine)


def get_session():
    with Session(engine) as session:
        yield session


@asynccontextmanager
async def lifespan(app: FastAPI):
    create_db_and_tables()
    yield


app = FastAPI(lifespan=lifespan)


@app.get("/products", response_model=list[ProductRead])
def read_products(session: Session = Depends(get_session)):
    statement = (
        select(Product)
        .where(Product.is_active.is_(True))
        .order_by(Product.name)
    )

    return session.exec(statement).all()


@app.get("/products/{product_id}", response_model=ProductRead)
def read_product(
    product_id: int,
    session: Session = Depends(get_session)
):
    product = session.get(Product, product_id)

    if product is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return product


@app.post("/products", response_model=ProductRead)
def create_product(
    product_create: ProductCreate,
    session: Session = Depends(get_session)
):
    product = Product.model_validate(product_create)

    session.add(product)
    session.commit()
    session.refresh(product)

    return product


@app.patch("/products/{product_id}", response_model=ProductRead)
def update_product(
    product_id: int,
    product_update: ProductUpdate,
    session: Session = Depends(get_session)
):
    product = session.get(Product, product_id)

    if product is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    update_data = product_update.model_dump(exclude_unset=True)

    for key, value in update_data.items():
        setattr(product, key, value)

    session.add(product)
    session.commit()
    session.refresh(product)

    return product
```

## Relación con otras librerías

`sqlmodel` se relaciona especialmente con:

```text
sqlalchemy
pydantic
fastapi
alembic
psycopg
pymysql
pyodbc
sqlite
python-dotenv
pytest
```

## Relación con SQLAlchemy

SQLModel se construye sobre SQLAlchemy.

SQLAlchemy aporta:

```text
engine
session
metadata
tablas
relaciones
consultas
dialectos de base de datos
```

## Relación con Pydantic

Pydantic aporta:

```text
validación
modelos basados en type hints
serialización
modelos de entrada y salida
```

## Relación con FastAPI

SQLModel encaja bien con FastAPI porque los modelos pueden usarse como modelos de request, response y base de datos.

Aun así, conviene separar modelos de tabla y modelos de entrada/salida.

## Relación con Alembic

Alembic se usa para migraciones cuando los modelos cambian.

SQLModel define la estructura.

Alembic registra y aplica cambios del esquema.

## Orden didáctico interno

```text
1. Propósito de sqlmodel
2. Instalación
3. Relación con SQLAlchemy y Pydantic
4. Modelos con SQLModel
5. table=True
6. Field()
7. Engine
8. Crear tablas
9. Session
10. Insertar, consultar, actualizar y eliminar
11. Modelos de tabla, entrada, salida y actualización
12. Relaciones
13. Uso con FastAPI
14. Variables de entorno
15. Migraciones con Alembic
16. Organización recomendada
17. Errores comunes
18. Buenas prácticas
```