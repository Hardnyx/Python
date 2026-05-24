# `pyodbc`

## Propósito

`pyodbc` es una librería externa para conectar Python con bases de datos mediante ODBC.

Se utiliza para ejecutar consultas SQL, leer datos, insertar registros, actualizar información, eliminar filas, llamar procedimientos almacenados y conectar scripts o aplicaciones Python con bases de datos que tengan un driver ODBC disponible.

Es especialmente frecuente en conexiones con SQL Server, Access, Azure SQL Database, bases corporativas y otros sistemas que exponen conectividad ODBC.

## Naturaleza de la librería

`pyodbc` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install pyodbc
```

La importación habitual es:

```python
import pyodbc
```

`pyodbc` es un driver DB API para ODBC. Su función principal no es modelar tablas como clases, sino permitir la comunicación directa entre Python y una base de datos mediante SQL.

## Relación con ODBC

ODBC significa Open Database Connectivity.

Es una capa estándar de conexión a bases de datos. En lugar de que Python se conecte directamente a una base específica, Python se comunica con un driver ODBC.

Flujo conceptual:

```text
Python -> pyodbc -> Driver ODBC -> Base de datos
```

Ejemplo:

```text
Python -> pyodbc -> ODBC Driver 18 for SQL Server -> SQL Server
```

## Cuándo usar `pyodbc`

Conviene usar `pyodbc` cuando se necesita:

```text
conectarse a SQL Server desde Python
conectarse a Access desde Python
usar un driver ODBC corporativo
ejecutar SQL directamente
leer datos desde una base mediante ODBC
insertar registros
actualizar registros
eliminar registros
usar procedimientos almacenados
integrar Python con bases empresariales
usar pandas con una fuente ODBC
usar SQLAlchemy con SQL Server mediante pyodbc
```

## Cuándo no usar `pyodbc`

No conviene usar `pyodbc` cuando se necesita:

```text
trabajar solo con archivos CSV
trabajar solo con archivos Excel
hacer análisis tabular sin base de datos
usar una base sin driver ODBC
evitar escribir SQL manualmente
usar el ORM propio de Django
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

Para PostgreSQL directo suele usarse:

```text
psycopg
```

Para MySQL o MariaDB directo suele usarse:

```text
pymysql
```

## Instalación

Instalación básica:

```bash
python -m pip install pyodbc
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
pyodbc==5.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import pyodbc

print(pyodbc.version)
```

## Requisito adicional: driver ODBC

Instalar `pyodbc` no siempre basta.

También debe existir un driver ODBC instalado para la base de datos correspondiente.

Ejemplos:

```text
ODBC Driver 18 for SQL Server
Microsoft Access Driver
Oracle ODBC Driver
PostgreSQL ODBC Driver
MySQL ODBC Driver
```

`pyodbc` es el puente desde Python.

El driver ODBC es el componente que sabe comunicarse con la base específica.

## Administrador ODBC

En Windows, el administrador ODBC suele venir integrado en el sistema.

En Linux o macOS puede requerirse un administrador ODBC, como:

```text
unixODBC
```

Instalación conceptual en macOS con Homebrew:

```bash
brew install unixodbc
```

En Linux, el comando depende de la distribución.

## Conexión básica

```python
import pyodbc

connection = pyodbc.connect(
    "DRIVER={ODBC Driver 18 for SQL Server};"
    "SERVER=localhost;"
    "DATABASE=app;"
    "UID=sa;"
    "PWD=secret;"
    "TrustServerCertificate=yes;"
)

connection.close()
```

## Cadena de conexión

La cadena de conexión contiene los datos necesarios para conectarse.

Ejemplo:

```text
DRIVER={ODBC Driver 18 for SQL Server};
SERVER=localhost;
DATABASE=app;
UID=sa;
PWD=secret;
TrustServerCertificate=yes;
```

En Python puede escribirse como una sola cadena:

```python
connection_string = (
    "DRIVER={ODBC Driver 18 for SQL Server};"
    "SERVER=localhost;"
    "DATABASE=app;"
    "UID=sa;"
    "PWD=secret;"
    "TrustServerCertificate=yes;"
)
```

## Parámetros frecuentes de conexión

```text
DRIVER    -> driver ODBC usado
SERVER    -> servidor
DATABASE  -> base de datos
UID       -> usuario
PWD       -> contraseña
PORT      -> puerto, si corresponde
Trusted_Connection -> autenticación integrada en Windows
Encrypt   -> cifrado de conexión
TrustServerCertificate -> confianza en certificado del servidor
```

## Conexión con autenticación SQL Server

```python
import pyodbc

connection_string = (
    "DRIVER={ODBC Driver 18 for SQL Server};"
    "SERVER=localhost;"
    "DATABASE=app;"
    "UID=sa;"
    "PWD=secret;"
    "TrustServerCertificate=yes;"
)

connection = pyodbc.connect(connection_string)

connection.close()
```

## Conexión con autenticación integrada de Windows

```python
import pyodbc

connection_string = (
    "DRIVER={ODBC Driver 18 for SQL Server};"
    "SERVER=localhost;"
    "DATABASE=app;"
    "Trusted_Connection=yes;"
    "TrustServerCertificate=yes;"
)

connection = pyodbc.connect(connection_string)

connection.close()
```

Este patrón depende del entorno, del sistema operativo y de la configuración del driver.

## Conexión con DSN

Un DSN es una fuente de datos ODBC configurada en el sistema.

```python
import pyodbc

connection = pyodbc.connect(
    "DSN=MiFuenteODBC;"
    "UID=usuario;"
    "PWD=secret;"
)

connection.close()
```

## DSN vs cadena completa

## DSN

Ventajas:

```text
la configuración queda centralizada en el sistema
la cadena en Python puede ser más corta
puede facilitar configuración corporativa
```

## Cadena completa

Ventajas:

```text
más explícita
más portable dentro del proyecto
más fácil de versionar como plantilla sin credenciales
más clara para despliegues controlados por variables de entorno
```

## Uso recomendado con `try` y `finally`

```python
import pyodbc

connection = pyodbc.connect(connection_string)

try:
    cursor = connection.cursor()
    cursor.execute("SELECT 1")
    row = cursor.fetchone()

    print(row)
finally:
    connection.close()
```

Este patrón asegura que la conexión se cierre incluso si ocurre un error.

## Connection

Una conexión representa una sesión abierta contra la base de datos.

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
connection = pyodbc.connect(connection_string)
```

## Cerrar conexión

```python
connection.close()
```

Cerrar conexiones es importante para no dejar recursos abiertos.

## Cursor

Un cursor permite ejecutar comandos SQL y leer resultados.

```python
cursor = connection.cursor()
```

Ejemplo:

```python
cursor.execute("SELECT 1")
row = cursor.fetchone()

print(row)
```

## Ejecutar una consulta

```python
import pyodbc

connection = pyodbc.connect(connection_string)

try:
    cursor = connection.cursor()

    cursor.execute("SELECT 1 AS value")

    row = cursor.fetchone()

    print(row)
finally:
    connection.close()
```

Salida conceptual:

```text
(1,)
```

## Crear una tabla

```python
import pyodbc

connection = pyodbc.connect(connection_string)

try:
    cursor = connection.cursor()

    cursor.execute("""
        IF OBJECT_ID('dbo.products', 'U') IS NULL
        CREATE TABLE dbo.products (
            id INT IDENTITY(1,1) PRIMARY KEY,
            name NVARCHAR(100) NOT NULL,
            price DECIMAL(10, 2) NOT NULL,
            stock INT NOT NULL DEFAULT 0,
            is_active BIT NOT NULL DEFAULT 1
        )
    """)

    connection.commit()
finally:
    connection.close()
```

Este ejemplo usa sintaxis de SQL Server.

La sintaxis puede cambiar según la base conectada mediante ODBC.

## Insertar datos

```python
connection = pyodbc.connect(connection_string)

try:
    cursor = connection.cursor()

    cursor.execute(
        """
        INSERT INTO dbo.products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        ("Laptop", 3500, 5)
    )

    connection.commit()
finally:
    connection.close()
```

## Parámetros SQL

En `pyodbc`, los valores se pasan con placeholders de signo de interrogación.

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE id = ?",
    (product_id,)
)
```

Aunque el valor sea texto, entero o decimal, el placeholder sigue siendo:

```python
?
```

No se usa:

```python
%s
%d
%f
```

## Tupla de un solo parámetro

Cuando se pasa un solo parámetro, debe usarse una tupla de un elemento.

Correcto:

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE id = ?",
    (product_id,)
)
```

Incorrecto:

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE id = ?",
    (product_id)
)
```

La coma es necesaria.

## Evitar concatenar SQL

Problemático:

```python
query = f"SELECT * FROM dbo.products WHERE name = '{name}'"
cursor.execute(query)
```

Más seguro:

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE name = ?",
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
    FROM dbo.products
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
    SELECT id, name, price
    FROM dbo.products
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
    FROM dbo.products
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
    FROM dbo.products
    ORDER BY name
""")

for row in cursor:
    print(row)
```

Este patrón puede ser útil cuando no se quiere cargar todo de golpe en una lista.

## Acceder a columnas de una fila

Las filas pueden accederse por posición.

```python
row = cursor.fetchone()

print(row[0])
print(row[1])
```

También pueden accederse por nombre de columna en muchos casos.

```python
print(row.id)
print(row.name)
```

Este acceso depende de que el nombre de columna sea válido como atributo.

## Convertir filas a diccionarios

```python
def row_to_dict(cursor, row):
    columns = [column[0] for column in cursor.description]

    return dict(zip(columns, row))


cursor.execute("""
    SELECT id, name, price
    FROM dbo.products
""")

rows = [
    row_to_dict(cursor, row)
    for row in cursor.fetchall()
]

print(rows)
```

## `cursor.description`

`cursor.description` contiene metadatos de las columnas devueltas por una consulta.

```python
cursor.execute("SELECT id, name, price FROM dbo.products")

columns = [column[0] for column in cursor.description]

print(columns)
```

## Insertar varios registros

## `executemany()`

```python
products = [
    ("Laptop", 3500, 5),
    ("Mouse", 80, 20),
    ("Teclado", 150, 10),
]

connection = pyodbc.connect(connection_string)

try:
    cursor = connection.cursor()

    cursor.executemany(
        """
        INSERT INTO dbo.products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        products
    )

    connection.commit()
finally:
    connection.close()
```

## `fast_executemany`

Para SQL Server, `fast_executemany` puede mejorar el rendimiento de inserciones masivas en ciertos escenarios.

```python
cursor.fast_executemany = True

cursor.executemany(
    """
    INSERT INTO dbo.products (name, price, stock)
    VALUES (?, ?, ?)
    """,
    products
)
```

Debe probarse con el driver, la base y el volumen real de datos, porque su comportamiento puede depender del entorno.

## Actualizar datos

```python
connection = pyodbc.connect(connection_string)

try:
    cursor = connection.cursor()

    cursor.execute(
        """
        UPDATE dbo.products
        SET price = ?
        WHERE id = ?
        """,
        (3600, 1)
    )

    connection.commit()
finally:
    connection.close()
```

## Eliminar datos

```python
connection = pyodbc.connect(connection_string)

try:
    cursor = connection.cursor()

    cursor.execute(
        """
        DELETE FROM dbo.products
        WHERE id = ?
        """,
        (1,)
    )

    connection.commit()
finally:
    connection.close()
```

## Eliminación lógica

En aplicaciones reales, a veces se prefiere marcar registros como inactivos en lugar de eliminarlos físicamente.

```python
cursor.execute(
    """
    UPDATE dbo.products
    SET is_active = 0
    WHERE id = ?
    """,
    (product_id,)
)

connection.commit()
```

## Obtener id generado

En SQL Server puede usarse `OUTPUT INSERTED.id`.

```python
cursor.execute(
    """
    INSERT INTO dbo.products (name, price, stock)
    OUTPUT INSERTED.id
    VALUES (?, ?, ?)
    """,
    ("Laptop", 3500, 5)
)

new_id = cursor.fetchone()[0]

connection.commit()

print(new_id)
```

También existen otras opciones según la base de datos, como funciones específicas del motor.

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
connection = pyodbc.connect(connection_string)

try:
    cursor = connection.cursor()

    cursor.execute(
        """
        INSERT INTO dbo.products (name, price, stock)
        VALUES (?, ?, ?)
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
    cursor.execute(
        """
        INSERT INTO dbo.products (name, price, stock)
        VALUES (?, ?, ?)
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
connection = pyodbc.connect(
    connection_string,
    autocommit=True
)
```

También puede configurarse después:

```python
connection.autocommit = True
```

## Cuándo usar autocommit

Puede ser útil para:

```text
scripts simples
operaciones administrativas
consultas donde no se quiere manejar transacciones manualmente
sentencias que no pueden ejecutarse dentro de una transacción normal
```

Para operaciones de negocio relacionadas, suele ser mejor controlar transacciones explícitamente.

## Consultas de lectura

```python
def list_products(connection):
    cursor = connection.cursor()

    cursor.execute("""
        SELECT id, name, price, stock
        FROM dbo.products
        WHERE is_active = 1
        ORDER BY name
    """)

    return cursor.fetchall()
```

## Consulta por id

```python
def get_product(connection, product_id):
    cursor = connection.cursor()

    cursor.execute(
        """
        SELECT id, name, price, stock
        FROM dbo.products
        WHERE id = ?
        """,
        (product_id,)
    )

    return cursor.fetchone()
```

## Crear registro

```python
def create_product(connection, name, price, stock=0):
    cursor = connection.cursor()

    cursor.execute(
        """
        INSERT INTO dbo.products (name, price, stock)
        OUTPUT INSERTED.id
        VALUES (?, ?, ?)
        """,
        (name, price, stock)
    )

    new_id = cursor.fetchone()[0]

    connection.commit()

    return new_id
```

## Actualizar registro

```python
def update_stock(connection, product_id, stock):
    cursor = connection.cursor()

    cursor.execute(
        """
        UPDATE dbo.products
        SET stock = ?
        WHERE id = ?
        """,
        (stock, product_id)
    )

    affected_rows = cursor.rowcount

    connection.commit()

    return affected_rows
```

## `rowcount`

`rowcount` indica cuántas filas fueron afectadas o devueltas según el tipo de operación y el driver.

```python
affected_rows = cursor.rowcount
```

Ejemplo:

```python
cursor.execute(
    """
    UPDATE dbo.products
    SET stock = ?
    WHERE id = ?
    """,
    (10, 1)
)

print(cursor.rowcount)
```

## Manejo de errores

`pyodbc` expone excepciones para errores de base de datos.

```python
import pyodbc

try:
    connection = pyodbc.connect(connection_string)
    cursor = connection.cursor()

    cursor.execute("SELECT * FROM table_that_does_not_exist")
except pyodbc.Error as error:
    print("Error de base de datos:", error)
finally:
    try:
        connection.close()
    except NameError:
        pass
```

## Errores frecuentes

```text
pyodbc.Error
pyodbc.DatabaseError
pyodbc.OperationalError
pyodbc.IntegrityError
pyodbc.ProgrammingError
```

## Error de integridad

```python
try:
    cursor.execute(
        """
        INSERT INTO dbo.products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        ("Laptop", 3500, 5)
    )

    connection.commit()
except pyodbc.IntegrityError:
    connection.rollback()
    print("No se pudo insertar el registro por una restricción de integridad")
```

## Error de conexión

```python
try:
    connection = pyodbc.connect(connection_string)
except pyodbc.OperationalError as error:
    print("No se pudo conectar a la base de datos")
    print(error)
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
connection_string = (
    "DRIVER={ODBC Driver 18 for SQL Server};"
    "SERVER=localhost;"
    "DATABASE=app;"
    "UID=sa;"
    "PWD=secret;"
)
```

Más conveniente:

```python
import os

connection_string = (
    f"DRIVER={{{os.getenv('ODBC_DRIVER')}}};"
    f"SERVER={os.getenv('DB_SERVER')};"
    f"DATABASE={os.getenv('DB_NAME')};"
    f"UID={os.getenv('DB_USER')};"
    f"PWD={os.getenv('DB_PASSWORD')};"
    "TrustServerCertificate=yes;"
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
ODBC_DRIVER=ODBC Driver 18 for SQL Server
DB_SERVER=localhost
DB_NAME=app
DB_USER=sa
DB_PASSWORD=secret
```

Código:

```python
import os

import pyodbc
from dotenv import load_dotenv

load_dotenv()

connection_string = (
    f"DRIVER={{{os.getenv('ODBC_DRIVER')}}};"
    f"SERVER={os.getenv('DB_SERVER')};"
    f"DATABASE={os.getenv('DB_NAME')};"
    f"UID={os.getenv('DB_USER')};"
    f"PWD={os.getenv('DB_PASSWORD')};"
    "TrustServerCertificate=yes;"
)

connection = pyodbc.connect(connection_string)

connection.close()
```

## Función para crear conexión

```python
import os

import pyodbc
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    connection_string = (
        f"DRIVER={{{os.getenv('ODBC_DRIVER')}}};"
        f"SERVER={os.getenv('DB_SERVER')};"
        f"DATABASE={os.getenv('DB_NAME')};"
        f"UID={os.getenv('DB_USER')};"
        f"PWD={os.getenv('DB_PASSWORD')};"
        "TrustServerCertificate=yes;"
    )

    return pyodbc.connect(connection_string)
```

## Uso de la función de conexión

```python
connection = create_connection()

try:
    cursor = connection.cursor()
    cursor.execute("SELECT 1 AS value")
    print(cursor.fetchone())
finally:
    connection.close()
```

## Conexión a Access

`pyodbc` también puede usarse con Microsoft Access si existe el driver ODBC correspondiente.

Ejemplo conceptual:

```python
import pyodbc

connection_string = (
    "DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};"
    "DBQ=C:\\ruta\\archivo.accdb;"
)

connection = pyodbc.connect(connection_string)

connection.close()
```

Este uso depende de tener instalado el driver adecuado y de la arquitectura correcta.

## Arquitectura de 32 y 64 bits

En conexiones ODBC, especialmente con Access o drivers antiguos, puede importar la arquitectura:

```text
Python 32 bits -> driver ODBC 32 bits
Python 64 bits -> driver ODBC 64 bits
```

Si Python y el driver no coinciden, la conexión puede fallar.

## Listar drivers ODBC disponibles

```python
import pyodbc

drivers = pyodbc.drivers()

for driver in drivers:
    print(driver)
```

Esto ayuda a saber el nombre exacto que debe usarse en `DRIVER={...}`.

## SQL dinámico seguro

Los valores deben pasarse como parámetros.

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE name = ?",
    (name,)
)
```

Sin embargo, los nombres de tablas o columnas no se parametrizan con `?`.

Problemático:

```python
cursor.execute(
    "SELECT * FROM ?",
    ("dbo.products",)
)
```

Para nombres dinámicos, lo mejor es evitar valores libres y usar una lista permitida.

```python
allowed_tables = {
    "products": "dbo.products",
    "clients": "dbo.clients"
}

table_name = allowed_tables.get(requested_table)

if table_name is None:
    raise ValueError("Tabla no permitida")

query = f"SELECT * FROM {table_name}"

cursor.execute(query)
```

Este patrón solo es aceptable cuando `table_name` proviene de una lista controlada por el programa.

## Tipos de datos

`pyodbc` convierte muchos tipos entre Python y la base de datos mediante el driver ODBC.

Ejemplos frecuentes:

```text
Python str              -> texto SQL
Python int              -> entero SQL
Python float            -> número flotante SQL
Decimal                 -> decimal SQL
datetime.date           -> fecha SQL
datetime.datetime       -> fecha y hora SQL
bool                    -> booleano o bit según la base
None                    -> NULL
```

## Insertar fechas

```python
from datetime import date

cursor.execute(
    """
    INSERT INTO dbo.events (name, event_date)
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
    INSERT INTO dbo.products (name, description)
    VALUES (?, ?)
    """,
    ("Laptop", None)
)

connection.commit()
```

`None` se convierte en `NULL`.

## Uso con pandas

Pandas puede leer consultas SQL desde una conexión `pyodbc`.

```python
import pandas as pd
import pyodbc

connection = pyodbc.connect(connection_string)

try:
    df = pd.read_sql(
        """
        SELECT id, name, price, stock
        FROM dbo.products
        """,
        connection
    )

    print(df.head())
finally:
    connection.close()
```

## Uso con SQLAlchemy

SQLAlchemy puede usar `pyodbc` como driver.

Ejemplo conceptual con SQL Server:

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mssql+pyodbc://user:password@server/database"
    "?driver=ODBC+Driver+18+for+SQL+Server"
)
```

En ese caso:

```text
SQLAlchemy -> construye consultas, sesiones y modelos
pyodbc     -> conecta mediante ODBC como driver
```

## Uso con Flask

```python
import os

import pyodbc
from flask import Flask

app = Flask(__name__)


def create_connection():
    connection_string = (
        f"DRIVER={{{os.getenv('ODBC_DRIVER')}}};"
        f"SERVER={os.getenv('DB_SERVER')};"
        f"DATABASE={os.getenv('DB_NAME')};"
        f"UID={os.getenv('DB_USER')};"
        f"PWD={os.getenv('DB_PASSWORD')};"
        "TrustServerCertificate=yes;"
    )

    return pyodbc.connect(connection_string)


@app.route("/products")
def read_products():
    connection = create_connection()

    try:
        cursor = connection.cursor()
        cursor.execute("""
            SELECT id, name, price, stock
            FROM dbo.products
            WHERE is_active = 1
            ORDER BY name
        """)

        columns = [column[0] for column in cursor.description]

        return [
            dict(zip(columns, row))
            for row in cursor.fetchall()
        ]
    finally:
        connection.close()
```

En aplicaciones reales con muchas solicitudes, conviene evaluar pooling o SQLAlchemy.

## Uso con FastAPI

```python
import os

import pyodbc
from fastapi import FastAPI

app = FastAPI()


def create_connection():
    connection_string = (
        f"DRIVER={{{os.getenv('ODBC_DRIVER')}}};"
        f"SERVER={os.getenv('DB_SERVER')};"
        f"DATABASE={os.getenv('DB_NAME')};"
        f"UID={os.getenv('DB_USER')};"
        f"PWD={os.getenv('DB_PASSWORD')};"
        "TrustServerCertificate=yes;"
    )

    return pyodbc.connect(connection_string)


@app.get("/products")
def read_products():
    connection = create_connection()

    try:
        cursor = connection.cursor()
        cursor.execute("""
            SELECT id, name, price, stock
            FROM dbo.products
            WHERE is_active = 1
            ORDER BY name
        """)

        columns = [column[0] for column in cursor.description]

        return [
            dict(zip(columns, row))
            for row in cursor.fetchall()
        ]
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

`pyodbc` no suele usarse directamente como gestor de pooling de alto nivel.

En aplicaciones web, suele evaluarse:

```text
SQLAlchemy Engine
pool del framework
infraestructura de conexión
```

Con SQLAlchemy:

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mssql+pyodbc://user:password@server/database"
    "?driver=ODBC+Driver+18+for+SQL+Server",
    pool_size=5,
    max_overflow=10
)
```

Este enfoque suele ser más robusto para aplicaciones con varias solicitudes concurrentes.

## Uso con Django

Django tiene su propio ORM y configuración de base de datos.

En proyectos Django, normalmente no se usa `pyodbc` directamente dentro de vistas o servicios salvo que exista una necesidad específica.

Para SQL Server en Django suelen usarse backends externos compatibles con SQL Server.

## Procedimientos almacenados

`pyodbc` puede ejecutar procedimientos almacenados mediante SQL del motor conectado.

Ejemplo conceptual en SQL Server:

```python
cursor.execute(
    "EXEC dbo.GetProductById ?",
    (product_id,)
)

rows = cursor.fetchall()
```

También puede usarse una sintaxis ODBC de llamada a procedimiento en ciertos entornos:

```python
cursor.execute(
    "{CALL dbo.GetProductById (?)}",
    (product_id,)
)
```

La sintaxis exacta puede depender del driver y del motor de base de datos.

## Múltiples result sets

Algunos procedimientos pueden devolver varios conjuntos de resultados.

Puede usarse:

```python
cursor.nextset()
```

Ejemplo conceptual:

```python
cursor.execute("EXEC dbo.ProcedureWithMultipleResults")

while True:
    rows = cursor.fetchall()

    print(rows)

    if not cursor.nextset():
        break
```

Este patrón depende del procedimiento y del driver.

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

import pyodbc
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    connection_string = (
        f"DRIVER={{{os.getenv('ODBC_DRIVER')}}};"
        f"SERVER={os.getenv('DB_SERVER')};"
        f"DATABASE={os.getenv('DB_NAME')};"
        f"UID={os.getenv('DB_USER')};"
        f"PWD={os.getenv('DB_PASSWORD')};"
        "TrustServerCertificate=yes;"
    )

    return pyodbc.connect(connection_string)
```

## `repositories.py`

```python
from .database import create_connection


def rows_to_dicts(cursor, rows):
    columns = [column[0] for column in cursor.description]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


def list_products():
    connection = create_connection()

    try:
        cursor = connection.cursor()

        cursor.execute("""
            SELECT id, name, price, stock
            FROM dbo.products
            WHERE is_active = 1
            ORDER BY name
        """)

        return rows_to_dicts(cursor, cursor.fetchall())
    finally:
        connection.close()


def get_product(product_id):
    connection = create_connection()

    try:
        cursor = connection.cursor()

        cursor.execute(
            """
            SELECT id, name, price, stock
            FROM dbo.products
            WHERE id = ?
            """,
            (product_id,)
        )

        row = cursor.fetchone()

        if row is None:
            return None

        return rows_to_dicts(cursor, [row])[0]
    finally:
        connection.close()


def create_product(name, price, stock):
    connection = create_connection()

    try:
        cursor = connection.cursor()

        cursor.execute(
            """
            INSERT INTO dbo.products (name, price, stock)
            OUTPUT INSERTED.id
            VALUES (?, ?, ?)
            """,
            (name, price, stock)
        )

        new_id = cursor.fetchone()[0]

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

## Instalar `pyodbc` sin instalar el driver ODBC

Problemático:

```bash
python -m pip install pyodbc
```

y luego intentar conectar sin tener el driver correspondiente.

Debe existir un driver ODBC instalado y visible para el sistema.

## Escribir mal el nombre del driver

Problemático:

```python
"DRIVER={SQL Server};"
```

cuando el driver instalado tiene otro nombre.

Puede revisarse con:

```python
import pyodbc

print(pyodbc.drivers())
```

## Confundir placeholders

Problemático:

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE id = %s",
    (product_id,)
)
```

Correcto en `pyodbc`:

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE id = ?",
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
cursor.execute(f"SELECT * FROM dbo.products WHERE name = '{name}'")
```

Correcto:

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE name = ?",
    (name,)
)
```

## Olvidar `commit()`

Problemático:

```python
cursor.execute(
    """
    INSERT INTO dbo.products (name, price)
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

## No cerrar conexiones

Menos recomendable:

```python
connection = pyodbc.connect(connection_string)
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

## Guardar credenciales en el código

Problemático:

```python
"PWD=secret;"
```

Mejor:

```python
os.getenv("DB_PASSWORD")
```

## Problemas de arquitectura 32 bits y 64 bits

Si se usa un driver de 32 bits con Python de 64 bits, o viceversa, la conexión puede fallar.

Esto es común con drivers antiguos o con Microsoft Access.

## Usar `pyodbc` cuando se necesita ORM

`pyodbc` ejecuta SQL directamente.

Si se necesita trabajar con clases y modelos, puede ser mejor usar:

```text
sqlalchemy
django ORM
```

## Buenas prácticas

## Usar parámetros en consultas

```python
cursor.execute(
    "SELECT * FROM dbo.products WHERE id = ?",
    (product_id,)
)
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

## Usar variables de entorno

```python
os.getenv("DB_PASSWORD")
```

## Listar drivers disponibles si hay errores de conexión

```python
pyodbc.drivers()
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

## Evitar SQL dinámico no controlado

Si una tabla o columna es dinámica, debe validarse contra una lista permitida.

## Usar SQLAlchemy si se requiere pooling u ORM

```python
mssql+pyodbc://...
```

## Probar `fast_executemany` antes de usarlo en producción

```python
cursor.fast_executemany = True
```

Debe validarse con el volumen real, el driver y la base destino.

## Ejemplo integrado

```python
import os
from decimal import Decimal

import pyodbc
from dotenv import load_dotenv

load_dotenv()


def create_connection():
    connection_string = (
        f"DRIVER={{{os.getenv('ODBC_DRIVER')}}};"
        f"SERVER={os.getenv('DB_SERVER')};"
        f"DATABASE={os.getenv('DB_NAME')};"
        f"UID={os.getenv('DB_USER')};"
        f"PWD={os.getenv('DB_PASSWORD')};"
        "TrustServerCertificate=yes;"
    )

    return pyodbc.connect(connection_string)


def rows_to_dicts(cursor, rows):
    columns = [column[0] for column in cursor.description]

    return [
        dict(zip(columns, row))
        for row in rows
    ]


def create_table():
    connection = create_connection()

    try:
        cursor = connection.cursor()

        cursor.execute("""
            IF OBJECT_ID('dbo.products', 'U') IS NULL
            CREATE TABLE dbo.products (
                id INT IDENTITY(1,1) PRIMARY KEY,
                name NVARCHAR(100) NOT NULL UNIQUE,
                price DECIMAL(10, 2) NOT NULL,
                stock INT NOT NULL DEFAULT 0,
                is_active BIT NOT NULL DEFAULT 1
            )
        """)

        connection.commit()
    finally:
        connection.close()


def create_product(name, price, stock=0):
    connection = create_connection()

    try:
        cursor = connection.cursor()

        cursor.execute(
            """
            INSERT INTO dbo.products (name, price, stock)
            OUTPUT INSERTED.id
            VALUES (?, ?, ?)
            """,
            (name, Decimal(str(price)), stock)
        )

        new_id = cursor.fetchone()[0]

        connection.commit()

        return new_id
    except pyodbc.IntegrityError:
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
        cursor = connection.cursor()

        cursor.execute("""
            SELECT id, name, price, stock, is_active
            FROM dbo.products
            WHERE is_active = 1
            ORDER BY name
        """)

        return rows_to_dicts(cursor, cursor.fetchall())
    finally:
        connection.close()


def update_stock(product_id, stock):
    connection = create_connection()

    try:
        cursor = connection.cursor()

        cursor.execute(
            """
            UPDATE dbo.products
            SET stock = ?
            WHERE id = ?
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
        cursor = connection.cursor()

        cursor.execute(
            """
            UPDATE dbo.products
            SET is_active = 0
            WHERE id = ?
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

`pyodbc` se relaciona especialmente con:

```text
sqlalchemy
pandas
flask
fastapi
python-dotenv
pytest
openpyxl
xlsxwriter
```

## Relación con SQLAlchemy

SQLAlchemy puede usar `pyodbc` como driver:

```python
engine = create_engine(
    "mssql+pyodbc://user:password@server/database"
    "?driver=ODBC+Driver+18+for+SQL+Server"
)
```

## Relación con pandas

Pandas puede leer resultados SQL mediante una conexión `pyodbc` o mediante SQLAlchemy.

```python
df = pd.read_sql("SELECT * FROM dbo.products", connection)
```

## Relación con Flask y FastAPI

`pyodbc` puede usarse directamente en backends web, pero para aplicaciones medianas o grandes conviene evaluar:

```text
pooling
SQLAlchemy
capa repository
manejo centralizado de errores
```

## Relación con Excel y Access

`pyodbc` puede aparecer en flujos corporativos donde existen fuentes ODBC hacia Access, Excel, SQL Server u otros sistemas.

Aun así, para manipulación directa de archivos Excel suele ser más apropiado:

```text
openpyxl
xlsxwriter
pandas
```

## Orden didáctico interno

```text
1. Propósito de pyodbc
2. Instalación
3. Relación con ODBC y drivers
4. Conexión con cadena completa o DSN
5. Connection y Cursor
6. execute()
7. Parámetros con ?
8. fetchone(), fetchall() y fetchmany()
9. Conversión de filas a diccionarios
10. INSERT, UPDATE, DELETE y OUTPUT INSERTED
11. Transacciones, commit y rollback
12. autocommit
13. Variables de entorno
14. pyodbc.drivers()
15. Uso con SQLAlchemy, pandas, Flask y FastAPI
16. Errores comunes
17. Buenas prácticas
```