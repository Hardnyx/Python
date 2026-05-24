# `psycopg`

## Propósito

`psycopg` es una librería externa para conectar Python con PostgreSQL.

Se utiliza para ejecutar consultas SQL, insertar registros, actualizar datos, eliminar filas, leer resultados, manejar transacciones, trabajar con cursores, consumir funciones de PostgreSQL y conectar aplicaciones Python con bases de datos PostgreSQL.

Es un driver de base de datos. Su función no es modelar tablas como clases, sino permitir la comunicación directa entre Python y PostgreSQL.

## Naturaleza de la librería

`psycopg` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install "psycopg[binary]"
```

La importación principal es:

```python
import psycopg
```

Aunque se suele hablar de Psycopg 3, el módulo se importa como:

```python
import psycopg
```

No como:

```python
import psycopg3
```

## Relación con PostgreSQL

`psycopg` está diseñado específicamente para PostgreSQL.

No se usa para conectarse a:

```text
SQLite
MySQL
SQL Server
Oracle
MariaDB
```

Para esas bases de datos se usan otros drivers.

Ejemplos:

```text
sqlite3     -> SQLite
pymysql     -> MySQL / MariaDB
pyodbc      -> SQL Server
oracledb    -> Oracle
psycopg     -> PostgreSQL
```

## Idea central

La idea principal de `psycopg` es abrir una conexión con PostgreSQL y ejecutar comandos SQL desde Python.

Flujo típico:

```text
Python -> psycopg.connect() -> Connection -> Cursor -> SQL -> PostgreSQL
```

Ejemplo conceptual:

```python
import psycopg

with psycopg.connect("dbname=app user=postgres password=secret") as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT 1")
        result = cur.fetchone()

        print(result)
```

## Cuándo usar `psycopg`

Conviene usar `psycopg` cuando se necesita:

```text
conectar Python con PostgreSQL
ejecutar SQL directamente
leer datos desde PostgreSQL
insertar registros
actualizar registros
eliminar registros
manejar transacciones
usar funciones propias de PostgreSQL
hacer scripts de carga o extracción
integrar PostgreSQL con FastAPI o Flask
usar SQLAlchemy con driver PostgreSQL
```

## Cuándo no usar `psycopg`

No conviene usar `psycopg` cuando se necesita:

```text
conectarse a una base que no sea PostgreSQL
trabajar solo con archivos CSV o Excel
hacer análisis tabular sin base de datos
usar el ORM de Django
evitar escribir SQL manualmente
```

Para análisis de archivos suele bastar con:

```text
pandas
polars
```

Para modelar tablas como clases puede ser más conveniente:

```text
sqlalchemy
django ORM
```

## Instalación

Instalación recomendada para desarrollo:

```bash
python -m pip install "psycopg[binary]"
```

Instalación del paquete principal:

```bash
python -m pip install psycopg
```

Instalación con soporte para pool de conexiones:

```bash
python -m pip install "psycopg[pool]"
```

Instalación combinada:

```bash
python -m pip install "psycopg[binary,pool]"
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
psycopg==3.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import psycopg

print(psycopg.__version__)
```

## Diferencia entre `psycopg` y `psycopg2`

## `psycopg`

Corresponde a Psycopg 3.

```bash
python -m pip install psycopg
```

Importación:

```python
import psycopg
```

## `psycopg2`

Corresponde a la generación anterior.

```bash
python -m pip install psycopg2
```

Importación:

```python
import psycopg2
```

## Regla práctica

Para proyectos nuevos, conviene estudiar `psycopg`.

Para mantener proyectos antiguos, puede encontrarse mucho código con `psycopg2`.

## Cadena de conexión

La conexión a PostgreSQL puede definirse como una cadena.

```python
conninfo = "dbname=app user=postgres password=secret host=localhost port=5432"
```

Uso:

```python
import psycopg

conn = psycopg.connect(conninfo)
```

También puede usarse una URL:

```python
conninfo = "postgresql://postgres:secret@localhost:5432/app"
```

## Parámetros frecuentes de conexión

```text
dbname    -> nombre de la base de datos
user      -> usuario
password  -> contraseña
host      -> servidor
port      -> puerto
```

Ejemplo:

```python
conninfo = (
    "dbname=app "
    "user=postgres "
    "password=secret "
    "host=localhost "
    "port=5432"
)
```

## Conexión básica

```python
import psycopg

conn = psycopg.connect(
    "dbname=app user=postgres password=secret host=localhost port=5432"
)

conn.close()
```

En código real, suele preferirse usar context managers.

## Conexión con context manager

```python
import psycopg

with psycopg.connect(
    "dbname=app user=postgres password=secret host=localhost port=5432"
) as conn:
    print("Conexión abierta")
```

Al salir del bloque, la conexión se cierra.

Si no ocurre una excepción, la transacción se confirma.

Si ocurre una excepción, la transacción se revierte.

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
conninfo = "dbname=app user=postgres password=secret"
```

Más conveniente:

```python
import os

import psycopg

conninfo = os.getenv("DATABASE_URL")

with psycopg.connect(conninfo) as conn:
    ...
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
import os

import psycopg
from dotenv import load_dotenv

load_dotenv()

database_url = os.getenv("DATABASE_URL")

with psycopg.connect(database_url) as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT 1")
        print(cur.fetchone())
```

## Connection

## Propósito

Una `Connection` representa una sesión abierta contra PostgreSQL.

Desde una conexión se puede:

```text
crear cursores
ejecutar comandos
confirmar transacciones
revertir transacciones
cerrar la comunicación
configurar autocommit
```

Ejemplo:

```python
import psycopg

with psycopg.connect("dbname=app user=postgres") as conn:
    print(type(conn))
```

## Cursor

## Propósito

Un `Cursor` permite ejecutar comandos SQL y leer resultados.

Se crea desde la conexión:

```python
with conn.cursor() as cur:
    ...
```

Ejemplo:

```python
import psycopg

with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT 1")
        result = cur.fetchone()

        print(result)
```

## Ejecutar una consulta

```python
with conn.cursor() as cur:
    cur.execute("SELECT 1")
    result = cur.fetchone()

    print(result)
```

Salida conceptual:

```text
(1,)
```

El resultado se devuelve como una tupla.

## Crear una tabla

```python
import psycopg

with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.execute("""
            CREATE TABLE IF NOT EXISTS products (
                id SERIAL PRIMARY KEY,
                name TEXT NOT NULL,
                price NUMERIC(10, 2) NOT NULL,
                stock INTEGER NOT NULL DEFAULT 0
            )
        """)
```

## Insertar datos

```python
import psycopg

with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
            """,
            ("Laptop", 3500, 5)
        )
```

## Parámetros SQL

En `psycopg`, los valores se pasan con placeholders `%s`.

```python
cur.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

Aunque el valor sea un entero, el placeholder sigue siendo:

```python
%s
```

No se usa:

```python
%d
%f
```

## Tupla de un solo parámetro

Cuando se pasa un solo parámetro, debe usarse una tupla de un elemento.

Correcto:

```python
cur.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

Incorrecto:

```python
cur.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id)
)
```

La coma es necesaria.

## Evitar concatenar SQL

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"
cur.execute(query)
```

Más seguro:

```python
cur.execute(
    "SELECT * FROM products WHERE name = %s",
    (name,)
)
```

Los valores deben pasarse como parámetros, no concatenarse dentro del texto SQL.

## Leer un resultado

## `fetchone()`

Devuelve una fila o `None`.

```python
cur.execute(
    "SELECT id, name, price FROM products WHERE id = %s",
    (1,)
)

row = cur.fetchone()

print(row)
```

## Leer varios resultados

## `fetchall()`

Devuelve todas las filas pendientes.

```python
cur.execute("SELECT id, name, price FROM products")

rows = cur.fetchall()

for row in rows:
    print(row)
```

## Leer una cantidad limitada

## `fetchmany()`

```python
cur.execute("SELECT id, name, price FROM products")

rows = cur.fetchmany(10)

for row in rows:
    print(row)
```

## Iterar sobre el cursor

```python
cur.execute("SELECT id, name, price FROM products")

for row in cur:
    print(row)
```

Este patrón evita cargar todos los resultados de golpe en una lista.

## Insertar varios registros

## `executemany()`

```python
products = [
    ("Laptop", 3500, 5),
    ("Mouse", 80, 20),
    ("Teclado", 150, 10),
]

with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.executemany(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
            """,
            products
        )
```

## Actualizar datos

```python
with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.execute(
            """
            UPDATE products
            SET price = %s
            WHERE id = %s
            """,
            (3600, 1)
        )
```

## Eliminar datos

```python
with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.execute(
            "DELETE FROM products WHERE id = %s",
            (1,)
        )
```

## `RETURNING`

PostgreSQL permite devolver datos después de un `INSERT`, `UPDATE` o `DELETE` usando `RETURNING`.

```python
with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
            RETURNING id
            """,
            ("Laptop", 3500, 5)
        )

        new_id = cur.fetchone()[0]

        print(new_id)
```

## Transacciones

Por defecto, las operaciones se ejecutan dentro de una transacción.

Los cambios no quedan confirmados de forma permanente hasta hacer:

```python
conn.commit()
```

Si ocurre un error, puede revertirse con:

```python
conn.rollback()
```

## Commit manual

```python
conn = psycopg.connect("dbname=app user=postgres")

try:
    with conn.cursor() as cur:
        cur.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
            """,
            ("Laptop", 3500, 5)
        )

    conn.commit()
except Exception:
    conn.rollback()
    raise
finally:
    conn.close()
```

## Commit automático con context manager

```python
with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
            """,
            ("Laptop", 3500, 5)
        )
```

Si el bloque termina sin excepción, se confirma la transacción.

Si ocurre una excepción, se revierte.

## Rollback

```python
try:
    cur.execute("INSERT INTO products (name) VALUES (%s)", ("Laptop",))
    conn.commit()
except Exception:
    conn.rollback()
    raise
```

Después de ciertos errores SQL, la transacción queda en estado fallido. Para seguir usando la conexión, normalmente se necesita `rollback()`.

## Autocommit

Algunas operaciones de PostgreSQL requieren ejecutarse fuera de una transacción normal.

Para eso puede usarse `autocommit`.

```python
import psycopg

with psycopg.connect(
    "dbname=app user=postgres",
    autocommit=True
) as conn:
    with conn.cursor() as cur:
        cur.execute("VACUUM")
```

## Cuándo usar autocommit

Puede ser necesario para operaciones como:

```text
VACUUM
CREATE DATABASE
DROP DATABASE
ciertas tareas administrativas
```

Para operaciones normales de aplicación, suele ser mejor manejar transacciones explícitas.

## Transaction context

Psycopg permite manejar bloques transaccionales.

```python
with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.transaction():
        conn.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
            """,
            ("Laptop", 3500, 5)
        )
```

Este patrón ayuda a delimitar el alcance de una transacción.

## `conn.execute()`

Además de usar cursores explícitos, se puede ejecutar directamente desde la conexión.

```python
with psycopg.connect("dbname=app user=postgres") as conn:
    result = conn.execute("SELECT 1")

    print(result.fetchone())
```

Para código donde se necesita mucho control, el cursor explícito sigue siendo claro.

## Row factories

Por defecto, las filas pueden recibirse como tuplas.

```python
(1, "Laptop", 3500)
```

Psycopg permite configurar formas distintas de filas.

## Filas como diccionario

```python
import psycopg
from psycopg.rows import dict_row

with psycopg.connect(
    "dbname=app user=postgres",
    row_factory=dict_row
) as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT id, name, price FROM products")

        rows = cur.fetchall()

        for row in rows:
            print(row["name"])
```

Esto puede hacer el código más legible cuando se trabaja con muchas columnas.

## Filas como namedtuple

```python
from psycopg.rows import namedtuple_row

with psycopg.connect(
    "dbname=app user=postgres",
    row_factory=namedtuple_row
) as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT id, name, price FROM products")

        product = cur.fetchone()

        print(product.name)
```

## Manejo de errores

Psycopg expone errores específicos para problemas de PostgreSQL.

```python
import psycopg

try:
    with psycopg.connect("dbname=app user=postgres") as conn:
        with conn.cursor() as cur:
            cur.execute("SELECT * FROM table_that_does_not_exist")
except psycopg.Error as error:
    print("Error de base de datos:", error)
```

## Errores frecuentes

```text
OperationalError
IntegrityError
ProgrammingError
DataError
DatabaseError
```

Ejemplo:

```python
import psycopg

try:
    ...
except psycopg.IntegrityError:
    ...
except psycopg.OperationalError:
    ...
except psycopg.Error:
    ...
```

## Error de unicidad

```python
import psycopg

try:
    with psycopg.connect("dbname=app user=postgres") as conn:
        with conn.cursor() as cur:
            cur.execute(
                """
                INSERT INTO products (name, price, stock)
                VALUES (%s, %s, %s)
                """,
                ("Laptop", 3500, 5)
            )
except psycopg.IntegrityError:
    print("No se pudo insertar el registro por una restricción de integridad")
```

## Consultas dinámicas seguras

Los valores se pasan con `%s`.

Pero nombres de tablas o columnas no deben pasarse como `%s`.

Problemático:

```python
cur.execute(
    "SELECT * FROM %s",
    ("products",)
)
```

Para identificadores SQL, debe usarse composición segura.

```python
from psycopg import sql

table_name = "products"

query = sql.SQL("SELECT * FROM {}").format(
    sql.Identifier(table_name)
)

cur.execute(query)
```

## Identificadores

Para nombres de tablas o columnas:

```python
from psycopg import sql

query = sql.SQL("SELECT {} FROM {}").format(
    sql.Identifier("name"),
    sql.Identifier("products")
)
```

## Valores

Para valores normales, usar parámetros:

```python
cur.execute(
    "SELECT * FROM products WHERE name = %s",
    ("Laptop",)
)
```

## Regla práctica

```text
valores del usuario          -> parámetros con %s
nombres de tablas o columnas -> psycopg.sql.Identifier
fragmentos SQL controlados   -> psycopg.sql.SQL
```

## Tipos de datos

Psycopg convierte muchos tipos entre Python y PostgreSQL.

Ejemplos frecuentes:

```text
Python str       -> PostgreSQL text / varchar
Python int       -> PostgreSQL integer / bigint
Python float     -> PostgreSQL double precision
Decimal          -> PostgreSQL numeric
datetime.date    -> PostgreSQL date
datetime.datetime -> PostgreSQL timestamp
bool             -> PostgreSQL boolean
None             -> SQL NULL
```

## Insertar fechas

```python
from datetime import date

with psycopg.connect("dbname=app user=postgres") as conn:
    with conn.cursor() as cur:
        cur.execute(
            """
            INSERT INTO events (name, event_date)
            VALUES (%s, %s)
            """,
            ("Cierre mensual", date(2026, 12, 31))
        )
```

## Insertar `None`

```python
cur.execute(
    """
    INSERT INTO products (name, description)
    VALUES (%s, %s)
    """,
    ("Laptop", None)
)
```

`None` se convierte en `NULL`.

## Leer datos como lista de diccionarios

```python
from psycopg.rows import dict_row

def list_products(conninfo):
    with psycopg.connect(conninfo, row_factory=dict_row) as conn:
        with conn.cursor() as cur:
            cur.execute("""
                SELECT id, name, price, stock
                FROM products
                ORDER BY name
            """)

            return cur.fetchall()
```

Uso:

```python
products = list_products("dbname=app user=postgres")

for product in products:
    print(product["name"])
```

## Funciones reutilizables

## Crear conexión

```python
import os

import psycopg
from dotenv import load_dotenv

load_dotenv()


def get_conninfo():
    database_url = os.getenv("DATABASE_URL")

    if database_url is None:
        raise RuntimeError("DATABASE_URL no está configurada")

    return database_url
```

## Ejecutar consulta de lectura

```python
from psycopg.rows import dict_row


def fetch_products():
    conninfo = get_conninfo()

    with psycopg.connect(conninfo, row_factory=dict_row) as conn:
        with conn.cursor() as cur:
            cur.execute("""
                SELECT id, name, price, stock
                FROM products
                ORDER BY name
            """)

            return cur.fetchall()
```

## Ejecutar inserción

```python
def create_product(name, price, stock):
    conninfo = get_conninfo()

    with psycopg.connect(conninfo) as conn:
        with conn.cursor() as cur:
            cur.execute(
                """
                INSERT INTO products (name, price, stock)
                VALUES (%s, %s, %s)
                RETURNING id
                """,
                (name, price, stock)
            )

            return cur.fetchone()[0]
```

## Connection pool

Abrir una conexión nueva en cada operación puede ser costoso en aplicaciones web o concurrentes.

Para esos casos se puede usar un pool de conexiones.

Instalación:

```bash
python -m pip install "psycopg[pool]"
```

Importación:

```python
from psycopg_pool import ConnectionPool
```

Ejemplo:

```python
from psycopg_pool import ConnectionPool

pool = ConnectionPool(
    conninfo="postgresql://postgres:secret@localhost:5432/app",
    min_size=1,
    max_size=10
)

with pool.connection() as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT 1")
        print(cur.fetchone())

pool.close()
```

## Pool con context manager

```python
from psycopg_pool import ConnectionPool

conninfo = "postgresql://postgres:secret@localhost:5432/app"

with ConnectionPool(conninfo, min_size=1, max_size=10) as pool:
    with pool.connection() as conn:
        result = conn.execute("SELECT 1")
        print(result.fetchone())
```

## Cuándo usar pool

Conviene usar pool cuando:

```text
hay muchas solicitudes
se usa una aplicación web
se hacen consultas frecuentes
se quiere reutilizar conexiones
se necesita controlar el número máximo de conexiones
```

Para scripts pequeños, una conexión directa puede ser suficiente.

## Uso con FastAPI

Patrón conceptual:

```text
FastAPI endpoint -> psycopg connection / pool -> PostgreSQL
```

Ejemplo simple:

```python
import os

import psycopg
from fastapi import FastAPI
from psycopg.rows import dict_row

app = FastAPI()

database_url = os.getenv("DATABASE_URL")


@app.get("/products")
def read_products():
    with psycopg.connect(database_url, row_factory=dict_row) as conn:
        with conn.cursor() as cur:
            cur.execute("""
                SELECT id, name, price, stock
                FROM products
                ORDER BY name
            """)

            return cur.fetchall()
```

En una aplicación real con muchas solicitudes, suele convenir un pool.

## Uso con Flask

```python
import os

import psycopg
from flask import Flask
from psycopg.rows import dict_row

app = Flask(__name__)

database_url = os.getenv("DATABASE_URL")


@app.route("/products")
def read_products():
    with psycopg.connect(database_url, row_factory=dict_row) as conn:
        with conn.cursor() as cur:
            cur.execute("""
                SELECT id, name, price, stock
                FROM products
                ORDER BY name
            """)

            return cur.fetchall()
```

## Uso con pandas

Pandas puede leer desde PostgreSQL usando una conexión compatible o mediante SQLAlchemy.

La opción más común en proyectos de datos es usar SQLAlchemy como capa de conexión.

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine(
    "postgresql+psycopg://postgres:secret@localhost:5432/app"
)

df = pd.read_sql(
    "SELECT * FROM products",
    engine
)

print(df.head())
```

## Uso con SQLAlchemy

SQLAlchemy puede usar `psycopg` como driver PostgreSQL.

```python
from sqlalchemy import create_engine

engine = create_engine(
    "postgresql+psycopg://postgres:secret@localhost:5432/app"
)
```

En ese caso, SQLAlchemy maneja la capa de consultas y sesiones, mientras `psycopg` actúa como driver de conexión.

## Operaciones administrativas

Algunas operaciones administrativas requieren `autocommit=True`.

Ejemplo conceptual:

```python
import psycopg

with psycopg.connect(
    "dbname=postgres user=postgres",
    autocommit=True
) as conn:
    conn.execute("CREATE DATABASE app")
```

Estas operaciones deben usarse con cuidado y solo cuando corresponda.

## COPY

PostgreSQL tiene una operación `COPY` para carga o descarga masiva de datos.

Psycopg puede trabajar con `COPY`, pero este tema suele ser más avanzado.

Uso conceptual:

```text
COPY tabla FROM archivo
COPY tabla TO archivo
```

Para una documentación inicial, conviene dominar primero:

```text
connect()
cursor()
execute()
fetchone()
fetchall()
commit()
rollback()
row_factory
pool
```

## LISTEN y NOTIFY

PostgreSQL permite notificaciones con `LISTEN` y `NOTIFY`.

Psycopg puede recibir notificaciones, pero este tema pertenece a un uso avanzado.

Uso conceptual:

```text
LISTEN canal
NOTIFY canal, mensaje
```

Se utiliza en flujos donde una sesión de PostgreSQL comunica eventos a otra.

## Estructura recomendada

Para una aplicación pequeña:

```text
app/
├─ database.py
├─ repositories.py
└─ main.py
```

## `database.py`

```python
import os

from dotenv import load_dotenv
from psycopg_pool import ConnectionPool

load_dotenv()

database_url = os.getenv("DATABASE_URL")

if database_url is None:
    raise RuntimeError("DATABASE_URL no está configurada")

pool = ConnectionPool(
    conninfo=database_url,
    min_size=1,
    max_size=10
)
```

## `repositories.py`

```python
from psycopg.rows import dict_row

from .database import pool


def list_products():
    with pool.connection() as conn:
        with conn.cursor(row_factory=dict_row) as cur:
            cur.execute("""
                SELECT id, name, price, stock
                FROM products
                ORDER BY name
            """)

            return cur.fetchall()


def get_product(product_id):
    with pool.connection() as conn:
        with conn.cursor(row_factory=dict_row) as cur:
            cur.execute(
                """
                SELECT id, name, price, stock
                FROM products
                WHERE id = %s
                """,
                (product_id,)
            )

            return cur.fetchone()


def create_product(name, price, stock):
    with pool.connection() as conn:
        with conn.cursor() as cur:
            cur.execute(
                """
                INSERT INTO products (name, price, stock)
                VALUES (%s, %s, %s)
                RETURNING id
                """,
                (name, price, stock)
            )

            return cur.fetchone()[0]
```

## Separación por responsabilidades

```text
database.py     -> conexión o pool
repositories.py -> consultas SQL
services.py     -> reglas de negocio
main.py         -> entrada de aplicación
```

## Errores comunes

## Instalar `psycopg2` cuando se quería Psycopg 3

Para Psycopg 3:

```bash
python -m pip install psycopg
```

Importación:

```python
import psycopg
```

## Importar `psycopg3`

Problemático:

```python
import psycopg3
```

Correcto:

```python
import psycopg
```

## Olvidar `commit()`

Problemático:

```python
cur.execute("INSERT INTO products (name) VALUES (%s)", ("Laptop",))
conn.close()
```

Más seguro:

```python
conn.commit()
```

o usar context manager:

```python
with psycopg.connect(conninfo) as conn:
    ...
```

## No hacer `rollback()` después de un error

Si una operación falla dentro de una transacción, la conexión puede quedar en estado fallido.

Debe hacerse:

```python
conn.rollback()
```

antes de seguir usando esa conexión.

## Concatenar valores dentro del SQL

Problemático:

```python
cur.execute(f"SELECT * FROM products WHERE name = '{name}'")
```

Correcto:

```python
cur.execute(
    "SELECT * FROM products WHERE name = %s",
    (name,)
)
```

## Usar `%d` o `%f` como placeholder

Problemático:

```python
cur.execute(
    "SELECT * FROM products WHERE id = %d",
    (product_id,)
)
```

Correcto:

```python
cur.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

## Olvidar la coma en una tupla de un solo parámetro

Problemático:

```python
(product_id)
```

Correcto:

```python
(product_id,)
```

## Usar parámetros para nombres de tablas

Problemático:

```python
cur.execute(
    "SELECT * FROM %s",
    ("products",)
)
```

Correcto:

```python
from psycopg import sql

query = sql.SQL("SELECT * FROM {}").format(
    sql.Identifier("products")
)

cur.execute(query)
```

## Dejar credenciales en el código

Problemático:

```python
conninfo = "postgresql://postgres:secret@localhost:5432/app"
```

Mejor:

```python
conninfo = os.getenv("DATABASE_URL")
```

## Abrir demasiadas conexiones

En aplicaciones web, abrir una conexión por cada operación sin pool puede generar sobrecarga.

Para muchas solicitudes, evaluar:

```python
psycopg_pool.ConnectionPool
```

## No cerrar conexiones

Menos recomendable:

```python
conn = psycopg.connect(conninfo)
```

sin:

```python
conn.close()
```

Más seguro:

```python
with psycopg.connect(conninfo) as conn:
    ...
```

## Buenas prácticas

## Usar context managers

```python
with psycopg.connect(conninfo) as conn:
    with conn.cursor() as cur:
        ...
```

## Usar parámetros para valores

```python
cur.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

## Usar `psycopg.sql` para identificadores

```python
sql.Identifier(table_name)
```

## Usar variables de entorno

```python
os.getenv("DATABASE_URL")
```

## Separar consultas en funciones

```python
def list_products():
    ...
```

## Usar `row_factory=dict_row` cuando mejore la legibilidad

```python
from psycopg.rows import dict_row
```

## Controlar transacciones

```python
conn.commit()
conn.rollback()
```

o usar context managers.

## Usar pool en aplicaciones concurrentes

```python
from psycopg_pool import ConnectionPool
```

## No mezclar lógica SQL con lógica HTTP

Separar:

```text
endpoint
service
repository
database
```

## Validar errores de integridad

```python
except psycopg.IntegrityError:
    ...
```

## Ejemplo integrado

```python
import os
from decimal import Decimal

import psycopg
from dotenv import load_dotenv
from psycopg.rows import dict_row

load_dotenv()


def get_conninfo():
    conninfo = os.getenv("DATABASE_URL")

    if conninfo is None:
        raise RuntimeError("DATABASE_URL no está configurada")

    return conninfo


def create_table():
    with psycopg.connect(get_conninfo()) as conn:
        with conn.cursor() as cur:
            cur.execute("""
                CREATE TABLE IF NOT EXISTS products (
                    id SERIAL PRIMARY KEY,
                    name TEXT NOT NULL UNIQUE,
                    price NUMERIC(10, 2) NOT NULL,
                    stock INTEGER NOT NULL DEFAULT 0,
                    is_active BOOLEAN NOT NULL DEFAULT TRUE
                )
            """)


def create_product(name, price, stock=0):
    with psycopg.connect(get_conninfo()) as conn:
        with conn.cursor() as cur:
            try:
                cur.execute(
                    """
                    INSERT INTO products (name, price, stock)
                    VALUES (%s, %s, %s)
                    RETURNING id
                    """,
                    (name, Decimal(str(price)), stock)
                )

                return cur.fetchone()[0]
            except psycopg.IntegrityError:
                conn.rollback()
                raise ValueError("El producto ya existe")


def list_products():
    with psycopg.connect(
        get_conninfo(),
        row_factory=dict_row
    ) as conn:
        with conn.cursor() as cur:
            cur.execute("""
                SELECT id, name, price, stock, is_active
                FROM products
                WHERE is_active = TRUE
                ORDER BY name
            """)

            return cur.fetchall()


def update_stock(product_id, stock):
    with psycopg.connect(get_conninfo()) as conn:
        with conn.cursor(row_factory=dict_row) as cur:
            cur.execute(
                """
                UPDATE products
                SET stock = %s
                WHERE id = %s
                RETURNING id, name, price, stock
                """,
                (stock, product_id)
            )

            return cur.fetchone()


def delete_product(product_id):
    with psycopg.connect(get_conninfo()) as conn:
        with conn.cursor() as cur:
            cur.execute(
                """
                UPDATE products
                SET is_active = FALSE
                WHERE id = %s
                """,
                (product_id,)
            )


create_table()

try:
    create_product("Laptop", 3500, stock=5)
    create_product("Mouse", 80, stock=20)
except ValueError:
    pass

products = list_products()

for product in products:
    print(product["id"], product["name"], product["price"], product["stock"])
```

## Relación con otras librerías

`psycopg` se relaciona especialmente con:

```text
postgresql
sqlalchemy
pandas
fastapi
flask
python-dotenv
pytest
alembic
```

## Relación con SQLAlchemy

SQLAlchemy puede usar `psycopg` como driver.

```python
engine = create_engine(
    "postgresql+psycopg://user:password@localhost:5432/database"
)
```

## Relación con pandas

Pandas suele conectarse a PostgreSQL mediante SQLAlchemy y `psycopg`.

```python
df = pd.read_sql("SELECT * FROM products", engine)
```

## Relación con FastAPI y Flask

`psycopg` puede usarse en backends web para consultar PostgreSQL desde endpoints.

En aplicaciones medianas o grandes, conviene evaluar pool de conexiones y separación de responsabilidades.

## Orden didáctico interno

```text
1. Propósito de psycopg
2. Instalación
3. Conexión a PostgreSQL
4. Connection y Cursor
5. execute()
6. Parámetros con %s
7. fetchone(), fetchall() y fetchmany()
8. INSERT, UPDATE, DELETE y RETURNING
9. Transacciones, commit y rollback
10. autocommit
11. row_factory
12. Manejo de errores
13. SQL dinámico seguro con psycopg.sql
14. Connection pool
15. Uso con FastAPI, Flask, pandas y SQLAlchemy
16. Errores comunes
17. Buenas prácticas
```