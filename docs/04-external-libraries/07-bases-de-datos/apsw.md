# `apsw`

## Propósito

`apsw` es una librería externa para trabajar con SQLite desde Python con mayor control que el módulo estándar `sqlite3`.

Su nombre significa `Another Python SQLite Wrapper`.

Permite usar SQLite de forma más cercana a su API nativa, ejecutar SQL, manejar conexiones, cursores, transacciones, backups, extensiones, funciones personalizadas, consultas avanzadas y características específicas de SQLite.

## Naturaleza de la librería

`apsw` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install apsw
```

La importación habitual es:

```python
import apsw
```

`apsw` está orientado a SQLite.

No se usa para conectarse a:

```text
PostgreSQL
MySQL
SQL Server
Oracle Database
MongoDB
Redis
```

## Relación con SQLite

SQLite es una base de datos relacional embebida basada en archivos.

No requiere servidor.

Una base SQLite suele ser un archivo local:

```text
app.db
database.sqlite
data.db
```

`apsw` permite acceder a SQLite desde Python con una capa más delgada y más cercana al comportamiento real de SQLite.

## Diferencia frente a `sqlite3`

Python ya incluye el módulo estándar `sqlite3`.

```python
import sqlite3
```

`sqlite3` suele ser suficiente para:

```text
scripts simples
aplicaciones pequeñas
uso DB-API estándar
consultas SQL comunes
compatibilidad con patrones similares a otros drivers
```

`apsw` conviene cuando se necesita:

```text
mayor control sobre SQLite
usar más partes de la API de SQLite
comportamiento más explícito
features avanzadas de SQLite
mejor acceso a extensiones
mayor control de transacciones
herramientas avanzadas de backup, tracing o virtual tables
```

## Regla práctica

Para aprender SQLite desde Python, empezar con:

```python
sqlite3
```

Para usar SQLite de forma más avanzada, evaluar:

```python
apsw
```

## Idea central

La idea principal de `apsw` es abrir una conexión SQLite y ejecutar SQL desde un cursor.

Ejemplo mínimo:

```python
import apsw

connection = apsw.Connection("app.db")
cursor = connection.cursor()

try:
    for row in cursor.execute("SELECT 1"):
        print(row)
finally:
    connection.close()
```

Salida conceptual:

```text
(1,)
```

## Cuándo usar APSW

Conviene usar `apsw` cuando se necesita:

```text
usar SQLite con mayor control
aprovechar extensiones avanzadas de SQLite
manejar transacciones explícitamente
trabajar con backups de SQLite
usar funciones personalizadas
usar virtual tables
usar full-text search
usar JSON de SQLite
usar tracing o profiling
evitar abstracciones adicionales sobre SQLite
```

## Cuándo no usar APSW

No siempre conviene usar `apsw` cuando se necesita:

```text
un uso básico de SQLite
compatibilidad DB-API tradicional
código parecido a otros drivers SQL
un ORM
una base de datos servidor
alta concurrencia de escritura
consultas analíticas grandes
```

Para esos casos pueden corresponder:

```text
sqlite3
sqlalchemy
peewee
sqlmodel
duckdb
postgresql
```

## Instalación

Instalación básica:

```bash
python -m pip install apsw
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
apsw==3.x.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import apsw

print(apsw.apswversion())
print(apsw.sqlitelibversion())
```

Esto permite revisar:

```text
versión de APSW
versión de SQLite usada por APSW
```

## Conexión básica

```python
import apsw

connection = apsw.Connection("app.db")

connection.close()
```

## Base en memoria

```python
connection = apsw.Connection(":memory:")
```

Una base en memoria existe solo durante la ejecución del programa.

## Cursor

Un cursor permite ejecutar SQL.

```python
cursor = connection.cursor()
```

Uso:

```python
for row in cursor.execute("SELECT 1"):
    print(row)
```

En APSW, una consulta puede iterarse directamente.

## Crear tabla

```python
import apsw

connection = apsw.Connection("app.db")
cursor = connection.cursor()

try:
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE,
            price REAL NOT NULL,
            stock INTEGER NOT NULL DEFAULT 0,
            is_active INTEGER NOT NULL DEFAULT 1
        )
    """)
finally:
    connection.close()
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
```

## Parámetros SQL

En APSW, como en SQLite, pueden usarse parámetros.

Parámetros posicionales:

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

Parámetros con nombre:

```python
cursor.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE name = :name
    """,
    {
        "name": "Laptop"
    }
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
    WHERE name = ?
    """,
    (name,)
)
```

Los valores externos deben pasarse como parámetros.

## Tupla de un solo parámetro

Correcto:

```python
(product_id,)
```

Incorrecto:

```python
(product_id)
```

La coma es necesaria para que Python lo interprete como tupla.

## Consultar datos

```python
for row in cursor.execute("""
    SELECT id, name, price, stock
    FROM products
    ORDER BY name
"""):
    print(row)
```

Por defecto, las filas se devuelven como tuplas.

```text
(1, "Laptop", 3500.0, 5)
```

## Consultar un registro

```python
def get_product(connection, product_id):
    cursor = connection.cursor()

    for row in cursor.execute(
        """
        SELECT id, name, price, stock
        FROM products
        WHERE id = ?
        """,
        (product_id,)
    ):
        return row

    return None
```

## Consultar todos los registros

```python
def list_products(connection):
    cursor = connection.cursor()

    return list(
        cursor.execute("""
            SELECT id, name, price, stock
            FROM products
            WHERE is_active = 1
            ORDER BY name
        """)
    )
```

## Convertir filas a diccionarios

Como las filas se reciben como tuplas, puede transformarse el resultado manualmente.

```python
def rows_to_dicts(columns, rows):
    return [
        dict(zip(columns, row))
        for row in rows
    ]
```

Uso:

```python
columns = [
    "id",
    "name",
    "price",
    "stock"
]

rows = list(
    cursor.execute("""
        SELECT id, name, price, stock
        FROM products
    """)
)

products = rows_to_dicts(columns, rows)
```

## Insertar varios registros

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
```

## Obtener último ID insertado

```python
cursor.execute(
    """
    INSERT INTO products (name, price, stock)
    VALUES (?, ?, ?)
    """,
    ("Laptop", 3500, 5)
)

product_id = connection.last_insert_rowid()

print(product_id)
```

## Actualizar datos

```python
cursor.execute(
    """
    UPDATE products
    SET price = ?,
        stock = ?
    WHERE id = ?
    """,
    (3600, 4, 1)
)
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
```

## Eliminación lógica

En aplicaciones reales, muchas veces se prefiere marcar registros como inactivos.

```python
cursor.execute(
    """
    UPDATE products
    SET is_active = 0
    WHERE id = ?
    """,
    (product_id,)
)
```

Consulta de activos:

```python
cursor.execute("""
    SELECT id, name, price, stock
    FROM products
    WHERE is_active = 1
""")
```

## Cambios realizados

APSW permite consultar cambios realizados por la conexión.

```python
changes = connection.changes()

print(changes)
```

También puede consultarse el total acumulado.

```python
total = connection.total_changes()

print(total)
```

## Transacciones

APSW no intenta ocultar el comportamiento de SQLite.

Por eso, conviene manejar transacciones de forma explícita.

```python
cursor.execute("BEGIN")

try:
    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        ("Laptop", 3500, 5)
    )

    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        ("Mouse", 80, 20)
    )

    cursor.execute("COMMIT")
except Exception:
    cursor.execute("ROLLBACK")
    raise
```

## Tipos de transacción en SQLite

SQLite permite distintos modos de inicio de transacción.

```sql
BEGIN
BEGIN DEFERRED
BEGIN IMMEDIATE
BEGIN EXCLUSIVE
```

Ejemplo:

```python
cursor.execute("BEGIN IMMEDIATE")
```

Uso conceptual:

```text
BEGIN DEFERRED   -> inicia de forma diferida
BEGIN IMMEDIATE  -> intenta tomar reserva de escritura al inicio
BEGIN EXCLUSIVE  -> toma un bloqueo más fuerte
```

## Commit

```python
cursor.execute("COMMIT")
```

## Rollback

```python
cursor.execute("ROLLBACK")
```

## PRAGMA

SQLite permite configurar comportamientos mediante `PRAGMA`.

Ejemplo para activar claves foráneas:

```python
cursor.execute("PRAGMA foreign_keys = ON")
```

Ejemplo para usar WAL:

```python
cursor.execute("PRAGMA journal_mode = WAL")
```

## Foreign keys

SQLite requiere activar claves foráneas por conexión.

```python
cursor.execute("PRAGMA foreign_keys = ON")
```

Esto es importante cuando se usan restricciones `FOREIGN KEY`.

## WAL mode

WAL significa `Write-Ahead Logging`.

Puede mejorar ciertos patrones de concurrencia de lectura y escritura.

```python
cursor.execute("PRAGMA journal_mode = WAL")
```

Debe evaluarse según el caso de uso.

## Configuración recomendada para conexión

```python
def create_connection(path):
    connection = apsw.Connection(path)
    cursor = connection.cursor()

    cursor.execute("PRAGMA foreign_keys = ON")
    cursor.execute("PRAGMA journal_mode = WAL")

    return connection
```

## Funciones reutilizables

## Crear tabla

```python
def create_tables(connection):
    cursor = connection.cursor()

    cursor.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE,
            price REAL NOT NULL,
            stock INTEGER NOT NULL DEFAULT 0,
            is_active INTEGER NOT NULL DEFAULT 1
        )
    """)
```

## Crear producto

```python
def create_product(connection, name, price, stock=0):
    cursor = connection.cursor()

    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        (name, price, stock)
    )

    return connection.last_insert_rowid()
```

## Listar productos

```python
def list_products(connection):
    cursor = connection.cursor()

    columns = [
        "id",
        "name",
        "price",
        "stock",
        "is_active"
    ]

    rows = list(
        cursor.execute("""
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE is_active = 1
            ORDER BY name
        """)
    )

    return rows_to_dicts(columns, rows)
```

## Obtener producto por ID

```python
def get_product(connection, product_id):
    cursor = connection.cursor()

    columns = [
        "id",
        "name",
        "price",
        "stock",
        "is_active"
    ]

    for row in cursor.execute(
        """
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE id = ?
        """,
        (product_id,)
    ):
        return dict(zip(columns, row))

    return None
```

## Actualizar stock

```python
def update_stock(connection, product_id, stock):
    cursor = connection.cursor()

    cursor.execute(
        """
        UPDATE products
        SET stock = ?
        WHERE id = ?
        """,
        (stock, product_id)
    )

    return connection.changes()
```

## Desactivar producto

```python
def deactivate_product(connection, product_id):
    cursor = connection.cursor()

    cursor.execute(
        """
        UPDATE products
        SET is_active = 0
        WHERE id = ?
        """,
        (product_id,)
    )

    return connection.changes()
```

## Manejo de errores

APSW expone errores propios para problemas de SQLite.

```python
import apsw

try:
    cursor.execute("SELECT * FROM table_that_does_not_exist")
except apsw.SQLError as error:
    print("Error SQL:", error)
```

## Errores frecuentes

```text
apsw.Error
apsw.SQLError
apsw.ConstraintError
apsw.BusyError
apsw.LockedError
apsw.CorruptError
apsw.IOError
```

## Error de restricción

```python
try:
    create_product(
        connection,
        name="Laptop",
        price=3500,
        stock=5
    )
except apsw.ConstraintError:
    print("No se pudo insertar el registro por una restricción")
```

Esto puede ocurrir por:

```text
UNIQUE
NOT NULL
CHECK
FOREIGN KEY
PRIMARY KEY
```

## Error de bloqueo

SQLite puede generar errores de bloqueo si otra conexión está escribiendo.

```python
try:
    cursor.execute(
        """
        UPDATE products
        SET stock = ?
        WHERE id = ?
        """,
        (10, 1)
    )
except apsw.BusyError:
    print("La base de datos está ocupada")
```

## Busy timeout

Puede configurarse un tiempo de espera ante bloqueos.

```python
connection.set_busy_timeout(5000)
```

El valor está en milisegundos.

Ejemplo:

```text
5000 -> 5 segundos
```

## Backup

APSW permite usar capacidades de backup de SQLite.

Ejemplo conceptual:

```python
source = apsw.Connection("app.db")
target = apsw.Connection("backup.db")

try:
    with target.backup("main", source, "main") as backup:
        backup.step()
finally:
    source.close()
    target.close()
```

Este patrón permite copiar una base SQLite a otra.

## Funciones SQL personalizadas

SQLite permite registrar funciones personalizadas desde Python.

```python
def double_value(value):
    return value * 2


connection.create_scalar_function(
    "double_value",
    double_value,
    1
)
```

Uso:

```python
for row in cursor.execute("""
    SELECT double_value(10)
"""):
    print(row)
```

Salida conceptual:

```text
(20,)
```

## Agregaciones personalizadas

SQLite también permite crear agregaciones personalizadas.

Ejemplo conceptual:

```python
class ProductCount:
    def __init__(self):
        self.count = 0

    def step(self, value):
        if value is not None:
            self.count += 1

    def final(self):
        return self.count
```

Registro:

```python
connection.create_aggregate_function(
    "custom_count",
    ProductCount,
    1
)
```

Uso:

```python
for row in cursor.execute("""
    SELECT custom_count(name)
    FROM products
"""):
    print(row)
```

## Collations personalizadas

Una collation define cómo comparar textos.

Ejemplo conceptual:

```python
def reverse_collation(left, right):
    if left == right:
        return 0

    if left < right:
        return 1

    return -1


connection.create_collation(
    "REVERSE_ORDER",
    reverse_collation
)
```

Uso:

```python
for row in cursor.execute("""
    SELECT name
    FROM products
    ORDER BY name COLLATE REVERSE_ORDER
"""):
    print(row)
```

## Full-text search

SQLite puede trabajar con FTS, como FTS5, si está disponible en la compilación usada.

Ejemplo conceptual:

```python
cursor.execute("""
    CREATE VIRTUAL TABLE IF NOT EXISTS product_search
    USING fts5(name, description)
""")
```

Insertar:

```python
cursor.execute(
    """
    INSERT INTO product_search (name, description)
    VALUES (?, ?)
    """,
    ("Laptop", "Equipo portátil para oficina")
)
```

Buscar:

```python
for row in cursor.execute(
    """
    SELECT rowid, name
    FROM product_search
    WHERE product_search MATCH ?
    """,
    ("portátil",)
):
    print(row)
```

## JSON en SQLite

SQLite puede incluir funciones JSON según la versión y compilación.

Ejemplo conceptual:

```python
cursor.execute("""
    CREATE TABLE IF NOT EXISTS events (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        payload TEXT NOT NULL
    )
""")
```

Consulta con función JSON:

```python
for row in cursor.execute("""
    SELECT json_extract(payload, '$.type')
    FROM events
"""):
    print(row)
```

## BLOBs

SQLite permite guardar datos binarios.

```python
from pathlib import Path

content = Path("file.pdf").read_bytes()

cursor.execute(
    """
    INSERT INTO files (filename, content)
    VALUES (?, ?)
    """,
    ("file.pdf", content)
)
```

Tabla:

```python
cursor.execute("""
    CREATE TABLE IF NOT EXISTS files (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        filename TEXT NOT NULL,
        content BLOB NOT NULL
    )
""")
```

## Uso con pathlib

```python
from pathlib import Path
import apsw

database_path = Path("data") / "app.db"

database_path.parent.mkdir(
    parents=True,
    exist_ok=True
)

connection = apsw.Connection(str(database_path))
```

## Uso con pandas

APSW puede alimentar un `DataFrame`.

```python
import pandas as pd

rows = list(
    cursor.execute("""
        SELECT id, name, price, stock
        FROM products
    """)
)

df = pd.DataFrame(
    rows,
    columns=[
        "id",
        "name",
        "price",
        "stock"
    ]
)

print(df.head())
```

Para análisis local intensivo, también puede evaluarse:

```text
duckdb
pandas
polars
```

## Uso con FastAPI

APSW puede usarse en una API pequeña o interna, aunque SQLite debe evaluarse con cuidado en escenarios de alta escritura concurrente.

```python
import apsw
from contextlib import asynccontextmanager
from fastapi import FastAPI, HTTPException


def create_connection():
    connection = apsw.Connection("app.db")
    connection.set_busy_timeout(5000)

    cursor = connection.cursor()
    cursor.execute("PRAGMA foreign_keys = ON")
    cursor.execute("PRAGMA journal_mode = WAL")

    return connection


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.connection = create_connection()
    create_tables(app.state.connection)

    yield

    app.state.connection.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
def read_products():
    return list_products(app.state.connection)


@app.get("/products/{product_id}")
def read_product(product_id: int):
    product = get_product(
        app.state.connection,
        product_id
    )

    if product is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return product
```

## Uso con scripts

```python
import apsw


def rows_to_dicts(columns, rows):
    return [
        dict(zip(columns, row))
        for row in rows
    ]


def create_connection(path):
    connection = apsw.Connection(path)
    connection.set_busy_timeout(5000)

    cursor = connection.cursor()
    cursor.execute("PRAGMA foreign_keys = ON")
    cursor.execute("PRAGMA journal_mode = WAL")

    return connection


def create_tables(connection):
    cursor = connection.cursor()

    cursor.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE,
            price REAL NOT NULL,
            stock INTEGER NOT NULL DEFAULT 0,
            is_active INTEGER NOT NULL DEFAULT 1
        )
    """)


def create_product(connection, name, price, stock=0):
    cursor = connection.cursor()

    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        (name, price, stock)
    )

    return connection.last_insert_rowid()


def list_products(connection):
    cursor = connection.cursor()

    columns = [
        "id",
        "name",
        "price",
        "stock",
        "is_active"
    ]

    rows = list(
        cursor.execute("""
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE is_active = 1
            ORDER BY name
        """)
    )

    return rows_to_dicts(columns, rows)


def main():
    connection = create_connection("app.db")

    try:
        create_tables(connection)

        cursor = connection.cursor()
        cursor.execute("BEGIN")

        try:
            create_product(
                connection,
                "Laptop",
                3500,
                stock=5
            )

            create_product(
                connection,
                "Mouse",
                80,
                stock=20
            )

            cursor.execute("COMMIT")
        except apsw.ConstraintError:
            cursor.execute("ROLLBACK")

        products = list_products(connection)

        for product in products:
            print(
                product["id"],
                product["name"],
                product["price"],
                product["stock"]
            )

    finally:
        connection.close()


if __name__ == "__main__":
    main()
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
import apsw


def create_connection(path="app.db"):
    connection = apsw.Connection(path)
    connection.set_busy_timeout(5000)

    cursor = connection.cursor()
    cursor.execute("PRAGMA foreign_keys = ON")
    cursor.execute("PRAGMA journal_mode = WAL")

    return connection
```

## `repositories.py`

```python
def rows_to_dicts(columns, rows):
    return [
        dict(zip(columns, row))
        for row in rows
    ]


def list_products(connection):
    cursor = connection.cursor()

    columns = [
        "id",
        "name",
        "price",
        "stock",
        "is_active"
    ]

    rows = list(
        cursor.execute("""
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE is_active = 1
            ORDER BY name
        """)
    )

    return rows_to_dicts(columns, rows)


def get_product(connection, product_id):
    cursor = connection.cursor()

    columns = [
        "id",
        "name",
        "price",
        "stock",
        "is_active"
    ]

    for row in cursor.execute(
        """
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE id = ?
        """,
        (product_id,)
    ):
        return dict(zip(columns, row))

    return None


def create_product(connection, name, price, stock=0):
    cursor = connection.cursor()

    cursor.execute(
        """
        INSERT INTO products (name, price, stock)
        VALUES (?, ?, ?)
        """,
        (name, price, stock)
    )

    return connection.last_insert_rowid()
```

## Separación por responsabilidades

```text
database.py      -> apertura y configuración de conexión SQLite
repositories.py  -> consultas SQL
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Migraciones

APSW no es una herramienta de migraciones.

Para cambios simples pueden usarse scripts SQL versionados.

Ejemplo:

```text
migrations/
├─ 001_create_products.sql
├─ 002_add_is_active.sql
└─ 003_create_product_search.sql
```

Para proyectos más estructurados, puede evaluarse:

```text
alembic
SQLAlchemy
herramientas propias de migración
```

## Concurrencia en SQLite

SQLite funciona muy bien para aplicaciones locales, herramientas internas y prototipos.

Sin embargo, no debe asumirse que tiene el mismo perfil de escritura concurrente que PostgreSQL o MySQL.

Regla práctica:

```text
muchas lecturas y pocas escrituras -> SQLite puede ser suficiente
muchas escrituras concurrentes     -> evaluar PostgreSQL u otra base servidor
```

APSW da más control sobre SQLite, pero no cambia la naturaleza de SQLite como base embebida.

## Errores comunes

## Usar APSW como si fuera `sqlite3`

APSW se parece en algunos aspectos, pero no busca ocultar SQLite ni copiar completamente la interfaz DB-API.

Para código DB-API convencional, usar `sqlite3`.

## Olvidar cerrar la conexión

Menos recomendable:

```python
connection = apsw.Connection("app.db")
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

## No activar foreign keys

Si se usan claves foráneas, activar:

```python
cursor.execute("PRAGMA foreign_keys = ON")
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

## Olvidar la coma en una tupla de un parámetro

Problemático:

```python
(product_id)
```

Correcto:

```python
(product_id,)
```

## No manejar transacciones explícitamente

Si varias operaciones deben completarse juntas, usar:

```sql
BEGIN
COMMIT
ROLLBACK
```

## Usar `fetchall()` mentalmente

En APSW es común iterar directamente sobre el resultado:

```python
for row in cursor.execute("SELECT ..."):
    ...
```

También puede convertirse a lista:

```python
rows = list(cursor.execute("SELECT ..."))
```

## Esperar diccionarios directamente

Por defecto, las filas se devuelven como tuplas.

Si se necesitan diccionarios, convertir manualmente o configurar una estrategia específica de filas.

## Usar SQLite como servidor multiusuario pesado

APSW no convierte SQLite en PostgreSQL.

Para alta escritura concurrente, conviene evaluar una base servidor.

## No configurar busy timeout

Si hay varias conexiones, puede aparecer `BusyError`.

Puede configurarse:

```python
connection.set_busy_timeout(5000)
```

## Guardar rutas sin controlar carpetas

Problemático:

```python
apsw.Connection("data/app.db")
```

si la carpeta `data` no existe.

Mejor:

```python
from pathlib import Path

path = Path("data") / "app.db"
path.parent.mkdir(parents=True, exist_ok=True)

connection = apsw.Connection(str(path))
```

## Buenas prácticas

## Usar parámetros SQL

```python
WHERE id = ?
```

## Manejar transacciones explícitamente

```python
BEGIN
COMMIT
ROLLBACK
```

## Activar foreign keys si hay relaciones

```python
PRAGMA foreign_keys = ON
```

## Configurar busy timeout

```python
connection.set_busy_timeout(5000)
```

## Usar WAL cuando convenga

```python
PRAGMA journal_mode = WAL
```

## Cerrar conexiones

```python
connection.close()
```

## Separar SQL en repositorios

```text
repositories.py
```

## Usar `sqlite3` para casos simples

APSW es más potente, pero `sqlite3` puede ser suficiente para proyectos básicos.

## Usar PostgreSQL cuando la concurrencia crece

SQLite es muy útil, pero tiene un perfil distinto al de una base servidor.

## Relación con otras librerías

`apsw` se relaciona especialmente con:

```text
sqlite
sqlite3
sqlalchemy
pandas
fastapi
pytest
pathlib
```

## Relación con `sqlite3`

`sqlite3` es el módulo estándar de Python.

`apsw` es una alternativa externa más cercana a SQLite y más orientada a control avanzado.

## Relación con SQLAlchemy

SQLAlchemy suele usarse cuando se quiere una capa ORM o Core más portable.

APSW se usa cuando se quiere trabajar directamente con SQLite.

## Relación con pandas

APSW puede alimentar DataFrames a partir de resultados SQL.

```python
pd.DataFrame(rows, columns=columns)
```

## Relación con FastAPI

Puede usarse en APIs pequeñas o herramientas internas, siempre evaluando la concurrencia de escritura.

## Relación con DuckDB

DuckDB suele ser más cómodo para análisis local de archivos y consultas analíticas.

SQLite con APSW es más adecuado para almacenamiento embebido, transaccional y control avanzado de SQLite.

## Orden didáctico interno

```text
1. Propósito de apsw
2. Instalación
3. Relación con SQLite
4. Diferencia frente a sqlite3
5. Connection
6. Cursor
7. execute() y executemany()
8. Parámetros SQL
9. INSERT, SELECT, UPDATE y DELETE
10. last_insert_rowid(), changes() y total_changes()
11. Transacciones explícitas
12. PRAGMA, foreign_keys y WAL
13. Funciones SQL personalizadas
14. Full-text search
15. JSON y BLOBs
16. Backup
17. Uso con pandas y FastAPI
18. Organización recomendada
19. Migraciones
20. Concurrencia en SQLite
21. Errores comunes
22. Buenas prácticas
```