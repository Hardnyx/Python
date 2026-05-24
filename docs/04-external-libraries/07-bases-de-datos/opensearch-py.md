# `opensearch-py`

## Propósito

`opensearch-py` es una librería externa para conectar Python con OpenSearch.

OpenSearch es un motor de búsqueda y análisis distribuido. Se utiliza para indexar documentos, ejecutar búsquedas de texto, filtrar datos, calcular agregaciones, analizar logs, construir buscadores internos y consultar información semiestructurada.

La librería permite enviar consultas desde Python hacia un clúster OpenSearch.

## Naturaleza de la librería

`opensearch-py` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install opensearch-py
```

La importación habitual es:

```python
from opensearchpy import OpenSearch
```

Para uso asíncrono:

```python
from opensearchpy import AsyncOpenSearch
```

Para operaciones masivas:

```python
from opensearchpy.helpers import bulk
```

## Relación con OpenSearch

OpenSearch organiza la información en índices y documentos.

Equivalencia conceptual aproximada:

```text
OpenSearch index    -> agrupación de documentos
OpenSearch document -> registro JSON
field               -> campo del documento
query               -> consulta de búsqueda
aggregation         -> cálculo resumido sobre documentos
```

OpenSearch no está pensado principalmente como una base transaccional clásica.

Su fortaleza está en búsqueda, filtrado, agregaciones y análisis sobre documentos indexados.

## Diferencia frente a Elasticsearch

OpenSearch nació como un proyecto separado del ecosistema Elasticsearch.

A nivel de uso en Python, las APIs pueden parecerse bastante porque `opensearch-py` deriva históricamente de `elasticsearch-py`.

Sin embargo, para proyectos basados en OpenSearch conviene usar:

```python
from opensearchpy import OpenSearch
```

En proyectos basados en Elasticsearch conviene usar:

```python
from elasticsearch import Elasticsearch
```

## Idea central

La idea principal de `opensearch-py` es crear un cliente y usarlo para comunicarse con un servidor o clúster OpenSearch.

Flujo típico:

```text
Python -> OpenSearch client -> OpenSearch cluster -> índices y documentos
```

Ejemplo mínimo:

```python
from opensearchpy import OpenSearch

client = OpenSearch(
    hosts=[
        {
            "host": "localhost",
            "port": 9200
        }
    ],
    http_compress=True,
    use_ssl=False
)

response = client.info()

print(response)
```

## Cuándo usar OpenSearch

Conviene usar OpenSearch cuando se necesita:

```text
búsqueda de texto
búsqueda por relevancia
filtros combinados
autocompletado
análisis de logs
análisis de eventos
agregaciones rápidas
monitoreo
búsqueda sobre documentos JSON
exploración de datos semiestructurados
```

## Cuándo no usar OpenSearch

No suele ser la mejor opción cuando se necesita:

```text
transacciones relacionales fuertes
integridad referencial clásica
joins frecuentes
base principal para operaciones contables
modelo altamente normalizado
actualizaciones transaccionales complejas
almacenamiento local simple
```

Para esos casos pueden corresponder:

```text
PostgreSQL
MySQL
SQL Server
SQLite
MongoDB
Redis
DuckDB
```

OpenSearch suele complementar a una base principal, no reemplazarla automáticamente.

## Instalación

Instalación básica:

```bash
python -m pip install opensearch-py
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
opensearch-py==2.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import opensearchpy

print(opensearchpy.__version__)
```

## Conexión básica local

```python
from opensearchpy import OpenSearch

client = OpenSearch(
    hosts=[
        {
            "host": "localhost",
            "port": 9200
        }
    ],
    use_ssl=False
)

response = client.info()

print(response)
```

## Conexión con usuario y contraseña

```python
from opensearchpy import OpenSearch

client = OpenSearch(
    hosts=[
        {
            "host": "localhost",
            "port": 9200
        }
    ],
    http_auth=("admin", "password"),
    use_ssl=True,
    verify_certs=False,
    ssl_show_warn=False
)
```

`verify_certs=False` puede usarse en entornos locales de prueba, pero no debe usarse como práctica general en producción.

## Conexión segura con certificados

```python
from opensearchpy import OpenSearch

client = OpenSearch(
    hosts=[
        {
            "host": "opensearch.example.com",
            "port": 9200
        }
    ],
    http_auth=("admin", "password"),
    use_ssl=True,
    verify_certs=True
)
```

En producción, lo recomendable es verificar certificados.

## Variables de entorno

Menos recomendable:

```python
client = OpenSearch(
    hosts=[{"host": "localhost", "port": 9200}],
    http_auth=("admin", "password")
)
```

Más conveniente:

```python
import os

from opensearchpy import OpenSearch

client = OpenSearch(
    hosts=[
        {
            "host": os.getenv("OPENSEARCH_HOST", "localhost"),
            "port": int(os.getenv("OPENSEARCH_PORT", "9200"))
        }
    ],
    http_auth=(
        os.getenv("OPENSEARCH_USER"),
        os.getenv("OPENSEARCH_PASSWORD")
    ),
    use_ssl=os.getenv("OPENSEARCH_USE_SSL", "false") == "true",
    verify_certs=os.getenv("OPENSEARCH_VERIFY_CERTS", "false") == "true"
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
OPENSEARCH_HOST=localhost
OPENSEARCH_PORT=9200
OPENSEARCH_USER=admin
OPENSEARCH_PASSWORD=password
OPENSEARCH_USE_SSL=false
OPENSEARCH_VERIFY_CERTS=false
```

Código:

```python
import os

from dotenv import load_dotenv
from opensearchpy import OpenSearch

load_dotenv()

client = OpenSearch(
    hosts=[
        {
            "host": os.getenv("OPENSEARCH_HOST", "localhost"),
            "port": int(os.getenv("OPENSEARCH_PORT", "9200"))
        }
    ],
    http_auth=(
        os.getenv("OPENSEARCH_USER"),
        os.getenv("OPENSEARCH_PASSWORD")
    ),
    use_ssl=os.getenv("OPENSEARCH_USE_SSL", "false") == "true",
    verify_certs=os.getenv("OPENSEARCH_VERIFY_CERTS", "false") == "true"
)
```

## Verificar conexión

```python
if client.ping():
    print("Conexión correcta")
else:
    print("No se pudo conectar con OpenSearch")
```

También puede consultarse información del clúster:

```python
response = client.info()

print(response)
```

## Índice

Un índice agrupa documentos.

Ejemplos conceptuales:

```text
products
clients
logs
transactions
events
```

Crear índice:

```python
client.indices.create(
    index="products"
)
```

Eliminar índice:

```python
client.indices.delete(
    index="products"
)
```

Verificar si existe:

```python
exists = client.indices.exists(
    index="products"
)

print(exists)
```

## Documento

Un documento es una estructura JSON.

Ejemplo:

```python
document = {
    "sku": "LAP-001",
    "name": "Laptop",
    "price": 3500,
    "stock": 5,
    "is_active": True
}
```

Indexar documento:

```python
client.index(
    index="products",
    id="LAP-001",
    body=document
)
```

## Indexar documento

```python
response = client.index(
    index="products",
    id="LAP-001",
    body={
        "sku": "LAP-001",
        "name": "Laptop",
        "price": 3500,
        "stock": 5,
        "is_active": True
    }
)

print(response)
```

## Obtener documento por ID

```python
response = client.get(
    index="products",
    id="LAP-001"
)

print(response["_source"])
```

## Verificar existencia de documento

```python
exists = client.exists(
    index="products",
    id="LAP-001"
)

print(exists)
```

## Actualizar documento

```python
client.update(
    index="products",
    id="LAP-001",
    body={
        "doc": {
            "price": 3600,
            "stock": 4
        }
    }
)
```

## Eliminar documento

```python
client.delete(
    index="products",
    id="LAP-001"
)
```

## Eliminación lógica

En aplicaciones reales, muchas veces se prefiere marcar el documento como inactivo.

```python
client.update(
    index="products",
    id="LAP-001",
    body={
        "doc": {
            "is_active": False
        }
    }
)
```

Consulta de activos:

```python
response = client.search(
    index="products",
    body={
        "query": {
            "term": {
                "is_active": True
            }
        }
    }
)
```

## Búsqueda básica

```python
response = client.search(
    index="products",
    body={
        "query": {
            "match_all": {}
        }
    }
)

for hit in response["hits"]["hits"]:
    print(hit["_source"])
```

## `match_all`

Devuelve documentos sin aplicar filtro.

```python
query = {
    "query": {
        "match_all": {}
    }
}
```

Uso:

```python
client.search(
    index="products",
    body=query
)
```

## Búsqueda por texto

```python
response = client.search(
    index="products",
    body={
        "query": {
            "match": {
                "name": "laptop"
            }
        }
    }
)
```

`match` se usa para búsqueda de texto analizado.

## Búsqueda exacta con `term`

```python
response = client.search(
    index="products",
    body={
        "query": {
            "term": {
                "sku": "LAP-001"
            }
        }
    }
)
```

`term` se usa para coincidencias exactas.

En campos de texto analizados puede ser necesario consultar una subpropiedad tipo `keyword`, según el mapping.

## Filtro por rango

```python
response = client.search(
    index="products",
    body={
        "query": {
            "range": {
                "price": {
                    "gte": 100,
                    "lte": 5000
                }
            }
        }
    }
)
```

Operadores comunes:

```text
gt  -> mayor que
gte -> mayor o igual que
lt  -> menor que
lte -> menor o igual que
```

## Consulta booleana

```python
response = client.search(
    index="products",
    body={
        "query": {
            "bool": {
                "must": [
                    {
                        "match": {
                            "name": "laptop"
                        }
                    }
                ],
                "filter": [
                    {
                        "term": {
                            "is_active": True
                        }
                    },
                    {
                        "range": {
                            "price": {
                                "gte": 100
                            }
                        }
                    }
                ]
            }
        }
    }
)
```

## Partes de `bool`

```text
must     -> condiciones que deben cumplirse y pueden afectar relevancia
filter   -> condiciones que deben cumplirse sin afectar relevancia
should   -> condiciones deseables
must_not -> condiciones excluidas
```

## Ordenar resultados

```python
response = client.search(
    index="products",
    body={
        "query": {
            "match_all": {}
        },
        "sort": [
            {
                "price": {
                    "order": "desc"
                }
            }
        ]
    }
)
```

## Limitar resultados

```python
response = client.search(
    index="products",
    body={
        "query": {
            "match_all": {}
        },
        "size": 10
    }
)
```

## Paginación simple

```python
response = client.search(
    index="products",
    body={
        "query": {
            "match_all": {}
        },
        "from": 0,
        "size": 10
    }
)
```

En OpenSearch, `from` y `size` permiten paginación básica.

## `_source`

`_source` contiene el documento original indexado.

```python
for hit in response["hits"]["hits"]:
    source = hit["_source"]

    print(source)
```

## Score

El campo `_score` indica relevancia de búsqueda.

```python
for hit in response["hits"]["hits"]:
    print(hit["_score"], hit["_source"])
```

En consultas filtradas exactas, el score puede no ser el elemento central.

## Seleccionar campos

```python
response = client.search(
    index="products",
    body={
        "_source": [
            "sku",
            "name",
            "price"
        ],
        "query": {
            "match_all": {}
        }
    }
)
```

Esto reduce los campos devueltos.

## Mapping

El mapping define tipos de campos y cómo se indexan.

Ejemplo:

```python
client.indices.create(
    index="products",
    body={
        "mappings": {
            "properties": {
                "sku": {
                    "type": "keyword"
                },
                "name": {
                    "type": "text"
                },
                "price": {
                    "type": "double"
                },
                "stock": {
                    "type": "integer"
                },
                "is_active": {
                    "type": "boolean"
                },
                "created_at": {
                    "type": "date"
                }
            }
        }
    }
)
```

## Tipos frecuentes de campos

```text
keyword -> texto exacto para filtros, IDs y categorías
text    -> texto analizado para búsqueda
integer -> entero
long    -> entero grande
double  -> número decimal
boolean -> verdadero o falso
date    -> fecha
object  -> objeto anidado básico
nested  -> arreglo de objetos con consulta especial
```

## `keyword` vs `text`

## `keyword`

Se usa para coincidencias exactas.

Ejemplos:

```text
sku
email
country_code
status
category
```

Consulta típica:

```python
{
    "term": {
        "sku": "LAP-001"
    }
}
```

## `text`

Se usa para búsqueda de texto.

Ejemplos:

```text
name
description
comment
body
```

Consulta típica:

```python
{
    "match": {
        "description": "laptop gamer"
    }
}
```

## Crear índice solo si no existe

```python
index_name = "products"

if not client.indices.exists(index=index_name):
    client.indices.create(
        index=index_name,
        body={
            "mappings": {
                "properties": {
                    "sku": {"type": "keyword"},
                    "name": {"type": "text"},
                    "price": {"type": "double"},
                    "stock": {"type": "integer"},
                    "is_active": {"type": "boolean"}
                }
            }
        }
    )
```

## Bulk indexing

Para indexar muchos documentos, conviene usar helpers.

```python
from opensearchpy.helpers import bulk

actions = [
    {
        "_index": "products",
        "_id": "LAP-001",
        "_source": {
            "sku": "LAP-001",
            "name": "Laptop",
            "price": 3500,
            "stock": 5,
            "is_active": True
        }
    },
    {
        "_index": "products",
        "_id": "MOU-001",
        "_source": {
            "sku": "MOU-001",
            "name": "Mouse",
            "price": 80,
            "stock": 20,
            "is_active": True
        }
    }
]

success, errors = bulk(
    client,
    actions,
    raise_on_error=False
)

print(success)
print(errors)
```

## Generador para bulk

Para grandes volúmenes, conviene generar acciones de forma progresiva.

```python
def generate_actions(products):
    for product in products:
        yield {
            "_index": "products",
            "_id": product["sku"],
            "_source": product
        }


success, errors = bulk(
    client,
    generate_actions(products),
    raise_on_error=False
)
```

Este patrón evita construir toda la lista en memoria cuando los datos son grandes.

## Agregaciones

Las agregaciones resumen documentos.

Ejemplo: contar por categoría.

```python
response = client.search(
    index="products",
    body={
        "size": 0,
        "aggs": {
            "by_category": {
                "terms": {
                    "field": "category"
                }
            }
        }
    }
)

buckets = response["aggregations"]["by_category"]["buckets"]

for bucket in buckets:
    print(bucket["key"], bucket["doc_count"])
```

## Agregación de suma

```python
response = client.search(
    index="products",
    body={
        "size": 0,
        "aggs": {
            "total_stock": {
                "sum": {
                    "field": "stock"
                }
            }
        }
    }
)

print(response["aggregations"]["total_stock"]["value"])
```

## Agregación de promedio

```python
response = client.search(
    index="products",
    body={
        "size": 0,
        "aggs": {
            "average_price": {
                "avg": {
                    "field": "price"
                }
            }
        }
    }
)

print(response["aggregations"]["average_price"]["value"])
```

## Fechas

Las fechas suelen guardarse en formato ISO.

```python
from datetime import datetime, timezone

client.index(
    index="events",
    body={
        "event_type": "created",
        "created_at": datetime.now(timezone.utc).isoformat()
    }
)
```

Consulta por rango de fechas:

```python
response = client.search(
    index="events",
    body={
        "query": {
            "range": {
                "created_at": {
                    "gte": "2026-01-01",
                    "lt": "2027-01-01"
                }
            }
        }
    }
)
```

## Refresh

Después de indexar, un documento puede no estar disponible inmediatamente para búsqueda.

Para pruebas puede usarse:

```python
client.index(
    index="products",
    id="LAP-001",
    body={
        "name": "Laptop"
    },
    refresh=True
)
```

En producción, usar `refresh=True` en cada operación puede afectar rendimiento.

## Count

```python
response = client.count(
    index="products",
    body={
        "query": {
            "term": {
                "is_active": True
            }
        }
    }
)

print(response["count"])
```

## Delete by query

```python
client.delete_by_query(
    index="products",
    body={
        "query": {
            "term": {
                "is_active": False
            }
        }
    }
)
```

Debe usarse con cuidado porque puede eliminar muchos documentos.

## Update by query

```python
client.update_by_query(
    index="products",
    body={
        "query": {
            "term": {
                "category": "old"
            }
        },
        "script": {
            "source": "ctx._source.category = params.category",
            "params": {
                "category": "new"
            }
        }
    }
)
```

Debe probarse cuidadosamente antes de aplicarse sobre datos importantes.

## Search after

Para búsquedas profundas, puede evaluarse `search_after`.

Primera búsqueda:

```python
response = client.search(
    index="products",
    body={
        "query": {
            "match_all": {}
        },
        "sort": [
            {"sku": "asc"}
        ],
        "size": 100
    }
)
```

Luego se toma el último valor de `sort` y se usa en la siguiente consulta.

```python
last_sort = response["hits"]["hits"][-1]["sort"]

next_response = client.search(
    index="products",
    body={
        "query": {
            "match_all": {}
        },
        "sort": [
            {"sku": "asc"}
        ],
        "search_after": last_sort,
        "size": 100
    }
)
```

## Manejo de errores

Importaciones frecuentes:

```python
from opensearchpy import OpenSearchException
from opensearchpy import NotFoundError
from opensearchpy import ConnectionError
from opensearchpy import AuthenticationException
```

Ejemplo:

```python
from opensearchpy import OpenSearchException

try:
    response = client.search(
        index="products",
        body={
            "query": {
                "match_all": {}
            }
        }
    )
except OpenSearchException as error:
    print("Error de OpenSearch:", error)
```

## Documento no encontrado

```python
from opensearchpy import NotFoundError

try:
    document = client.get(
        index="products",
        id="LAP-001"
    )
except NotFoundError:
    print("Documento no encontrado")
```

## Error de conexión

```python
from opensearchpy import ConnectionError

try:
    client.info()
except ConnectionError:
    print("No se pudo conectar con OpenSearch")
```

## API asíncrona

Importación:

```python
from opensearchpy import AsyncOpenSearch
```

Ejemplo:

```python
import asyncio

from opensearchpy import AsyncOpenSearch


async def main():
    client = AsyncOpenSearch(
        hosts=[
            {
                "host": "localhost",
                "port": 9200
            }
        ],
        use_ssl=False
    )

    try:
        response = await client.info()

        print(response)
    finally:
        await client.close()


asyncio.run(main())
```

## Indexar de forma asíncrona

```python
await client.index(
    index="products",
    id="LAP-001",
    body={
        "sku": "LAP-001",
        "name": "Laptop",
        "price": 3500
    }
)
```

## Buscar de forma asíncrona

```python
response = await client.search(
    index="products",
    body={
        "query": {
            "match": {
                "name": "laptop"
            }
        }
    }
)
```

## Cerrar cliente asíncrono

```python
await client.close()
```

En aplicaciones web asíncronas, el cliente suele crearse al iniciar la aplicación y cerrarse al finalizar.

## Uso con FastAPI

```python
import os
from contextlib import asynccontextmanager

from fastapi import FastAPI
from opensearchpy import AsyncOpenSearch


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.search_client = AsyncOpenSearch(
        hosts=[
            {
                "host": os.getenv("OPENSEARCH_HOST", "localhost"),
                "port": int(os.getenv("OPENSEARCH_PORT", "9200"))
            }
        ],
        http_auth=(
            os.getenv("OPENSEARCH_USER"),
            os.getenv("OPENSEARCH_PASSWORD")
        ),
        use_ssl=os.getenv("OPENSEARCH_USE_SSL", "false") == "true",
        verify_certs=os.getenv("OPENSEARCH_VERIFY_CERTS", "false") == "true"
    )

    yield

    await app.state.search_client.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products/search")
async def search_products(q: str):
    response = await app.state.search_client.search(
        index="products",
        body={
            "query": {
                "bool": {
                    "must": [
                        {
                            "match": {
                                "name": q
                            }
                        }
                    ],
                    "filter": [
                        {
                            "term": {
                                "is_active": True
                            }
                        }
                    ]
                }
            }
        }
    )

    return [
        {
            "id": hit["_id"],
            "score": hit["_score"],
            **hit["_source"]
        }
        for hit in response["hits"]["hits"]
    ]
```

## Uso con Flask

```python
import os

from flask import Flask, request
from opensearchpy import OpenSearch

app = Flask(__name__)

client = OpenSearch(
    hosts=[
        {
            "host": os.getenv("OPENSEARCH_HOST", "localhost"),
            "port": int(os.getenv("OPENSEARCH_PORT", "9200"))
        }
    ],
    http_auth=(
        os.getenv("OPENSEARCH_USER"),
        os.getenv("OPENSEARCH_PASSWORD")
    ),
    use_ssl=os.getenv("OPENSEARCH_USE_SSL", "false") == "true",
    verify_certs=os.getenv("OPENSEARCH_VERIFY_CERTS", "false") == "true"
)


@app.route("/products/search")
def search_products():
    query = request.args.get("q", "")

    response = client.search(
        index="products",
        body={
            "query": {
                "match": {
                    "name": query
                }
            }
        }
    )

    return [
        {
            "id": hit["_id"],
            "score": hit["_score"],
            **hit["_source"]
        }
        for hit in response["hits"]["hits"]
    ]
```

## Uso con pandas

OpenSearch puede alimentar un `DataFrame`.

```python
import pandas as pd

response = client.search(
    index="products",
    body={
        "query": {
            "match_all": {}
        },
        "size": 100
    }
)

documents = [
    {
        "id": hit["_id"],
        **hit["_source"]
    }
    for hit in response["hits"]["hits"]
]

df = pd.DataFrame(documents)

print(df.head())
```

Para grandes volúmenes, no conviene depender de una sola búsqueda con `size` muy grande.

## Carga desde pandas

```python
from opensearchpy.helpers import bulk

def dataframe_to_actions(df, index_name):
    for row in df.to_dict(orient="records"):
        yield {
            "_index": index_name,
            "_id": row["sku"],
            "_source": row
        }


success, errors = bulk(
    client,
    dataframe_to_actions(df, "products"),
    raise_on_error=False
)
```

## Uso como complemento de una base principal

Patrón frecuente:

```text
PostgreSQL / MySQL / MongoDB -> fuente principal
OpenSearch                   -> búsqueda y análisis
```

Flujo conceptual:

```text
1. Guardar dato principal en una base transaccional o documental.
2. Indexar copia útil para búsqueda en OpenSearch.
3. Consultar OpenSearch para búsqueda rápida.
4. Consultar base principal para operaciones críticas si corresponde.
```

## Organización recomendada

```text
app/
├─ search.py
├─ repositories.py
├─ schemas.py
├─ services.py
└─ main.py
```

## `search.py`

```python
import os

from dotenv import load_dotenv
from opensearchpy import OpenSearch

load_dotenv()


def create_search_client():
    return OpenSearch(
        hosts=[
            {
                "host": os.getenv("OPENSEARCH_HOST", "localhost"),
                "port": int(os.getenv("OPENSEARCH_PORT", "9200"))
            }
        ],
        http_auth=(
            os.getenv("OPENSEARCH_USER"),
            os.getenv("OPENSEARCH_PASSWORD")
        ),
        use_ssl=os.getenv("OPENSEARCH_USE_SSL", "false") == "true",
        verify_certs=os.getenv("OPENSEARCH_VERIFY_CERTS", "false") == "true"
    )
```

## `repositories.py`

```python
def create_products_index(client):
    index_name = "products"

    if client.indices.exists(index=index_name):
        return

    client.indices.create(
        index=index_name,
        body={
            "mappings": {
                "properties": {
                    "sku": {"type": "keyword"},
                    "name": {"type": "text"},
                    "category": {"type": "keyword"},
                    "price": {"type": "double"},
                    "stock": {"type": "integer"},
                    "is_active": {"type": "boolean"}
                }
            }
        }
    )


def index_product(client, product):
    client.index(
        index="products",
        id=product["sku"],
        body=product
    )


def get_product(client, sku):
    response = client.get(
        index="products",
        id=sku
    )

    return {
        "id": response["_id"],
        **response["_source"]
    }


def search_products(client, query):
    response = client.search(
        index="products",
        body={
            "query": {
                "bool": {
                    "must": [
                        {
                            "match": {
                                "name": query
                            }
                        }
                    ],
                    "filter": [
                        {
                            "term": {
                                "is_active": True
                            }
                        }
                    ]
                }
            }
        }
    )

    return [
        {
            "id": hit["_id"],
            "score": hit["_score"],
            **hit["_source"]
        }
        for hit in response["hits"]["hits"]
    ]
```

## Separación por responsabilidades

```text
search.py        -> creación del cliente OpenSearch
repositories.py  -> consultas e indexación
schemas.py       -> validación de entrada
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Errores comunes

## Instalar el paquete y tratar de importar otro nombre

Instalación correcta:

```bash
python -m pip install opensearch-py
```

Importación correcta:

```python
from opensearchpy import OpenSearch
```

No se importa como:

```python
import opensearch_py
```

## Usar OpenSearch como base relacional principal

OpenSearch es excelente para búsqueda, pero no reemplaza automáticamente una base transaccional.

## No cerrar el cliente asíncrono

Problemático:

```python
client = AsyncOpenSearch(...)
```

sin:

```python
await client.close()
```

## No definir mapping cuando importa la precisión

Si se deja que OpenSearch infiera tipos, puede crear mappings que no coinciden con lo esperado.

Conviene definir mappings para índices importantes.

## Confundir `text` y `keyword`

Problemático:

```python
{
    "term": {
        "name": "Laptop"
    }
}
```

si `name` es `text` analizado y se esperaba coincidencia exacta.

Para exactitud, usar un campo `keyword`.

## Pedir demasiados resultados con `size`

Problemático:

```python
"size": 100000
```

Mejor usar paginación adecuada o `search_after`, según el caso.

## Construir nombres de índices desde entrada libre

Problemático:

```python
client.search(index=user_input, body={...})
```

Mejor validar contra una lista permitida:

```python
allowed_indices = {
    "products": "products",
    "clients": "clients"
}
```

## No manejar errores de conexión

Conviene capturar errores como:

```python
ConnectionError
AuthenticationException
OpenSearchException
```

## Usar `refresh=True` en exceso

Puede ser útil en pruebas, pero afectar rendimiento en producción.

## Indexar documentos enormes sin controlar campos

Documentos muy grandes pueden afectar rendimiento de indexación, búsqueda y almacenamiento.

Conviene indexar solo lo necesario para búsqueda.

## Buenas prácticas

## Usar variables de entorno

```text
OPENSEARCH_HOST
OPENSEARCH_PORT
OPENSEARCH_USER
OPENSEARCH_PASSWORD
OPENSEARCH_USE_SSL
OPENSEARCH_VERIFY_CERTS
```

## Verificar certificados en producción

```python
verify_certs=True
```

## Definir mappings para índices importantes

```python
client.indices.create(
    index="products",
    body={...}
)
```

## Usar `keyword` para filtros exactos

```text
sku
status
category
country
```

## Usar `text` para búsqueda textual

```text
name
description
body
comment
```

## Usar bulk para cargas grandes

```python
from opensearchpy.helpers import bulk
```

## Usar generadores para bulk

```python
yield {
    "_index": "...",
    "_id": "...",
    "_source": ...
}
```

## Reutilizar el cliente

No crear un cliente nuevo por cada búsqueda.

## Validar nombres de índices y campos dinámicos

Usar listas permitidas cuando el usuario decide qué índice o campo consultar.

## Separar búsqueda de lógica de negocio

```text
search.py
repositories.py
services.py
```

## Usar OpenSearch como buscador complementario

Mantener datos críticos en una fuente principal cuando se requieran transacciones fuertes.

## Ejemplo integrado

```python
import os

from dotenv import load_dotenv
from opensearchpy import OpenSearch
from opensearchpy import OpenSearchException
from opensearchpy.helpers import bulk

load_dotenv()

INDEX_NAME = "products"


def create_client():
    return OpenSearch(
        hosts=[
            {
                "host": os.getenv("OPENSEARCH_HOST", "localhost"),
                "port": int(os.getenv("OPENSEARCH_PORT", "9200"))
            }
        ],
        http_auth=(
            os.getenv("OPENSEARCH_USER"),
            os.getenv("OPENSEARCH_PASSWORD")
        ),
        use_ssl=os.getenv("OPENSEARCH_USE_SSL", "false") == "true",
        verify_certs=os.getenv("OPENSEARCH_VERIFY_CERTS", "false") == "true",
        ssl_show_warn=False
    )


def create_index(client):
    if client.indices.exists(index=INDEX_NAME):
        return

    client.indices.create(
        index=INDEX_NAME,
        body={
            "mappings": {
                "properties": {
                    "sku": {"type": "keyword"},
                    "name": {"type": "text"},
                    "category": {"type": "keyword"},
                    "price": {"type": "double"},
                    "stock": {"type": "integer"},
                    "is_active": {"type": "boolean"},
                    "created_at": {"type": "date"}
                }
            }
        }
    )


def index_product(client, product):
    client.index(
        index=INDEX_NAME,
        id=product["sku"],
        body=product
    )


def bulk_index_products(client, products):
    def generate_actions():
        for product in products:
            yield {
                "_index": INDEX_NAME,
                "_id": product["sku"],
                "_source": product
            }

    return bulk(
        client,
        generate_actions(),
        raise_on_error=False
    )


def get_product(client, sku):
    response = client.get(
        index=INDEX_NAME,
        id=sku
    )

    return {
        "id": response["_id"],
        **response["_source"]
    }


def search_products(client, query, limit=10):
    response = client.search(
        index=INDEX_NAME,
        body={
            "query": {
                "bool": {
                    "must": [
                        {
                            "match": {
                                "name": query
                            }
                        }
                    ],
                    "filter": [
                        {
                            "term": {
                                "is_active": True
                            }
                        }
                    ]
                }
            },
            "size": limit
        }
    )

    return [
        {
            "id": hit["_id"],
            "score": hit["_score"],
            **hit["_source"]
        }
        for hit in response["hits"]["hits"]
    ]


def deactivate_product(client, sku):
    client.update(
        index=INDEX_NAME,
        id=sku,
        body={
            "doc": {
                "is_active": False
            }
        }
    )


def main():
    client = create_client()

    try:
        if not client.ping():
            raise RuntimeError("No se pudo conectar con OpenSearch")

        create_index(client)

        products = [
            {
                "sku": "LAP-001",
                "name": "Laptop",
                "category": "Tecnología",
                "price": 3500,
                "stock": 5,
                "is_active": True
            },
            {
                "sku": "MOU-001",
                "name": "Mouse",
                "category": "Tecnología",
                "price": 80,
                "stock": 20,
                "is_active": True
            }
        ]

        bulk_index_products(client, products)

        results = search_products(client, "laptop")

        for result in results:
            print(
                result["id"],
                result["name"],
                result["price"],
                result["score"]
            )

    except OpenSearchException as error:
        print("Error de OpenSearch:", error)


if __name__ == "__main__":
    main()
```

## Ejemplo integrado asíncrono

```python
import asyncio
import os

from dotenv import load_dotenv
from opensearchpy import AsyncOpenSearch

load_dotenv()

INDEX_NAME = "products"


async def create_client():
    return AsyncOpenSearch(
        hosts=[
            {
                "host": os.getenv("OPENSEARCH_HOST", "localhost"),
                "port": int(os.getenv("OPENSEARCH_PORT", "9200"))
            }
        ],
        http_auth=(
            os.getenv("OPENSEARCH_USER"),
            os.getenv("OPENSEARCH_PASSWORD")
        ),
        use_ssl=os.getenv("OPENSEARCH_USE_SSL", "false") == "true",
        verify_certs=os.getenv("OPENSEARCH_VERIFY_CERTS", "false") == "true",
        ssl_show_warn=False
    )


async def create_index(client):
    exists = await client.indices.exists(index=INDEX_NAME)

    if exists:
        return

    await client.indices.create(
        index=INDEX_NAME,
        body={
            "mappings": {
                "properties": {
                    "sku": {"type": "keyword"},
                    "name": {"type": "text"},
                    "category": {"type": "keyword"},
                    "price": {"type": "double"},
                    "stock": {"type": "integer"},
                    "is_active": {"type": "boolean"}
                }
            }
        }
    )


async def index_product(client, product):
    await client.index(
        index=INDEX_NAME,
        id=product["sku"],
        body=product
    )


async def search_products(client, query):
    response = await client.search(
        index=INDEX_NAME,
        body={
            "query": {
                "match": {
                    "name": query
                }
            },
            "size": 10
        }
    )

    return [
        {
            "id": hit["_id"],
            "score": hit["_score"],
            **hit["_source"]
        }
        for hit in response["hits"]["hits"]
    ]


async def main():
    client = await create_client()

    try:
        await create_index(client)

        await index_product(
            client,
            {
                "sku": "LAP-001",
                "name": "Laptop",
                "category": "Tecnología",
                "price": 3500,
                "stock": 5,
                "is_active": True
            }
        )

        results = await search_products(client, "laptop")

        for result in results:
            print(result)

    finally:
        await client.close()


asyncio.run(main())
```

## Relación con otras librerías

`opensearch-py` se relaciona especialmente con:

```text
fastapi
flask
pandas
pydantic
python-dotenv
loguru
pytest
```

## Relación con FastAPI

Puede usarse como motor de búsqueda para endpoints de consulta.

```text
API -> OpenSearch -> resultados buscables
```

## Relación con pandas

Pandas puede alimentar cargas hacia OpenSearch o recibir resultados de búsqueda para análisis.

## Relación con Pydantic

Pydantic puede validar documentos antes de indexarlos.

```python
product.model_dump()
```

## Relación con bases relacionales

OpenSearch suele usarse junto con bases relacionales.

```text
PostgreSQL / MySQL -> fuente de verdad
OpenSearch         -> búsqueda y agregaciones rápidas
```

## Relación con MongoDB

MongoDB almacena documentos como base principal documental.

OpenSearch indexa documentos para búsqueda avanzada.

Pueden coexistir:

```text
MongoDB     -> almacenamiento principal
OpenSearch  -> búsqueda textual y filtros rápidos
```

## Relación con Elasticsearch

OpenSearch y Elasticsearch tienen similitudes conceptuales y de uso, pero no deben tratarse como la misma plataforma sin revisar compatibilidad.

Para OpenSearch:

```python
from opensearchpy import OpenSearch
```

Para Elasticsearch:

```python
from elasticsearch import Elasticsearch
```

## Orden didáctico interno

```text
1. Propósito de opensearch-py
2. Instalación
3. Importación con opensearchpy
4. Conexión con OpenSearch
5. Índices
6. Documentos
7. index(), get(), update() y delete()
8. search()
9. match_all, match, term, range y bool
10. Ordenamiento, paginación y _source
11. Mapping
12. keyword vs text
13. Bulk indexing
14. Agregaciones
15. Fechas
16. Delete by query y update by query
17. API asíncrona
18. Uso con FastAPI, Flask y pandas
19. Organización recomendada
20. Errores comunes
21. Buenas prácticas
```