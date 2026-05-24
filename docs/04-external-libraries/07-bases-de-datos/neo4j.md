# `neo4j`

## Propósito

`neo4j` es una librería externa para conectar Python con Neo4j.

Neo4j es una base de datos de grafos. Se utiliza para almacenar y consultar información representada mediante nodos, relaciones y propiedades.

La librería `neo4j` permite ejecutar consultas Cypher desde Python, crear nodos, crear relaciones, consultar caminos, manejar transacciones y conectar aplicaciones Python con una base Neo4j local, remota o alojada en la nube.

## Naturaleza de la librería

`neo4j` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install neo4j
```

La importación habitual es:

```python
from neo4j import GraphDatabase
```

Para uso asíncrono:

```python
from neo4j import AsyncGraphDatabase
```

`neo4j` es un driver de base de datos. No es un ORM tradicional.

## Relación con Neo4j

Neo4j organiza los datos como un grafo.

Elementos principales:

```text
nodos
relaciones
propiedades
etiquetas
tipos de relación
```

Equivalencia conceptual aproximada frente a bases relacionales:

```text
nodo             -> entidad o registro
relación         -> vínculo explícito entre entidades
propiedad        -> atributo
etiqueta         -> categoría de nodo
tipo de relación -> nombre del vínculo
```

La comparación es solo conceptual, porque Neo4j no trabaja como una base de tablas.

## Idea central

La idea principal de `neo4j` es crear un driver de conexión y ejecutar consultas Cypher.

Flujo típico:

```text
Python -> neo4j driver -> Neo4j database -> nodos y relaciones
```

Ejemplo mínimo:

```python
from neo4j import GraphDatabase

uri = "neo4j://localhost:7687"
user = "neo4j"
password = "secret"

driver = GraphDatabase.driver(
    uri,
    auth=(user, password)
)

try:
    records, summary, keys = driver.execute_query(
        "RETURN 1 AS value",
        database_="neo4j"
    )

    for record in records:
        print(record["value"])
finally:
    driver.close()
```

## Cuándo usar Neo4j

Conviene usar Neo4j cuando se necesita:

```text
modelar relaciones complejas
consultar conexiones entre entidades
analizar redes
buscar caminos
trabajar con grafos de conocimiento
representar recomendaciones
modelar jerarquías o dependencias
analizar fraude o vínculos indirectos
consultar relaciones con profundidad variable
```

## Cuándo no usar Neo4j

No siempre conviene usar Neo4j cuando se necesita:

```text
tablas simples
consultas agregadas tradicionales
reportes tabulares clásicos
transacciones relacionales convencionales
almacenamiento clave-valor
análisis local sobre archivos CSV o Parquet
```

Para esos casos pueden corresponder:

```text
PostgreSQL
MySQL
SQLite
DuckDB
Redis
MongoDB
```

## Instalación

Instalación básica:

```bash
python -m pip install neo4j
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
neo4j==6.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import neo4j

print(neo4j.__version__)
```

## Paquete antiguo `neo4j-driver`

Antes era común ver:

```bash
python -m pip install neo4j-driver
```

Para proyectos nuevos, debe usarse:

```bash
python -m pip install neo4j
```

La importación del driver se mantiene desde el paquete `neo4j`.

```python
from neo4j import GraphDatabase
```

## Conexión básica

```python
from neo4j import GraphDatabase

driver = GraphDatabase.driver(
    "neo4j://localhost:7687",
    auth=("neo4j", "secret")
)

driver.close()
```

## URI de conexión

Una URI de Neo4j puede tener distintas formas.

```text
bolt://localhost:7687
neo4j://localhost:7687
neo4j+s://host
neo4j+ssc://host
```

Uso local frecuente:

```python
uri = "neo4j://localhost:7687"
```

Uso remoto seguro frecuente:

```python
uri = "neo4j+s://example.databases.neo4j.io"
```

## Parámetros principales

```text
uri      -> dirección del servidor Neo4j
user     -> usuario
password -> contraseña
database -> base de datos lógica
```

Ejemplo:

```python
driver = GraphDatabase.driver(
    uri,
    auth=(user, password)
)
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
driver = GraphDatabase.driver(
    "neo4j://localhost:7687",
    auth=("neo4j", "secret")
)
```

Más conveniente:

```python
import os

from neo4j import GraphDatabase

driver = GraphDatabase.driver(
    os.getenv("NEO4J_URI"),
    auth=(
        os.getenv("NEO4J_USER"),
        os.getenv("NEO4J_PASSWORD")
    )
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
NEO4J_URI=neo4j://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=secret
NEO4J_DATABASE=neo4j
```

Código:

```python
import os

from dotenv import load_dotenv
from neo4j import GraphDatabase

load_dotenv()

driver = GraphDatabase.driver(
    os.getenv("NEO4J_URI"),
    auth=(
        os.getenv("NEO4J_USER"),
        os.getenv("NEO4J_PASSWORD")
    )
)

driver.close()
```

## Driver

El driver representa la conexión lógica con Neo4j.

```python
driver = GraphDatabase.driver(
    uri,
    auth=(user, password)
)
```

El driver debe reutilizarse durante la vida de la aplicación y cerrarse al finalizar.

```python
driver.close()
```

## Verificar conexión

```python
from neo4j import GraphDatabase

driver = GraphDatabase.driver(
    "neo4j://localhost:7687",
    auth=("neo4j", "secret")
)

try:
    driver.verify_connectivity()
    print("Conexión correcta")
finally:
    driver.close()
```

## `execute_query()`

`execute_query()` permite ejecutar una consulta con menos código repetitivo.

```python
records, summary, keys = driver.execute_query(
    """
    RETURN $message AS message
    """,
    message="Hola Neo4j",
    database_="neo4j"
)

for record in records:
    print(record["message"])
```

Devuelve:

```text
records -> registros resultantes
summary -> resumen de ejecución
keys    -> columnas devueltas
```

## Consulta básica

```python
records, summary, keys = driver.execute_query(
    """
    RETURN 1 AS value
    """,
    database_="neo4j"
)

for record in records:
    print(record["value"])
```

## Cypher

Cypher es el lenguaje de consulta usado por Neo4j.

Ejemplo:

```cypher
MATCH (p:Product)
RETURN p.name AS name, p.price AS price
```

Desde Python:

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    RETURN p.name AS name, p.price AS price
    """,
    database_="neo4j"
)
```

## Nodos

Un nodo representa una entidad.

Ejemplo conceptual:

```text
(:Product {name: "Laptop", price: 3500})
```

Crear nodo:

```python
driver.execute_query(
    """
    CREATE (:Product {
        name: $name,
        price: $price,
        stock: $stock
    })
    """,
    name="Laptop",
    price=3500,
    stock=5,
    database_="neo4j"
)
```

## Etiquetas

Una etiqueta clasifica nodos.

Ejemplos:

```text
Product
Client
Account
Transaction
City
Company
```

Cypher:

```cypher
CREATE (:Product {name: "Laptop"})
```

Un nodo puede tener más de una etiqueta.

```cypher
CREATE (:Person:Employee {name: "Ana"})
```

## Propiedades

Las propiedades son pares clave-valor asociados a nodos o relaciones.

Ejemplo:

```cypher
CREATE (:Product {
    name: "Laptop",
    price: 3500,
    stock: 5
})
```

Desde Python:

```python
driver.execute_query(
    """
    CREATE (:Product {
        name: $name,
        price: $price,
        stock: $stock
    })
    """,
    name="Laptop",
    price=3500,
    stock=5,
    database_="neo4j"
)
```

## Relaciones

Una relación conecta dos nodos.

Ejemplo conceptual:

```text
(:Client)-[:BOUGHT]->(:Product)
```

Crear nodos y relación:

```python
driver.execute_query(
    """
    CREATE (c:Client {name: $client_name})
    CREATE (p:Product {name: $product_name})
    CREATE (c)-[:BOUGHT]->(p)
    """,
    client_name="Ana",
    product_name="Laptop",
    database_="neo4j"
)
```

## Tipos de relación

El tipo de relación describe el vínculo.

Ejemplos:

```text
BOUGHT
KNOWS
WORKS_AT
TRANSFERRED_TO
LOCATED_IN
OWNS
```

Cypher:

```cypher
(a)-[:KNOWS]->(b)
```

## Dirección de relaciones

Las relaciones tienen dirección.

```cypher
(a)-[:BOUGHT]->(b)
```

También pueden consultarse ignorando dirección:

```cypher
(a)-[:BOUGHT]-(b)
```

## Crear nodo con `CREATE`

```python
driver.execute_query(
    """
    CREATE (p:Product {
        name: $name,
        price: $price,
        stock: $stock
    })
    RETURN p
    """,
    name="Laptop",
    price=3500,
    stock=5,
    database_="neo4j"
)
```

## Buscar nodos con `MATCH`

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    RETURN p.name AS name, p.price AS price, p.stock AS stock
    ORDER BY p.name
    """,
    database_="neo4j"
)

for record in records:
    print(record["name"], record["price"])
```

## Filtrar con `WHERE`

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    WHERE p.price >= $min_price
    RETURN p.name AS name, p.price AS price
    """,
    min_price=100,
    database_="neo4j"
)
```

## Parámetros

Los valores externos deben pasarse como parámetros.

Correcto:

```python
driver.execute_query(
    """
    MATCH (p:Product)
    WHERE p.name = $name
    RETURN p
    """,
    name="Laptop",
    database_="neo4j"
)
```

Problemático:

```python
query = f"""
MATCH (p:Product)
WHERE p.name = '{name}'
RETURN p
"""
```

Los parámetros evitan errores de formato y reducen riesgos al construir consultas.

## Crear o encontrar con `MERGE`

`MERGE` busca un patrón. Si no existe, lo crea.

```python
driver.execute_query(
    """
    MERGE (p:Product {sku: $sku})
    SET p.name = $name,
        p.price = $price,
        p.stock = $stock
    RETURN p
    """,
    sku="LAP-001",
    name="Laptop",
    price=3500,
    stock=5,
    database_="neo4j"
)
```

`MERGE` es útil para cargas idempotentes.

## Actualizar propiedades

```python
driver.execute_query(
    """
    MATCH (p:Product {sku: $sku})
    SET p.price = $price,
        p.stock = $stock
    RETURN p
    """,
    sku="LAP-001",
    price=3600,
    stock=4,
    database_="neo4j"
)
```

## Eliminar propiedad

```python
driver.execute_query(
    """
    MATCH (p:Product {sku: $sku})
    REMOVE p.temporary_field
    RETURN p
    """,
    sku="LAP-001",
    database_="neo4j"
)
```

## Eliminar nodo

Un nodo sin relaciones puede eliminarse con `DELETE`.

```python
driver.execute_query(
    """
    MATCH (p:Product {sku: $sku})
    DELETE p
    """,
    sku="LAP-001",
    database_="neo4j"
)
```

## Eliminar nodo con relaciones

Si el nodo tiene relaciones, puede usarse `DETACH DELETE`.

```python
driver.execute_query(
    """
    MATCH (p:Product {sku: $sku})
    DETACH DELETE p
    """,
    sku="LAP-001",
    database_="neo4j"
)
```

Debe usarse con cuidado porque elimina también las relaciones asociadas.

## Eliminación lógica

En aplicaciones reales, muchas veces conviene marcar nodos como inactivos.

```python
driver.execute_query(
    """
    MATCH (p:Product {sku: $sku})
    SET p.is_active = false
    RETURN p
    """,
    sku="LAP-001",
    database_="neo4j"
)
```

Consulta de activos:

```cypher
MATCH (p:Product)
WHERE p.is_active = true
RETURN p
```

## Crear relación entre nodos existentes

```python
driver.execute_query(
    """
    MATCH (c:Client {id: $client_id})
    MATCH (p:Product {sku: $sku})
    MERGE (c)-[r:BOUGHT]->(p)
    SET r.quantity = $quantity,
        r.created_at = datetime()
    RETURN c, r, p
    """,
    client_id="C001",
    sku="LAP-001",
    quantity=1,
    database_="neo4j"
)
```

## Consultar relaciones

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (c:Client)-[r:BOUGHT]->(p:Product)
    RETURN c.name AS client,
           p.name AS product,
           r.quantity AS quantity
    """,
    database_="neo4j"
)

for record in records:
    print(record["client"], record["product"], record["quantity"])
```

## Caminos

Un camino representa una secuencia de nodos y relaciones.

```python
records, summary, keys = driver.execute_query(
    """
    MATCH path = (a:Client)-[:KNOWS*1..3]->(b:Client)
    WHERE a.id = $client_id
    RETURN path
    """,
    client_id="C001",
    database_="neo4j"
)
```

La expresión `*1..3` indica relaciones de longitud variable entre 1 y 3 saltos.

## Agregaciones

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (:Client)-[r:BOUGHT]->(p:Product)
    RETURN p.name AS product,
           count(r) AS purchases
    ORDER BY purchases DESC
    """,
    database_="neo4j"
)
```

## Ordenar resultados

```cypher
ORDER BY purchases DESC
```

Ejemplo:

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    RETURN p.name AS name, p.price AS price
    ORDER BY p.price DESC
    """,
    database_="neo4j"
)
```

## Limitar resultados

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    RETURN p.name AS name, p.price AS price
    ORDER BY p.price DESC
    LIMIT $limit
    """,
    limit=10,
    database_="neo4j"
)
```

## Paginación simple

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    RETURN p.name AS name, p.price AS price
    ORDER BY p.name
    SKIP $offset
    LIMIT $limit
    """,
    offset=0,
    limit=10,
    database_="neo4j"
)
```

## Convertir registros a diccionarios

Cada `Record` puede convertirse con `data()`.

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    RETURN p.name AS name, p.price AS price
    """,
    database_="neo4j"
)

products = [
    record.data()
    for record in records
]

print(products)
```

## Acceso a valores de un registro

```python
for record in records:
    print(record["name"])
    print(record["price"])
```

## Nodos devueltos por Neo4j

Si se devuelve un nodo completo:

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    RETURN p
    LIMIT 1
    """,
    database_="neo4j"
)

node = records[0]["p"]

print(node.labels)
print(dict(node))
```

Convertir propiedades del nodo:

```python
properties = dict(node)
```

## Relaciones devueltas por Neo4j

```python
records, summary, keys = driver.execute_query(
    """
    MATCH (:Client)-[r:BOUGHT]->(:Product)
    RETURN r
    LIMIT 1
    """,
    database_="neo4j"
)

relationship = records[0]["r"]

print(relationship.type)
print(dict(relationship))
```

## Sesiones

Además de `execute_query()`, puede usarse una sesión explícita.

```python
with driver.session(database="neo4j") as session:
    result = session.run(
        """
        RETURN $message AS message
        """,
        message="Hola"
    )

    for record in result:
        print(record["message"])
```

## Cuándo usar sesiones explícitas

Conviene usar sesiones explícitas cuando se necesita:

```text
mayor control de transacciones
consultas agrupadas
patrones avanzados
lecturas y escrituras separadas
control explícito de database
```

Para consultas simples, `execute_query()` suele ser suficiente.

## Transacciones gestionadas

```python
def create_product(tx, sku, name, price):
    result = tx.run(
        """
        MERGE (p:Product {sku: $sku})
        SET p.name = $name,
            p.price = $price
        RETURN p
        """,
        sku=sku,
        name=name,
        price=price
    )

    return result.single()["p"]


with driver.session(database="neo4j") as session:
    product = session.execute_write(
        create_product,
        "LAP-001",
        "Laptop",
        3500
    )
```

## Lectura con transacción gestionada

```python
def get_product(tx, sku):
    result = tx.run(
        """
        MATCH (p:Product {sku: $sku})
        RETURN p
        """,
        sku=sku
    )

    record = result.single()

    if record is None:
        return None

    return dict(record["p"])


with driver.session(database="neo4j") as session:
    product = session.execute_read(
        get_product,
        "LAP-001"
    )
```

## Transacción explícita

```python
with driver.session(database="neo4j") as session:
    tx = session.begin_transaction()

    try:
        tx.run(
            """
            CREATE (:Product {name: $name})
            """,
            name="Laptop"
        )

        tx.commit()
    except Exception:
        tx.rollback()
        raise
```

Para la mayoría de casos, las transacciones gestionadas son más limpias.

## Restricciones de unicidad

Para evitar duplicados, puede crearse una restricción.

```python
driver.execute_query(
    """
    CREATE CONSTRAINT product_sku_unique IF NOT EXISTS
    FOR (p:Product)
    REQUIRE p.sku IS UNIQUE
    """,
    database_="neo4j"
)
```

Esto permite que `sku` sea único para nodos `Product`.

## Índices

Crear índice:

```python
driver.execute_query(
    """
    CREATE INDEX product_name_index IF NOT EXISTS
    FOR (p:Product)
    ON (p.name)
    """,
    database_="neo4j"
)
```

Los índices mejoran consultas frecuentes.

## Consultar constraints e índices

```python
records, summary, keys = driver.execute_query(
    """
    SHOW INDEXES
    """,
    database_="neo4j"
)
```

```python
records, summary, keys = driver.execute_query(
    """
    SHOW CONSTRAINTS
    """,
    database_="neo4j"
)
```

## Fechas y horas

Cypher puede usar funciones temporales.

```cypher
datetime()
date()
time()
```

Ejemplo:

```python
driver.execute_query(
    """
    CREATE (:Event {
        name: $name,
        created_at: datetime()
    })
    """,
    name="Carga inicial",
    database_="neo4j"
)
```

También puede pasarse una fecha desde Python, según el tipo requerido.

## Tipos de datos frecuentes

Neo4j puede trabajar con propiedades como:

```text
string
integer
float
boolean
date
datetime
listas
valores espaciales
```

Desde Python suelen pasarse valores simples:

```python
name = "Laptop"
price = 3500
is_active = True
tags = ["tecnologia", "oferta"]
```

## Uso con pandas

Neo4j puede alimentar un `DataFrame`.

```python
import pandas as pd

records, summary, keys = driver.execute_query(
    """
    MATCH (p:Product)
    RETURN p.sku AS sku,
           p.name AS name,
           p.price AS price,
           p.stock AS stock
    """,
    database_="neo4j"
)

df = pd.DataFrame([
    record.data()
    for record in records
])

print(df.head())
```

## Cargar datos desde pandas

```python
import pandas as pd

df = pd.DataFrame([
    {"sku": "LAP-001", "name": "Laptop", "price": 3500},
    {"sku": "MOU-001", "name": "Mouse", "price": 80},
])

for row in df.to_dict(orient="records"):
    driver.execute_query(
        """
        MERGE (p:Product {sku: $sku})
        SET p.name = $name,
            p.price = $price
        """,
        sku=row["sku"],
        name=row["name"],
        price=row["price"],
        database_="neo4j"
    )
```

Para cargas grandes, conviene usar patrones de carga por lotes.

## Carga por lotes con `UNWIND`

`UNWIND` permite enviar una lista de diccionarios como parámetro.

```python
products = [
    {"sku": "LAP-001", "name": "Laptop", "price": 3500},
    {"sku": "MOU-001", "name": "Mouse", "price": 80},
]

driver.execute_query(
    """
    UNWIND $products AS product
    MERGE (p:Product {sku: product.sku})
    SET p.name = product.name,
        p.price = product.price
    """,
    products=products,
    database_="neo4j"
)
```

Este patrón suele ser mejor que ejecutar una consulta por cada fila.

## Uso con FastAPI

```python
import os
from contextlib import asynccontextmanager

from fastapi import FastAPI, HTTPException
from neo4j import GraphDatabase


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.driver = GraphDatabase.driver(
        os.getenv("NEO4J_URI"),
        auth=(
            os.getenv("NEO4J_USER"),
            os.getenv("NEO4J_PASSWORD")
        )
    )

    yield

    app.state.driver.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
def read_products():
    records, summary, keys = app.state.driver.execute_query(
        """
        MATCH (p:Product)
        WHERE p.is_active = true
        RETURN p.sku AS sku,
               p.name AS name,
               p.price AS price,
               p.stock AS stock
        ORDER BY p.name
        """,
        database_=os.getenv("NEO4J_DATABASE", "neo4j")
    )

    return [
        record.data()
        for record in records
    ]


@app.get("/products/{sku}")
def read_product(sku: str):
    records, summary, keys = app.state.driver.execute_query(
        """
        MATCH (p:Product {sku: $sku})
        RETURN p.sku AS sku,
               p.name AS name,
               p.price AS price,
               p.stock AS stock
        """,
        sku=sku,
        database_=os.getenv("NEO4J_DATABASE", "neo4j")
    )

    if not records:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado"
        )

    return records[0].data()
```

## API asíncrona

El driver también ofrece API asíncrona.

Importación:

```python
from neo4j import AsyncGraphDatabase
```

Ejemplo:

```python
import asyncio

from neo4j import AsyncGraphDatabase


async def main():
    driver = AsyncGraphDatabase.driver(
        "neo4j://localhost:7687",
        auth=("neo4j", "secret")
    )

    try:
        records, summary, keys = await driver.execute_query(
            """
            RETURN $message AS message
            """,
            message="Hola async",
            database_="neo4j"
        )

        for record in records:
            print(record["message"])
    finally:
        await driver.close()


asyncio.run(main())
```

## Uso asíncrono con sesión

```python
async with driver.session(database="neo4j") as session:
    result = await session.run(
        """
        RETURN $message AS message
        """,
        message="Hola"
    )

    async for record in result:
        print(record["message"])
```

## Uso con FastAPI async

```python
import os
from contextlib import asynccontextmanager

from fastapi import FastAPI
from neo4j import AsyncGraphDatabase


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.driver = AsyncGraphDatabase.driver(
        os.getenv("NEO4J_URI"),
        auth=(
            os.getenv("NEO4J_USER"),
            os.getenv("NEO4J_PASSWORD")
        )
    )

    yield

    await app.state.driver.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products")
async def read_products():
    records, summary, keys = await app.state.driver.execute_query(
        """
        MATCH (p:Product)
        RETURN p.sku AS sku,
               p.name AS name,
               p.price AS price
        ORDER BY p.name
        """,
        database_=os.getenv("NEO4J_DATABASE", "neo4j")
    )

    return [
        record.data()
        for record in records
    ]
```

## Manejo de errores

Importación frecuente:

```python
from neo4j.exceptions import Neo4jError
```

Ejemplo:

```python
from neo4j.exceptions import Neo4jError

try:
    driver.execute_query(
        """
        MATCH (p:Product)
        RETURN p
        """,
        database_="neo4j"
    )
except Neo4jError as error:
    print("Error de Neo4j:", error)
```

## Errores frecuentes

```text
Neo4jError
ServiceUnavailable
AuthError
ClientError
TransientError
SessionExpired
```

## Error de autenticación

```python
from neo4j.exceptions import AuthError

try:
    driver.verify_connectivity()
except AuthError:
    print("Credenciales incorrectas")
```

## Error de conexión

```python
from neo4j.exceptions import ServiceUnavailable

try:
    driver.verify_connectivity()
except ServiceUnavailable:
    print("No se pudo conectar con Neo4j")
```

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

from dotenv import load_dotenv
from neo4j import GraphDatabase

load_dotenv()


def create_driver():
    return GraphDatabase.driver(
        os.getenv("NEO4J_URI"),
        auth=(
            os.getenv("NEO4J_USER"),
            os.getenv("NEO4J_PASSWORD")
        )
    )


NEO4J_DATABASE = os.getenv("NEO4J_DATABASE", "neo4j")
```

## `repositories.py`

```python
from .database import NEO4J_DATABASE


def create_constraints(driver):
    driver.execute_query(
        """
        CREATE CONSTRAINT product_sku_unique IF NOT EXISTS
        FOR (p:Product)
        REQUIRE p.sku IS UNIQUE
        """,
        database_=NEO4J_DATABASE
    )


def create_product(driver, sku, name, price, stock=0):
    records, summary, keys = driver.execute_query(
        """
        MERGE (p:Product {sku: $sku})
        SET p.name = $name,
            p.price = $price,
            p.stock = $stock,
            p.is_active = true
        RETURN p.sku AS sku,
               p.name AS name,
               p.price AS price,
               p.stock AS stock
        """,
        sku=sku,
        name=name,
        price=price,
        stock=stock,
        database_=NEO4J_DATABASE
    )

    return records[0].data()


def list_products(driver):
    records, summary, keys = driver.execute_query(
        """
        MATCH (p:Product)
        WHERE p.is_active = true
        RETURN p.sku AS sku,
               p.name AS name,
               p.price AS price,
               p.stock AS stock
        ORDER BY p.name
        """,
        database_=NEO4J_DATABASE
    )

    return [
        record.data()
        for record in records
    ]


def get_product(driver, sku):
    records, summary, keys = driver.execute_query(
        """
        MATCH (p:Product {sku: $sku})
        WHERE p.is_active = true
        RETURN p.sku AS sku,
               p.name AS name,
               p.price AS price,
               p.stock AS stock
        """,
        sku=sku,
        database_=NEO4J_DATABASE
    )

    if not records:
        return None

    return records[0].data()


def deactivate_product(driver, sku):
    records, summary, keys = driver.execute_query(
        """
        MATCH (p:Product {sku: $sku})
        SET p.is_active = false
        RETURN p.sku AS sku,
               p.name AS name,
               p.is_active AS is_active
        """,
        sku=sku,
        database_=NEO4J_DATABASE
    )

    if not records:
        return None

    return records[0].data()
```

## Separación por responsabilidades

```text
database.py      -> creación del driver
repositories.py  -> consultas Cypher
schemas.py       -> validación de entrada y salida
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Migraciones y esquema

Neo4j no usa migraciones del mismo modo que una base relacional tradicional.

Sin embargo, en proyectos reales conviene versionar cambios como:

```text
constraints
índices
consultas de carga inicial
scripts de mantenimiento
cambios de modelo de grafo
```

Ejemplo de archivo conceptual:

```text
migrations/
├─ 001_create_product_constraints.cypher
├─ 002_create_client_constraints.cypher
└─ 003_create_indexes.cypher
```

## Errores comunes

## Instalar `neo4j-driver` en lugar de `neo4j`

Para proyectos nuevos:

```bash
python -m pip install neo4j
```

## Olvidar cerrar el driver

Menos recomendable:

```python
driver = GraphDatabase.driver(...)
```

sin:

```python
driver.close()
```

Más seguro:

```python
try:
    ...
finally:
    driver.close()
```

## Concatenar valores dentro de Cypher

Problemático:

```python
query = f"""
MATCH (p:Product)
WHERE p.name = '{name}'
RETURN p
"""
```

Correcto:

```python
driver.execute_query(
    """
    MATCH (p:Product)
    WHERE p.name = $name
    RETURN p
    """,
    name=name,
    database_="neo4j"
)
```

## No crear restricciones de unicidad

Si se usa `MERGE` sobre un identificador lógico, conviene crear una restricción.

```cypher
CREATE CONSTRAINT product_sku_unique IF NOT EXISTS
FOR (p:Product)
REQUIRE p.sku IS UNIQUE
```

## Usar `CREATE` cuando se quería evitar duplicados

`CREATE` siempre crea un nuevo patrón.

Si se quiere crear solo si no existe, suele corresponder `MERGE`.

## Devolver nodos completos sin convertirlos

Si una API intenta devolver objetos Node directamente, puede fallar la serialización.

Mejor devolver propiedades específicas:

```cypher
RETURN p.sku AS sku, p.name AS name
```

o convertir:

```python
dict(node)
```

## No especificar base de datos

En entornos con múltiples bases, conviene indicar:

```python
database_="neo4j"
```

o:

```python
driver.session(database="neo4j")
```

## Usar Neo4j como si fuera una base relacional

Neo4j no está pensado para reemplazar tablas en todos los casos.

Debe usarse cuando las relaciones son parte central del problema.

## Crear un driver por cada consulta

Menos recomendable:

```python
def query():
    driver = GraphDatabase.driver(...)
```

Mejor crear el driver una vez y reutilizarlo.

## Buenas prácticas

## Usar parámetros

```cypher
WHERE p.sku = $sku
```

## Reutilizar el driver

Crear el driver al iniciar la aplicación y cerrarlo al finalizar.

## Usar `execute_query()` para consultas comunes

```python
driver.execute_query(...)
```

## Crear constraints e índices

```cypher
CREATE CONSTRAINT ... IF NOT EXISTS
CREATE INDEX ... IF NOT EXISTS
```

## Modelar relaciones explícitamente

En Neo4j, las relaciones no son un detalle secundario, sino parte central del modelo.

## Devolver datos serializables en APIs

```cypher
RETURN p.sku AS sku, p.name AS name
```

## Usar `MERGE` para cargas idempotentes

```cypher
MERGE (p:Product {sku: $sku})
```

## Separar consultas Cypher en repositorios

```text
repositories.py
```

## Usar `UNWIND` para cargas por lotes

```cypher
UNWIND $rows AS row
```

## Ejemplo integrado

```python
import os

from dotenv import load_dotenv
from neo4j import GraphDatabase
from neo4j.exceptions import Neo4jError

load_dotenv()

NEO4J_URI = os.getenv("NEO4J_URI", "neo4j://localhost:7687")
NEO4J_USER = os.getenv("NEO4J_USER", "neo4j")
NEO4J_PASSWORD = os.getenv("NEO4J_PASSWORD", "secret")
NEO4J_DATABASE = os.getenv("NEO4J_DATABASE", "neo4j")


def create_driver():
    return GraphDatabase.driver(
        NEO4J_URI,
        auth=(NEO4J_USER, NEO4J_PASSWORD)
    )


def create_constraints(driver):
    driver.execute_query(
        """
        CREATE CONSTRAINT product_sku_unique IF NOT EXISTS
        FOR (p:Product)
        REQUIRE p.sku IS UNIQUE
        """,
        database_=NEO4J_DATABASE
    )


def create_product(driver, sku, name, price, stock=0):
    records, summary, keys = driver.execute_query(
        """
        MERGE (p:Product {sku: $sku})
        SET p.name = $name,
            p.price = $price,
            p.stock = $stock,
            p.is_active = true
        RETURN p.sku AS sku,
               p.name AS name,
               p.price AS price,
               p.stock AS stock,
               p.is_active AS is_active
        """,
        sku=sku,
        name=name,
        price=price,
        stock=stock,
        database_=NEO4J_DATABASE
    )

    return records[0].data()


def list_products(driver):
    records, summary, keys = driver.execute_query(
        """
        MATCH (p:Product)
        WHERE p.is_active = true
        RETURN p.sku AS sku,
               p.name AS name,
               p.price AS price,
               p.stock AS stock
        ORDER BY p.name
        """,
        database_=NEO4J_DATABASE
    )

    return [
        record.data()
        for record in records
    ]


def create_purchase(driver, client_id, client_name, sku, quantity):
    records, summary, keys = driver.execute_query(
        """
        MERGE (c:Client {id: $client_id})
        SET c.name = $client_name

        MATCH (p:Product {sku: $sku})

        MERGE (c)-[r:BOUGHT]->(p)
        SET r.quantity = coalesce(r.quantity, 0) + $quantity,
            r.updated_at = datetime()

        RETURN c.id AS client_id,
               c.name AS client_name,
               p.sku AS sku,
               p.name AS product_name,
               r.quantity AS quantity
        """,
        client_id=client_id,
        client_name=client_name,
        sku=sku,
        quantity=quantity,
        database_=NEO4J_DATABASE
    )

    return records[0].data()


def list_purchases(driver):
    records, summary, keys = driver.execute_query(
        """
        MATCH (c:Client)-[r:BOUGHT]->(p:Product)
        RETURN c.id AS client_id,
               c.name AS client_name,
               p.sku AS sku,
               p.name AS product_name,
               r.quantity AS quantity
        ORDER BY c.name, p.name
        """,
        database_=NEO4J_DATABASE
    )

    return [
        record.data()
        for record in records
    ]


def main():
    driver = create_driver()

    try:
        driver.verify_connectivity()

        create_constraints(driver)

        create_product(driver, "LAP-001", "Laptop", 3500, stock=5)
        create_product(driver, "MOU-001", "Mouse", 80, stock=20)

        create_purchase(driver, "C001", "Ana", "LAP-001", 1)
        create_purchase(driver, "C001", "Ana", "MOU-001", 2)

        for product in list_products(driver):
            print(
                product["sku"],
                product["name"],
                product["price"],
                product["stock"]
            )

        for purchase in list_purchases(driver):
            print(
                purchase["client_name"],
                purchase["product_name"],
                purchase["quantity"]
            )

    except Neo4jError as error:
        print("Error de Neo4j:", error)
    finally:
        driver.close()


if __name__ == "__main__":
    main()
```

## Relación con otras librerías

`neo4j` se relaciona especialmente con:

```text
pandas
fastapi
flask
pydantic
python-dotenv
pytest
networkx
```

## Relación con pandas

Pandas puede recibir resultados de Neo4j como diccionarios.

```python
pd.DataFrame([record.data() for record in records])
```

## Relación con FastAPI

Neo4j puede usarse como base de datos para APIs que consultan relaciones, redes y grafos.

## Relación con NetworkX

NetworkX se usa para análisis de grafos en memoria.

Neo4j se usa como base de datos de grafos persistente.

Pueden complementarse:

```text
Neo4j   -> almacenamiento y consulta persistente
NetworkX -> análisis de grafos en memoria
```

## Relación con bases relacionales

Neo4j no reemplaza automáticamente a PostgreSQL, MySQL o SQL Server.

Su ventaja aparece cuando las relaciones y caminos son parte central del problema.

## Orden didáctico interno

```text
1. Propósito de neo4j
2. Instalación
3. Conexión con GraphDatabase.driver()
4. Driver y cierre de recursos
5. execute_query()
6. Cypher
7. Nodos, etiquetas, propiedades y relaciones
8. CREATE, MATCH, WHERE, MERGE, SET y DELETE
9. Parámetros
10. Registros y conversión a diccionarios
11. Sesiones y transacciones
12. Constraints e índices
13. Cargas por lotes con UNWIND
14. Uso con pandas y FastAPI
15. API asíncrona con AsyncGraphDatabase
16. Manejo de errores
17. Organización recomendada
18. Errores comunes
19. Buenas prácticas
```