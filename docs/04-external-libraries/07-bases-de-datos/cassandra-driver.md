# `cassandra-driver`

## Propósito

`cassandra-driver` es una librería externa para conectar Python con Apache Cassandra.

Se utiliza para ejecutar consultas CQL, crear keyspaces, crear tablas, insertar registros, consultar datos, actualizar información, eliminar filas, trabajar con prepared statements, manejar sesiones y conectar aplicaciones Python con clústeres Cassandra.

Apache Cassandra es una base de datos distribuida, orientada a alta disponibilidad, escalabilidad horizontal y manejo de grandes volúmenes de datos.

## Naturaleza de la librería

`cassandra-driver` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install cassandra-driver
```

La importación habitual es:

```python
from cassandra.cluster import Cluster
```

Aunque el paquete se instala como:

```text
cassandra-driver
```

se importa desde:

```python
cassandra
```

## Relación con Apache Cassandra

Apache Cassandra no es una base relacional tradicional.

No trabaja principalmente con:

```text
joins
foreign keys
transacciones relacionales complejas
normalización clásica
```

Trabaja con conceptos como:

```text
cluster
node
keyspace
table
partition key
clustering columns
CQL
replication factor
consistency level
```

## Idea central

La idea principal de `cassandra-driver` es conectarse a un clúster Cassandra y ejecutar consultas CQL mediante una sesión.

Flujo típico:

```text
Python -> Cluster -> Session -> CQL -> Cassandra
```

Ejemplo mínimo:

```python
from cassandra.cluster import Cluster

cluster = Cluster(["127.0.0.1"])

session = cluster.connect()

try:
    rows = session.execute("SELECT release_version FROM system.local")

    for row in rows:
        print(row.release_version)
finally:
    cluster.shutdown()
```

## Cuándo usar Cassandra

Conviene usar Cassandra cuando se necesita:

```text
alta disponibilidad
escalabilidad horizontal
datos distribuidos en varios nodos
grandes volúmenes de escritura
lecturas por patrones conocidos
replicación entre nodos o regiones
sistemas tolerantes a fallos
series de eventos de gran escala
datos con acceso por clave o partición
```

## Cuándo no usar Cassandra

No suele ser la mejor opción cuando se necesita:

```text
joins frecuentes
consultas ad hoc complejas
reportes tabulares tradicionales
transacciones relacionales fuertes
modelo de datos altamente normalizado
consultas con filtros arbitrarios
agregaciones analíticas complejas
base pequeña local
```

Para esos casos pueden corresponder:

```text
PostgreSQL
MySQL
SQLite
SQL Server
DuckDB
MongoDB
Neo4j
```

## Instalación

Instalación básica:

```bash
python -m pip install cassandra-driver
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
cassandra-driver==3.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import cassandra

print(cassandra.__version__)
```

## Importaciones frecuentes

```python
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider
from cassandra.query import SimpleStatement
from cassandra.query import BatchStatement
from cassandra.query import ConsistencyLevel
```

## Conexión básica

```python
from cassandra.cluster import Cluster

cluster = Cluster(["127.0.0.1"])

session = cluster.connect()

cluster.shutdown()
```

## Contact points

Los contact points son direcciones de nodos Cassandra usados para iniciar conexión con el clúster.

```python
cluster = Cluster([
    "127.0.0.1",
    "127.0.0.2",
    "127.0.0.3"
])
```

No es necesario listar todos los nodos, pero sí suficientes puntos de contacto para descubrir el clúster.

## Puerto

El puerto nativo frecuente de Cassandra es:

```text
9042
```

Ejemplo:

```python
cluster = Cluster(
    ["127.0.0.1"],
    port=9042
)
```

## Crear sesión

```python
session = cluster.connect()
```

También puede conectarse directamente a un keyspace:

```python
session = cluster.connect("app")
```

## Cerrar recursos

```python
cluster.shutdown()
```

Cerrar el cluster libera conexiones y recursos internos.

Patrón recomendado:

```python
cluster = Cluster(["127.0.0.1"])
session = cluster.connect()

try:
    ...
finally:
    cluster.shutdown()
```

## Conexión con autenticación

```python
from cassandra.auth import PlainTextAuthProvider
from cassandra.cluster import Cluster

auth_provider = PlainTextAuthProvider(
    username="cassandra",
    password="cassandra"
)

cluster = Cluster(
    ["127.0.0.1"],
    auth_provider=auth_provider
)

session = cluster.connect()

cluster.shutdown()
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

```python
import os

from cassandra.auth import PlainTextAuthProvider
from cassandra.cluster import Cluster

hosts = os.getenv("CASSANDRA_HOSTS", "127.0.0.1").split(",")

auth_provider = PlainTextAuthProvider(
    username=os.getenv("CASSANDRA_USER"),
    password=os.getenv("CASSANDRA_PASSWORD")
)

cluster = Cluster(
    hosts,
    port=int(os.getenv("CASSANDRA_PORT", "9042")),
    auth_provider=auth_provider
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
CASSANDRA_HOSTS=127.0.0.1
CASSANDRA_PORT=9042
CASSANDRA_USER=cassandra
CASSANDRA_PASSWORD=cassandra
CASSANDRA_KEYSPACE=app
```

Código:

```python
import os

from cassandra.auth import PlainTextAuthProvider
from cassandra.cluster import Cluster
from dotenv import load_dotenv

load_dotenv()

hosts = os.getenv("CASSANDRA_HOSTS", "127.0.0.1").split(",")

auth_provider = PlainTextAuthProvider(
    username=os.getenv("CASSANDRA_USER"),
    password=os.getenv("CASSANDRA_PASSWORD")
)

cluster = Cluster(
    hosts,
    port=int(os.getenv("CASSANDRA_PORT", "9042")),
    auth_provider=auth_provider
)

session = cluster.connect(
    os.getenv("CASSANDRA_KEYSPACE")
)

cluster.shutdown()
```

## CQL

CQL significa Cassandra Query Language.

Se parece visualmente a SQL, pero no debe entenderse como SQL relacional completo.

Ejemplo:

```sql
SELECT id, name, price
FROM products
WHERE id = 1;
```

En Cassandra, el diseño de tablas depende mucho de las consultas que se quieren ejecutar.

## Keyspace

Un keyspace es un contenedor lógico de tablas.

Es parecido a una base de datos en otros sistemas, aunque tiene configuración propia de replicación.

Crear keyspace:

```python
session.execute("""
    CREATE KEYSPACE IF NOT EXISTS app
    WITH replication = {
        'class': 'SimpleStrategy',
        'replication_factor': 1
    }
""")
```

## Usar keyspace

```python
session.set_keyspace("app")
```

o al conectar:

```python
session = cluster.connect("app")
```

## Replicación

La replicación define cuántas copias de los datos se guardan en el clúster.

Para desarrollo local suele verse:

```text
replication_factor = 1
```

Para producción, debe diseñarse según la cantidad de nodos, data centers y requisitos de disponibilidad.

## Crear tabla

```python
session.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id uuid PRIMARY KEY,
        name text,
        price decimal,
        stock int,
        is_active boolean
    )
""")
```

## Clave primaria

En Cassandra, la clave primaria define cómo se distribuyen y ordenan los datos.

Ejemplo simple:

```sql
PRIMARY KEY (id)
```

Ejemplo con partition key y clustering column:

```sql
PRIMARY KEY ((client_id), created_at)
```

## Partition key

La partition key decide en qué partición se guardan los datos.

Ejemplo:

```sql
PRIMARY KEY ((client_id), created_at)
```

Aquí:

```text
client_id -> partition key
created_at -> clustering column
```

## Clustering columns

Las clustering columns ordenan datos dentro de una partición.

Ejemplo:

```sql
PRIMARY KEY ((client_id), created_at)
```

Esto permite consultar eventos de un cliente ordenados o filtrados por `created_at`.

## Modelo orientado a consultas

En Cassandra, primero se piensa en las consultas necesarias.

Después se diseña la tabla.

Ejemplo de consulta deseada:

```text
listar compras por cliente ordenadas por fecha
```

Tabla posible:

```sql
CREATE TABLE purchases_by_client (
    client_id text,
    created_at timestamp,
    purchase_id uuid,
    product_name text,
    amount decimal,
    PRIMARY KEY ((client_id), created_at)
) WITH CLUSTERING ORDER BY (created_at DESC);
```

## Insertar datos

```python
from uuid import uuid4
from decimal import Decimal

product_id = uuid4()

session.execute(
    """
    INSERT INTO products (id, name, price, stock, is_active)
    VALUES (%s, %s, %s, %s, %s)
    """,
    (product_id, "Laptop", Decimal("3500.00"), 5, True)
)
```

## Parámetros en consultas simples

En consultas simples, se pueden usar placeholders `%s`.

```python
session.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = %s
    """,
    (product_id,)
)
```

Aunque el valor sea texto, entero o decimal, el placeholder sigue siendo:

```text
%s
```

No debe usarse formateo manual con f-strings para valores externos.

## Evitar concatenar CQL

Problemático:

```python
query = f"""
SELECT *
FROM products
WHERE name = '{name}'
"""

session.execute(query)
```

Más seguro:

```python
session.execute(
    """
    SELECT *
    FROM products
    WHERE id = %s
    """,
    (product_id,)
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

La coma es necesaria.

## Consultar datos

```python
rows = session.execute("""
    SELECT id, name, price, stock, is_active
    FROM products
""")

for row in rows:
    print(row.id, row.name, row.price, row.stock)
```

## Consultar por clave primaria

```python
row = session.execute(
    """
    SELECT id, name, price, stock
    FROM products
    WHERE id = %s
    """,
    (product_id,)
).one()

if row is not None:
    print(row.name)
```

## `ResultSet`

`session.execute()` devuelve un resultado iterable.

```python
rows = session.execute("SELECT * FROM products")
```

Se puede recorrer:

```python
for row in rows:
    print(row)
```

## `.one()`

Devuelve una fila o `None`.

```python
row = session.execute(
    """
    SELECT *
    FROM products
    WHERE id = %s
    """,
    (product_id,)
).one()
```

## `.all()`

Devuelve todas las filas en una lista.

```python
rows = session.execute("""
    SELECT *
    FROM products
""").all()
```

Debe usarse con cuidado si la consulta puede devolver muchos datos.

## Acceso a columnas

Las filas permiten acceso por atributo:

```python
print(row.name)
```

También puede usarse acceso por posición:

```python
print(row[0])
```

## Convertir fila a diccionario

```python
data = row._asdict()
```

Ejemplo:

```python
row = session.execute(
    """
    SELECT id, name, price
    FROM products
    WHERE id = %s
    """,
    (product_id,)
).one()

if row is not None:
    product = row._asdict()
```

## Actualizar datos

```python
session.execute(
    """
    UPDATE products
    SET price = %s,
        stock = %s
    WHERE id = %s
    """,
    (Decimal("3600.00"), 4, product_id)
)
```

## Eliminar datos

```python
session.execute(
    """
    DELETE FROM products
    WHERE id = %s
    """,
    (product_id,)
)
```

## Eliminación lógica

En aplicaciones reales, muchas veces se prefiere marcar registros como inactivos.

```python
session.execute(
    """
    UPDATE products
    SET is_active = false
    WHERE id = %s
    """,
    (product_id,)
)
```

Consulta de activos:

```python
rows = session.execute("""
    SELECT id, name, price, stock
    FROM products
    WHERE is_active = true
    ALLOW FILTERING
""")
```

Sin embargo, `ALLOW FILTERING` debe evitarse en diseños normales. Es mejor diseñar tablas que permitan consultas por clave.

## `ALLOW FILTERING`

`ALLOW FILTERING` fuerza a Cassandra a permitir filtros que pueden ser ineficientes.

Ejemplo:

```sql
SELECT *
FROM products
WHERE is_active = true
ALLOW FILTERING;
```

No debe usarse como solución habitual.

En Cassandra, lo correcto suele ser crear una tabla diseñada para esa consulta.

## Tabla diseñada para consulta

Si se necesita listar productos activos, puede diseñarse una tabla específica.

```sql
CREATE TABLE active_products (
    is_active boolean,
    name text,
    id uuid,
    price decimal,
    stock int,
    PRIMARY KEY ((is_active), name, id)
);
```

Consulta:

```sql
SELECT *
FROM active_products
WHERE is_active = true;
```

## Prepared statements

Los prepared statements preparan una consulta una vez y permiten ejecutarla muchas veces con distintos valores.

```python
insert_product = session.prepare("""
    INSERT INTO products (id, name, price, stock, is_active)
    VALUES (?, ?, ?, ?, ?)
""")

session.execute(
    insert_product,
    (product_id, "Laptop", Decimal("3500.00"), 5, True)
)
```

## Placeholders en prepared statements

En prepared statements se usan signos de interrogación:

```text
?
```

Ejemplo:

```python
statement = session.prepare("""
    SELECT id, name, price
    FROM products
    WHERE id = ?
""")
```

Uso:

```python
row = session.execute(
    statement,
    (product_id,)
).one()
```

## Ventajas de prepared statements

Los prepared statements ayudan a:

```text
evitar repetir parseo de consulta
mejorar rendimiento en consultas repetidas
pasar parámetros de forma segura
reducir errores de formato
```

## Insertar varios registros con prepared statement

```python
from uuid import uuid4
from decimal import Decimal

insert_product = session.prepare("""
    INSERT INTO products (id, name, price, stock, is_active)
    VALUES (?, ?, ?, ?, ?)
""")

products = [
    (uuid4(), "Laptop", Decimal("3500.00"), 5, True),
    (uuid4(), "Mouse", Decimal("80.00"), 20, True),
    (uuid4(), "Teclado", Decimal("150.00"), 10, True),
]

for product in products:
    session.execute(insert_product, product)
```

## Batch statements

Cassandra permite agrupar operaciones con `BatchStatement`.

```python
from cassandra.query import BatchStatement

batch = BatchStatement()

insert_product = session.prepare("""
    INSERT INTO products (id, name, price, stock, is_active)
    VALUES (?, ?, ?, ?, ?)
""")

batch.add(
    insert_product,
    (uuid4(), "Laptop", Decimal("3500.00"), 5, True)
)

batch.add(
    insert_product,
    (uuid4(), "Mouse", Decimal("80.00"), 20, True)
)

session.execute(batch)
```

## Cuidado con batches

En Cassandra, un batch no debe usarse como reemplazo general para inserciones masivas.

Los batches son más apropiados cuando varias escrituras relacionadas deben aplicarse juntas, especialmente dentro de una misma partición.

Para cargas grandes, suele ser mejor usar escrituras individuales preparadas y control de concurrencia.

## Consistency level

El consistency level define cuántas réplicas deben responder para considerar exitosa una operación.

Importación:

```python
from cassandra import ConsistencyLevel
from cassandra.query import SimpleStatement
```

Ejemplo:

```python
statement = SimpleStatement(
    """
    SELECT id, name, price
    FROM products
    WHERE id = %s
    """,
    consistency_level=ConsistencyLevel.ONE
)

rows = session.execute(
    statement,
    (product_id,)
)
```

## Niveles frecuentes de consistencia

```text
ONE
QUORUM
LOCAL_QUORUM
ALL
```

La elección depende del diseño del clúster, replicación, latencia y requisitos de consistencia.

## `SimpleStatement`

`SimpleStatement` permite configurar opciones de una consulta simple.

```python
from cassandra.query import SimpleStatement
from cassandra import ConsistencyLevel

statement = SimpleStatement(
    "SELECT * FROM products WHERE id = %s",
    consistency_level=ConsistencyLevel.LOCAL_QUORUM
)

rows = session.execute(
    statement,
    (product_id,)
)
```

## Paginación

El driver puede paginar resultados.

```python
statement = SimpleStatement(
    """
    SELECT id, name, price
    FROM products
    """,
    fetch_size=100
)

rows = session.execute(statement)

for row in rows:
    print(row.name)
```

La paginación evita traer todo el resultado en una sola respuesta.

## Consultas asíncronas

El driver permite ejecutar consultas de forma asíncrona mediante `execute_async()`.

```python
future = session.execute_async(
    """
    SELECT id, name, price
    FROM products
    WHERE id = %s
    """,
    (product_id,)
)

rows = future.result()

for row in rows:
    print(row.name)
```

Este patrón no es igual que `asyncio`, sino que usa futuros propios del driver.

## Callbacks

```python
def handle_success(rows):
    for row in rows:
        print(row)


def handle_error(error):
    print("Error:", error)


future = session.execute_async(
    "SELECT * FROM products"
)

future.add_callbacks(
    callback=handle_success,
    errback=handle_error
)
```

## Manejo de errores

Importaciones frecuentes:

```python
from cassandra import OperationTimedOut
from cassandra import ReadTimeout
from cassandra import WriteTimeout
from cassandra.cluster import NoHostAvailable
```

Ejemplo:

```python
from cassandra.cluster import NoHostAvailable

try:
    cluster = Cluster(["127.0.0.1"])
    session = cluster.connect()
except NoHostAvailable as error:
    print("No se pudo conectar al clúster")
    print(error)
```

## Errores frecuentes

```text
NoHostAvailable
OperationTimedOut
ReadTimeout
WriteTimeout
Unavailable
InvalidRequest
AlreadyExists
```

## Error de consulta inválida

```python
from cassandra import InvalidRequest

try:
    session.execute("SELECT * FROM table_that_does_not_exist")
except InvalidRequest as error:
    print("Consulta inválida:", error)
```

## Error de timeout

```python
from cassandra import OperationTimedOut

try:
    rows = session.execute("SELECT * FROM products")
except OperationTimedOut:
    print("La operación excedió el tiempo de espera")
```

## Crear funciones reutilizables

## Crear conexión

```python
import os

from cassandra.auth import PlainTextAuthProvider
from cassandra.cluster import Cluster


def create_cluster():
    hosts = os.getenv("CASSANDRA_HOSTS", "127.0.0.1").split(",")

    username = os.getenv("CASSANDRA_USER")
    password = os.getenv("CASSANDRA_PASSWORD")

    auth_provider = None

    if username and password:
        auth_provider = PlainTextAuthProvider(
            username=username,
            password=password
        )

    return Cluster(
        hosts,
        port=int(os.getenv("CASSANDRA_PORT", "9042")),
        auth_provider=auth_provider
    )
```

## Inicializar keyspace

```python
def create_keyspace(session):
    session.execute("""
        CREATE KEYSPACE IF NOT EXISTS app
        WITH replication = {
            'class': 'SimpleStrategy',
            'replication_factor': 1
        }
    """)
```

## Crear tabla

```python
def create_products_table(session):
    session.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id uuid PRIMARY KEY,
            name text,
            price decimal,
            stock int,
            is_active boolean
        )
    """)
```

## Crear producto

```python
from decimal import Decimal
from uuid import uuid4


def create_product(session, name, price, stock=0):
    product_id = uuid4()

    session.execute(
        """
        INSERT INTO products (id, name, price, stock, is_active)
        VALUES (%s, %s, %s, %s, %s)
        """,
        (product_id, name, Decimal(str(price)), stock, True)
    )

    return product_id
```

## Obtener producto

```python
def get_product(session, product_id):
    row = session.execute(
        """
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE id = %s
        """,
        (product_id,)
    ).one()

    if row is None:
        return None

    return row._asdict()
```

## Actualizar stock

```python
def update_stock(session, product_id, stock):
    session.execute(
        """
        UPDATE products
        SET stock = %s
        WHERE id = %s
        """,
        (stock, product_id)
    )
```

## Desactivar producto

```python
def deactivate_product(session, product_id):
    session.execute(
        """
        UPDATE products
        SET is_active = false
        WHERE id = %s
        """,
        (product_id,)
    )
```

## Uso con FastAPI

```python
import os
from contextlib import asynccontextmanager
from decimal import Decimal
from uuid import uuid4

from cassandra.auth import PlainTextAuthProvider
from cassandra.cluster import Cluster
from fastapi import FastAPI, HTTPException


def create_cluster():
    hosts = os.getenv("CASSANDRA_HOSTS", "127.0.0.1").split(",")

    username = os.getenv("CASSANDRA_USER")
    password = os.getenv("CASSANDRA_PASSWORD")

    auth_provider = None

    if username and password:
        auth_provider = PlainTextAuthProvider(
            username=username,
            password=password
        )

    return Cluster(
        hosts,
        port=int(os.getenv("CASSANDRA_PORT", "9042")),
        auth_provider=auth_provider
    )


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.cluster = create_cluster()
    app.state.session = app.state.cluster.connect(
        os.getenv("CASSANDRA_KEYSPACE", "app")
    )

    yield

    app.state.cluster.shutdown()


app = FastAPI(lifespan=lifespan)


@app.get("/products/{product_id}")
def read_product(product_id: str):
    row = app.state.session.execute(
        """
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE id = %s
        """,
        (product_id,)
    ).one()

    if row is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return row._asdict()
```

## Uso con pandas

Cassandra puede alimentar un `DataFrame`, pero no es una base analítica tradicional.

```python
import pandas as pd

rows = session.execute("""
    SELECT id, name, price, stock
    FROM products
""")

df = pd.DataFrame([
    row._asdict()
    for row in rows
])

print(df.head())
```

Para análisis complejos puede ser mejor extraer datos hacia:

```text
pandas
polars
duckdb
Spark
```

según volumen y arquitectura.

## Uso con Pydantic

En APIs, Pydantic puede validar datos antes de insertarlos.

```python
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

Uso:

```python
def create_product_from_schema(session, product_create: ProductCreate):
    product_id = uuid4()

    session.execute(
        """
        INSERT INTO products (id, name, price, stock, is_active)
        VALUES (%s, %s, %s, %s, %s)
        """,
        (
            product_id,
            product_create.name,
            Decimal(str(product_create.price)),
            product_create.stock,
            True
        )
    )

    return product_id
```

## Uso con SSL

En entornos remotos o administrados puede requerirse SSL.

Ejemplo conceptual:

```python
from ssl import SSLContext, PROTOCOL_TLS_CLIENT

from cassandra.cluster import Cluster

ssl_context = SSLContext(PROTOCOL_TLS_CLIENT)

cluster = Cluster(
    ["host"],
    ssl_context=ssl_context
)
```

La configuración exacta depende del proveedor y de los certificados requeridos.

## Uso con servicios administrados

Cassandra puede usarse en servicios administrados compatibles, como bases basadas en Cassandra o APIs compatibles.

En esos casos suelen requerirse:

```text
host específico
puerto
usuario
contraseña
certificados
SSL
auth provider
configuración de consistencia
```

La configuración exacta depende del proveedor.

## Organización recomendada

```text
app/
├─ database.py
├─ repositories.py
├─ schemas.py
├─ services.py
└─ main.py
```

## `database.py`

```python
import os

from cassandra.auth import PlainTextAuthProvider
from cassandra.cluster import Cluster
from dotenv import load_dotenv

load_dotenv()


def create_cluster():
    hosts = os.getenv("CASSANDRA_HOSTS", "127.0.0.1").split(",")

    username = os.getenv("CASSANDRA_USER")
    password = os.getenv("CASSANDRA_PASSWORD")

    auth_provider = None

    if username and password:
        auth_provider = PlainTextAuthProvider(
            username=username,
            password=password
        )

    return Cluster(
        hosts,
        port=int(os.getenv("CASSANDRA_PORT", "9042")),
        auth_provider=auth_provider
    )


def create_session(cluster):
    return cluster.connect(
        os.getenv("CASSANDRA_KEYSPACE", "app")
    )
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
from decimal import Decimal
from uuid import uuid4


def create_product(session, product_create):
    product_id = uuid4()

    session.execute(
        """
        INSERT INTO products (id, name, price, stock, is_active)
        VALUES (%s, %s, %s, %s, %s)
        """,
        (
            product_id,
            product_create.name,
            Decimal(str(product_create.price)),
            product_create.stock,
            True
        )
    )

    return product_id


def get_product(session, product_id):
    row = session.execute(
        """
        SELECT id, name, price, stock, is_active
        FROM products
        WHERE id = %s
        """,
        (product_id,)
    ).one()

    if row is None:
        return None

    return row._asdict()


def update_stock(session, product_id, stock):
    session.execute(
        """
        UPDATE products
        SET stock = %s
        WHERE id = %s
        """,
        (stock, product_id)
    )


def deactivate_product(session, product_id):
    session.execute(
        """
        UPDATE products
        SET is_active = false
        WHERE id = %s
        """,
        (product_id,)
    )
```

## Separación por responsabilidades

```text
database.py      -> conexión a Cassandra
schemas.py       -> validación de entrada
repositories.py  -> consultas CQL
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Migraciones y esquema

Cassandra no se gestiona igual que una base relacional tradicional.

Aun así, conviene versionar scripts CQL como:

```text
migrations/
├─ 001_create_keyspace.cql
├─ 002_create_products_table.cql
├─ 003_create_products_by_client_table.cql
└─ 004_add_indexes.cql
```

En proyectos reales, los cambios de esquema deben planificarse con cuidado porque Cassandra está muy ligada al diseño de consultas y particiones.

## Índices secundarios

Cassandra soporta índices secundarios, pero no deben usarse como reemplazo general de un buen modelo de datos.

Ejemplo:

```sql
CREATE INDEX IF NOT EXISTS products_name_idx
ON products (name);
```

Antes de crear índices, conviene revisar el patrón de consulta y el tamaño esperado de los datos.

## Materialized views

Cassandra tiene materialized views en ciertas versiones, pero requieren cuidado operativo.

En muchos diseños, se prefieren tablas duplicadas diseñadas para consultas específicas.

## Duplicación controlada de datos

En Cassandra es común duplicar datos en varias tablas para responder distintas consultas.

Ejemplo:

```text
products_by_id
products_by_category
purchases_by_client
purchases_by_day
```

Esto no es un error necesariamente. Es parte del modelo orientado a consultas.

## Errores comunes

## Instalar el paquete y tratar de importar otro nombre

Instalación correcta:

```bash
python -m pip install cassandra-driver
```

Importación correcta:

```python
from cassandra.cluster import Cluster
```

## Olvidar cerrar el cluster

Menos recomendable:

```python
cluster = Cluster(["127.0.0.1"])
```

sin:

```python
cluster.shutdown()
```

## Usar Cassandra como si fuera una base relacional

Cassandra no está pensada para joins, consultas arbitrarias y normalización relacional clásica.

El modelo debe diseñarse según las consultas.

## Usar `ALLOW FILTERING` como solución habitual

`ALLOW FILTERING` puede generar consultas ineficientes.

Lo correcto suele ser rediseñar la tabla.

## Consultar por columnas que no forman parte de la clave

Problemático:

```sql
SELECT *
FROM products
WHERE name = 'Laptop';
```

si `name` no está modelado para esa consulta.

Solución típica:

```text
crear una tabla específica para esa consulta
ajustar partition key y clustering columns
```

## Crear particiones demasiado grandes

Una mala partition key puede concentrar demasiados datos en una misma partición.

Esto afecta rendimiento y estabilidad.

## Crear particiones demasiado pequeñas o dispersas

Una clave mal diseñada también puede generar demasiadas particiones poco útiles.

El diseño debe equilibrar distribución y patrones de lectura.

## Usar batches para cargas masivas indiscriminadas

Los batches no deben usarse como una forma genérica de acelerar cargas grandes.

## Concatenar valores en CQL

Problemático:

```python
query = f"SELECT * FROM products WHERE id = {product_id}"
```

Correcto:

```python
session.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

## Confundir placeholders

En consulta simple:

```python
session.execute(
    "SELECT * FROM products WHERE id = %s",
    (product_id,)
)
```

En prepared statement:

```python
statement = session.prepare(
    "SELECT * FROM products WHERE id = ?"
)
```

## No diseñar claves según consultas

En Cassandra, no basta con crear una tabla parecida a una tabla SQL.

La clave primaria determina cómo se podrá consultar eficientemente.

## Buenas prácticas

## Diseñar tablas desde las consultas

Primero definir:

```text
qué se necesita consultar
por qué columnas se filtra
cómo se ordena
cuánto puede crecer cada partición
```

Después crear la tabla.

## Usar prepared statements para consultas repetidas

```python
statement = session.prepare(...)
```

## Usar parámetros

Consulta simple:

```python
WHERE id = %s
```

Prepared statement:

```python
WHERE id = ?
```

## Evitar `ALLOW FILTERING`

Rediseñar tabla cuando sea necesario.

## Cerrar recursos

```python
cluster.shutdown()
```

## Usar variables de entorno

```text
CASSANDRA_HOSTS
CASSANDRA_PORT
CASSANDRA_USER
CASSANDRA_PASSWORD
CASSANDRA_KEYSPACE
```

## Controlar consistency level según el caso

```python
ConsistencyLevel.LOCAL_QUORUM
```

## No abusar de batches

Usarlos solo cuando tenga sentido en el modelo de Cassandra.

## Separar CQL en repositorios

```text
repositories.py
```

## Versionar scripts de esquema

```text
migrations/*.cql
```

## Ejemplo integrado

```python
import os
from decimal import Decimal
from uuid import UUID, uuid4

from cassandra.auth import PlainTextAuthProvider
from cassandra.cluster import Cluster
from cassandra.cluster import NoHostAvailable
from cassandra import InvalidRequest
from dotenv import load_dotenv

load_dotenv()

KEYSPACE = os.getenv("CASSANDRA_KEYSPACE", "app")


def create_cluster():
    hosts = os.getenv("CASSANDRA_HOSTS", "127.0.0.1").split(",")

    username = os.getenv("CASSANDRA_USER")
    password = os.getenv("CASSANDRA_PASSWORD")

    auth_provider = None

    if username and password:
        auth_provider = PlainTextAuthProvider(
            username=username,
            password=password
        )

    return Cluster(
        hosts,
        port=int(os.getenv("CASSANDRA_PORT", "9042")),
        auth_provider=auth_provider
    )


def create_keyspace(session):
    session.execute(f"""
        CREATE KEYSPACE IF NOT EXISTS {KEYSPACE}
        WITH replication = {{
            'class': 'SimpleStrategy',
            'replication_factor': 1
        }}
    """)


def create_tables(session):
    session.execute("""
        CREATE TABLE IF NOT EXISTS products (
            id uuid PRIMARY KEY,
            name text,
            price decimal,
            stock int,
            is_active boolean
        )
    """)


def prepare_statements(session):
    return {
        "insert_product": session.prepare("""
            INSERT INTO products (id, name, price, stock, is_active)
            VALUES (?, ?, ?, ?, ?)
        """),
        "get_product": session.prepare("""
            SELECT id, name, price, stock, is_active
            FROM products
            WHERE id = ?
        """),
        "update_stock": session.prepare("""
            UPDATE products
            SET stock = ?
            WHERE id = ?
        """),
        "deactivate_product": session.prepare("""
            UPDATE products
            SET is_active = false
            WHERE id = ?
        """),
    }


def create_product(session, statements, name, price, stock=0):
    product_id = uuid4()

    session.execute(
        statements["insert_product"],
        (
            product_id,
            name,
            Decimal(str(price)),
            stock,
            True
        )
    )

    return product_id


def get_product(session, statements, product_id):
    if isinstance(product_id, str):
        product_id = UUID(product_id)

    row = session.execute(
        statements["get_product"],
        (product_id,)
    ).one()

    if row is None:
        return None

    return row._asdict()


def update_stock(session, statements, product_id, stock):
    if isinstance(product_id, str):
        product_id = UUID(product_id)

    session.execute(
        statements["update_stock"],
        (stock, product_id)
    )


def deactivate_product(session, statements, product_id):
    if isinstance(product_id, str):
        product_id = UUID(product_id)

    session.execute(
        statements["deactivate_product"],
        (product_id,)
    )


def main():
    cluster = create_cluster()

    try:
        session = cluster.connect()

        create_keyspace(session)
        session.set_keyspace(KEYSPACE)
        create_tables(session)

        statements = prepare_statements(session)

        product_id = create_product(
            session,
            statements,
            "Laptop",
            3500,
            stock=5
        )

        product = get_product(
            session,
            statements,
            product_id
        )

        print(product)

        update_stock(
            session,
            statements,
            product_id,
            4
        )

        updated_product = get_product(
            session,
            statements,
            product_id
        )

        print(updated_product)

    except NoHostAvailable as error:
        print("No se pudo conectar al clúster:", error)
    except InvalidRequest as error:
        print("Consulta inválida:", error)
    finally:
        cluster.shutdown()


if __name__ == "__main__":
    main()
```

## Relación con otras librerías

`cassandra-driver` se relaciona especialmente con:

```text
apache cassandra
cql
pandas
fastapi
pydantic
python-dotenv
pytest
```

## Relación con CQL

CQL es el lenguaje usado para definir y consultar datos en Cassandra.

Aunque se parece a SQL, sus restricciones y modelo mental son distintos.

## Relación con FastAPI

Puede usarse en APIs cuando Cassandra es la base principal de lectura o escritura.

En aplicaciones de alta concurrencia debe cuidarse el uso de sesiones, timeouts y consistencia.

## Relación con pandas

Pandas puede recibir resultados de Cassandra, pero Cassandra no es una base analítica tipo DuckDB.

## Relación con bases relacionales

Cassandra no reemplaza automáticamente a PostgreSQL, MySQL o SQL Server.

Su fortaleza aparece cuando se necesita distribución, disponibilidad y escalabilidad horizontal bajo patrones de consulta conocidos.

## Relación con MongoDB

Ambas son bases NoSQL, pero tienen modelos diferentes.

```text
MongoDB   -> documentos
Cassandra -> filas distribuidas por particiones y columnas
```

## Orden didáctico interno

```text
1. Propósito de cassandra-driver
2. Instalación
3. Relación con Apache Cassandra
4. Cluster y Session
5. Keyspaces
6. CQL
7. Tablas, partition key y clustering columns
8. execute()
9. Parámetros con %s en consultas simples
10. Prepared statements con ?
11. INSERT, SELECT, UPDATE y DELETE
12. ResultSet, one() y all()
13. BatchStatement
14. ConsistencyLevel
15. Paginación
16. Consultas asíncronas con execute_async()
17. Manejo de errores
18. Uso con FastAPI, pandas y Pydantic
19. Diseño orientado a consultas
20. Errores comunes
21. Buenas prácticas
```