# `tinydb`

## Propósito

`tinydb` es una librería externa para trabajar con una base de datos documental ligera en Python.

Permite guardar datos como documentos, normalmente en un archivo JSON, sin instalar ni configurar un servidor de base de datos.

Se utiliza para proyectos pequeños, scripts, prototipos, herramientas locales, configuraciones persistentes, catálogos simples y casos donde una base SQL completa sería innecesaria.

## Naturaleza de la librería

`tinydb` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install tinydb
```

Importaciones frecuentes:

```python
from tinydb import TinyDB
from tinydb import Query
from tinydb import where
```

`TinyDB` trabaja con documentos parecidos a diccionarios de Python.

## Idea central

La idea principal de `tinydb` es guardar documentos en una base local simple.

Ejemplo mínimo:

```python
from tinydb import TinyDB

db = TinyDB("db.json")

db.insert({
    "name": "Laptop",
    "price": 3500,
    "stock": 5
})

products = db.all()

print(products)

db.close()
```

Esto crea o usa un archivo:

```text
db.json
```

## Cuándo usar TinyDB

Conviene usar `tinydb` cuando se necesita:

```text
guardar pocos datos de forma simple
crear prototipos rápidos
evitar configurar un servidor
trabajar con documentos tipo diccionario
guardar configuraciones locales
hacer scripts con persistencia sencilla
crear herramientas personales o internas pequeñas
almacenar datos en JSON de forma estructurada
```

## Cuándo no usar TinyDB

No conviene usar `tinydb` cuando se necesita:

```text
alta concurrencia
muchos usuarios escribiendo al mismo tiempo
consultas complejas
joins
transacciones fuertes
garantías ACID completas
índices avanzados
base de datos centralizada
volúmenes grandes de datos
aplicaciones críticas de producción
```

Para esos casos suelen corresponder:

```text
SQLite
PostgreSQL
MySQL
SQL Server
MongoDB
DuckDB
```

## Instalación

Instalación básica:

```bash
python -m pip install tinydb
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
tinydb==4.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import tinydb

print(tinydb.__version__)
```

## Crear una base

```python
from tinydb import TinyDB

db = TinyDB("db.json")
```

Esto crea una base persistente en el archivo:

```text
db.json
```

Si el archivo no existe, TinyDB puede crearlo al insertar datos.

## Cerrar la base

```python
db.close()
```

En scripts pequeños puede no notarse, pero cerrar recursos es una buena práctica.

## Uso con context manager

TinyDB puede usarse con context manager.

```python
from tinydb import TinyDB

with TinyDB("db.json") as db:
    db.insert({
        "name": "Laptop",
        "price": 3500
    })
```

Al salir del bloque, la base se cierra.

## Documento

Un documento en TinyDB es un diccionario de Python.

```python
product = {
    "name": "Laptop",
    "price": 3500,
    "stock": 5,
    "is_active": True
}
```

Cada documento puede tener campos distintos.

Esto lo hace más flexible que una tabla relacional, pero también exige más cuidado en la validación.

## Insertar un documento

```python
from tinydb import TinyDB

db = TinyDB("db.json")

document_id = db.insert({
    "name": "Laptop",
    "price": 3500,
    "stock": 5
})

print(document_id)

db.close()
```

`insert()` devuelve el ID interno del documento.

## Insertar varios documentos

```python
db.insert_multiple([
    {
        "name": "Laptop",
        "price": 3500,
        "stock": 5
    },
    {
        "name": "Mouse",
        "price": 80,
        "stock": 20
    },
    {
        "name": "Teclado",
        "price": 150,
        "stock": 10
    }
])
```

## Leer todos los documentos

```python
products = db.all()

print(products)
```

## Iterar documentos

```python
for product in db:
    print(product)
```

## Consultas con `Query`

Para buscar documentos se usa normalmente `Query`.

```python
from tinydb import Query

Product = Query()

results = db.search(
    Product.name == "Laptop"
)

print(results)
```

## Buscar un documento

```python
Product = Query()

product = db.get(
    Product.name == "Laptop"
)

print(product)
```

`get()` devuelve un documento o `None`.

## Buscar por ID de documento

```python
product = db.get(doc_id=1)

print(product)
```

## Verificar existencia

```python
exists = db.contains(
    Product.name == "Laptop"
)

print(exists)
```

También puede verificarse por `doc_id`.

```python
exists = db.contains(doc_id=1)
```

## Filtros simples

Igualdad:

```python
db.search(Product.name == "Laptop")
```

Distinto:

```python
db.search(Product.name != "Laptop")
```

Mayor que:

```python
db.search(Product.price > 100)
```

Mayor o igual que:

```python
db.search(Product.price >= 100)
```

Menor que:

```python
db.search(Product.price < 1000)
```

Menor o igual que:

```python
db.search(Product.price <= 1000)
```

## Condiciones compuestas

Condición `AND`:

```python
results = db.search(
    (Product.price >= 100) &
    (Product.is_active == True)
)
```

Condición `OR`:

```python
results = db.search(
    (Product.name == "Laptop") |
    (Product.name == "Mouse")
)
```

Los paréntesis son importantes.

## Buscar texto

```python
results = db.search(
    Product.name.search("Lap")
)
```

## Campo dentro de lista

```python
results = db.search(
    Product.tags.any(["oferta"])
)
```

## Campo que existe

```python
results = db.search(
    Product.description.exists()
)
```

## Uso de `where`

También puede usarse `where`.

```python
from tinydb import where

results = db.search(
    where("name") == "Laptop"
)
```

Este estilo puede ser útil para consultas simples.

## Actualizar documentos

```python
Product = Query()

db.update(
    {
        "price": 3600,
        "stock": 4
    },
    Product.name == "Laptop"
)
```

Esto actualiza los documentos que cumplan la condición.

## Actualizar por ID

```python
db.update(
    {"stock": 10},
    doc_ids=[1]
)
```

## Modificar con función

```python
def increment_stock(document):
    document["stock"] = document.get("stock", 0) + 1


db.update(
    increment_stock,
    Product.name == "Laptop"
)
```

## Eliminar documentos

```python
db.remove(
    Product.name == "Laptop"
)
```

## Eliminar por ID

```python
db.remove(doc_ids=[1])
```

## Eliminar todos los documentos

```python
db.truncate()
```

Este método debe usarse con cuidado porque borra el contenido de la tabla actual.

## Eliminación lógica

En aplicaciones reales, muchas veces conviene marcar documentos como inactivos.

```python
db.update(
    {"is_active": False},
    Product.name == "Laptop"
)
```

Consulta de activos:

```python
active_products = db.search(
    Product.is_active == True
)
```

## IDs de documento

TinyDB asigna IDs internos a los documentos.

Al insertar:

```python
doc_id = db.insert({
    "name": "Laptop"
})
```

Luego puede consultarse:

```python
document = db.get(doc_id=doc_id)
```

## Acceder al ID de un documento encontrado

Los documentos devueltos por TinyDB pueden tener atributo `doc_id`.

```python
product = db.get(Product.name == "Laptop")

if product is not None:
    print(product.doc_id)
```

## Tablas

TinyDB permite separar datos en tablas.

```python
products = db.table("products")
clients = db.table("clients")
```

Uso:

```python
products.insert({
    "name": "Laptop",
    "price": 3500
})

clients.insert({
    "name": "Ana"
})
```

## Tabla por defecto

Cuando se usa directamente `db.insert()`, se trabaja con la tabla por defecto.

```python
db.insert({
    "name": "Laptop"
})
```

Esto equivale a usar la tabla principal de la base.

## Consultar tablas

```python
products = db.table("products")

Product = Query()

results = products.search(
    Product.price >= 100
)
```

## Eliminar una tabla

```python
db.drop_table("products")
```

## Eliminar todas las tablas

```python
db.drop_tables()
```

Debe usarse con mucho cuidado.

## Almacenamiento JSON

Por defecto, TinyDB guarda datos en un archivo JSON.

Ejemplo conceptual:

```json
{
    "_default": {
        "1": {
            "name": "Laptop",
            "price": 3500
        }
    }
}
```

El formato exacto puede depender de la versión y configuración.

No conviene editar manualmente el archivo mientras la aplicación está usando la base.

## Storage en memoria

TinyDB también puede trabajar en memoria.

```python
from tinydb import TinyDB
from tinydb.storages import MemoryStorage

db = TinyDB(storage=MemoryStorage)

db.insert({
    "name": "Laptop"
})
```

Este almacenamiento desaparece al terminar la ejecución.

Es útil para pruebas o datos temporales.

## JSONStorage

El almacenamiento por defecto usa JSON.

```python
from tinydb import TinyDB
from tinydb.storages import JSONStorage

db = TinyDB(
    "db.json",
    storage=JSONStorage
)
```

## Middleware de caché

TinyDB permite usar middlewares.

Ejemplo con caché:

```python
from tinydb import TinyDB
from tinydb.middlewares import CachingMiddleware
from tinydb.storages import JSONStorage

db = TinyDB(
    "db.json",
    storage=CachingMiddleware(JSONStorage)
)
```

El middleware de caché puede mejorar rendimiento en ciertos usos, pero los datos pueden escribirse en disco con retraso.

## Vaciar caché

```python
db.storage.flush()
```

Cuando se usa caché, puede ser relevante asegurar que los cambios se escriban en disco.

## Validación de datos

TinyDB no impone un esquema rígido.

Esto permite flexibilidad, pero significa que la aplicación debe validar los datos.

Ejemplo de validación simple:

```python
def validate_product(data):
    if not data.get("name"):
        raise ValueError("El nombre es obligatorio")

    if data.get("price", 0) <= 0:
        raise ValueError("El precio debe ser mayor que cero")
```

Uso:

```python
product = {
    "name": "Laptop",
    "price": 3500,
    "stock": 5
}

validate_product(product)

db.insert(product)
```

## Uso con Pydantic

Para validación más estructurada puede usarse Pydantic.

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

Uso:

```python
product = ProductCreate(
    name="Laptop",
    price=3500,
    stock=5
)

db.insert(product.model_dump())
```

## Serialización de tipos

TinyDB usa JSON por defecto.

JSON maneja bien tipos simples como:

```text
str
int
float
bool
None
list
dict
```

Puede tener problemas con objetos que JSON no serializa directamente, como:

```text
datetime
Decimal
Path
clases personalizadas
```

## Guardar fechas

Una forma simple es convertir fechas a texto ISO.

```python
from datetime import date

product = {
    "name": "Laptop",
    "created_at": date.today().isoformat()
}

db.insert(product)
```

Al leer:

```python
from datetime import date

created_at = date.fromisoformat(product["created_at"])
```

## Guardar `Decimal`

Puede convertirse a texto o número, según el nivel de precisión requerido.

```python
from decimal import Decimal

product = {
    "name": "Laptop",
    "price": str(Decimal("3500.00"))
}

db.insert(product)
```

Al leer:

```python
price = Decimal(product["price"])
```

## Uso con pathlib

```python
from pathlib import Path
from tinydb import TinyDB

database_path = Path("data") / "db.json"

database_path.parent.mkdir(
    parents=True,
    exist_ok=True
)

db = TinyDB(database_path)
```

## Funciones reutilizables

## Crear base

```python
from pathlib import Path

from tinydb import TinyDB


def create_database(path="db.json"):
    database_path = Path(path)
    database_path.parent.mkdir(
        parents=True,
        exist_ok=True
    )

    return TinyDB(database_path)
```

## Crear producto

```python
def create_product(db, name, price, stock=0):
    return db.insert({
        "name": name,
        "price": price,
        "stock": stock,
        "is_active": True
    })
```

## Listar productos activos

```python
from tinydb import Query


def list_products(db):
    Product = Query()

    return db.search(
        Product.is_active == True
    )
```

## Obtener producto por ID

```python
def get_product(db, product_id):
    return db.get(doc_id=product_id)
```

## Actualizar stock

```python
def update_stock(db, product_id, stock):
    db.update(
        {"stock": stock},
        doc_ids=[product_id]
    )
```

## Desactivar producto

```python
def deactivate_product(db, product_id):
    db.update(
        {"is_active": False},
        doc_ids=[product_id]
    )
```

## Uso con FastAPI

TinyDB puede usarse en una API pequeña, aunque no es ideal para alta concurrencia.

```python
from contextlib import asynccontextmanager

from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, Field
from tinydb import Query, TinyDB


class ProductCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.db = TinyDB("db.json")
    yield
    app.state.db.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
def read_products():
    Product = Query()

    return app.state.db.search(
        Product.is_active == True
    )


@app.get("/products/{product_id}")
def read_product(product_id: int):
    product = app.state.db.get(doc_id=product_id)

    if product is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return product


@app.post("/products")
def create_product(product: ProductCreate):
    data = product.model_dump()
    data["is_active"] = True

    doc_id = app.state.db.insert(data)

    data["id"] = doc_id

    return data
```

## Uso con scripts

```python
from tinydb import Query, TinyDB


def main():
    db = TinyDB("db.json")

    try:
        Product = Query()

        if not db.contains(Product.name == "Laptop"):
            db.insert({
                "name": "Laptop",
                "price": 3500,
                "stock": 5,
                "is_active": True
            })

        products = db.search(
            Product.is_active == True
        )

        for product in products:
            print(
                product.doc_id,
                product["name"],
                product["price"],
                product["stock"]
            )

    finally:
        db.close()


main()
```

## Uso con pandas

TinyDB puede alimentar un `DataFrame`.

```python
import pandas as pd
from tinydb import TinyDB

db = TinyDB("db.json")

try:
    documents = db.all()

    df = pd.DataFrame(documents)

    print(df.head())
finally:
    db.close()
```

También puede insertarse desde un DataFrame.

```python
records = df.to_dict(orient="records")

db.insert_multiple(records)
```

Antes de insertar, conviene revisar tipos y valores nulos.

## Uso con archivos de configuración

TinyDB puede servir para guardar configuraciones pequeñas.

```python
settings = db.table("settings")

settings.upsert(
    {
        "name": "theme",
        "value": "dark"
    },
    Query().name == "theme"
)
```

Leer:

```python
setting = settings.get(
    Query().name == "theme"
)

print(setting["value"])
```

## `upsert`

`upsert()` actualiza si existe coincidencia o inserta si no existe.

```python
Product = Query()

db.upsert(
    {
        "name": "Laptop",
        "price": 3500,
        "stock": 5
    },
    Product.name == "Laptop"
)
```

Es útil para cargas idempotentes.

## Diferencia frente a SQLite

## TinyDB

```text
documentos
JSON
sin esquema rígido
API de Python simple
consultas básicas
ideal para datos pequeños y flexibles
```

## SQLite

```text
tablas
SQL
restricciones
joins
índices
transacciones más formales
mejor para datos relacionales locales
```

## Regla práctica

Si se necesitan tablas, relaciones, restricciones e integridad, SQLite suele ser mejor.

Si se necesita guardar documentos simples sin esquema, TinyDB puede ser suficiente.

## Diferencia frente a MongoDB

## TinyDB

```text
local
sin servidor
archivo JSON
proyectos pequeños
sin capacidades avanzadas de servidor
```

## MongoDB

```text
servidor de base de datos
colecciones documentales
índices avanzados
consultas más potentes
mayor escalabilidad
ecosistema más amplio
```

TinyDB puede verse como una opción documental mínima para uso local, no como reemplazo completo de MongoDB.

## Diferencia frente a Redis

## TinyDB

```text
persistencia en archivo JSON
documentos simples
uso local
```

## Redis

```text
base en memoria
estructuras clave-valor
caché
expiraciones
pub/sub
colas
```

Redis se usa más para caché, sesiones o datos temporales rápidos.

TinyDB se usa más para persistencia documental pequeña.

## Organización recomendada

```text
app/
├─ database.py
├─ repositories.py
├─ schemas.py
└─ main.py
```

## `database.py`

```python
from pathlib import Path

from tinydb import TinyDB


DATABASE_PATH = Path("data") / "db.json"


def create_database():
    DATABASE_PATH.parent.mkdir(
        parents=True,
        exist_ok=True
    )

    return TinyDB(DATABASE_PATH)
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
from tinydb import Query


def list_products(db):
    Product = Query()

    return db.search(
        Product.is_active == True
    )


def get_product(db, product_id):
    return db.get(doc_id=product_id)


def create_product(db, product_create):
    data = product_create.model_dump()
    data["is_active"] = True

    return db.insert(data)


def update_stock(db, product_id, stock):
    db.update(
        {"stock": stock},
        doc_ids=[product_id]
    )


def deactivate_product(db, product_id):
    db.update(
        {"is_active": False},
        doc_ids=[product_id]
    )
```

## Separación por responsabilidades

```text
database.py      -> apertura de TinyDB
schemas.py       -> validación de datos
repositories.py  -> operaciones sobre documentos
services.py      -> reglas de negocio
main.py          -> punto de entrada
```

## Errores comunes

## Usar TinyDB como base de producción pesada

TinyDB es útil para proyectos pequeños.

No debe asumirse que reemplaza a PostgreSQL, MySQL o MongoDB en sistemas con alta concurrencia o datos críticos.

## No cerrar la base

Menos recomendable:

```python
db = TinyDB("db.json")
```

sin:

```python
db.close()
```

Más seguro:

```python
with TinyDB("db.json") as db:
    ...
```

## Guardar objetos no serializables

Problemático:

```python
from datetime import date

db.insert({
    "created_at": date.today()
})
```

Mejor:

```python
db.insert({
    "created_at": date.today().isoformat()
})
```

## No validar datos

TinyDB no impone esquema.

Si se insertan documentos con campos distintos, luego las consultas pueden fallar o devolver datos inconsistentes.

## Editar manualmente el JSON mientras se usa la base

Modificar el archivo directamente puede corromper o desordenar la información.

## Usar `truncate()` accidentalmente

```python
db.truncate()
```

borra todos los documentos de la tabla actual.

## Confundir tablas de TinyDB con tablas SQL

Las tablas de TinyDB son separaciones lógicas dentro de la base JSON.

No son tablas relacionales con constraints, joins e índices SQL.

## Esperar consultas avanzadas

TinyDB tiene una API de consultas simple, pero no está pensado para análisis complejos ni optimización avanzada.

## Usar TinyDB con múltiples procesos escribiendo al mismo archivo

Puede generar problemas de consistencia.

Para ese escenario, conviene evaluar SQLite, PostgreSQL u otra base más adecuada.

## Buenas prácticas

## Usar TinyDB para datos pequeños

```text
configuraciones
catálogos simples
prototipos
datos locales
```

## Cerrar la base

```python
db.close()
```

o:

```python
with TinyDB("db.json") as db:
    ...
```

## Validar datos antes de insertar

```python
Pydantic
funciones propias de validación
```

## Usar nombres claros de tablas

```python
db.table("products")
db.table("settings")
```

## Usar `upsert()` para operaciones idempotentes

```python
db.upsert(data, condition)
```

## Serializar tipos especiales

```python
date.isoformat()
str(Decimal("3500.00"))
```

## Evitar TinyDB para alta concurrencia

En caso de múltiples usuarios o procesos, evaluar una base más robusta.

## Mantener el archivo de base en una carpeta clara

```text
data/db.json
```

## Ejemplo integrado

```python
from pathlib import Path

from pydantic import BaseModel, Field
from tinydb import Query, TinyDB


class ProductCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)


def create_database(path):
    database_path = Path(path)
    database_path.parent.mkdir(
        parents=True,
        exist_ok=True
    )

    return TinyDB(database_path)


def create_product(db, product_create):
    data = product_create.model_dump()
    data["is_active"] = True

    Product = Query()

    db.upsert(
        data,
        Product.name == data["name"]
    )


def list_products(db):
    Product = Query()

    return db.search(
        Product.is_active == True
    )


def get_product(db, product_id):
    return db.get(doc_id=product_id)


def update_stock(db, product_id, stock):
    db.update(
        {"stock": stock},
        doc_ids=[product_id]
    )


def deactivate_product(db, product_id):
    db.update(
        {"is_active": False},
        doc_ids=[product_id]
    )


def main():
    db = create_database("data/db.json")

    try:
        create_product(
            db,
            ProductCreate(
                name="Laptop",
                price=3500,
                stock=5
            )
        )

        create_product(
            db,
            ProductCreate(
                name="Mouse",
                price=80,
                stock=20
            )
        )

        products = list_products(db)

        for product in products:
            print(
                product.doc_id,
                product["name"],
                product["price"],
                product["stock"]
            )

    finally:
        db.close()


if __name__ == "__main__":
    main()
```

## Relación con otras librerías

`tinydb` se relaciona especialmente con:

```text
json
pathlib
pydantic
pandas
fastapi
pytest
```

## Relación con JSON

TinyDB usa almacenamiento JSON por defecto.

Esto facilita inspección manual, pero también impone límites de serialización.

## Relación con Pydantic

Pydantic puede validar documentos antes de insertarlos en TinyDB.

## Relación con pandas

TinyDB puede exportar documentos a DataFrames.

```python
pd.DataFrame(db.all())
```

## Relación con FastAPI

TinyDB puede alimentar APIs pequeñas o prototipos.

No es la mejor opción para APIs con alta concurrencia o escritura intensiva.

## Relación con SQLite

SQLite es una alternativa más robusta cuando se necesitan tablas, SQL, restricciones e integridad relacional.

## Relación con MongoDB

MongoDB es una base documental completa con servidor, índices y más capacidades.

TinyDB es una alternativa mínima para documentos locales pequeños.

## Orden didáctico interno

```text
1. Propósito de tinydb
2. Instalación
3. Crear una base con TinyDB
4. Documentos como diccionarios
5. insert() e insert_multiple()
6. all(), iteración y get()
7. Query, where() y filtros
8. update(), upsert() y remove()
9. doc_id
10. Tablas
11. Storage y MemoryStorage
12. Validación de datos
13. Serialización de tipos especiales
14. Uso con FastAPI, pandas y Pydantic
15. Comparación con SQLite, MongoDB y Redis
16. Organización recomendada
17. Errores comunes
18. Buenas prácticas
```