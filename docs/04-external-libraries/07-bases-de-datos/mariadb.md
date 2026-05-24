# `mariadb`

## Propósito

`mariadb` es una librería externa para conectar Python con bases de datos MariaDB.

También puede conectarse con bases MySQL compatibles.

Se utiliza para ejecutar consultas SQL, insertar registros, actualizar datos, eliminar filas, leer resultados, manejar transacciones, usar cursores, trabajar con procedimientos almacenados y conectar aplicaciones Python con servidores MariaDB.

Es un driver de base de datos. Su función principal no es modelar tablas como clases, sino permitir la comunicación directa entre Python y MariaDB.

## Naturaleza de la librería

`mariadb` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install mariadb
```

La importación habitual es:

```python
import mariadb
```

El paquete se instala e importa con el mismo nombre:

```text
mariadb
```

## Relación con MariaDB y MySQL

`mariadb` está diseñado principalmente para MariaDB, aunque también puede conectarse a servidores MySQL compatibles.

No se usa para conectarse a:

```text
SQLite
PostgreSQL
SQL Server
Oracle Database
MongoDB
Redis
```

Para esas bases se usan otros drivers:

```text
sqlite3        -> SQLite
psycopg        -> PostgreSQL
pyodbc         -> SQL Server / ODBC
oracledb       -> Oracle Database
pymongo        -> MongoDB
redis          -> Redis
mariadb        -> MariaDB / MySQL compatible
```

## Idea central

La idea principal de `mariadb` es abrir una conexión con MariaDB y ejecutar comandos SQL desde Python.

Flujo típico:

```text
Python -> mariadb.connect() -> Connection -> Cursor -> MariaDB
```

Ejemplo mínimo:

```python
import mariadb

connection = mariadb.connect(
    host="localhost",
    port=3306,
    user="root",
    password="secret",
    database="app"
)

try:
    cursor = connection.cursor()

    cursor.execute("SELECT 1")

    row = cursor.fetchone()

    print(row)
finally:
    cursor.close()
    connection.close()
```

## Cuándo usar `mariadb`

Conviene usar `mariadb` cuando se necesita:

```text
conectar Python con MariaDB
usar el conector oficial de MariaDB
ejecutar SQL directamente
leer datos desde MariaDB
insertar registros
actualizar registros
eliminar registros
manejar transacciones
usar procedimientos almacenados
crear scripts de carga o extracción
integrar MariaDB con FastAPI o Flask
usar SQLAlchemy con MariaDB
```

## Cuándo no usar `mariadb`

No conviene usar `mariadb` cuando se necesita:

```text
conectarse a PostgreSQL
conectarse a SQLite
conectarse a SQL Server
trabajar solo con archivos CSV o Excel
usar MongoDB
usar Redis
evitar SQL manual completamente
usar el ORM propio de Django
```

Para análisis tabular puede corresponder:

```text
pandas
polars
duckdb
```

Para ORM puede corresponder:

```text
sqlalchemy
sqlmodel
django ORM
peewee
```

## Instalación

Instalación básica:

```bash
python -m pip install mariadb
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
mariadb==1.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import mariadb

print(mariadb.__version__)
```

## Conexión básica

```python
import mariadb

connection = mariadb.connect(
    host="localhost",
    port=3306,
    user="root",
    password="secret",
    database="app"
)

connection.close()
```

## Parámetros frecuentes de conexión

```text
host      -> servidor MariaDB
port      -> puerto, normalmente 3306
user      -> usuario
password  -> contraseña
database  -> base de datos
autocommit -> confirmación automática
```

Ejemplo:

```python
connection = mariadb.connect(
    host="localhost",
    port=3306,
    user="root",
    password="secret",
    database="app",
    autocommit=False
)
```

## Uso recomendado con `try` y `finally`

```python
import mariadb

connection = mariadb.connect(
    host="localhost",
    port=3306,
    user="root",
    password="secret",
    database="app"
)

cursor = connection.cursor()

try:
    cursor.execute("SELECT 1")
    print(cursor.fetchone())
finally:
    cursor.close()
    connection.close()
```

Este patrón asegura que los recursos se cierren aunque ocurra un error.

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
connection = mariadb.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app"
)
```

Más conveniente:

```python
import os

import mariadb

connection = mariadb.connect(
    host=os.getenv("MARIADB_HOST"),
    port=int(os.getenv("MARIADB_PORT", "3306")),
    user=os.getenv("MARIADB_USER"),
    password=os.getenv("MARIADB_PASSWORD"),
    database=os.getenv("MARIADB_DATABASE"),
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
MARIADB_HOST=localhost
MARIADB_PORT=3306
MARIADB_USER=root
MARIADB_PASSWORD=secret
MARIADB_DATABASE=app
```

Código:

```python
import os

import mariadb
from dotenv import load_dotenv

load_dotenv()

connection = mariadb.connect(
    host=os.getenv("MARIADB_HOST"),
    port=int(os.getenv("MARIADB_PORT", "3306")),
    user=os.getenv("MARIADB_USER"),
    password=os.getenv("MARIADB_PASSWORD"),
    database=os.getenv("MARIADB_DATABASE"),
)

connection.close()
```

## Connection

Una conexión representa una sesión abierta contra MariaDB.

Desde una conexión se puede:

```text
crear cursores
ejecutar consultas
confirmar transacciones
revertir cambios
configurar autocommit
cerrar la conexión
```

Ejemplo:

```python
connection = mariadb.connect(
    host="localhost",
    port=3306,
    user="root",
    password="secret",
    database="app"
)
```

## Cerrar conexión

```python
connection.close()
```

Cerrar conexiones es importante para liberar recursos.

## Cursor

Un cursor permite ejecutar comandos SQL y leer resultados.

```python
cursor = connection.cursor()
```

Uso:

```python
cursor.execute("SELECT 1")
row = cursor.fetchone()

print(row)
```

## Cerrar cursor

```python
cursor.close()
```

## Ejecutar una consulta

```python
cursor.execute("SELECT 1")

row = cursor.fetchone()

print(row)
```

Salida conceptual:

```text
(1,)
```

## Crear una tabla

```python
cursor.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100) NOT NULL UNIQUE,
        price DECIMAL(10, 2) NOT NULL,
        stock INT NOT NULL DEFAULT 0,
        is_active BOOLEAN NOT NULL DEFAULT TRUE
    )
""")

connection.commit()
```

## Insertar datos

```python
cursor.execute(
    """
    INSERT INTO products (name, price, stock)
    VALUES (?, ?, ?)
    """,
    ("Laptop", 3500, 5)
)

connection.commit()
```

## Parámetros SQL

En `mariadb`, los valores pueden pasarse con placeholders.

Un patrón habitual es usar signos de interrogación:

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = ?
    """,
    (product_id,)
)
```

No conviene insertar valores directamente dentro del texto SQL.

## Tupla de un solo parámetro

Cuando se pasa un solo parámetro, debe usarse una tupla de un elemento.

Correcto:

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = ?
    """,
    (product_id,)
)
```

Incorrecto:

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = ?
    """,
    (product_id)
)
```

La coma es necesaria.

## Evitar concatenar SQL

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"
cursor.execute(query)
```

Más seguro:

```python
cursor.execute(
    """
    SELECT *
    FROM products
    WHERE name = ?
    """,
    (name,)
)
```

Los valores externos deben pasarse como parámetros.

## Leer un resultado

## `fetchone()`

Devuelve una fila o `None`.

```python
cursor.execute(
    """
    SELECT id, name, price, stock
    FROM products
    WHERE id = ?
    """,
    (1,)
)

row = cursor.fetchone()

print(row)
```

## Leer todos los resultados

## `fetchall()`

```python
cursor.execute("""
    SELECT id, name, price, stock
    FROM products
    ORDER BY name
""")

rows = cursor.fetchall()

for row in rows:
    print(row)
```

## Leer una cantidad limitada

## `fetchmany()`

```python
cursor.execute("""
    SELECT id, name, price, stock
    FROM products
    ORDER BY name
""")

rows = cursor.fetchmany(10)

for row in rows:
    print(row)
```

## Iterar sobre el cursor

```python
cursor.execute("""
    SELECT id, name, price, stock
    FROM products
    ORDER BY name
""")

for row in cursor:
    print(row)
```

Este patrón puede evitar cargar todas las filas en una lista.

## Filas como tuplas

Por defecto, las filas suelen recibirse como tuplas.

```text
(1, "Laptop", Decimal("3500.00"), 5)
```

Acceso por posición:

```python
print(row[0])
print(row[1])
```

## Convertir filas a diccionarios

Si se recibe una fila como tupla, puede convertirse usando los metadatos del cursor.

```python
def rows_to_dicts(cursor, rows):
    columns = [
        column[0]
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]
```

Uso:

```python
cursor.execute("""
    SELECT id, name, price, stock
    FROM products
""")

rows = rows_to_dicts(cursor, cursor.fetchall())

print(rows)
```

## Insertar varios registros

## `executemany()`

```python
products = [
    ("Laptop", 3500, 5),
    ("Mouse", 80, 20),
    ("Teclado", 150, 10),
]

cursor.executemany(
    """
    INSERT INTO products (name, price, stock)
    VALUES (?, ?, ?)
    """,
    products
)

connection.commit()
```

`executemany()` ejecuta la misma sentencia para varios conjuntos de valores.

## Obtener último id insertado

```python
cursor.execute(
    """
    INSERT INTO products (name, price, stock)
    VALUES (?, ?, ?)
    """,
    ("Laptop", 3500, 5)
)

connection.commit()

print(cursor.lastrowid)
```

`lastrowid` devuelve el identificador generado por una columna `AUTO_INCREMENT`.

## Actualizar datos

```python
cursor.execute(
    """
    UPDATE products
    SET price = ?, stock = ?
    WHERE id = ?
    """,
    (3600, 4, 1)
)

connection.commit()
```

## Eliminar datos

```python
cursor.execute(
    """
    DELETE FROM products
    WHERE id = ?
    """,
    (1,)
)

connection.commit()
```

## Eliminación lógica

En aplicaciones reales, muchas veces se prefiere marcar registros como inactivos.

```python
cursor.execute(
    """
    UPDATE products
    SET is_active = FALSE
    WHERE id = ?
    """,
    (product_id,)
)

connection.commit()
```

Consulta de activos:

```python
cursor.execute("""
    SELECT id, name, price, stock
    FROM products
    WHERE is_active = TRUE
""")
```

## `rowcount`

`rowcount` indica cuántas filas fueron afectadas por operaciones como `UPDATE` o `DELETE`.

```python
cursor.execute(
    """
    UPDATE products
    SET stock = ?
    WHERE id = ?
    """,
    (10, 1)
)

connection.commit()

print(cursor.rowcount)
```

## Transacciones

Las operaciones que modifican datos deben confirmarse con:

```python
connection.commit()
```

Si ocurre un error, puede revertirse con:

```python
connection.rollback()
```

## Commit manual

```python
try:
    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        ("Laptop", 3500, 5)
    )

    connection.commit()
except Exception:
    connection.rollback()
    raise
```

## Rollback

```python
try:
    cursor.execute(...)
    connection.commit()
except Exception:
    connection.rollback()
    raise
```

Después de ciertos errores, conviene hacer `rollback()` antes de seguir usando la conexión.

## Autocommit

`autocommit` confirma automáticamente cada sentencia.

Puede configurarse al conectar:

```python
connection = mariadb.connect(
    host="localhost",
    port=3306,
    user="root",
    password="secret",
    database="app",
    autocommit=True
)
```

Para operaciones relacionadas, suele ser mejor manejar transacciones explícitamente.

## Procedimientos almacenados

MariaDB permite procedimientos almacenados.

Ejemplo conceptual:

```python
cursor.callproc(
    "update_stock",
    (1, 10)
)
```

Luego pueden procesarse los resultados según el procedimiento.

## Consultas de lectura

```python
def list_products(connection):
    cursor = connection.cursor()

    try:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return rows_to_dicts(cursor, cursor.fetchall())
    finally:
        cursor.close()
```

## Consulta por id

```python
def get_product(connection, product_id):
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            SELECT id, name, price, stock
            FROM products
            WHERE id = ?
            """,
            (product_id,)
        )

        row = cursor.fetchone()

        if row is None:
            return None

        return rows_to_dicts(cursor, [row])[0]
    finally:
        cursor.close()
```

## Crear registro

```python
def create_product(connection, name, price, stock=0):
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (?, ?, ?)
            """,
            (name, price, stock)
        )

        connection.commit()

        return cursor.lastrowid
    except Exception:
        connection.rollback()
        raise
    finally:
        cursor.close()
```

## Actualizar registro

```python
def update_stock(connection, product_id, stock):
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            UPDATE products
            SET stock = ?
            WHERE id = ?
            """,
            (stock, product_id)
        )

        connection.commit()

        return cursor.rowcount
    except Exception:
        connection.rollback()
        raise
    finally:
        cursor.close()
```

## Manejo de errores

`mariadb` expone excepciones para errores de base de datos.

```python
import mariadb

try:
    cursor.execute("SELECT * FROM table_that_does_not_exist")
except mariadb.Error as error:
    print("Error de MariaDB:", error)
```

## Errores frecuentes

```text
mariadb.Error
mariadb.InterfaceError
mariadb.DatabaseError
mariadb.IntegrityError
mariadb.ProgrammingError
mariadb.OperationalError
```

## Error de integridad

```python
try:
    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        ("Laptop", 3500, 5)
    )

    connection.commit()
except mariadb.IntegrityError:
    connection.rollback()
    print("No se pudo insertar el registro por una restricción de integridad")
```

## Error de conexión

```python
try:
    connection = mariadb.connect(
        host="localhost",
        port=3306,
        user="root",
        password="secret",
        database="app"
    )
except mariadb.OperationalError as error:
    print("No se pudo conectar a MariaDB")
    print(error)
```

## Pool de conexiones

En versiones recientes de MariaDB Connector/Python puede existir soporte de pool de conexiones.

Ejemplo conceptual:

```python
pool = mariadb.ConnectionPool(
    pool_name="app_pool",
    pool_size=5,
    host="localhost",
    port=3306,
    user="root",
    password="secret",
    database="app"
)
```

Uso conceptual:

```python
connection = pool.get_connection()

try:
    cursor = connection.cursor()
    cursor.execute("SELECT 1")
    print(cursor.fetchone())
finally:
    cursor.close()
    connection.close()
```

Cuando se usa pool, cerrar la conexión normalmente la devuelve al pool.

## Cuándo usar pool

Conviene usar pool cuando:

```text
hay muchas solicitudes
se usa una aplicación web
se realizan consultas frecuentes
se quiere reutilizar conexiones
se necesita controlar el número máximo de conexiones
```

Para scripts pequeños, una conexión directa puede ser suficiente.

## API asíncrona

En documentación reciente de MariaDB Connector/Python aparece soporte asíncrono.

Ejemplo conceptual:

```python
import asyncio
import mariadb


async def main():
    connection = await mariadb.async_connect(
        host="localhost",
        port=3306,
        user="root",
        password="secret",
        database="app"
    )

    try:
        cursor = await connection.cursor()
        await cursor.execute("SELECT 1")

        row = await cursor.fetchone()

        print(row)
    finally:
        await cursor.close()
        await connection.close()


asyncio.run(main())
```

La disponibilidad exacta de esta API puede depender de la versión instalada.

Para proyectos que usan `asyncio`, conviene revisar la versión del conector y su documentación específica.

## Uso con FastAPI

Ejemplo sencillo con conexión síncrona:

```python
import os
from contextlib import asynccontextmanager

import mariadb
from fastapi import FastAPI, HTTPException

connection_config = {
    "host": os.getenv("MARIADB_HOST", "localhost"),
    "port": int(os.getenv("MARIADB_PORT", "3306")),
    "user": os.getenv("MARIADB_USER"),
    "password": os.getenv("MARIADB_PASSWORD"),
    "database": os.getenv("MARIADB_DATABASE"),
}


def rows_to_dicts(cursor, rows):
    columns = [
        column[0]
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


def create_connection():
    return mariadb.connect(**connection_config)


app = FastAPI()


@app.get("/products")
def read_products():
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return rows_to_dicts(cursor, cursor.fetchall())
    finally:
        cursor.close()
        connection.close()
```

Para una aplicación con muchas solicitudes, conviene evaluar pool de conexiones.

## Uso con Flask

```python
import os

import mariadb
from flask import Flask

app = Flask(__name__)


def create_connection():
    return mariadb.connect(
        host=os.getenv("MARIADB_HOST", "localhost"),
        port=int(os.getenv("MARIADB_PORT", "3306")),
        user=os.getenv("MARIADB_USER"),
        password=os.getenv("MARIADB_PASSWORD"),
        database=os.getenv("MARIADB_DATABASE"),
    )


def rows_to_dicts(cursor, rows):
    columns = [
        column[0]
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


@app.route("/products")
def read_products():
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return rows_to_dicts(cursor, cursor.fetchall())
    finally:
        cursor.close()
        connection.close()
```

En aplicaciones con muchas solicitudes, conviene usar pool de conexiones.

## Uso con pandas

Pandas suele conectarse a MariaDB mediante SQLAlchemy.

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine(
    "mariadb+mariadbconnector://root:secret@localhost:3306/app"
)

df = pd.read_sql(
    "SELECT * FROM products",
    engine
)

print(df.head())
```

También puede construirse un DataFrame desde resultados del cursor:

```python
import pandas as pd

cursor.execute("""
    SELECT id, name, price, stock
    FROM products
""")

rows = rows_to_dicts(cursor, cursor.fetchall())

df = pd.DataFrame(rows)
```

## Uso con SQLAlchemy

SQLAlchemy puede usar MariaDB Connector/Python como driver.

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mariadb+mariadbconnector://user:password@localhost:3306/database"
)
```

En ese caso:

```text
SQLAlchemy -> ORM / Core / engine
mariadb    -> driver hacia MariaDB
```

## Comparación con `mysql-connector-python`

## `mariadb`

```text
conector oficial de MariaDB
orientado principalmente a MariaDB
también puede conectarse a MySQL compatible
importación mediante import mariadb
```

## `mysql-connector-python`

```text
conector oficial de MySQL
orientado principalmente a MySQL
importación mediante import mysql.connector
```

## Regla práctica

Si el servidor principal es MariaDB, `mariadb` es una opción natural.

Si el servidor principal es MySQL, `mysql-connector-python` o `pymysql` suelen ser opciones comunes.

## Tipos de datos

`mariadb` convierte muchos tipos entre MariaDB y Python.

Ejemplos conceptuales:

```text
MariaDB VARCHAR / TEXT    -> Python str
MariaDB INT / BIGINT      -> Python int
MariaDB DECIMAL           -> Decimal
MariaDB FLOAT / DOUBLE    -> Python float
MariaDB DATE              -> datetime.date
MariaDB DATETIME          -> datetime.datetime
MariaDB BOOLEAN / TINYINT -> Python int o bool según contexto
MariaDB NULL              -> None
```

## Insertar fechas

```python
from datetime import date

cursor.execute(
    """
    INSERT INTO events (name, event_date)
    VALUES (?, ?)
    """,
    ("Cierre mensual", date(2026, 12, 31))
)

connection.commit()
```

## Insertar `None`

```python
cursor.execute(
    """
    INSERT INTO products (name, description)
    VALUES (?, ?)
    """,
    ("Laptop", None)
)

connection.commit()
```

`None` se convierte en `NULL`.

## Consultas dinámicas seguras

Los valores deben pasarse como parámetros.

```python
cursor.execute(
    """
    SELECT *
    FROM products
    WHERE name = ?
    """,
    (name,)
)
```

Sin embargo, los nombres de tablas o columnas no deben pasarse como parámetros.

Problemático:

```python
cursor.execute(
    "SELECT * FROM ?",
    ("products",)
)
```

Para nombres dinámicos, conviene usar una lista permitida.

```python
allowed_tables = {
    "products": "products",
    "clients": "clients"
}

table_name = allowed_tables.get(requested_table)

if table_name is None:
    raise ValueError("Tabla no permitida")

query = f"SELECT * FROM `{table_name}`"

cursor.execute(query)
```

Este patrón solo es aceptable cuando el nombre proviene de una lista controlada por el programa.

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
import os

import mariadb
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    return mariadb.connect(
        host=os.getenv("MARIADB_HOST", "localhost"),
        port=int(os.getenv("MARIADB_PORT", "3306")),
        user=os.getenv("MARIADB_USER"),
        password=os.getenv("MARIADB_PASSWORD"),
        database=os.getenv("MARIADB_DATABASE"),
    )
```

## `repositories.py`

```python
from .database import create_connection


def rows_to_dicts(cursor, rows):
    columns = [
        column[0]
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


def list_products():
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return rows_to_dicts(cursor, cursor.fetchall())
    finally:
        cursor.close()
        connection.close()


def get_product(product_id):
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            SELECT id, name, price, stock
            FROM products
            WHERE id = ?
            """,
            (product_id,)
        )

        row = cursor.fetchone()

        if row is None:
            return None

        return rows_to_dicts(cursor, [row])[0]
    finally:
        cursor.close()
        connection.close()


def create_product(name, price, stock):
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (?, ?, ?)
            """,
            (name, price, stock)
        )

        connection.commit()

        return cursor.lastrowid
    except Exception:
        connection.rollback()
        raise
    finally:
        cursor.close()
        connection.close()
```

## Separación por responsabilidades

```text
database.py      -> conexión o pool
repositories.py  -> consultas SQL
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Errores comunes

## Instalar el paquete y tratar de importar otro nombre

Instalación:

```bash
python -m pip install mariadb
```

Importación correcta:

```python
import mariadb
```

No se importa como:

```python
import maria_db
```

## Olvidar cerrar cursor y conexión

Menos recomendable:

```python
connection = mariadb.connect(...)
cursor = connection.cursor()
```

sin:

```python
cursor.close()
connection.close()
```

Más seguro:

```python
try:
    ...
finally:
    cursor.close()
    connection.close()
```

## Olvidar `commit()`

Problemático:

```python
cursor.execute(
    """
    INSERT INTO products (name, price)
    VALUES (?, ?)
    """,
    ("Laptop", 3500)
)
```

sin:

```python
connection.commit()
```

Los cambios pueden no quedar guardados.

## No hacer `rollback()` después de un error

Si ocurre un error durante una transacción, conviene revertir:

```python
connection.rollback()
```

## Usar placeholders de otro driver

Problemático:

```python
cursor.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

si el driver espera `?`.

Patrón seguro para este conector:

```python
cursor.execute(
    "SELECT * FROM products WHERE id = ?",
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

## Concatenar valores dentro del SQL

Problemático:

```python
cursor.execute(f"SELECT * FROM products WHERE name = '{name}'")
```

Correcto:

```python
cursor.execute(
    "SELECT * FROM products WHERE name = ?",
    (name,)
)
```

## Esperar diccionarios sin convertir filas

Si el cursor devuelve tuplas, esto falla:

```python
row = cursor.fetchone()
print(row["name"])
```

Solución:

```python
rows_to_dicts(cursor, rows)
```

## Guardar credenciales en el código

Problemático:

```python
password="secret"
```

Mejor:

```python
password=os.getenv("MARIADB_PASSWORD")
```

## Abrir una conexión por cada operación sin evaluar carga

En scripts pequeños está bien.

En aplicaciones web, conviene evaluar pool de conexiones.

## Buenas prácticas

## Usar parámetros en consultas

```python
cursor.execute(
    "SELECT * FROM products WHERE id = ?",
    (product_id,)
)
```

## Cerrar cursor y conexión

```python
cursor.close()
connection.close()
```

## Usar variables de entorno

```text
MARIADB_HOST
MARIADB_PORT
MARIADB_USER
MARIADB_PASSWORD
MARIADB_DATABASE
```

## Controlar transacciones

```python
connection.commit()
connection.rollback()
```

## Usar pool en aplicaciones web

```text
pool de conexiones
```

## Separar consultas en repositorios

```text
repositories.py
```

## Evitar SQL dinámico no controlado

Validar tablas y columnas dinámicas contra listas permitidas.

## Usar SQLAlchemy si se requiere ORM o integración amplia

```python
mariadb+mariadbconnector://user:password@host:3306/database
```

## Manejar errores específicos

```python
mariadb.IntegrityError
mariadb.Error
```

## Ejemplo integrado

```python
import os
from decimal import Decimal

import mariadb
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    return mariadb.connect(
        host=os.getenv("MARIADB_HOST", "localhost"),
        port=int(os.getenv("MARIADB_PORT", "3306")),
        user=os.getenv("MARIADB_USER"),
        password=os.getenv("MARIADB_PASSWORD"),
        database=os.getenv("MARIADB_DATABASE"),
    )


def rows_to_dicts(cursor, rows):
    columns = [
        column[0]
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


def create_table():
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS products (
                id INT AUTO_INCREMENT PRIMARY KEY,
                name VARCHAR(100) NOT NULL UNIQUE,
                price DECIMAL(10, 2) NOT NULL,
                stock INT NOT NULL DEFAULT 0,
                is_active BOOLEAN NOT NULL DEFAULT TRUE
            )
        """)

        connection.commit()
    finally:
        cursor.close()
        connection.close()


def create_product(name, price, stock=0):
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (?, ?, ?)
            """,
            (name, Decimal(str(price)), stock)
        )

        connection.commit()

        return cursor.lastrowid
    except mariadb.IntegrityError:
        connection.rollback()
        raise ValueError("El producto ya existe")
    except Exception:
        connection.rollback()
        raise
    finally:
        cursor.close()
        connection.close()


def list_products():
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute("""
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return rows_to_dicts(cursor, cursor.fetchall())
    finally:
        cursor.close()
        connection.close()


def get_product(product_id):
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE id = ?
            """,
            (product_id,)
        )

        row = cursor.fetchone()

        if row is None:
            return None

        return rows_to_dicts(cursor, [row])[0]
    finally:
        cursor.close()
        connection.close()


def update_stock(product_id, stock):
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            UPDATE products
            SET stock = ?
            WHERE id = ?
            """,
            (stock, product_id)
        )

        connection.commit()

        return cursor.rowcount
    except Exception:
        connection.rollback()
        raise
    finally:
        cursor.close()
        connection.close()


def deactivate_product(product_id):
    connection = create_connection()
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            UPDATE products
            SET is_active = FALSE
            WHERE id = ?
            """,
            (product_id,)
        )

        connection.commit()

        return cursor.rowcount
    except Exception:
        connection.rollback()
        raise
    finally:
        cursor.close()
        connection.close()


try:
    create_table()

    try:
        create_product("Laptop", 3500, stock=5)
        create_product("Mouse", 80, stock=20)
    except ValueError:
        pass

    products = list_products()

    for product in products:
        print(
            product["id"],
            product["name"],
            product["price"],
            product["stock"]
        )

except mariadb.Error as error:
    print("Error de MariaDB:", error)
```

## Relación con otras librerías

`mariadb` se relaciona especialmente con:

```text
mariadb
mysql
sqlalchemy
pandas
fastapi
flask
python-dotenv
pytest
alembic
```

## Relación con SQLAlchemy

SQLAlchemy puede usar MariaDB Connector/Python como driver:

```python
engine = create_engine(
    "mariadb+mariadbconnector://user:password@localhost:3306/database"
)
```

## Relación con pandas

Pandas suele conectarse a MariaDB mediante SQLAlchemy y un driver compatible.

```python
df = pd.read_sql("SELECT * FROM products", engine)
```

## Relación con FastAPI y Flask

Puede usarse directamente en backends web, pero para aplicaciones medianas o grandes conviene evaluar:

```text
pool de conexiones
capa repository
manejo centralizado de errores
variables de entorno
```

## Relación con Alembic

Si el proyecto usa SQLAlchemy sobre MariaDB, Alembic puede gestionar migraciones de esquema.

```text
SQLAlchemy -> modelos y engine
Alembic    -> migraciones
mariadb    -> driver hacia MariaDB
```

## Relación con PyMySQL

Ambos permiten conectar Python con servidores compatibles con MySQL.

`mariadb` es el conector oficial de MariaDB.

`pymysql` es una alternativa Python puro muy usada en proyectos y compatible con SQLAlchemy.

## Orden didáctico interno

```text
1. Propósito de mariadb
2. Instalación
3. Importación con import mariadb
4. Conexión a MariaDB
5. Connection y Cursor
6. execute()
7. Parámetros con ?
8. fetchone(), fetchall() y fetchmany()
9. Conversión de filas a diccionarios
10. INSERT, UPDATE, DELETE y lastrowid
11. Transacciones, commit y rollback
12. Procedimientos almacenados
13. Manejo de errores
14. Pool de conexiones
15. API asíncrona en versiones recientes
16. Uso con SQLAlchemy, pandas, FastAPI y Flask
17. Organización recomendada
18. Errores comunes
19. Buenas prácticas
```