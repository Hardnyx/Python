# `aiosqlite`

## Propósito

`aiosqlite` es una librería externa para trabajar con SQLite desde código asíncrono en Python.

Permite ejecutar consultas SQL, insertar registros, actualizar datos, eliminar filas, leer resultados y manejar transacciones usando `async` y `await`.

Su función principal es ofrecer una interfaz compatible con programación asíncrona sobre SQLite.

## Naturaleza de la librería

`aiosqlite` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install aiosqlite
```

La importación habitual es:

```python
import aiosqlite
```

`aiosqlite` se apoya conceptualmente en `sqlite3`, pero adapta el uso a un flujo asíncrono.

## Relación con SQLite

SQLite es una base de datos ligera basada en archivos.

No requiere servidor.

Una base SQLite suele ser un archivo local:

```text
app.db
database.sqlite
data.db
```

`aiosqlite` permite interactuar con ese archivo desde aplicaciones asíncronas.

## Idea central

La idea principal de `aiosqlite` es abrir una conexión asíncrona con SQLite y ejecutar operaciones usando `await`.

Ejemplo mínimo:

```python
import asyncio

import aiosqlite


async def main():
    async with aiosqlite.connect("app.db") as database:
        async with database.execute("SELECT 1") as cursor:
            row = await cursor.fetchone()

            print(row)


asyncio.run(main())
```

## Cuándo usar `aiosqlite`

Conviene usar `aiosqlite` cuando se necesita:

```text
usar SQLite en una aplicación async
trabajar con FastAPI usando endpoints async
ejecutar consultas sin bloquear directamente el flujo async
crear herramientas locales con SQLite y asyncio
usar una base de datos liviana en archivo
prototipar aplicaciones pequeñas
manejar datos locales con una interfaz async
```

## Cuándo no usar `aiosqlite`

No siempre conviene usar `aiosqlite` cuando se necesita:

```text
alta concurrencia de escritura
base central compartida por muchos usuarios
consultas complejas de producción multiusuario
servidor SQL dedicado
replicación o administración avanzada de base de datos
modelo relacional empresarial grande
```

Para esos casos pueden corresponder:

```text
PostgreSQL
MySQL
SQL Server
Oracle Database
```

Para un script síncrono simple, puede bastar con el módulo estándar:

```python
sqlite3
```

## Instalación

Instalación básica:

```bash
python -m pip install aiosqlite
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
aiosqlite==0.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import aiosqlite

print(aiosqlite.__version__)
```

## Diferencia frente a `sqlite3`

## `sqlite3`

Es síncrono y forma parte de la biblioteca estándar.

```python
import sqlite3

connection = sqlite3.connect("app.db")
cursor = connection.execute("SELECT 1")
row = cursor.fetchone()
connection.close()
```

## `aiosqlite`

Es asíncrono y debe instalarse aparte.

```python
import aiosqlite

async with aiosqlite.connect("app.db") as database:
    async with database.execute("SELECT 1") as cursor:
        row = await cursor.fetchone()
```

## Regla práctica

Para scripts simples, `sqlite3` suele ser suficiente.

Para aplicaciones asíncronas, `aiosqlite` puede encajar mejor.

## Conexión básica

```python
import aiosqlite

database = await aiosqlite.connect("app.db")
```

Al finalizar, debe cerrarse:

```python
await database.close()
```

Sin embargo, suele preferirse usar context manager.

## Conexión con context manager

```python
async with aiosqlite.connect("app.db") as database:
    ...
```

Este patrón ayuda a cerrar la conexión automáticamente al salir del bloque.

## Base en memoria

SQLite también puede trabajar en memoria.

```python
async with aiosqlite.connect(":memory:") as database:
    ...
```

Una base en memoria existe solo durante la ejecución del programa.

## Crear tabla

```python
import asyncio

import aiosqlite


async def main():
    async with aiosqlite.connect("app.db") as database:
        await database.execute("""
            CREATE TABLE IF NOT EXISTS products (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                name TEXT NOT NULL UNIQUE,
                price REAL NOT NULL,
                stock INTEGER NOT NULL DEFAULT 0,
                is_active INTEGER NOT NULL DEFAULT 1
            )
        """)

        await database.commit()


asyncio.run(main())
```

## `execute()`

`execute()` permite ejecutar una sentencia SQL.

```python
await database.execute("SELECT 1")
```

Para consultas que devuelven datos, suele usarse con cursor:

```python
async with database.execute("SELECT 1") as cursor:
    row = await cursor.fetchone()
```

## `commit()`

`commit()` confirma los cambios.

```python
await database.commit()
```

Debe usarse después de operaciones como:

```text
INSERT
UPDATE
DELETE
CREATE TABLE
DROP TABLE
ALTER TABLE
```

## `rollback()`

`rollback()` revierte cambios pendientes.

```python
await database.rollback()
```

Es útil cuando ocurre un error durante una transacción.

## Insertar datos

```python
async with aiosqlite.connect("app.db") as database:
    await database.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        ("Laptop", 3500, 5)
    )

    await database.commit()
```

## Parámetros SQL

En `aiosqlite`, los parámetros suelen usarse con `?`.

```python
await database.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = ?
    """,
    (product_id,)
)
```

No se deben concatenar valores dentro del SQL.

## Evitar concatenar SQL

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"
await database.execute(query)
```

Más seguro:

```python
await database.execute(
    """
    SELECT *
    FROM products
    WHERE name = ?
    """,
    (name,)
)
```

Los valores externos deben pasarse como parámetros.

## Tupla de un solo parámetro

Cuando se pasa un solo parámetro, debe usarse una tupla de un elemento.

Correcto:

```python
(product_id,)
```

Incorrecto:

```python
(product_id)
```

La coma es necesaria.

## Insertar varios registros

```python
products = [
    ("Laptop", 3500, 5),
    ("Mouse", 80, 20),
    ("Teclado", 150, 10),
]

async with aiosqlite.connect("app.db") as database:
    await database.executemany(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        products
    )

    await database.commit()
```

## `executemany()`

`executemany()` ejecuta la misma sentencia para varios conjuntos de valores.

Es útil para cargas pequeñas o medianas de registros.

```python
await database.executemany(sql, values)
```

## Consultar un registro

```python
async with aiosqlite.connect("app.db") as database:
    async with database.execute(
        """
        SELECT id, name, price, stock
        FROM products
        WHERE id = ?
        """,
        (1,)
    ) as cursor:
        row = await cursor.fetchone()

        print(row)
```

## `fetchone()`

`fetchone()` devuelve una fila o `None`.

```python
row = await cursor.fetchone()
```

## Consultar todos los registros

```python
async with aiosqlite.connect("app.db") as database:
    async with database.execute("""
        SELECT id, name, price, stock
        FROM products
        ORDER BY name
    """) as cursor:
        rows = await cursor.fetchall()

        for row in rows:
            print(row)
```

## `fetchall()`

`fetchall()` devuelve todas las filas pendientes.

```python
rows = await cursor.fetchall()
```

Debe usarse con cuidado si la consulta puede devolver demasiadas filas.

## `fetchmany()`

```python
rows = await cursor.fetchmany(10)
```

Permite leer una cantidad limitada de filas.

## Iterar sobre cursor

```python
async with aiosqlite.connect("app.db") as database:
    async with database.execute("""
        SELECT id, name, price
        FROM products
        ORDER BY id
    """) as cursor:
        async for row in cursor:
            print(row)
```

Este patrón evita cargar todos los resultados de golpe.

## Actualizar datos

```python
async with aiosqlite.connect("app.db") as database:
    await database.execute(
        """
        UPDATE products
        SET price = ?, stock = ?
        WHERE id = ?
        """,
        (3600, 4, 1)
    )

    await database.commit()
```

## Eliminar datos

```python
async with aiosqlite.connect("app.db") as database:
    await database.execute(
        """
        DELETE FROM products
        WHERE id = ?
        """,
        (1,)
    )

    await database.commit()
```

## Eliminación lógica

En aplicaciones reales, muchas veces conviene marcar registros como inactivos.

```python
async with aiosqlite.connect("app.db") as database:
    await database.execute(
        """
        UPDATE products
        SET is_active = 0
        WHERE id = ?
        """,
        (product_id,)
    )

    await database.commit()
```

Consulta de activos:

```python
async with database.execute("""
    SELECT id, name, price, stock
    FROM products
    WHERE is_active = 1
""") as cursor:
    rows = await cursor.fetchall()
```

## Obtener el id insertado

```python
async with aiosqlite.connect("app.db") as database:
    cursor = await database.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        ("Laptop", 3500, 5)
    )

    await database.commit()

    print(cursor.lastrowid)
```

## `rowcount`

`rowcount` indica cuántas filas fueron afectadas en operaciones como `UPDATE` o `DELETE`.

```python
cursor = await database.execute(
    """
    UPDATE products
    SET stock = ?
    WHERE id = ?
    """,
    (10, 1)
)

await database.commit()

print(cursor.rowcount)
```

## Filas como tuplas

Por defecto, las filas pueden recibirse como tuplas.

```text
(1, "Laptop", 3500.0, 5)
```

Acceso por posición:

```python
print(row[0])
print(row[1])
```

## Filas como diccionarios

Puede configurarse `row_factory`.

```python
import aiosqlite


async with aiosqlite.connect("app.db") as database:
    database.row_factory = aiosqlite.Row

    async with database.execute("""
        SELECT id, name, price
        FROM products
    """) as cursor:
        row = await cursor.fetchone()

        print(row["name"])
```

## Convertir fila a diccionario

```python
data = dict(row)
```

Ejemplo:

```python
database.row_factory = aiosqlite.Row

async with database.execute("""
    SELECT id, name, price
    FROM products
""") as cursor:
    rows = await cursor.fetchall()

    products = [
        dict(row)
        for row in rows
    ]
```

## Transacciones

Una transacción agrupa operaciones que deben confirmarse juntas.

```python
async with aiosqlite.connect("app.db") as database:
    try:
        await database.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (?, ?, ?)
            """,
            ("Laptop", 3500, 5)
        )

        await database.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (?, ?, ?)
            """,
            ("Mouse", 80, 20)
        )

        await database.commit()
    except Exception:
        await database.rollback()
        raise
```

## Transacción explícita

```python
async with aiosqlite.connect("app.db") as database:
    try:
        await database.execute("BEGIN")

        await database.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (?, ?, ?)
            """,
            ("Laptop", 3500, 5)
        )

        await database.execute("COMMIT")
    except Exception:
        await database.execute("ROLLBACK")
        raise
```

En la mayoría de casos, `commit()` y `rollback()` resultan suficientes.

## Configurar pragmas

SQLite permite configurar opciones mediante `PRAGMA`.

Ejemplo para activar claves foráneas:

```python
async with aiosqlite.connect("app.db") as database:
    await database.execute("PRAGMA foreign_keys = ON")
```

También puede usarse WAL:

```python
await database.execute("PRAGMA journal_mode = WAL")
```

## Foreign keys en SQLite

SQLite requiere activar claves foráneas por conexión.

```python
await database.execute("PRAGMA foreign_keys = ON")
```

Esto es importante si se usan relaciones con restricciones `FOREIGN KEY`.

## WAL mode

WAL significa Write-Ahead Logging.

Puede mejorar ciertos patrones de concurrencia de lectura y escritura.

```python
await database.execute("PRAGMA journal_mode = WAL")
```

Debe evaluarse según el caso de uso.

## Función para abrir conexión

```python
import aiosqlite


async def get_database():
    database = await aiosqlite.connect("app.db")
    database.row_factory = aiosqlite.Row

    await database.execute("PRAGMA foreign_keys = ON")

    return database
```

Uso:

```python
database = await get_database()

try:
    ...
finally:
    await database.close()
```

## Funciones reutilizables

## Crear tabla

```python
async def create_tables(database):
    await database.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE,
            price REAL NOT NULL,
            stock INTEGER NOT NULL DEFAULT 0,
            is_active INTEGER NOT NULL DEFAULT 1
        )
    """)

    await database.commit()
```

## Crear producto

```python
async def create_product(database, name, price, stock=0):
    cursor = await database.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        (name, price, stock)
    )

    await database.commit()

    return cursor.lastrowid
```

## Listar productos

```python
async def list_products(database):
    database.row_factory = aiosqlite.Row

    async with database.execute("""
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE is_active = 1
        ORDER BY name
    """) as cursor:
        rows = await cursor.fetchall()

    return [
        dict(row)
        for row in rows
    ]
```

## Obtener producto por id

```python
async def get_product(database, product_id):
    database.row_factory = aiosqlite.Row

    async with database.execute(
        """
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE id = ?
        """,
        (product_id,)
    ) as cursor:
        row = await cursor.fetchone()

    if row is None:
        return None

    return dict(row)
```

## Actualizar stock

```python
async def update_stock(database, product_id, stock):
    cursor = await database.execute(
        """
        UPDATE products
        SET stock = ?
        WHERE id = ?
        """,
        (stock, product_id)
    )

    await database.commit()

    return cursor.rowcount
```

## Desactivar producto

```python
async def deactivate_product(database, product_id):
    cursor = await database.execute(
        """
        UPDATE products
        SET is_active = 0
        WHERE id = ?
        """,
        (product_id,)
    )

    await database.commit()

    return cursor.rowcount
```

## Manejo de errores

`aiosqlite` expone errores compatibles con SQLite.

```python
import aiosqlite


try:
    await database.execute(
        """
        INSERT INTO products (name, price)
        VALUES (?, ?)
        """,
        ("Laptop", 3500)
    )

    await database.commit()
except aiosqlite.IntegrityError:
    await database.rollback()
    print("No se pudo insertar el registro por una restricción de integridad")
```

## Errores frecuentes

```text
aiosqlite.Error
aiosqlite.IntegrityError
aiosqlite.OperationalError
aiosqlite.ProgrammingError
```

## Error de unicidad

```python
try:
    await create_product(database, "Laptop", 3500, stock=5)
except aiosqlite.IntegrityError:
    print("El producto ya existe")
```

Debe existir una restricción `UNIQUE` para que este error sea relevante.

## Uso con FastAPI

```python
from contextlib import asynccontextmanager

import aiosqlite
from fastapi import FastAPI, HTTPException


async def create_tables(database):
    await database.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE,
            price REAL NOT NULL,
            stock INTEGER NOT NULL DEFAULT 0,
            is_active INTEGER NOT NULL DEFAULT 1
        )
    """)

    await database.commit()


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.database = await aiosqlite.connect("app.db")
    app.state.database.row_factory = aiosqlite.Row

    await app.state.database.execute("PRAGMA foreign_keys = ON")
    await create_tables(app.state.database)

    yield

    await app.state.database.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    async with app.state.database.execute("""
        SELECT id, name, price, stock
        FROM products
        WHERE is_active = 1
        ORDER BY name
    """) as cursor:
        rows = await cursor.fetchall()

    return [
        dict(row)
        for row in rows
    ]


@app.get("/products/{product_id}")
async def read_product(product_id: int):
    async with app.state.database.execute(
        """
        SELECT id, name, price, stock
        FROM products
        WHERE id = ? AND is_active = 1
        """,
        (product_id,)
    ) as cursor:
        row = await cursor.fetchone()

    if row is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return dict(row)
```

## Uso con scripts async

```python
import asyncio

import aiosqlite


async def main():
    async with aiosqlite.connect("app.db") as database:
        database.row_factory = aiosqlite.Row

        await create_tables(database)

        try:
            await create_product(database, "Laptop", 3500, stock=5)
            await create_product(database, "Mouse", 80, stock=20)
        except aiosqlite.IntegrityError:
            pass

        products = await list_products(database)

        for product in products:
            print(product["id"], product["name"], product["price"])


asyncio.run(main())
```

## Uso con pandas

`aiosqlite` no está pensado principalmente para análisis tabular, pero puede alimentar DataFrames.

```python
import pandas as pd


async with aiosqlite.connect("app.db") as database:
    database.row_factory = aiosqlite.Row

    async with database.execute("""
        SELECT id, name, price, stock
        FROM products
    """) as cursor:
        rows = await cursor.fetchall()

df = pd.DataFrame(
    [dict(row) for row in rows]
)

print(df.head())
```

Para análisis intensivo sobre archivos o tablas locales puede ser más conveniente usar:

```text
pandas
polars
duckdb
```

## Uso con SQLAlchemy async

SQLAlchemy puede usar `aiosqlite` como driver asíncrono para SQLite.

Ejemplo conceptual:

```python
from sqlalchemy.ext.asyncio import create_async_engine

engine = create_async_engine(
    "sqlite+aiosqlite:///app.db"
)
```

En ese caso:

```text
SQLAlchemy -> ORM / Core asíncrono
aiosqlite  -> driver SQLite asíncrono
SQLite     -> base de datos en archivo
```

## Organización recomendada

```text
app/
├─ database.py
├─ repositories.py
├─ services.py
└─ main.py
```

## `database.py`

```python
import aiosqlite


async def create_database_connection():
    database = await aiosqlite.connect("app.db")
    database.row_factory = aiosqlite.Row

    await database.execute("PRAGMA foreign_keys = ON")

    return database


async def close_database_connection(database):
    await database.close()
```

## `repositories.py`

```python
async def list_products(database):
    async with database.execute("""
        SELECT id, name, price, stock
        FROM products
        WHERE is_active = 1
        ORDER BY name
    """) as cursor:
        rows = await cursor.fetchall()

    return [
        dict(row)
        for row in rows
    ]


async def get_product(database, product_id):
    async with database.execute(
        """
        SELECT id, name, price, stock
        FROM products
        WHERE id = ? AND is_active = 1
        """,
        (product_id,)
    ) as cursor:
        row = await cursor.fetchone()

    if row is None:
        return None

    return dict(row)


async def create_product(database, name, price, stock=0):
    cursor = await database.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        (name, price, stock)
    )

    await database.commit()

    return cursor.lastrowid
```

## Separación por responsabilidades

```text
database.py      -> conexión y configuración de SQLite
repositories.py  -> consultas SQL
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Migraciones

`aiosqlite` no es una herramienta de migraciones.

Para cambios simples pueden usarse scripts SQL controlados.

Para proyectos más estructurados puede evaluarse:

```text
alembic
SQLAlchemy
herramientas propias de migración
```

En SQLite, muchos proyectos pequeños usan scripts versionados con sentencias `CREATE TABLE` y `ALTER TABLE`.

## Concurrencia en SQLite

SQLite funciona muy bien para aplicaciones pequeñas y locales.

Sin embargo, no debe asumirse que soporta la misma concurrencia de escritura que un servidor como PostgreSQL.

Regla práctica:

```text
muchas lecturas y pocas escrituras -> SQLite puede ser suficiente
muchas escrituras concurrentes     -> evaluar PostgreSQL u otra base servidor
```

## Errores comunes

## Olvidar `await`

Problemático:

```python
database.execute("SELECT 1")
```

Correcto:

```python
await database.execute("SELECT 1")
```

## Usar `sqlite3` esperando comportamiento async

`sqlite3` es síncrono.

Para código async, usar:

```python
aiosqlite
```

## No hacer `commit()`

Problemático:

```python
await database.execute(
    """
    INSERT INTO products (name, price)
    VALUES (?, ?)
    """,
    ("Laptop", 3500)
)
```

sin:

```python
await database.commit()
```

Los cambios pueden no quedar guardados.

## No cerrar la conexión

Más seguro:

```python
async with aiosqlite.connect("app.db") as database:
    ...
```

o:

```python
await database.close()
```

## Concatenar valores dentro del SQL

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"
```

Correcto:

```python
await database.execute(
    "SELECT * FROM products WHERE name = ?",
    (name,)
)
```

## Usar placeholders de otro driver

Problemático:

```python
await database.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

Correcto:

```python
await database.execute(
    "SELECT * FROM products WHERE id = ?",
    (product_id,)
)
```

## Olvidar la coma en una tupla de un parámetro

Problemático:

```python
(product_id)
```

Correcto:

```python
(product_id,)
```

## Esperar diccionarios sin configurar `row_factory`

Problemático:

```python
print(row["name"])
```

si la fila es una tupla.

Solución:

```python
database.row_factory = aiosqlite.Row
```

## Usar `fetchall()` con resultados muy grandes

Problemático:

```python
rows = await cursor.fetchall()
```

si la tabla puede tener millones de registros.

Mejor:

```python
async for row in cursor:
    ...
```

o paginar resultados.

## Usar SQLite como servidor multiusuario pesado

SQLite es excelente para muchos casos locales, pero no reemplaza automáticamente a PostgreSQL o MySQL en escenarios de alta concurrencia.

## Buenas prácticas

## Usar context managers

```python
async with aiosqlite.connect("app.db") as database:
    ...
```

## Usar parámetros SQL

```python
WHERE id = ?
```

## Hacer `commit()` después de cambios

```python
await database.commit()
```

## Usar `rollback()` ante errores

```python
await database.rollback()
```

## Configurar `row_factory` si se necesitan diccionarios

```python
database.row_factory = aiosqlite.Row
```

## Activar foreign keys si se usan relaciones

```python
await database.execute("PRAGMA foreign_keys = ON")
```

## Separar SQL en funciones o repositorios

```text
repositories.py
```

## Usar paginación para resultados grandes

```sql
LIMIT ? OFFSET ?
```

## Evaluar PostgreSQL si la concurrencia crece

SQLite es muy útil, pero tiene un perfil distinto al de una base servidor.

## Ejemplo integrado

```python
import asyncio

import aiosqlite


async def create_tables(database):
    await database.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE,
            price REAL NOT NULL,
            stock INTEGER NOT NULL DEFAULT 0,
            is_active INTEGER NOT NULL DEFAULT 1
        )
    """)

    await database.commit()


async def create_product(database, name, price, stock=0):
    cursor = await database.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        (name, price, stock)
    )

    await database.commit()

    return cursor.lastrowid


async def list_products(database):
    async with database.execute("""
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE is_active = 1
        ORDER BY name
    """) as cursor:
        rows = await cursor.fetchall()

    return [
        dict(row)
        for row in rows
    ]


async def get_product(database, product_id):
    async with database.execute(
        """
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE id = ?
        """,
        (product_id,)
    ) as cursor:
        row = await cursor.fetchone()

    if row is None:
        return None

    return dict(row)


async def update_stock(database, product_id, stock):
    cursor = await database.execute(
        """
        UPDATE products
        SET stock = ?
        WHERE id = ?
        """,
        (stock, product_id)
    )

    await database.commit()

    return cursor.rowcount


async def deactivate_product(database, product_id):
    cursor = await database.execute(
        """
        UPDATE products
        SET is_active = 0
        WHERE id = ?
        """,
        (product_id,)
    )

    await database.commit()

    return cursor.rowcount


async def main():
    async with aiosqlite.connect("app.db") as database:
        database.row_factory = aiosqlite.Row

        await database.execute("PRAGMA foreign_keys = ON")

        await create_tables(database)

        try:
            await create_product(database, "Laptop", 3500, stock=5)
            await create_product(database, "Mouse", 80, stock=20)
        except aiosqlite.IntegrityError:
            pass

        products = await list_products(database)

        for product in products:
            print(
                product["id"],
                product["name"],
                product["price"],
                product["stock"]
            )


asyncio.run(main())
```

## Relación con otras librerías

`aiosqlite` se relaciona especialmente con:

```text
sqlite3
sqlite
asyncio
fastapi
sqlalchemy
pandas
python-dotenv
pytest
pytest-asyncio
```

## Relación con `sqlite3`

`sqlite3` es el módulo estándar síncrono.

`aiosqlite` ofrece una interfaz asíncrona inspirada en ese módulo.

## Relación con FastAPI

`aiosqlite` puede usarse en endpoints `async`, especialmente en aplicaciones pequeñas o prototipos.

## Relación con SQLAlchemy

SQLAlchemy puede usar `aiosqlite` como driver asíncrono de SQLite.

```text
sqlite+aiosqlite:///app.db
```

## Relación con pandas

Los resultados de consultas pueden convertirse a DataFrames.

```python
pd.DataFrame([dict(row) for row in rows])
```

## Relación con DuckDB

DuckDB puede ser más cómodo para análisis de archivos y consultas analíticas.

SQLite con `aiosqlite` puede ser más adecuado para almacenamiento local simple dentro de una aplicación async.

## Orden didáctico interno

```text
1. Propósito de aiosqlite
2. Instalación
3. Relación con SQLite y sqlite3
4. Conexión con aiosqlite.connect()
5. execute()
6. Parámetros con ?
7. fetchone(), fetchall(), fetchmany() e iteración async
8. INSERT, UPDATE y DELETE
9. commit() y rollback()
10. row_factory y aiosqlite.Row
11. PRAGMA, foreign_keys y WAL
12. Funciones reutilizables
13. Uso con FastAPI
14. Uso con pandas
15. Uso con SQLAlchemy async
16. Migraciones
17. Concurrencia en SQLite
18. Errores comunes
19. Buenas prácticas
```