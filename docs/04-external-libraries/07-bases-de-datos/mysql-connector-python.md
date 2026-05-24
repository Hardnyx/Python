# `mysql-connector-python`

## Propósito

`mysql-connector-python` es una librería externa para conectar Python con bases de datos MySQL.

Se utiliza para ejecutar consultas SQL, insertar registros, actualizar datos, eliminar filas, leer resultados, manejar transacciones, usar cursores, trabajar con procedimientos almacenados y conectar aplicaciones Python con servidores MySQL.

Es un driver de base de datos. Su función principal no es modelar tablas como clases, sino permitir la comunicación directa entre Python y MySQL.

## Naturaleza de la librería

`mysql-connector-python` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install mysql-connector-python
```

La importación habitual es:

```python
import mysql.connector
```

Aunque el paquete se instala como:

```text
mysql-connector-python
```

se importa como:

```python
mysql.connector
```

## Relación con MySQL

`mysql-connector-python` está diseñado para conectarse a servidores MySQL.

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
sqlite3                  -> SQLite
psycopg                  -> PostgreSQL
pyodbc                   -> SQL Server / ODBC
oracledb                 -> Oracle Database
pymongo                  -> MongoDB
redis                    -> Redis
mysql-connector-python   -> MySQL
```

## Idea central

La idea principal de `mysql-connector-python` es abrir una conexión con MySQL y ejecutar comandos SQL desde Python.

Flujo típico:

```text
Python -> mysql.connector.connect() -> Connection -> Cursor -> MySQL
```

Ejemplo mínimo:

```python
import mysql.connector

connection = mysql.connector.connect(
    host="localhost",
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

## Cuándo usar `mysql-connector-python`

Conviene usar `mysql-connector-python` cuando se necesita:

```text
conectar Python con MySQL
usar el driver oficial de MySQL
ejecutar SQL directamente
leer datos desde MySQL
insertar registros
actualizar registros
eliminar registros
manejar transacciones
usar procedimientos almacenados
crear scripts de carga o extracción
integrar MySQL con FastAPI o Flask
usar SQLAlchemy con MySQL
```

## Cuándo no usar `mysql-connector-python`

No conviene usar `mysql-connector-python` cuando se necesita:

```text
conectarse a PostgreSQL
conectarse a SQLite
conectarse a SQL Server
trabajar solo con archivos CSV o Excel
usar MongoDB
usar Redis
evitar escribir SQL manualmente
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
python -m pip install mysql-connector-python
```

Actualizar:

```bash
python -m pip install mysql-connector-python --upgrade
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
mysql-connector-python==9.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import mysql.connector

print(mysql.connector.__version__)
```

## Conexión básica

```python
import mysql.connector

connection = mysql.connector.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    port=3306
)

connection.close()
```

## Parámetros frecuentes de conexión

```text
host       -> servidor MySQL
user       -> usuario
password   -> contraseña
database   -> base de datos
port       -> puerto, normalmente 3306
charset    -> codificación
autocommit -> confirmación automática
connection_timeout -> tiempo máximo de espera
```

Ejemplo:

```python
connection = mysql.connector.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    port=3306,
    charset="utf8mb4"
)
```

## Uso recomendado con `try` y `finally`

```python
import mysql.connector

connection = mysql.connector.connect(
    host="localhost",
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
connection = mysql.connector.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app"
)
```

Más conveniente:

```python
import os

import mysql.connector

connection = mysql.connector.connect(
    host=os.getenv("MYSQL_HOST"),
    user=os.getenv("MYSQL_USER"),
    password=os.getenv("MYSQL_PASSWORD"),
    database=os.getenv("MYSQL_DATABASE"),
    port=int(os.getenv("MYSQL_PORT", "3306")),
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=secret
MYSQL_DATABASE=app
```

Código:

```python
import os

import mysql.connector
from dotenv import load_dotenv

load_dotenv()

connection = mysql.connector.connect(
    host=os.getenv("MYSQL_HOST"),
    port=int(os.getenv("MYSQL_PORT", "3306")),
    user=os.getenv("MYSQL_USER"),
    password=os.getenv("MYSQL_PASSWORD"),
    database=os.getenv("MYSQL_DATABASE"),
)

connection.close()
```

## Connection

Una conexión representa una sesión abierta contra MySQL.

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
connection = mysql.connector.connect(
    host="localhost",
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

## Verificar conexión

```python
if connection.is_connected():
    print("Conexión activa")
```

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
    VALUES (%s, %s, %s)
    """,
    ("Laptop", 3500, 5)
)

connection.commit()
```

## Parámetros SQL

En `mysql-connector-python`, los valores se pasan con placeholders `%s`.

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = %s
    """,
    (product_id,)
)
```

Aunque el valor sea entero o decimal, el placeholder sigue siendo:

```text
%s
```

No se usa:

```text
%d
%f
?
```

## Tupla de un solo parámetro

Cuando se pasa un solo parámetro, debe usarse una tupla de un elemento.

Correcto:

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = %s
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
    WHERE id = %s
    """,
    (product_id)
)
```

La coma es necesaria.

## Parámetros con diccionario

También pueden usarse parámetros con nombre.

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE name = %(name)s
    """,
    {
        "name": "Laptop"
    }
)
```

Este patrón puede mejorar la legibilidad cuando hay muchos parámetros.

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
    WHERE name = %s
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
    WHERE id = %s
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

## Cursor con diccionarios

Por defecto, las filas suelen recibirse como tuplas.

```text
(1, "Laptop", Decimal("3500.00"), 5)
```

Para recibir diccionarios:

```python
cursor = connection.cursor(dictionary=True)

cursor.execute("""
    SELECT id, name, price, stock
    FROM products
""")

rows = cursor.fetchall()

for row in rows:
    print(row["name"])
```

Salida conceptual:

```text
{"id": 1, "name": "Laptop", "price": Decimal("3500.00"), "stock": 5}
```

## Cursor buffered

Un cursor buffered obtiene el conjunto de resultados completo después de ejecutar la consulta.

```python
cursor = connection.cursor(buffered=True)
```

Puede ser útil cuando se necesita ejecutar otra consulta antes de consumir todos los resultados del cursor anterior.

Debe usarse con cuidado si la consulta devuelve muchas filas.

## Cursor prepared

Un cursor preparado permite usar prepared statements.

```python
cursor = connection.cursor(prepared=True)

cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = %s
    """,
    (1,)
)

row = cursor.fetchone()
```

Este patrón puede ser útil para consultas repetidas, aunque en aplicaciones normales no siempre es necesario.

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
    VALUES (%s, %s, %s)
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
    VALUES (%s, %s, %s)
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
    SET price = %s, stock = %s
    WHERE id = %s
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
    WHERE id = %s
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
    WHERE id = %s
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
    SET stock = %s
    WHERE id = %s
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
        VALUES (%s, %s, %s)
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

## `start_transaction()`

También puede iniciarse una transacción explícitamente.

```python
connection.start_transaction()

try:
    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (%s, %s, %s)
        """,
        ("Laptop", 3500, 5)
    )

    connection.commit()
except Exception:
    connection.rollback()
    raise
```

## Autocommit

`autocommit` confirma automáticamente cada sentencia.

```python
connection.autocommit = True
```

También puede configurarse al conectar:

```python
connection = mysql.connector.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    autocommit=True
)
```

Para operaciones relacionadas, suele ser mejor manejar transacciones explícitamente.

## Procedimientos almacenados

MySQL permite procedimientos almacenados.

```python
cursor.callproc(
    "update_stock",
    [1, 10]
)
```

Después se pueden revisar resultados según lo que devuelva el procedimiento.

Ejemplo conceptual:

```python
for result in cursor.stored_results():
    print(result.fetchall())
```

## Consultas de lectura

```python
def list_products(connection):
    cursor = connection.cursor(dictionary=True)

    try:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return cursor.fetchall()
    finally:
        cursor.close()
```

## Consulta por id

```python
def get_product(connection, product_id):
    cursor = connection.cursor(dictionary=True)

    try:
        cursor.execute(
            """
            SELECT id, name, price, stock
            FROM products
            WHERE id = %s
            """,
            (product_id,)
        )

        return cursor.fetchone()
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
            VALUES (%s, %s, %s)
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
            SET stock = %s
            WHERE id = %s
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

Importación:

```python
import mysql.connector
from mysql.connector import Error
```

Ejemplo:

```python
try:
    cursor.execute("SELECT * FROM table_that_does_not_exist")
except Error as error:
    print("Error de MySQL:", error)
```

## Errores frecuentes

```text
mysql.connector.Error
mysql.connector.InterfaceError
mysql.connector.DatabaseError
mysql.connector.IntegrityError
mysql.connector.ProgrammingError
mysql.connector.OperationalError
```

## Error de integridad

```python
try:
    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (%s, %s, %s)
        """,
        ("Laptop", 3500, 5)
    )

    connection.commit()
except mysql.connector.IntegrityError:
    connection.rollback()
    print("No se pudo insertar el registro por una restricción de integridad")
```

## Error de conexión

```python
try:
    connection = mysql.connector.connect(
        host="localhost",
        user="root",
        password="secret",
        database="app"
    )
except mysql.connector.Error as error:
    print("No se pudo conectar a MySQL")
    print(error)
```

## Pool de conexiones

`mysql-connector-python` incluye soporte para pool de conexiones.

Importación:

```python
from mysql.connector import pooling
```

Crear pool:

```python
from mysql.connector import pooling

pool = pooling.MySQLConnectionPool(
    pool_name="app_pool",
    pool_size=5,
    host="localhost",
    user="root",
    password="secret",
    database="app"
)
```

Usar conexión del pool:

```python
connection = pool.get_connection()

try:
    cursor = connection.cursor(dictionary=True)
    cursor.execute("SELECT 1")
    print(cursor.fetchone())
finally:
    cursor.close()
    connection.close()
```

Cuando se usa pool, `connection.close()` devuelve la conexión al pool.

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

## Conexión asíncrona

`mysql-connector-python` también incluye una API asíncrona mediante `mysql.connector.aio`.

Importación:

```python
from mysql.connector.aio import connect
```

Ejemplo básico:

```python
import asyncio

from mysql.connector.aio import connect


async def main():
    connection = await connect(
        host="localhost",
        user="root",
        password="secret",
        database="app"
    )

    cursor = await connection.cursor()

    try:
        await cursor.execute("SELECT 1")
        rows = await cursor.fetchall()

        print(rows)
    finally:
        await cursor.close()
        await connection.close()


asyncio.run(main())
```

## Cuándo usar la API asíncrona

Conviene evaluarla cuando:

```text
la aplicación usa asyncio
se trabaja con FastAPI en endpoints async
hay muchas operaciones de entrada y salida
se quiere evitar bloquear el event loop
```

Para scripts simples, la API síncrona suele ser más directa.

## Uso con FastAPI

Ejemplo sencillo con conexión síncrona:

```python
import os
from contextlib import asynccontextmanager

import mysql.connector
from fastapi import FastAPI, HTTPException

pool = None


def create_pool():
    from mysql.connector import pooling

    return pooling.MySQLConnectionPool(
        pool_name="app_pool",
        pool_size=5,
        host=os.getenv("MYSQL_HOST", "localhost"),
        user=os.getenv("MYSQL_USER", "root"),
        password=os.getenv("MYSQL_PASSWORD", "secret"),
        database=os.getenv("MYSQL_DATABASE", "app"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
    )


@asynccontextmanager
async def lifespan(app: FastAPI):
    global pool

    pool = create_pool()

    yield


app = FastAPI(lifespan=lifespan)


@app.get("/products")
def read_products():
    connection = pool.get_connection()
    cursor = connection.cursor(dictionary=True)

    try:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return cursor.fetchall()
    finally:
        cursor.close()
        connection.close()
```

Para una aplicación totalmente asíncrona, puede evaluarse `mysql.connector.aio` u otro driver asíncrono compatible.

## Uso con Flask

```python
import os

import mysql.connector
from flask import Flask

app = Flask(__name__)


def create_connection():
    return mysql.connector.connect(
        host=os.getenv("MYSQL_HOST", "localhost"),
        user=os.getenv("MYSQL_USER", "root"),
        password=os.getenv("MYSQL_PASSWORD", "secret"),
        database=os.getenv("MYSQL_DATABASE", "app"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
    )


@app.route("/products")
def read_products():
    connection = create_connection()
    cursor = connection.cursor(dictionary=True)

    try:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return cursor.fetchall()
    finally:
        cursor.close()
        connection.close()
```

En aplicaciones con muchas solicitudes, conviene usar pool de conexiones.

## Uso con pandas

Pandas suele conectarse a MySQL mediante SQLAlchemy.

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+mysqlconnector://root:secret@localhost:3306/app"
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

cursor = connection.cursor(dictionary=True)

cursor.execute("""
    SELECT id, name, price, stock
    FROM products
""")

df = pd.DataFrame(cursor.fetchall())
```

## Uso con SQLAlchemy

SQLAlchemy puede usar `mysql-connector-python` como driver.

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+mysqlconnector://root:secret@localhost:3306/app"
)
```

En ese caso:

```text
SQLAlchemy              -> ORM / Core / engine
mysql-connector-python  -> driver hacia MySQL
```

## Comparación con PyMySQL

## `mysql-connector-python`

```text
driver oficial de MySQL
paquete mantenido por Oracle
importación mediante mysql.connector
incluye API síncrona y API async en mysql.connector.aio
```

## `pymysql`

```text
driver Python puro para MySQL y MariaDB
importación mediante pymysql
muy usado en proyectos y compatibilidad con SQLAlchemy
API DB API sencilla
```

## Regla práctica

Si se quiere usar el driver oficial de MySQL, `mysql-connector-python` es una opción natural.

Si se busca compatibilidad simple y amplia en proyectos Python, `pymysql` también es común.

En proyectos con SQLAlchemy, conviene revisar qué driver se adapta mejor al entorno específico.

## Tipos de datos

`mysql-connector-python` convierte muchos tipos entre MySQL y Python.

Ejemplos conceptuales:

```text
MySQL VARCHAR / TEXT    -> Python str
MySQL INT / BIGINT      -> Python int
MySQL DECIMAL           -> Decimal
MySQL FLOAT / DOUBLE    -> Python float
MySQL DATE              -> datetime.date
MySQL DATETIME          -> datetime.datetime
MySQL BOOLEAN / TINYINT -> Python int o bool según contexto
MySQL NULL              -> None
```

## Insertar fechas

```python
from datetime import date

cursor.execute(
    """
    INSERT INTO events (name, event_date)
    VALUES (%s, %s)
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
    VALUES (%s, %s)
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
    WHERE name = %s
    """,
    (name,)
)
```

Sin embargo, los nombres de tablas o columnas no deben pasarse como `%s`.

Problemático:

```python
cursor.execute(
    "SELECT * FROM %s",
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

import mysql.connector
from dotenv import load_dotenv
from mysql.connector import pooling

load_dotenv()


def create_pool():
    return pooling.MySQLConnectionPool(
        pool_name="app_pool",
        pool_size=5,
        host=os.getenv("MYSQL_HOST", "localhost"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
        user=os.getenv("MYSQL_USER"),
        password=os.getenv("MYSQL_PASSWORD"),
        database=os.getenv("MYSQL_DATABASE"),
    )
```

## `repositories.py`

```python
from .database import create_pool

pool = create_pool()


def list_products():
    connection = pool.get_connection()
    cursor = connection.cursor(dictionary=True)

    try:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return cursor.fetchall()
    finally:
        cursor.close()
        connection.close()


def get_product(product_id):
    connection = pool.get_connection()
    cursor = connection.cursor(dictionary=True)

    try:
        cursor.execute(
            """
            SELECT id, name, price, stock
            FROM products
            WHERE id = %s
            """,
            (product_id,)
        )

        return cursor.fetchone()
    finally:
        cursor.close()
        connection.close()


def create_product(name, price, stock):
    connection = pool.get_connection()
    cursor = connection.cursor()

    try:
        cursor.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
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
python -m pip install mysql-connector-python
```

Importación correcta:

```python
import mysql.connector
```

No se importa como:

```python
import mysql_connector_python
```

## Olvidar cerrar cursor y conexión

Menos recomendable:

```python
connection = mysql.connector.connect(...)
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
    VALUES (%s, %s)
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

## Usar placeholders incorrectos

Problemático:

```python
cursor.execute(
    "SELECT * FROM products WHERE id = ?",
    (product_id,)
)
```

Correcto:

```python
cursor.execute(
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

## Concatenar valores dentro del SQL

Problemático:

```python
cursor.execute(f"SELECT * FROM products WHERE name = '{name}'")
```

Correcto:

```python
cursor.execute(
    "SELECT * FROM products WHERE name = %s",
    (name,)
)
```

## Esperar diccionarios sin usar `dictionary=True`

Problemático:

```python
row = cursor.fetchone()
print(row["name"])
```

si el cursor devuelve tuplas.

Solución:

```python
cursor = connection.cursor(dictionary=True)
```

## Usar un cursor no buffered y dejar resultados pendientes

Si no se consumen todos los resultados de una consulta, puede haber problemas al ejecutar otra consulta con el mismo cursor o conexión.

Opciones:

```text
consumir los resultados
cerrar el cursor
usar cursor buffered cuando corresponda
```

## Guardar credenciales en el código

Problemático:

```python
password="secret"
```

Mejor:

```python
password=os.getenv("MYSQL_PASSWORD")
```

## Abrir una conexión por cada operación sin evaluar carga

En scripts pequeños está bien.

En aplicaciones web, conviene evaluar pool de conexiones.

## Buenas prácticas

## Usar parámetros en consultas

```python
cursor.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

## Cerrar cursor y conexión

```python
cursor.close()
connection.close()
```

## Usar `dictionary=True` cuando mejore la legibilidad

```python
cursor = connection.cursor(dictionary=True)
```

## Usar variables de entorno

```text
MYSQL_HOST
MYSQL_PORT
MYSQL_USER
MYSQL_PASSWORD
MYSQL_DATABASE
```

## Controlar transacciones

```python
connection.commit()
connection.rollback()
```

## Usar pool en aplicaciones web

```python
pooling.MySQLConnectionPool(...)
```

## Separar consultas en repositorios

```text
repositories.py
```

## Evitar SQL dinámico no controlado

Validar tablas y columnas dinámicas contra listas permitidas.

## Usar SQLAlchemy si se requiere ORM o integración amplia

```python
mysql+mysqlconnector://user:password@host:3306/database
```

## Manejar errores específicos

```python
mysql.connector.IntegrityError
mysql.connector.Error
```

## Ejemplo integrado

```python
import os
from decimal import Decimal

import mysql.connector
from dotenv import load_dotenv
from mysql.connector import Error, IntegrityError

load_dotenv()


def create_connection():
    return mysql.connector.connect(
        host=os.getenv("MYSQL_HOST", "localhost"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
        user=os.getenv("MYSQL_USER"),
        password=os.getenv("MYSQL_PASSWORD"),
        database=os.getenv("MYSQL_DATABASE"),
        charset="utf8mb4",
    )


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
            VALUES (%s, %s, %s)
            """,
            (name, Decimal(str(price)), stock)
        )

        connection.commit()

        return cursor.lastrowid
    except IntegrityError:
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
    cursor = connection.cursor(dictionary=True)

    try:
        cursor.execute("""
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return cursor.fetchall()
    finally:
        cursor.close()
        connection.close()


def get_product(product_id):
    connection = create_connection()
    cursor = connection.cursor(dictionary=True)

    try:
        cursor.execute(
            """
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE id = %s
            """,
            (product_id,)
        )

        return cursor.fetchone()
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
            SET stock = %s
            WHERE id = %s
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
            WHERE id = %s
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

except Error as error:
    print("Error de MySQL:", error)
```

## Ejemplo integrado asíncrono

```python
import asyncio
import os

from dotenv import load_dotenv
from mysql.connector.aio import connect

load_dotenv()


async def create_connection():
    return await connect(
        host=os.getenv("MYSQL_HOST", "localhost"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
        user=os.getenv("MYSQL_USER"),
        password=os.getenv("MYSQL_PASSWORD"),
        database=os.getenv("MYSQL_DATABASE"),
    )


async def list_products():
    connection = await create_connection()
    cursor = await connection.cursor(dictionary=True)

    try:
        await cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return await cursor.fetchall()
    finally:
        await cursor.close()
        await connection.close()


async def main():
    products = await list_products()

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

`mysql-connector-python` se relaciona especialmente con:

```text
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

SQLAlchemy puede usar `mysql-connector-python` como driver:

```python
engine = create_engine(
    "mysql+mysqlconnector://user:password@localhost:3306/database"
)
```

## Relación con pandas

Pandas suele conectarse a MySQL mediante SQLAlchemy y un driver compatible.

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

Si el proyecto usa SQLAlchemy sobre MySQL, Alembic puede gestionar migraciones de esquema.

```text
SQLAlchemy -> modelos y engine
Alembic    -> migraciones
mysql-connector-python -> driver hacia MySQL
```

## Relación con PyMySQL

Ambos permiten conectar Python con MySQL.

`mysql-connector-python` es el driver oficial de MySQL.

`pymysql` es una alternativa muy usada en Python y también puede integrarse con SQLAlchemy.

## Orden didáctico interno

```text
1. Propósito de mysql-connector-python
2. Instalación
3. Importación con mysql.connector
4. Conexión a MySQL
5. Connection y Cursor
6. execute()
7. Parámetros con %s
8. fetchone(), fetchall() y fetchmany()
9. Cursor dictionary, buffered y prepared
10. INSERT, UPDATE, DELETE y lastrowid
11. Transacciones, commit y rollback
12. Procedimientos almacenados
13. Manejo de errores
14. Pool de conexiones
15. API asíncrona con mysql.connector.aio
16. Uso con SQLAlchemy, pandas, FastAPI y Flask
17. Organización recomendada
18. Errores comunes
19. Buenas prácticas
```