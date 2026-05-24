# `oracledb`

## Propósito

`oracledb` es una librería externa para conectar Python con Oracle Database.

Se utiliza para ejecutar consultas SQL, insertar registros, actualizar datos, eliminar filas, llamar procedimientos PL/SQL, manejar transacciones, leer resultados, usar pools de conexiones y conectar aplicaciones Python con bases de datos Oracle.

La librería forma parte del proyecto conocido como `python-oracledb`.

## Naturaleza de la librería

`oracledb` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install oracledb
```

La importación habitual es:

```python
import oracledb
```

Aunque el proyecto se llama `python-oracledb`, el paquete se instala e importa como:

```text
oracledb
```

## Relación con Oracle Database

`oracledb` está diseñado específicamente para Oracle Database.

No se usa para conectarse a:

```text
SQLite
PostgreSQL
MySQL
SQL Server
MongoDB
Redis
```

Para esas bases se usan otros drivers:

```text
sqlite3    -> SQLite
psycopg    -> PostgreSQL
pymysql    -> MySQL / MariaDB
pyodbc     -> ODBC / SQL Server
pymongo    -> MongoDB
redis      -> Redis
oracledb   -> Oracle Database
```

## Idea central

La idea principal de `oracledb` es abrir una conexión con Oracle Database y ejecutar SQL o PL/SQL desde Python.

Flujo típico:

```text
Python -> oracledb.connect() -> Connection -> Cursor -> Oracle Database
```

Ejemplo mínimo:

```python
import oracledb

connection = oracledb.connect(
    user="usuario",
    password="contraseña",
    dsn="localhost:1521/XEPDB1"
)

try:
    with connection.cursor() as cursor:
        cursor.execute("SELECT 1 FROM dual")
        row = cursor.fetchone()

        print(row)
finally:
    connection.close()
```

## Cuándo usar `oracledb`

Conviene usar `oracledb` cuando se necesita:

```text
conectar Python con Oracle Database
ejecutar SQL directamente
leer datos desde Oracle
insertar registros
actualizar registros
eliminar registros
llamar procedimientos PL/SQL
trabajar con funciones almacenadas
usar pools de conexiones
integrar Oracle con FastAPI o Flask
usar SQLAlchemy con Oracle
```

## Cuándo no usar `oracledb`

No conviene usar `oracledb` cuando se necesita:

```text
conectarse a PostgreSQL
conectarse a MySQL
conectarse a SQLite
trabajar solo con archivos CSV o Excel
usar MongoDB
usar Redis
evitar SQL manual completamente
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
django ORM
sqlmodel
```

## Instalación

Instalación básica:

```bash
python -m pip install oracledb
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
oracledb==3.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import oracledb

print(oracledb.__version__)
```

## Modo Thin y modo Thick

`oracledb` puede trabajar en dos modos:

```text
Thin mode
Thick mode
```

## Thin mode

Es el modo por defecto.

Permite conectar directamente con Oracle Database sin instalar Oracle Client libraries.

```python
import oracledb

connection = oracledb.connect(
    user="usuario",
    password="contraseña",
    dsn="localhost:1521/XEPDB1"
)
```

## Thick mode

Usa Oracle Client libraries.

Puede ser necesario para ciertas funciones avanzadas o compatibilidad con entornos específicos.

Ejemplo conceptual:

```python
import oracledb

oracledb.init_oracle_client(
    lib_dir=r"C:\oracle\instantclient_23_5"
)
```

Después de inicializar el cliente, se crean conexiones normalmente.

```python
connection = oracledb.connect(
    user="usuario",
    password="contraseña",
    dsn="localhost:1521/XEPDB1"
)
```

## Regla práctica

Para proyectos nuevos, conviene comenzar con Thin mode.

Si se requiere una funcionalidad específica que necesita Oracle Client libraries, puede evaluarse Thick mode.

## DSN

El DSN indica a qué base Oracle conectarse.

Ejemplo simple:

```python
dsn = "localhost:1521/XEPDB1"
```

Partes conceptuales:

```text
localhost -> servidor
1521      -> puerto
XEPDB1    -> servicio de base de datos
```

## `makedsn()`

También puede construirse un DSN con `makedsn()`.

```python
dsn = oracledb.makedsn(
    host="localhost",
    port=1521,
    service_name="XEPDB1"
)
```

Uso:

```python
connection = oracledb.connect(
    user="usuario",
    password="contraseña",
    dsn=dsn
)
```

## Conexión básica

```python
import oracledb

connection = oracledb.connect(
    user="usuario",
    password="contraseña",
    dsn="localhost:1521/XEPDB1"
)

connection.close()
```

## Uso recomendado con `try` y `finally`

```python
import oracledb

connection = oracledb.connect(
    user="usuario",
    password="contraseña",
    dsn="localhost:1521/XEPDB1"
)

try:
    with connection.cursor() as cursor:
        cursor.execute("SELECT 1 FROM dual")
        print(cursor.fetchone())
finally:
    connection.close()
```

Este patrón asegura que la conexión se cierre aunque ocurra un error.

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
connection = oracledb.connect(
    user="usuario",
    password="contraseña",
    dsn="localhost:1521/XEPDB1"
)
```

Más conveniente:

```python
import os

import oracledb

connection = oracledb.connect(
    user=os.getenv("ORACLE_USER"),
    password=os.getenv("ORACLE_PASSWORD"),
    dsn=os.getenv("ORACLE_DSN")
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
ORACLE_USER=usuario
ORACLE_PASSWORD=contraseña
ORACLE_DSN=localhost:1521/XEPDB1
```

Código:

```python
import os

import oracledb
from dotenv import load_dotenv

load_dotenv()

connection = oracledb.connect(
    user=os.getenv("ORACLE_USER"),
    password=os.getenv("ORACLE_PASSWORD"),
    dsn=os.getenv("ORACLE_DSN")
)

connection.close()
```

## Connection

Una `Connection` representa una conexión abierta contra Oracle Database.

Desde una conexión se puede:

```text
crear cursores
ejecutar consultas
confirmar transacciones
revertir cambios
llamar procedimientos
cerrar la conexión
```

Ejemplo:

```python
connection = oracledb.connect(
    user="usuario",
    password="contraseña",
    dsn="localhost:1521/XEPDB1"
)
```

## Cerrar conexión

```python
connection.close()
```

Cerrar conexiones es importante para liberar recursos.

## Cursor

Un cursor permite ejecutar SQL y leer resultados.

```python
cursor = connection.cursor()
```

Uso con context manager:

```python
with connection.cursor() as cursor:
    cursor.execute("SELECT 1 FROM dual")
    row = cursor.fetchone()
```

## Ejecutar una consulta

```python
with connection.cursor() as cursor:
    cursor.execute("SELECT 1 AS value FROM dual")
    row = cursor.fetchone()

    print(row)
```

Salida conceptual:

```text
(1,)
```

## Tabla `dual`

En Oracle, `dual` es una tabla especial usada frecuentemente para consultas de una sola fila.

Ejemplo:

```sql
SELECT SYSDATE FROM dual
```

En Python:

```python
with connection.cursor() as cursor:
    cursor.execute("SELECT SYSDATE FROM dual")
    print(cursor.fetchone())
```

## Crear una tabla

```python
with connection.cursor() as cursor:
    cursor.execute("""
        CREATE TABLE products (
            id NUMBER GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
            name VARCHAR2(100) NOT NULL,
            price NUMBER(10, 2) NOT NULL,
            stock NUMBER DEFAULT 0 NOT NULL,
            is_active NUMBER(1) DEFAULT 1 NOT NULL
        )
    """)
```

En Oracle, si la tabla ya existe, `CREATE TABLE` puede generar error. En proyectos reales se suelen usar migraciones o scripts controlados.

## Insertar datos

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (:name, :price, :stock)
        """,
        {
            "name": "Laptop",
            "price": 3500,
            "stock": 5
        }
    )

connection.commit()
```

## Parámetros SQL

En `oracledb`, los parámetros pueden escribirse con nombres precedidos por `:`.

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = :product_id
    """,
    {
        "product_id": 1
    }
)
```

También pueden usarse parámetros posicionales.

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = :1
    """,
    [1]
)
```

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
    WHERE name = :name
    """,
    {
        "name": name
    }
)
```

Los valores deben pasarse como parámetros.

## Leer un resultado

## `fetchone()`

Devuelve una fila o `None`.

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        SELECT id, name, price
        FROM products
        WHERE id = :product_id
        """,
        {
            "product_id": 1
        }
    )

    row = cursor.fetchone()

    print(row)
```

## Leer todos los resultados

## `fetchall()`

```python
with connection.cursor() as cursor:
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
with connection.cursor() as cursor:
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
with connection.cursor() as cursor:
    cursor.execute("""
        SELECT id, name, price, stock
        FROM products
        ORDER BY name
    """)

    for row in cursor:
        print(row)
```

Este patrón puede evitar cargar todos los resultados de golpe.

## Convertir filas a diccionarios

```python
def rows_to_dicts(cursor, rows):
    columns = [
        column[0].lower()
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


with connection.cursor() as cursor:
    cursor.execute("""
        SELECT id, name, price, stock
        FROM products
    """)

    rows = rows_to_dicts(cursor, cursor.fetchall())

    print(rows)
```

## `cursor.description`

`cursor.description` contiene metadatos de las columnas devueltas.

```python
with connection.cursor() as cursor:
    cursor.execute("""
        SELECT id, name, price
        FROM products
    """)

    columns = [
        column[0]
        for column in cursor.description
    ]

    print(columns)
```

## Insertar varios registros

## `executemany()`

```python
products = [
    {"name": "Laptop", "price": 3500, "stock": 5},
    {"name": "Mouse", "price": 80, "stock": 20},
    {"name": "Teclado", "price": 150, "stock": 10},
]

with connection.cursor() as cursor:
    cursor.executemany(
        """
        INSERT INTO products (name, price, stock)
        VALUES (:name, :price, :stock)
        """,
        products
    )

connection.commit()
```

## Actualizar datos

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        UPDATE products
        SET price = :price
        WHERE id = :product_id
        """,
        {
            "price": 3600,
            "product_id": 1
        }
    )

connection.commit()
```

## Eliminar datos

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        DELETE FROM products
        WHERE id = :product_id
        """,
        {
            "product_id": 1
        }
    )

connection.commit()
```

## Eliminación lógica

En aplicaciones reales, puede preferirse marcar registros como inactivos.

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        UPDATE products
        SET is_active = 0
        WHERE id = :product_id
        """,
        {
            "product_id": 1
        }
    )

connection.commit()
```

## Obtener id generado

Oracle puede devolver valores usando `RETURNING INTO`.

```python
with connection.cursor() as cursor:
    new_id = cursor.var(oracledb.NUMBER)

    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (:name, :price, :stock)
        RETURNING id INTO :new_id
        """,
        {
            "name": "Laptop",
            "price": 3500,
            "stock": 5,
            "new_id": new_id
        }
    )

    connection.commit()

    print(new_id.getvalue()[0])
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
    with connection.cursor() as cursor:
        cursor.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (:name, :price, :stock)
            """,
            {
                "name": "Laptop",
                "price": 3500,
                "stock": 5
            }
        )

    connection.commit()
except Exception:
    connection.rollback()
    raise
```

## Rollback

```python
try:
    with connection.cursor() as cursor:
        cursor.execute(...)
    connection.commit()
except Exception:
    connection.rollback()
    raise
```

Después de ciertos errores, conviene hacer `rollback()` antes de continuar usando la conexión.

## Autocommit

Puede activarse `autocommit`.

```python
connection.autocommit = True
```

Con `autocommit=True`, cada sentencia se confirma automáticamente.

Para operaciones de negocio relacionadas, suele ser preferible manejar transacciones de forma explícita.

## Llamar procedimientos PL/SQL

Oracle Database usa PL/SQL para procedimientos y funciones almacenadas.

## `callproc()`

```python
with connection.cursor() as cursor:
    cursor.callproc(
        "actualizar_stock",
        [1, 10]
    )

connection.commit()
```

## `callfunc()`

Para llamar una función almacenada:

```python
with connection.cursor() as cursor:
    result = cursor.callfunc(
        "calcular_precio_total",
        oracledb.NUMBER,
        [1]
    )

    print(result)
```

El segundo argumento indica el tipo de retorno.

## Bloque PL/SQL anónimo

```python
with connection.cursor() as cursor:
    cursor.execute("""
        BEGIN
            actualizar_stock(:product_id, :new_stock);
        END;
    """,
    {
        "product_id": 1,
        "new_stock": 10
    })

connection.commit()
```

## Tipos de datos frecuentes

Oracle y Python convierten muchos tipos automáticamente.

Ejemplos conceptuales:

```text
Oracle VARCHAR2  -> Python str
Oracle NUMBER    -> Python int, float o Decimal según contexto
Oracle DATE      -> datetime.datetime
Oracle TIMESTAMP -> datetime.datetime
Oracle CLOB      -> texto grande
Oracle BLOB      -> datos binarios
NULL             -> None
```

## Insertar fechas

```python
from datetime import datetime

with connection.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO events (name, event_date)
        VALUES (:name, :event_date)
        """,
        {
            "name": "Cierre mensual",
            "event_date": datetime(2026, 12, 31)
        }
    )

connection.commit()
```

## Insertar `None`

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO products (name, description)
        VALUES (:name, :description)
        """,
        {
            "name": "Laptop",
            "description": None
        }
    )

connection.commit()
```

`None` se convierte en `NULL`.

## LOBs

Oracle puede trabajar con objetos grandes como CLOB y BLOB.

## CLOB

Un CLOB almacena texto grande.

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO documents (title, content)
        VALUES (:title, :content)
        """,
        {
            "title": "Documento",
            "content": "Texto largo"
        }
    )

connection.commit()
```

## BLOB

Un BLOB almacena datos binarios.

```python
from pathlib import Path

content = Path("archivo.pdf").read_bytes()

with connection.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO files (filename, content)
        VALUES (:filename, :content)
        """,
        {
            "filename": "archivo.pdf",
            "content": content
        }
    )

connection.commit()
```

## Pool de conexiones

En aplicaciones web no conviene abrir y cerrar una conexión por cada operación.

Puede usarse un pool.

```python
pool = oracledb.create_pool(
    user="usuario",
    password="contraseña",
    dsn="localhost:1521/XEPDB1",
    min=1,
    max=5,
    increment=1
)
```

Uso:

```python
with pool.acquire() as connection:
    with connection.cursor() as cursor:
        cursor.execute("SELECT 1 FROM dual")
        print(cursor.fetchone())
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

## Manejo de errores

`oracledb` expone excepciones para errores de base de datos.

```python
import oracledb

try:
    with connection.cursor() as cursor:
        cursor.execute("SELECT * FROM tabla_que_no_existe")
except oracledb.DatabaseError as error:
    print("Error de base de datos:", error)
```

## Errores frecuentes

```text
oracledb.Error
oracledb.DatabaseError
oracledb.IntegrityError
oracledb.OperationalError
oracledb.ProgrammingError
```

## Error de integridad

```python
try:
    with connection.cursor() as cursor:
        cursor.execute(
            """
            INSERT INTO products (name, price)
            VALUES (:name, :price)
            """,
            {
                "name": "Laptop",
                "price": 3500
            }
        )

    connection.commit()
except oracledb.IntegrityError:
    connection.rollback()
    print("No se pudo insertar el registro por una restricción de integridad")
```

## Error de conexión

```python
try:
    connection = oracledb.connect(
        user="usuario",
        password="contraseña",
        dsn="localhost:1521/XEPDB1"
    )
except oracledb.OperationalError as error:
    print("No se pudo conectar a Oracle Database")
    print(error)
```

## Uso con pandas

Pandas puede leer datos de Oracle usando SQLAlchemy o una conexión compatible.

Una forma común es usar SQLAlchemy con `oracledb`.

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine(
    "oracle+oracledb://usuario:contraseña@localhost:1521/?service_name=XEPDB1"
)

df = pd.read_sql(
    "SELECT * FROM products",
    engine
)

print(df.head())
```

## Uso con SQLAlchemy

SQLAlchemy puede usar `oracledb` como driver.

```python
from sqlalchemy import create_engine

engine = create_engine(
    "oracle+oracledb://usuario:contraseña@localhost:1521/?service_name=XEPDB1"
)
```

En ese caso:

```text
SQLAlchemy -> ORM / Core / engine
oracledb   -> driver hacia Oracle Database
```

## Uso con FastAPI

```python
import os

import oracledb
from fastapi import FastAPI

app = FastAPI()

pool = oracledb.create_pool(
    user=os.getenv("ORACLE_USER"),
    password=os.getenv("ORACLE_PASSWORD"),
    dsn=os.getenv("ORACLE_DSN"),
    min=1,
    max=5,
    increment=1
)


def rows_to_dicts(cursor, rows):
    columns = [
        column[0].lower()
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


@app.get("/products")
def read_products():
    with pool.acquire() as connection:
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT id, name, price, stock
                FROM products
                WHERE is_active = 1
                ORDER BY name
            """)

            return rows_to_dicts(cursor, cursor.fetchall())
```

## Uso con Flask

```python
import os

import oracledb
from flask import Flask

app = Flask(__name__)

pool = oracledb.create_pool(
    user=os.getenv("ORACLE_USER"),
    password=os.getenv("ORACLE_PASSWORD"),
    dsn=os.getenv("ORACLE_DSN"),
    min=1,
    max=5,
    increment=1
)


@app.route("/products")
def read_products():
    with pool.acquire() as connection:
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT id, name, price, stock
                FROM products
                WHERE is_active = 1
                ORDER BY name
            """)

            columns = [
                column[0].lower()
                for column in cursor.description
            ]

            return [
                dict(zip(columns, row))
                for row in cursor.fetchall()
            ]
```

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

import oracledb
from dotenv import load_dotenv

load_dotenv()


def create_pool():
    return oracledb.create_pool(
        user=os.getenv("ORACLE_USER"),
        password=os.getenv("ORACLE_PASSWORD"),
        dsn=os.getenv("ORACLE_DSN"),
        min=1,
        max=5,
        increment=1
    )
```

## `repositories.py`

```python
from .database import create_pool

pool = create_pool()


def rows_to_dicts(cursor, rows):
    columns = [
        column[0].lower()
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


def list_products():
    with pool.acquire() as connection:
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT id, name, price, stock
                FROM products
                WHERE is_active = 1
                ORDER BY name
            """)

            return rows_to_dicts(cursor, cursor.fetchall())


def get_product(product_id):
    with pool.acquire() as connection:
        with connection.cursor() as cursor:
            cursor.execute(
                """
                SELECT id, name, price, stock
                FROM products
                WHERE id = :product_id
                """,
                {
                    "product_id": product_id
                }
            )

            row = cursor.fetchone()

            if row is None:
                return None

            return rows_to_dicts(cursor, [row])[0]


def create_product(name, price, stock):
    with pool.acquire() as connection:
        try:
            with connection.cursor() as cursor:
                new_id = cursor.var(oracledb.NUMBER)

                cursor.execute(
                    """
                    INSERT INTO products (name, price, stock)
                    VALUES (:name, :price, :stock)
                    RETURNING id INTO :new_id
                    """,
                    {
                        "name": name,
                        "price": price,
                        "stock": stock,
                        "new_id": new_id
                    }
                )

            connection.commit()

            return new_id.getvalue()[0]
        except Exception:
            connection.rollback()
            raise
```

## Separación por responsabilidades

```text
database.py      -> conexión o pool
repositories.py  -> consultas SQL
services.py      -> reglas de negocio
main.py          -> entrada de aplicación
```

## Errores comunes

## Instalar el paquete con otro nombre

La instalación correcta es:

```bash
python -m pip install oracledb
```

La importación correcta es:

```python
import oracledb
```

## Confundir `oracledb` con `cx_Oracle`

`cx_Oracle` fue el driver anterior.

Para proyectos nuevos, conviene usar `oracledb`.

## No cerrar conexiones

Menos recomendable:

```python
connection = oracledb.connect(...)
```

sin:

```python
connection.close()
```

Más seguro:

```python
try:
    ...
finally:
    connection.close()
```

## Olvidar `commit()`

Problemático:

```python
cursor.execute(
    """
    INSERT INTO products (name, price)
    VALUES (:name, :price)
    """,
    {
        "name": "Laptop",
        "price": 3500
    }
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

## Concatenar valores dentro del SQL

Problemático:

```python
cursor.execute(f"SELECT * FROM products WHERE name = '{name}'")
```

Correcto:

```python
cursor.execute(
    """
    SELECT *
    FROM products
    WHERE name = :name
    """,
    {
        "name": name
    }
)
```

## Usar placeholders de otro driver

Problemático:

```python
cursor.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

o:

```python
cursor.execute(
    "SELECT * FROM products WHERE id = ?",
    (product_id,)
)
```

En `oracledb`, es más habitual:

```python
cursor.execute(
    """
    SELECT *
    FROM products
    WHERE id = :product_id
    """,
    {
        "product_id": product_id
    }
)
```

## Guardar credenciales en el código

Problemático:

```python
password="contraseña"
```

Mejor:

```python
password=os.getenv("ORACLE_PASSWORD")
```

## Usar una conexión nueva por cada solicitud sin evaluar carga

En aplicaciones web, conviene usar pool de conexiones.

```python
oracledb.create_pool(...)
```

## No distinguir Thin mode y Thick mode

El modo Thin suele ser suficiente para empezar.

El modo Thick requiere Oracle Client libraries y se usa cuando se necesita funcionalidad específica o compatibilidad particular.

## Buenas prácticas

## Usar parámetros en consultas

```python
WHERE id = :product_id
```

## Usar variables de entorno

```text
ORACLE_USER
ORACLE_PASSWORD
ORACLE_DSN
```

## Cerrar conexiones

```python
connection.close()
```

## Controlar transacciones

```python
connection.commit()
connection.rollback()
```

## Usar pool en aplicaciones web

```python
oracledb.create_pool(...)
```

## Separar consultas en funciones

```python
def list_products():
    ...
```

## Convertir filas a diccionarios para APIs

```python
dict(zip(columns, row))
```

## Usar SQLAlchemy si se requiere ORM

```python
oracle+oracledb://...
```

## Manejar errores de base de datos

```python
except oracledb.DatabaseError:
    ...
```

## Ejemplo integrado

```python
import os
from decimal import Decimal

import oracledb
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    return oracledb.connect(
        user=os.getenv("ORACLE_USER"),
        password=os.getenv("ORACLE_PASSWORD"),
        dsn=os.getenv("ORACLE_DSN")
    )


def rows_to_dicts(cursor, rows):
    columns = [
        column[0].lower()
        for column in cursor.description
    ]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


def create_table():
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            try:
                cursor.execute("""
                    CREATE TABLE products (
                        id NUMBER GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
                        name VARCHAR2(100) NOT NULL UNIQUE,
                        price NUMBER(10, 2) NOT NULL,
                        stock NUMBER DEFAULT 0 NOT NULL,
                        is_active NUMBER(1) DEFAULT 1 NOT NULL
                    )
                """)
            except oracledb.DatabaseError:
                pass

        connection.commit()
    finally:
        connection.close()


def create_product(name, price, stock=0):
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            new_id = cursor.var(oracledb.NUMBER)

            cursor.execute(
                """
                INSERT INTO products (name, price, stock)
                VALUES (:name, :price, :stock)
                RETURNING id INTO :new_id
                """,
                {
                    "name": name,
                    "price": Decimal(str(price)),
                    "stock": stock,
                    "new_id": new_id
                }
            )

        connection.commit()

        return new_id.getvalue()[0]
    except oracledb.IntegrityError:
        connection.rollback()
        raise ValueError("El producto ya existe")
    except Exception:
        connection.rollback()
        raise
    finally:
        connection.close()


def list_products():
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT id, name, price, stock, is_active
                FROM products
                WHERE is_active = 1
                ORDER BY name
            """)

            return rows_to_dicts(cursor, cursor.fetchall())
    finally:
        connection.close()


def update_stock(product_id, stock):
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute(
                """
                UPDATE products
                SET stock = :stock
                WHERE id = :product_id
                """,
                {
                    "stock": stock,
                    "product_id": product_id
                }
            )

            affected_rows = cursor.rowcount

        connection.commit()

        return affected_rows
    except Exception:
        connection.rollback()
        raise
    finally:
        connection.close()


def deactivate_product(product_id):
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute(
                """
                UPDATE products
                SET is_active = 0
                WHERE id = :product_id
                """,
                {
                    "product_id": product_id
                }
            )

            affected_rows = cursor.rowcount

        connection.commit()

        return affected_rows
    except Exception:
        connection.rollback()
        raise
    finally:
        connection.close()


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
```

## Relación con otras librerías

`oracledb` se relaciona especialmente con:

```text
oracle database
sqlalchemy
pandas
fastapi
flask
python-dotenv
pytest
alembic
```

## Relación con SQLAlchemy

SQLAlchemy puede usar `oracledb` como driver:

```python
engine = create_engine(
    "oracle+oracledb://usuario:contraseña@localhost:1521/?service_name=XEPDB1"
)
```

## Relación con pandas

Pandas puede leer datos de Oracle mediante SQLAlchemy.

```python
df = pd.read_sql("SELECT * FROM products", engine)
```

## Relación con FastAPI y Flask

`oracledb` puede usarse directamente en backends web.

En aplicaciones medianas o grandes, conviene evaluar:

```text
pool de conexiones
capa repository
manejo centralizado de errores
variables de entorno
```

## Relación con Alembic

Si el proyecto usa SQLAlchemy sobre Oracle, Alembic puede gestionar migraciones de esquema.

```text
SQLAlchemy -> modelos y engine
Alembic    -> migraciones
oracledb   -> driver hacia Oracle Database
```

## Orden didáctico interno

```text
1. Propósito de oracledb
2. Instalación
3. Relación con Oracle Database
4. Thin mode y Thick mode
5. Conexión y DSN
6. Connection y Cursor
7. execute()
8. Parámetros con :nombre
9. fetchone(), fetchall() y fetchmany()
10. INSERT, UPDATE, DELETE y RETURNING INTO
11. Transacciones, commit y rollback
12. PL/SQL, callproc() y callfunc()
13. Tipos de datos y LOBs
14. Pool de conexiones
15. Uso con SQLAlchemy, pandas, FastAPI y Flask
16. Errores comunes
17. Buenas prácticas
```