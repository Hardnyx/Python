# `asyncpg`

## Propósito

`asyncpg` es una librería externa para conectar Python con PostgreSQL usando programación asíncrona.

Se utiliza para ejecutar consultas SQL, leer datos, insertar registros, actualizar información, eliminar filas, manejar transacciones y trabajar con pools de conexiones en aplicaciones basadas en `asyncio`.

Es especialmente útil en backends asíncronos, microservicios, APIs con alta concurrencia y aplicaciones donde no se quiere bloquear el event loop mientras se espera una respuesta de PostgreSQL.

## Naturaleza de la librería

`asyncpg` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install asyncpg
```

La importación habitual es:

```python
import asyncpg
```

`asyncpg` es un driver asíncrono para PostgreSQL.

No se usa para conectarse a:

```text
SQLite
MySQL
SQL Server
Oracle
MongoDB
Redis
```

## Relación con PostgreSQL

`asyncpg` está diseñado específicamente para PostgreSQL.

Trabaja directamente con SQL, pero usando una API asíncrona basada en `async` y `await`.

Ejemplo conceptual:

```text
Python async -> asyncpg -> PostgreSQL
```

No es un ORM.

No convierte automáticamente tablas en clases.

No reemplaza a SQLAlchemy ORM.

Su función principal es ejecutar SQL de forma asíncrona contra PostgreSQL.

## Idea central

La idea principal de `asyncpg` es abrir una conexión asíncrona y ejecutar comandos SQL usando `await`.

Ejemplo mínimo:

```python
import asyncio

import asyncpg


async def main():
    connection = await asyncpg.connect(
        user="postgres",
        password="secret",
        database="app",
        host="localhost",
        port=5432
    )

    value = await connection.fetchval("SELECT 1")

    print(value)

    await connection.close()


asyncio.run(main())
```

## Cuándo usar `asyncpg`

Conviene usar `asyncpg` cuando se necesita:

```text
conectarse a PostgreSQL desde código asíncrono
usar FastAPI con endpoints async
ejecutar consultas sin bloquear el event loop
manejar muchas solicitudes concurrentes
usar pools de conexiones asíncronos
trabajar directamente con SQL
aprovechar características específicas de PostgreSQL
crear microservicios asíncronos
```

## Cuándo no usar `asyncpg`

No conviene usar `asyncpg` cuando se necesita:

```text
conectarse a una base que no sea PostgreSQL
trabajar en scripts síncronos simples
evitar escribir SQL manualmente
usar modelos ORM tradicionales
usar Django ORM
hacer análisis de archivos CSV o Excel
```

Para scripts síncronos con PostgreSQL puede bastar:

```text
psycopg
```

Para ORM puede corresponder:

```text
sqlalchemy
django ORM
```

Para análisis tabular puede corresponder:

```text
pandas
polars
duckdb
```

## Instalación

Instalación básica:

```bash
python -m pip install asyncpg
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
asyncpg==0.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import asyncpg

print(asyncpg.__version__)
```

## Diferencia frente a `psycopg`

## `psycopg`

Puede usarse en modo síncrono y también tiene soporte asíncrono.

Suele ser una opción general moderna para PostgreSQL.

## `asyncpg`

Está diseñado específicamente para PostgreSQL con `asyncio`.

Su API no sigue exactamente el estilo DB API clásico.

Usa métodos como:

```python
fetch()
fetchrow()
fetchval()
execute()
executemany()
```

y parámetros con:

```text
$1, $2, $3
```

## Regla práctica

Para un script sencillo y síncrono, `psycopg` puede ser más directo.

Para una API asíncrona con alta concurrencia, `asyncpg` puede ser una opción muy adecuada.

## Cadena de conexión

Se puede conectar usando parámetros separados:

```python
connection = await asyncpg.connect(
    user="postgres",
    password="secret",
    database="app",
    host="localhost",
    port=5432
)
```

También se puede usar una URL:

```python
connection = await asyncpg.connect(
    "postgresql://postgres:secret@localhost:5432/app"
)
```

## Parámetros frecuentes

```text
user      -> usuario de PostgreSQL
password  -> contraseña
database  -> base de datos
host      -> servidor
port      -> puerto, normalmente 5432
```

## Conexión básica

```python
import asyncio

import asyncpg


async def main():
    connection = await asyncpg.connect(
        user="postgres",
        password="secret",
        database="app",
        host="localhost",
        port=5432
    )

    try:
        value = await connection.fetchval("SELECT 1")
        print(value)
    finally:
        await connection.close()


asyncio.run(main())
```

## Cerrar conexión

Una conexión abierta debe cerrarse cuando ya no se usa.

```python
await connection.close()
```

Patrón recomendado:

```python
connection = await asyncpg.connect(...)

try:
    ...
finally:
    await connection.close()
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
connection = await asyncpg.connect(
    "postgresql://postgres:secret@localhost:5432/app"
)
```

Más conveniente:

```python
import os

import asyncpg

database_url = os.getenv("DATABASE_URL")

connection = await asyncpg.connect(database_url)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
DATABASE_URL=postgresql://postgres:secret@localhost:5432/app
```

Código:

```python
import asyncio
import os

import asyncpg
from dotenv import load_dotenv

load_dotenv()


async def main():
    database_url = os.getenv("DATABASE_URL")

    if database_url is None:
        raise RuntimeError("DATABASE_URL no está configurada")

    connection = await asyncpg.connect(database_url)

    try:
        value = await connection.fetchval("SELECT 1")
        print(value)
    finally:
        await connection.close()


asyncio.run(main())
```

## Connection

Una `Connection` representa una conexión abierta contra PostgreSQL.

Desde una conexión se puede:

```text
ejecutar comandos
leer filas
crear transacciones
preparar sentencias
usar cursores
escuchar notificaciones
cerrar la conexión
```

Ejemplo:

```python
connection = await asyncpg.connect(database_url)
```

## `execute()`

`execute()` se usa para ejecutar comandos que no necesitan devolver filas completas.

Ejemplos:

```text
CREATE TABLE
INSERT
UPDATE
DELETE
ALTER TABLE
DROP TABLE
```

Ejemplo:

```python
status = await connection.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id SERIAL PRIMARY KEY,
        name TEXT NOT NULL,
        price NUMERIC(10, 2) NOT NULL,
        stock INTEGER NOT NULL DEFAULT 0
    )
""")

print(status)
```

## Crear tabla

```python
await connection.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id SERIAL PRIMARY KEY,
        name TEXT NOT NULL UNIQUE,
        price NUMERIC(10, 2) NOT NULL,
        stock INTEGER NOT NULL DEFAULT 0,
        is_active BOOLEAN NOT NULL DEFAULT TRUE
    )
""")
```

## Insertar datos

```python
await connection.execute(
    """
    INSERT INTO products (name, price, stock)
    VALUES ($1, $2, $3)
    """,
    "Laptop",
    3500,
    5
)
```

## Parámetros SQL

En `asyncpg`, los parámetros se escriben como:

```text
$1, $2, $3
```

Ejemplo:

```python
await connection.fetchrow(
    """
    SELECT id, name, price
    FROM products
    WHERE id = $1
    """,
    product_id
)
```

No se usan placeholders como:

```text
%s
?
```

## Evitar concatenar SQL

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"
row = await connection.fetchrow(query)
```

Más seguro:

```python
row = await connection.fetchrow(
    """
    SELECT *
    FROM products
    WHERE name = $1
    """,
    name
)
```

Los valores deben pasarse como parámetros.

## `fetch()`

`fetch()` devuelve una lista de filas.

```python
rows = await connection.fetch("""
    SELECT id, name, price, stock
    FROM products
    ORDER BY name
""")

for row in rows:
    print(row)
```

Cada fila es un objeto tipo `Record`.

## `fetchrow()`

`fetchrow()` devuelve una sola fila o `None`.

```python
row = await connection.fetchrow(
    """
    SELECT id, name, price, stock
    FROM products
    WHERE id = $1
    """,
    1
)

print(row)
```

## `fetchval()`

`fetchval()` devuelve un solo valor.

```python
count = await connection.fetchval("""
    SELECT COUNT(*)
    FROM products
""")

print(count)
```

Es útil para consultas que devuelven una celda.

## `executemany()`

`executemany()` permite ejecutar la misma sentencia con múltiples conjuntos de parámetros.

```python
products = [
    ("Laptop", 3500, 5),
    ("Mouse", 80, 20),
    ("Teclado", 150, 10),
]

await connection.executemany(
    """
    INSERT INTO products (name, price, stock)
    VALUES ($1, $2, $3)
    """,
    products
)
```

## `Record`

Las filas devueltas por `asyncpg` son objetos `Record`.

```python
row = await connection.fetchrow("""
    SELECT id, name, price
    FROM products
    LIMIT 1
""")

print(row["name"])
print(row["price"])
```

También se puede acceder por posición:

```python
print(row[0])
```

## Convertir `Record` a diccionario

```python
row = await connection.fetchrow("""
    SELECT id, name, price
    FROM products
    LIMIT 1
""")

if row is not None:
    data = dict(row)
    print(data)
```

## Convertir lista de registros

```python
rows = await connection.fetch("""
    SELECT id, name, price
    FROM products
""")

products = [
    dict(row)
    for row in rows
]

print(products)
```

## Insertar con `RETURNING`

PostgreSQL permite devolver datos después de insertar.

```python
new_id = await connection.fetchval(
    """
    INSERT INTO products (name, price, stock)
    VALUES ($1, $2, $3)
    RETURNING id
    """,
    "Laptop",
    3500,
    5
)

print(new_id)
```

## Actualizar datos

```python
status = await connection.execute(
    """
    UPDATE products
    SET price = $1
    WHERE id = $2
    """,
    3600,
    1
)

print(status)
```

## Actualizar y devolver fila

```python
row = await connection.fetchrow(
    """
    UPDATE products
    SET stock = $1
    WHERE id = $2
    RETURNING id, name, price, stock
    """,
    10,
    1
)

print(dict(row))
```

## Eliminar datos

```python
status = await connection.execute(
    """
    DELETE FROM products
    WHERE id = $1
    """,
    1
)

print(status)
```

## Eliminación lógica

En aplicaciones reales, a veces se prefiere marcar registros como inactivos.

```python
await connection.execute(
    """
    UPDATE products
    SET is_active = FALSE
    WHERE id = $1
    """,
    product_id
)
```

## Transacciones

`asyncpg` permite usar transacciones con context manager asíncrono.

```python
async with connection.transaction():
    await connection.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES ($1, $2, $3)
        """,
        "Laptop",
        3500,
        5
    )
```

Si ocurre un error dentro del bloque, la transacción se revierte.

Si el bloque termina correctamente, se confirma.

## Transacción con varias operaciones

```python
async with connection.transaction():
    product_id = await connection.fetchval(
        """
        INSERT INTO products (name, price, stock)
        VALUES ($1, $2, $3)
        RETURNING id
        """,
        "Laptop",
        3500,
        5
    )

    await connection.execute(
        """
        INSERT INTO product_logs (product_id, message)
        VALUES ($1, $2)
        """,
        product_id,
        "Producto creado"
    )
```

## Transacción manual

```python
transaction = connection.transaction()

await transaction.start()

try:
    await connection.execute(...)
except Exception:
    await transaction.rollback()
    raise
else:
    await transaction.commit()
```

El patrón con `async with connection.transaction()` suele ser más claro.

## Pools de conexiones

En aplicaciones web no conviene abrir y cerrar una conexión por cada consulta.

Un pool permite reutilizar conexiones.

```python
pool = await asyncpg.create_pool(
    dsn=database_url,
    min_size=1,
    max_size=10
)
```

## Uso básico de pool

```python
pool = await asyncpg.create_pool(database_url)

try:
    async with pool.acquire() as connection:
        value = await connection.fetchval("SELECT 1")
        print(value)
finally:
    await pool.close()
```

## Pool con context manager

```python
async with asyncpg.create_pool(database_url) as pool:
    async with pool.acquire() as connection:
        value = await connection.fetchval("SELECT 1")
        print(value)
```

## Crear pool en una aplicación

```python
import asyncpg


async def create_pool(database_url):
    return await asyncpg.create_pool(
        dsn=database_url,
        min_size=1,
        max_size=10
    )
```

## Usar pool para consultas

```python
async def list_products(pool):
    async with pool.acquire() as connection:
        rows = await connection.fetch("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

    return [
        dict(row)
        for row in rows
    ]
```

## Prepared statements

`asyncpg` permite preparar sentencias.

```python
statement = await connection.prepare("""
    SELECT id, name, price
    FROM products
    WHERE id = $1
""")

row = await statement.fetchrow(1)

print(row)
```

Esto puede ser útil cuando se ejecuta la misma consulta muchas veces.

## Prepared statement con valor único

```python
statement = await connection.prepare("""
    SELECT COUNT(*)
    FROM products
    WHERE is_active = $1
""")

count = await statement.fetchval(True)

print(count)
```

## Cursores

Los cursores permiten recorrer resultados grandes de forma progresiva.

```python
async with connection.transaction():
    async for row in connection.cursor("""
        SELECT id, name, price
        FROM products
        ORDER BY id
    """):
        print(row["id"], row["name"])
```

Normalmente se usan dentro de una transacción.

## Uso con listas grandes

Para resultados pequeños o medianos:

```python
rows = await connection.fetch(...)
```

Para resultados grandes:

```python
async for row in connection.cursor(...):
    ...
```

Esto evita cargar todo de golpe en memoria.

## Timeouts

Algunas operaciones permiten definir timeout.

```python
row = await connection.fetchrow(
    """
    SELECT pg_sleep(1), 1
    """,
    timeout=5
)
```

El timeout ayuda a evitar operaciones que quedan esperando demasiado tiempo.

## Tipos de datos

`asyncpg` convierte muchos tipos entre PostgreSQL y Python.

Ejemplos frecuentes:

```text
PostgreSQL text / varchar -> Python str
PostgreSQL integer        -> Python int
PostgreSQL numeric        -> Decimal
PostgreSQL boolean        -> Python bool
PostgreSQL date           -> datetime.date
PostgreSQL timestamp      -> datetime.datetime
PostgreSQL json / jsonb   -> objeto compatible según configuración
PostgreSQL uuid           -> uuid.UUID
```

## Insertar fechas

```python
from datetime import date

await connection.execute(
    """
    INSERT INTO events (name, event_date)
    VALUES ($1, $2)
    """,
    "Cierre mensual",
    date(2026, 12, 31)
)
```

## Insertar `None`

```python
await connection.execute(
    """
    INSERT INTO products (name, description)
    VALUES ($1, $2)
    """,
    "Laptop",
    None
)
```

`None` se convierte en `NULL`.

## JSON y JSONB

PostgreSQL puede almacenar JSON.

```python
import json

payload = {
    "source": "api",
    "status": "created"
}

await connection.execute(
    """
    INSERT INTO logs (payload)
    VALUES ($1::jsonb)
    """,
    json.dumps(payload)
)
```

Al leer, según configuración y tipo, puede recibirse como texto o como estructura adaptada.

## Manejo de errores

`asyncpg` expone excepciones específicas.

```python
import asyncpg

try:
    await connection.execute("""
        INSERT INTO products (name, price)
        VALUES ($1, $2)
    """, "Laptop", 3500)
except asyncpg.PostgresError as error:
    print("Error de PostgreSQL:", error)
```

## Errores frecuentes

```text
asyncpg.PostgresError
asyncpg.UniqueViolationError
asyncpg.ForeignKeyViolationError
asyncpg.NotNullViolationError
asyncpg.PostgresConnectionError
asyncpg.InvalidCatalogNameError
```

## Error de unicidad

```python
try:
    await connection.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES ($1, $2, $3)
        """,
        "Laptop",
        3500,
        5
    )
except asyncpg.UniqueViolationError:
    print("El producto ya existe")
```

## Error de conexión

```python
try:
    connection = await asyncpg.connect(database_url)
except asyncpg.PostgresConnectionError:
    print("No se pudo conectar con PostgreSQL")
```

## Funciones reutilizables

## Crear tabla

```python
async def create_table(connection):
    await connection.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id SERIAL PRIMARY KEY,
            name TEXT NOT NULL UNIQUE,
            price NUMERIC(10, 2) NOT NULL,
            stock INTEGER NOT NULL DEFAULT 0,
            is_active BOOLEAN NOT NULL DEFAULT TRUE
        )
    """)
```

## Crear producto

```python
async def create_product(connection, name, price, stock=0):
    try:
        product_id = await connection.fetchval(
            """
            INSERT INTO products (name, price, stock)
            VALUES ($1, $2, $3)
            RETURNING id
            """,
            name,
            price,
            stock
        )

        return product_id
    except asyncpg.UniqueViolationError:
        raise ValueError("El producto ya existe")
```

## Listar productos

```python
async def list_products(connection):
    rows = await connection.fetch("""
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE is_active = TRUE
        ORDER BY name
    """)

    return [
        dict(row)
        for row in rows
    ]
```

## Obtener producto por id

```python
async def get_product(connection, product_id):
    row = await connection.fetchrow(
        """
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE id = $1
        """,
        product_id
    )

    if row is None:
        return None

    return dict(row)
```

## Actualizar stock

```python
async def update_stock(connection, product_id, stock):
    row = await connection.fetchrow(
        """
        UPDATE products
        SET stock = $1
        WHERE id = $2
        RETURNING id, name, price, stock
        """,
        stock,
        product_id
    )

    if row is None:
        return None

    return dict(row)
```

## Uso con FastAPI

`asyncpg` encaja bien con FastAPI cuando se usan endpoints asíncronos.

Ejemplo conceptual:

```python
import os

import asyncpg
from fastapi import FastAPI, HTTPException

app = FastAPI()

pool = None


@app.on_event("startup")
async def startup():
    global pool

    database_url = os.getenv("DATABASE_URL")

    pool = await asyncpg.create_pool(
        dsn=database_url,
        min_size=1,
        max_size=10
    )


@app.on_event("shutdown")
async def shutdown():
    await pool.close()


@app.get("/products")
async def read_products():
    async with pool.acquire() as connection:
        rows = await connection.fetch("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

    return [
        dict(row)
        for row in rows
    ]


@app.get("/products/{product_id}")
async def read_product(product_id: int):
    async with pool.acquire() as connection:
        row = await connection.fetchrow(
            """
            SELECT id, name, price, stock
            FROM products
            WHERE id = $1
            """,
            product_id
        )

    if row is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return dict(row)
```

## Uso con FastAPI lifespan

En proyectos nuevos puede usarse un lifespan manager.

```python
import os
from contextlib import asynccontextmanager

import asyncpg
from fastapi import FastAPI


@asynccontextmanager
async def lifespan(app: FastAPI):
    database_url = os.getenv("DATABASE_URL")

    app.state.pool = await asyncpg.create_pool(
        dsn=database_url,
        min_size=1,
        max_size=10
    )

    yield

    await app.state.pool.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    async with app.state.pool.acquire() as connection:
        rows = await connection.fetch("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

    return [
        dict(row)
        for row in rows
    ]
```

## Uso con scripts

```python
import asyncio
import os

import asyncpg
from dotenv import load_dotenv

load_dotenv()


async def main():
    database_url = os.getenv("DATABASE_URL")

    connection = await asyncpg.connect(database_url)

    try:
        await create_table(connection)

        await create_product(
            connection,
            name="Laptop",
            price=3500,
            stock=5
        )

        products = await list_products(connection)

        for product in products:
            print(product)

    finally:
        await connection.close()


asyncio.run(main())
```

## Uso con pandas

`asyncpg` devuelve registros. Para convertirlos a `DataFrame`, primero se transforman a diccionarios.

```python
import pandas as pd

rows = await connection.fetch("""
    SELECT id, name, price, stock
    FROM products
""")

df = pd.DataFrame(
    [dict(row) for row in rows]
)

print(df.head())
```

Para análisis grandes, también puede evaluarse `pandas` con SQLAlchemy o exportaciones específicas según el caso.

## Uso con SQLAlchemy

SQLAlchemy puede usar drivers asíncronos, incluido `asyncpg`, mediante su motor asíncrono.

Ejemplo conceptual:

```python
from sqlalchemy.ext.asyncio import create_async_engine

engine = create_async_engine(
    "postgresql+asyncpg://user:password@localhost:5432/app"
)
```

En ese caso:

```text
SQLAlchemy -> ORM / Core asíncrono
asyncpg    -> driver PostgreSQL asíncrono
```

## Uso con Alembic

Alembic puede coexistir con proyectos que usan `asyncpg`.

En muchos casos:

```text
aplicación -> asyncpg o SQLAlchemy async
migraciones -> Alembic
base de datos -> PostgreSQL
```

Si se usa SQLAlchemy para definir modelos, Alembic puede gestionar migraciones.

Si se usa solo SQL manual con `asyncpg`, las migraciones pueden escribirse manualmente con Alembic o con scripts SQL.

## LISTEN y NOTIFY

PostgreSQL permite enviar notificaciones entre sesiones con `LISTEN` y `NOTIFY`.

`asyncpg` puede trabajar con listeners.

Ejemplo conceptual:

```python
async def handle_notification(connection, pid, channel, payload):
    print(channel, payload)


await connection.add_listener(
    "events",
    handle_notification
)
```

Luego, desde PostgreSQL:

```sql
NOTIFY events, 'nuevo evento';
```

Este uso pertenece a patrones más avanzados.

## COPY

PostgreSQL permite carga y descarga eficiente con `COPY`.

`asyncpg` tiene métodos para operaciones de copia.

Ejemplo conceptual:

```text
copy_records_to_table()
copy_from_table()
copy_to_table()
```

Para una documentación inicial conviene dominar primero:

```text
connect()
fetch()
fetchrow()
fetchval()
execute()
transactions
pools
```

## Estructura recomendada

Para una aplicación pequeña:

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

import asyncpg
from dotenv import load_dotenv

load_dotenv()


async def create_pool():
    database_url = os.getenv("DATABASE_URL")

    if database_url is None:
        raise RuntimeError("DATABASE_URL no está configurada")

    return await asyncpg.create_pool(
        dsn=database_url,
        min_size=1,
        max_size=10
    )
```

## `repositories.py`

```python
async def list_products(pool):
    async with pool.acquire() as connection:
        rows = await connection.fetch("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

    return [
        dict(row)
        for row in rows
    ]


async def get_product(pool, product_id):
    async with pool.acquire() as connection:
        row = await connection.fetchrow(
            """
            SELECT id, name, price, stock
            FROM products
            WHERE id = $1
            """,
            product_id
        )

    if row is None:
        return None

    return dict(row)


async def create_product(pool, name, price, stock=0):
    async with pool.acquire() as connection:
        try:
            product_id = await connection.fetchval(
                """
                INSERT INTO products (name, price, stock)
                VALUES ($1, $2, $3)
                RETURNING id
                """,
                name,
                price,
                stock
            )

            return product_id
        except asyncpg.UniqueViolationError:
            raise ValueError("El producto ya existe")
```

## Separación por responsabilidades

```text
database.py      -> pool de conexiones
repositories.py  -> consultas SQL
services.py      -> reglas de negocio
routers.py       -> endpoints HTTP
schemas.py       -> validación de entrada y salida
```

## Errores comunes

## No usar `await`

Problemático:

```python
connection = asyncpg.connect(database_url)
```

Correcto:

```python
connection = await asyncpg.connect(database_url)
```

## Usar placeholders incorrectos

Problemático:

```python
await connection.fetchrow(
    "SELECT * FROM products WHERE id = %s",
    product_id
)
```

Correcto:

```python
await connection.fetchrow(
    "SELECT * FROM products WHERE id = $1",
    product_id
)
```

## Concatenar valores dentro del SQL

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"
row = await connection.fetchrow(query)
```

Correcto:

```python
row = await connection.fetchrow(
    "SELECT * FROM products WHERE name = $1",
    name
)
```

## Abrir una conexión por cada solicitud

En aplicaciones web, esto puede ser ineficiente.

Mejor usar:

```python
asyncpg.create_pool()
```

## No cerrar conexiones o pools

Conexión:

```python
await connection.close()
```

Pool:

```python
await pool.close()
```

## Usar `asyncpg` en código síncrono sin entender `asyncio`

`asyncpg` requiere `async` y `await`.

Para scripts síncronos simples, puede ser más directo usar `psycopg`.

## Convertir resultados grandes a lista sin control

```python
rows = await connection.fetch("SELECT * FROM large_table")
```

puede consumir mucha memoria si el resultado es demasiado grande.

Para resultados grandes, evaluar cursores o paginación.

## No manejar errores de integridad

Conviene capturar errores específicos como:

```python
asyncpg.UniqueViolationError
```

## Guardar credenciales en el código

Problemático:

```python
database_url = "postgresql://postgres:secret@localhost:5432/app"
```

Mejor:

```python
database_url = os.getenv("DATABASE_URL")
```

## Reutilizar prepared statements después de liberar conexión al pool

Los prepared statements asociados a una conexión no deben tratarse como objetos globales si la conexión vuelve al pool.

Conviene mantenerlos dentro del ciclo de vida de la conexión correspondiente.

## Buenas prácticas

## Usar variables de entorno

```python
DATABASE_URL
```

## Usar pools en aplicaciones web

```python
pool = await asyncpg.create_pool(...)
```

## Usar parámetros con `$1`, `$2`, `$3`

```python
WHERE id = $1
```

## Convertir `Record` a diccionario al responder APIs

```python
dict(row)
```

## Usar transacciones para operaciones relacionadas

```python
async with connection.transaction():
    ...
```

## Cerrar conexiones y pools

```python
await connection.close()
await pool.close()
```

## Separar SQL en repositories

```text
repositories.py
```

## Usar cursores o paginación para resultados grandes

```python
connection.cursor(...)
```

## Manejar errores específicos

```python
asyncpg.UniqueViolationError
asyncpg.PostgresError
```

## No mezclar lógica SQL con lógica HTTP

Separar:

```text
router
service
repository
database
```

## Ejemplo integrado

```python
import asyncio
import os
from decimal import Decimal

import asyncpg
from dotenv import load_dotenv

load_dotenv()


async def create_pool():
    database_url = os.getenv("DATABASE_URL")

    if database_url is None:
        raise RuntimeError("DATABASE_URL no está configurada")

    return await asyncpg.create_pool(
        dsn=database_url,
        min_size=1,
        max_size=10
    )


async def create_table(pool):
    async with pool.acquire() as connection:
        await connection.execute("""
            CREATE TABLE IF NOT EXISTS products (
                id SERIAL PRIMARY KEY,
                name TEXT NOT NULL UNIQUE,
                price NUMERIC(10, 2) NOT NULL,
                stock INTEGER NOT NULL DEFAULT 0,
                is_active BOOLEAN NOT NULL DEFAULT TRUE
            )
        """)


async def create_product(pool, name, price, stock=0):
    async with pool.acquire() as connection:
        try:
            product_id = await connection.fetchval(
                """
                INSERT INTO products (name, price, stock)
                VALUES ($1, $2, $3)
                RETURNING id
                """,
                name,
                Decimal(str(price)),
                stock
            )

            return product_id
        except asyncpg.UniqueViolationError:
            raise ValueError("El producto ya existe")


async def list_products(pool):
    async with pool.acquire() as connection:
        rows = await connection.fetch("""
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

    return [
        dict(row)
        for row in rows
    ]


async def update_stock(pool, product_id, stock):
    async with pool.acquire() as connection:
        row = await connection.fetchrow(
            """
            UPDATE products
            SET stock = $1
            WHERE id = $2
            RETURNING id, name, price, stock
            """,
            stock,
            product_id
        )

    if row is None:
        return None

    return dict(row)


async def deactivate_product(pool, product_id):
    async with pool.acquire() as connection:
        row = await connection.fetchrow(
            """
            UPDATE products
            SET is_active = FALSE
            WHERE id = $1
            RETURNING id, name, is_active
            """,
            product_id
        )

    if row is None:
        return None

    return dict(row)


async def main():
    pool = await create_pool()

    try:
        await create_table(pool)

        try:
            await create_product(pool, "Laptop", 3500, stock=5)
            await create_product(pool, "Mouse", 80, stock=20)
        except ValueError:
            pass

        products = await list_products(pool)

        for product in products:
            print(
                product["id"],
                product["name"],
                product["price"],
                product["stock"]
            )

    finally:
        await pool.close()


asyncio.run(main())
```

## Relación con otras librerías

`asyncpg` se relaciona especialmente con:

```text
postgresql
asyncio
fastapi
sqlalchemy
alembic
pydantic
python-dotenv
pytest
pytest-asyncio
```

## Relación con FastAPI

FastAPI puede usar endpoints asíncronos.

`asyncpg` puede manejar consultas PostgreSQL sin bloquear el event loop.

## Relación con SQLAlchemy

SQLAlchemy puede usar `asyncpg` como driver en su motor asíncrono.

```python
postgresql+asyncpg://user:password@localhost:5432/database
```

## Relación con Alembic

Alembic puede gestionar migraciones de proyectos que usan PostgreSQL, aunque la aplicación use `asyncpg` para ejecutar consultas.

## Relación con pytest

Para probar código asíncrono suele usarse:

```text
pytest
pytest-asyncio
```

## Relación con psycopg

`psycopg` es una opción moderna y general para PostgreSQL.

`asyncpg` se enfoca directamente en PostgreSQL con `asyncio`.

## Orden didáctico interno

```text
1. Propósito de asyncpg
2. Instalación
3. Relación con PostgreSQL y asyncio
4. Conexión con asyncpg.connect()
5. execute()
6. fetch(), fetchrow() y fetchval()
7. Parámetros con $1, $2, $3
8. Record y conversión a dict
9. INSERT, UPDATE, DELETE y RETURNING
10. Transacciones
11. Pools de conexiones
12. Prepared statements
13. Cursores
14. Tipos de datos
15. Manejo de errores
16. Uso con FastAPI, pandas y SQLAlchemy
17. Estructura recomendada
18. Errores comunes
19. Buenas prácticas
```