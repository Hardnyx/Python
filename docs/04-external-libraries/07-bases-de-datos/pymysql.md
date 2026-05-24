# `pymysql`

## Propósito

`pymysql` es una librería externa para conectar Python con bases de datos MySQL o MariaDB.

Se utiliza para ejecutar consultas SQL, insertar registros, actualizar datos, eliminar filas, leer resultados, manejar transacciones y conectar aplicaciones Python con servidores MySQL o MariaDB.

Es un driver de base de datos. Su función principal no es modelar tablas como clases, sino permitir la comunicación directa entre Python y una base de datos MySQL compatible.

## Naturaleza de la librería

`pymysql` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install pymysql
```

La importación habitual es:

```python
import pymysql
```

También suele importarse el módulo de cursores:

```python
import pymysql.cursors
```

## Relación con MySQL y MariaDB

`pymysql` se usa para conectarse a bases de datos compatibles con MySQL.

Casos frecuentes:

```text
MySQL
MariaDB
Amazon Aurora MySQL
servidores compatibles con protocolo MySQL
```

No se usa para conectarse a:

```text
SQLite
PostgreSQL
SQL Server
Oracle
```

Para esas bases se usan otros drivers:

```text
sqlite3   -> SQLite
psycopg   -> PostgreSQL
pyodbc    -> SQL Server
oracledb  -> Oracle
pymysql   -> MySQL / MariaDB
```

## Idea central

La idea principal de `pymysql` es abrir una conexión a MySQL o MariaDB y ejecutar comandos SQL desde Python.

Flujo típico:

```text
Python -> pymysql.connect() -> Connection -> Cursor -> SQL -> MySQL / MariaDB
```

Ejemplo mínimo:

```python
import pymysql

connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    port=3306
)

try:
    with connection.cursor() as cursor:
        cursor.execute("SELECT 1")
        result = cursor.fetchone()

        print(result)
finally:
    connection.close()
```

## Cuándo usar `pymysql`

Conviene usar `pymysql` cuando se necesita:

```text
conectar Python con MySQL o MariaDB
ejecutar SQL directamente
leer datos desde una base MySQL
insertar registros
actualizar registros
eliminar registros
manejar transacciones
crear scripts de carga o extracción
conectar Flask o FastAPI con MySQL
usar SQLAlchemy con driver MySQL
```

## Cuándo no usar `pymysql`

No conviene usar `pymysql` cuando se necesita:

```text
conectarse a PostgreSQL
conectarse a SQLite
trabajar solo con archivos CSV o Excel
hacer análisis tabular sin base de datos
usar el ORM propio de Django
evitar escribir SQL manualmente
```

Para archivos tabulares suele corresponder:

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

Instalación básica:

```bash
python -m pip install pymysql
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
PyMySQL==1.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import pymysql

print(pymysql.__version__)
```

## Conexión básica

```python
import pymysql

connection = pymysql.connect(
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
host       -> servidor donde está la base de datos
user       -> usuario de MySQL
password   -> contraseña
database   -> base de datos a usar
port       -> puerto, normalmente 3306
charset    -> codificación de caracteres
cursorclass -> tipo de cursor
autocommit -> modo de confirmación automática
```

Ejemplo más completo:

```python
import pymysql
import pymysql.cursors

connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    port=3306,
    charset="utf8mb4",
    cursorclass=pymysql.cursors.DictCursor
)
```

## Uso recomendado con `try` y `finally`

```python
import pymysql

connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app"
)

try:
    with connection.cursor() as cursor:
        cursor.execute("SELECT 1")
        print(cursor.fetchone())
finally:
    connection.close()
```

Este patrón asegura que la conexión se cierre incluso si ocurre un error.

## Connection

Una conexión representa una sesión abierta contra MySQL o MariaDB.

Desde una conexión se puede:

```text
crear cursores
ejecutar transacciones
confirmar cambios
revertir cambios
cerrar la comunicación
configurar autocommit
```

Ejemplo:

```python
connection = pymysql.connect(
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

Cerrar conexiones es importante para no dejar recursos abiertos.

## Cursor

Un cursor permite ejecutar comandos SQL y leer resultados.

Se crea desde la conexión:

```python
with connection.cursor() as cursor:
    ...
```

Ejemplo:

```python
with connection.cursor() as cursor:
    cursor.execute("SELECT 1")
    result = cursor.fetchone()

    print(result)
```

## Ejecutar una consulta

```python
with connection.cursor() as cursor:
    cursor.execute("SELECT 1")
    result = cursor.fetchone()

    print(result)
```

Salida conceptual:

```text
(1,)
```

Si se usa `DictCursor`, la salida puede ser un diccionario.

## Crear una tabla

```python
import pymysql

connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    charset="utf8mb4"
)

try:
    with connection.cursor() as cursor:
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS products (
                id INT AUTO_INCREMENT PRIMARY KEY,
                name VARCHAR(100) NOT NULL,
                price DECIMAL(10, 2) NOT NULL,
                stock INT NOT NULL DEFAULT 0,
                is_active BOOLEAN NOT NULL DEFAULT TRUE
            )
        """)

    connection.commit()
finally:
    connection.close()
```

## Insertar datos

```python
with connection.cursor() as cursor:
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

En `pymysql`, los valores se pasan con placeholders `%s`.

```python
cursor.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

Aunque el valor sea entero o decimal, el placeholder sigue siendo:

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
cursor.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

Incorrecto:

```python
cursor.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id)
)
```

La coma es necesaria.

## Parámetros con diccionario

También puede usarse un diccionario con nombres.

```python
cursor.execute(
    """
    SELECT *
    FROM products
    WHERE name = %(name)s
    """,
    {"name": "Laptop"}
)
```

Este patrón puede ser más legible cuando hay muchos parámetros.

## Evitar concatenar SQL

Problemático:

```python
query = f"SELECT * FROM products WHERE name = '{name}'"
cursor.execute(query)
```

Más seguro:

```python
cursor.execute(
    "SELECT * FROM products WHERE name = %s",
    (name,)
)
```

Los valores deben pasarse como parámetros, no concatenarse dentro del texto SQL.

## Leer un resultado

## `fetchone()`

Devuelve una fila o `None`.

```python
cursor.execute(
    """
    SELECT id, name, price
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
    SELECT id, name, price
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
    SELECT id, name, price
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
    SELECT id, name, price
    FROM products
    ORDER BY name
""")

for row in cursor:
    print(row)
```

Este patrón puede ser útil cuando no se quiere cargar todo de golpe en una lista.

## Tuplas vs diccionarios

Por defecto, las filas suelen recibirse como tuplas.

```text
(1, "Laptop", Decimal("3500.00"))
```

Con `DictCursor`, las filas se reciben como diccionarios.

```text
{"id": 1, "name": "Laptop", "price": Decimal("3500.00")}
```

## Usar `DictCursor`

```python
import pymysql
import pymysql.cursors

connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    charset="utf8mb4",
    cursorclass=pymysql.cursors.DictCursor
)

try:
    with connection.cursor() as cursor:
        cursor.execute("""
            SELECT id, name, price
            FROM products
            ORDER BY name
        """)

        rows = cursor.fetchall()

        for row in rows:
            print(row["name"])
finally:
    connection.close()
```

## Insertar varios registros

## `executemany()`

```python
products = [
    ("Laptop", 3500, 5),
    ("Mouse", 80, 20),
    ("Teclado", 150, 10),
]

with connection.cursor() as cursor:
    cursor.executemany(
        """
        INSERT INTO products (name, price, stock)
        VALUES (%s, %s, %s)
        """,
        products
    )

connection.commit()
```

`executemany()` es útil para insertar múltiples filas con una misma sentencia.

## Actualizar datos

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        UPDATE products
        SET price = %s
        WHERE id = %s
        """,
        (3600, 1)
    )

connection.commit()
```

## Eliminar datos

```python
with connection.cursor() as cursor:
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

En aplicaciones reales, a veces se prefiere marcar registros como inactivos en lugar de eliminarlos físicamente.

```python
with connection.cursor() as cursor:
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

## Último id insertado

Después de un `INSERT`, puede obtenerse el id generado.

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (%s, %s, %s)
        """,
        ("Laptop", 3500, 5)
    )

    new_id = cursor.lastrowid

connection.commit()

print(new_id)
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
import pymysql

connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app"
)

try:
    with connection.cursor() as cursor:
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
finally:
    connection.close()
```

## Rollback

```python
try:
    with connection.cursor() as cursor:
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

Después de ciertos errores SQL, conviene hacer `rollback()` antes de seguir usando la conexión.

## Autocommit

`autocommit=True` confirma automáticamente cada sentencia.

```python
connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    autocommit=True
)
```

Con `autocommit=True`, no se necesita llamar a `commit()` después de cada operación.

## Cuándo usar autocommit

Puede ser útil para:

```text
scripts simples
operaciones administrativas puntuales
consultas donde no se desea manejar transacciones manualmente
```

Para operaciones de negocio relacionadas, suele ser mejor controlar transacciones explícitamente.

## Consultas de lectura

```python
def list_products(connection):
    with connection.cursor() as cursor:
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = TRUE
            ORDER BY name
        """)

        return cursor.fetchall()
```

## Consulta por id

```python
def get_product(connection, product_id):
    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT id, name, price, stock
            FROM products
            WHERE id = %s
            """,
            (product_id,)
        )

        return cursor.fetchone()
```

## Crear registro

```python
def create_product(connection, name, price, stock=0):
    with connection.cursor() as cursor:
        cursor.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
            """,
            (name, price, stock)
        )

        new_id = cursor.lastrowid

    connection.commit()

    return new_id
```

## Actualizar registro

```python
def update_stock(connection, product_id, stock):
    with connection.cursor() as cursor:
        cursor.execute(
            """
            UPDATE products
            SET stock = %s
            WHERE id = %s
            """,
            (stock, product_id)
        )

        affected_rows = cursor.rowcount

    connection.commit()

    return affected_rows
```

## `rowcount`

`rowcount` indica cuántas filas fueron afectadas o devueltas según el tipo de operación y el contexto.

```python
affected_rows = cursor.rowcount
```

Ejemplo:

```python
with connection.cursor() as cursor:
    cursor.execute(
        """
        UPDATE products
        SET stock = %s
        WHERE id = %s
        """,
        (10, 1)
    )

    print(cursor.rowcount)
```

## Manejo de errores

`pymysql` expone excepciones para errores de base de datos.

```python
import pymysql

try:
    with connection.cursor() as cursor:
        cursor.execute("SELECT * FROM table_that_does_not_exist")
except pymysql.MySQLError as error:
    print("Error de base de datos:", error)
```

## Errores frecuentes

```text
pymysql.MySQLError
pymysql.err.OperationalError
pymysql.err.IntegrityError
pymysql.err.ProgrammingError
pymysql.err.DataError
```

## Error de integridad

```python
import pymysql

try:
    with connection.cursor() as cursor:
        cursor.execute(
            """
            INSERT INTO products (name, price, stock)
            VALUES (%s, %s, %s)
            """,
            ("Laptop", 3500, 5)
        )

    connection.commit()
except pymysql.err.IntegrityError:
    connection.rollback()
    print("No se pudo insertar el registro por una restricción de integridad")
```

## Error de conexión

```python
import pymysql

try:
    connection = pymysql.connect(
        host="localhost",
        user="root",
        password="secret",
        database="app"
    )
except pymysql.err.OperationalError as error:
    print("No se pudo conectar a la base de datos")
    print(error)
```

## Conexión con variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app"
)
```

Más conveniente:

```python
import os

import pymysql

connection = pymysql.connect(
    host=os.getenv("MYSQL_HOST"),
    user=os.getenv("MYSQL_USER"),
    password=os.getenv("MYSQL_PASSWORD"),
    database=os.getenv("MYSQL_DATABASE"),
    port=int(os.getenv("MYSQL_PORT", "3306")),
    charset="utf8mb4"
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

import pymysql
from dotenv import load_dotenv

load_dotenv()

connection = pymysql.connect(
    host=os.getenv("MYSQL_HOST"),
    port=int(os.getenv("MYSQL_PORT", "3306")),
    user=os.getenv("MYSQL_USER"),
    password=os.getenv("MYSQL_PASSWORD"),
    database=os.getenv("MYSQL_DATABASE"),
    charset="utf8mb4"
)
```

## Función para crear conexión

```python
import os

import pymysql
import pymysql.cursors
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    return pymysql.connect(
        host=os.getenv("MYSQL_HOST"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
        user=os.getenv("MYSQL_USER"),
        password=os.getenv("MYSQL_PASSWORD"),
        database=os.getenv("MYSQL_DATABASE"),
        charset="utf8mb4",
        cursorclass=pymysql.cursors.DictCursor
    )
```

## Uso de la función de conexión

```python
connection = create_connection()

try:
    with connection.cursor() as cursor:
        cursor.execute("SELECT 1 AS value")
        print(cursor.fetchone())
finally:
    connection.close()
```

## Cursores no bufferizados

`pymysql` incluye cursores no bufferizados, como `SSCursor`.

Estos cursores pueden ser útiles para resultados grandes, porque no cargan todas las filas de golpe en memoria.

```python
import pymysql.cursors

connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    cursorclass=pymysql.cursors.SSCursor
)
```

## Cuándo usar cursores no bufferizados

Conviene evaluarlos cuando:

```text
una consulta devuelve muchas filas
no se necesita cargar todo en memoria
se procesan resultados por streaming
se exportan datos grandes
```

Para consultas normales, un cursor estándar suele ser suficiente.

## `DictCursor` no bufferizado

```python
connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    cursorclass=pymysql.cursors.SSDictCursor
)
```

Este cursor devuelve filas como diccionarios y puede leer resultados grandes de forma no bufferizada.

## Tipos de datos

`pymysql` convierte muchos tipos entre Python y MySQL.

Ejemplos frecuentes:

```text
Python str              -> VARCHAR / TEXT
Python int              -> INT / BIGINT
Python float            -> FLOAT / DOUBLE
Decimal                 -> DECIMAL
datetime.date           -> DATE
datetime.datetime       -> DATETIME / TIMESTAMP
bool                    -> TINYINT(1) / BOOLEAN
None                    -> NULL
```

## Insertar fechas

```python
from datetime import date

with connection.cursor() as cursor:
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
with connection.cursor() as cursor:
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

## Codificación de caracteres

Es recomendable usar `utf8mb4` en conexiones modernas con MySQL o MariaDB.

```python
connection = pymysql.connect(
    host="localhost",
    user="root",
    password="secret",
    database="app",
    charset="utf8mb4"
)
```

`utf8mb4` permite almacenar caracteres Unicode de forma más completa que ciertas configuraciones antiguas de `utf8`.

## Consultas dinámicas seguras

Los valores deben pasarse como parámetros.

```python
cursor.execute(
    "SELECT * FROM products WHERE name = %s",
    (name,)
)
```

Sin embargo, los nombres de tablas o columnas no se parametrizan con `%s`.

Problemático:

```python
cursor.execute(
    "SELECT * FROM %s",
    ("products",)
)
```

Para nombres dinámicos, lo mejor es evitar aceptar cualquier valor externo y usar una lista permitida.

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

Este patrón solo es aceptable cuando `table_name` proviene de una lista controlada por el programa.

## Uso con pandas

Pandas puede leer consultas SQL desde una conexión.

En proyectos de datos, suele ser más común usar SQLAlchemy como capa de conexión.

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+pymysql://root:secret@localhost:3306/app"
)

df = pd.read_sql(
    "SELECT * FROM products",
    engine
)

print(df.head())
```

También puede escribirse un DataFrame:

```python
df.to_sql(
    "products_export",
    engine,
    if_exists="replace",
    index=False
)
```

## Uso con SQLAlchemy

SQLAlchemy puede usar `pymysql` como driver de MySQL.

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+pymysql://root:secret@localhost:3306/app"
)
```

En ese caso:

```text
SQLAlchemy -> construye consultas, sesiones y modelos
pymysql    -> conecta con MySQL como driver
```

## Uso con Flask

```python
import os

import pymysql
import pymysql.cursors
from flask import Flask

app = Flask(__name__)


def create_connection():
    return pymysql.connect(
        host=os.getenv("MYSQL_HOST"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
        user=os.getenv("MYSQL_USER"),
        password=os.getenv("MYSQL_PASSWORD"),
        database=os.getenv("MYSQL_DATABASE"),
        charset="utf8mb4",
        cursorclass=pymysql.cursors.DictCursor
    )


@app.route("/products")
def read_products():
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT id, name, price, stock
                FROM products
                WHERE is_active = TRUE
                ORDER BY name
            """)

            return cursor.fetchall()
    finally:
        connection.close()
```

En aplicaciones reales con muchas solicitudes, conviene evaluar un pool de conexiones o usar SQLAlchemy.

## Uso con FastAPI

```python
import os

import pymysql
import pymysql.cursors
from fastapi import FastAPI

app = FastAPI()


def create_connection():
    return pymysql.connect(
        host=os.getenv("MYSQL_HOST"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
        user=os.getenv("MYSQL_USER"),
        password=os.getenv("MYSQL_PASSWORD"),
        database=os.getenv("MYSQL_DATABASE"),
        charset="utf8mb4",
        cursorclass=pymysql.cursors.DictCursor
    )


@app.get("/products")
def read_products():
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT id, name, price, stock
                FROM products
                WHERE is_active = TRUE
                ORDER BY name
            """)

            return cursor.fetchall()
    finally:
        connection.close()
```

Para APIs medianas o grandes, suele ser mejor separar:

```text
database.py
repositories.py
services.py
routers.py
```

## Pool de conexiones

`pymysql` no se usa normalmente como gestor de pool por sí solo.

Para aplicaciones web, suele evaluarse:

```text
SQLAlchemy Engine
DBUtils
pool propio del framework o infraestructura
```

Con SQLAlchemy:

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+pymysql://root:secret@localhost:3306/app",
    pool_size=5,
    max_overflow=10
)
```

Este enfoque suele ser más robusto para aplicaciones con varias solicitudes concurrentes.

## Uso con Django

Django tiene su propio ORM y sus propios backends de base de datos.

En proyectos Django con MySQL, normalmente no se usa `pymysql` directamente en el código de negocio.

Se configura la base en `settings.py` y se usa el ORM de Django.

Ejemplo conceptual:

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": "app",
        "USER": "root",
        "PASSWORD": "secret",
        "HOST": "localhost",
        "PORT": "3306",
    }
}
```

El uso directo de `pymysql` en Django solo se justifica en casos específicos.

## Procedimientos almacenados

`pymysql` permite llamar procedimientos almacenados con `callproc()`.

```python
with connection.cursor() as cursor:
    cursor.callproc("procedure_name", (1, 2))

    results = cursor.fetchall()
```

Los procedimientos almacenados pueden devolver varios result sets. En esos casos puede ser necesario avanzar entre resultados.

Este tema suele ser más avanzado y depende del diseño de la base.

## Múltiples result sets

Algunas operaciones pueden devolver varios conjuntos de resultados.

Puede usarse:

```python
cursor.nextset()
```

Ejemplo conceptual:

```python
while cursor.nextset():
    rows = cursor.fetchall()
    print(rows)
```

No es un patrón necesario para consultas simples, pero puede aparecer con procedimientos almacenados.

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

import pymysql
import pymysql.cursors
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    return pymysql.connect(
        host=os.getenv("MYSQL_HOST"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
        user=os.getenv("MYSQL_USER"),
        password=os.getenv("MYSQL_PASSWORD"),
        database=os.getenv("MYSQL_DATABASE"),
        charset="utf8mb4",
        cursorclass=pymysql.cursors.DictCursor
    )
```

## `repositories.py`

```python
from .database import create_connection


def list_products():
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT id, name, price, stock
                FROM products
                WHERE is_active = TRUE
                ORDER BY name
            """)

            return cursor.fetchall()
    finally:
        connection.close()


def get_product(product_id):
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
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
        connection.close()


def create_product(name, price, stock):
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute(
                """
                INSERT INTO products (name, price, stock)
                VALUES (%s, %s, %s)
                """,
                (name, price, stock)
            )

            new_id = cursor.lastrowid

        connection.commit()

        return new_id
    except Exception:
        connection.rollback()
        raise
    finally:
        connection.close()
```

## Separación por responsabilidades

```text
database.py      -> conexión
repositories.py  -> consultas SQL
services.py      -> reglas de negocio
main.py          -> entrada de aplicación
```

## Errores comunes

## No instalar `pymysql`

Problemático:

```python
import pymysql
```

si no se instaló previamente.

Instalación:

```bash
python -m pip install pymysql
```

## Olvidar cerrar la conexión

Problemático:

```python
connection = pymysql.connect(...)
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

## Usar `%d` o `%f` como placeholder

Problemático:

```python
cursor.execute(
    "SELECT * FROM products WHERE id = %d",
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

## Esperar diccionarios sin usar `DictCursor`

Problemático:

```python
row = cursor.fetchone()
print(row["name"])
```

si el cursor devuelve tuplas.

Solución:

```python
cursorclass=pymysql.cursors.DictCursor
```

## No especificar `charset`

Es recomendable definir:

```python
charset="utf8mb4"
```

para evitar problemas de caracteres especiales.

## Guardar credenciales en el código

Problemático:

```python
password="secret"
```

Mejor:

```python
password=os.getenv("MYSQL_PASSWORD")
```

## Abrir una conexión por cada función sin evaluar carga

En scripts pequeños está bien.

En aplicaciones web con muchas solicitudes, conviene evaluar pooling o SQLAlchemy.

## Usar `pymysql` cuando se necesita ORM

`pymysql` ejecuta SQL directamente.

Si se necesita trabajar con clases y modelos, puede ser mejor usar:

```text
sqlalchemy
django ORM
```

## Buenas prácticas

## Usar parámetros en consultas

```python
cursor.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

## Usar `DictCursor` cuando mejore la legibilidad

```python
cursorclass=pymysql.cursors.DictCursor
```

## Usar `utf8mb4`

```python
charset="utf8mb4"
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

## Separar consultas en funciones

```python
def list_products():
    ...
```

## Usar variables de entorno

```python
os.getenv("MYSQL_PASSWORD")
```

## Evitar SQL dinámico no controlado

Si una tabla o columna es dinámica, debe validarse contra una lista permitida.

## Usar SQLAlchemy si se requiere pooling u ORM

```python
mysql+pymysql://user:password@host:3306/database
```

## Manejar errores de base de datos

```python
except pymysql.MySQLError:
    ...
```

## Ejemplo integrado

```python
import os
from decimal import Decimal

import pymysql
import pymysql.cursors
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    return pymysql.connect(
        host=os.getenv("MYSQL_HOST"),
        port=int(os.getenv("MYSQL_PORT", "3306")),
        user=os.getenv("MYSQL_USER"),
        password=os.getenv("MYSQL_PASSWORD"),
        database=os.getenv("MYSQL_DATABASE"),
        charset="utf8mb4",
        cursorclass=pymysql.cursors.DictCursor
    )


def create_table():
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
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
        connection.close()


def create_product(name, price, stock=0):
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute(
                """
                INSERT INTO products (name, price, stock)
                VALUES (%s, %s, %s)
                """,
                (name, Decimal(str(price)), stock)
            )

            new_id = cursor.lastrowid

        connection.commit()

        return new_id
    except pymysql.err.IntegrityError:
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
                WHERE is_active = TRUE
                ORDER BY name
            """)

            return cursor.fetchall()
    finally:
        connection.close()


def update_stock(product_id, stock):
    connection = create_connection()

    try:
        with connection.cursor() as cursor:
            cursor.execute(
                """
                UPDATE products
                SET stock = %s
                WHERE id = %s
                """,
                (stock, product_id)
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
                SET is_active = FALSE
                WHERE id = %s
                """,
                (product_id,)
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

`pymysql` se relaciona especialmente con:

```text
mysql
mariadb
sqlalchemy
pandas
flask
fastapi
python-dotenv
pytest
alembic
```

## Relación con SQLAlchemy

SQLAlchemy puede usar `pymysql` como driver:

```python
engine = create_engine(
    "mysql+pymysql://user:password@localhost:3306/database"
)
```

## Relación con pandas

Pandas suele conectarse a MySQL mediante SQLAlchemy y `pymysql`.

```python
df = pd.read_sql("SELECT * FROM products", engine)
```

## Relación con Flask y FastAPI

`pymysql` puede usarse directamente en backends web, pero para aplicaciones medianas o grandes conviene evaluar:

```text
pooling
SQLAlchemy
capa repository
manejo centralizado de errores
```

## Relación con Django

Django normalmente usa su propio ORM y configuración de base de datos.

En proyectos Django, `pymysql` no suele usarse directamente en las vistas o servicios salvo que exista una necesidad puntual.

## Orden didáctico interno

```text
1. Propósito de pymysql
2. Instalación
3. Conexión a MySQL o MariaDB
4. Connection y Cursor
5. execute()
6. Parámetros con %s
7. fetchone(), fetchall() y fetchmany()
8. DictCursor
9. INSERT, UPDATE, DELETE y lastrowid
10. Transacciones, commit y rollback
11. autocommit
12. Variables de entorno
13. Manejo de errores
14. Cursores no bufferizados
15. Uso con SQLAlchemy, pandas, Flask y FastAPI
16. Errores comunes
17. Buenas prácticas
```