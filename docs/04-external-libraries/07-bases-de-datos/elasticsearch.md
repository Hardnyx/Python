# `elasticsearch`

## Propósito

`elasticsearch` es una librería externa para conectar Python con Elasticsearch.

Elasticsearch es un motor de búsqueda y análisis distribuido. Se utiliza para indexar documentos, ejecutar búsquedas de texto, filtrar información, calcular agregaciones, consultar logs, construir buscadores internos, analizar eventos y trabajar con datos semiestructurados orientados a búsqueda.

La librería `elasticsearch` permite enviar consultas desde Python hacia un clúster Elasticsearch.

## Naturaleza de la librería

`elasticsearch` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install elasticsearch
```

Importación principal:

```python
from elasticsearch import Elasticsearch
```

Para uso asíncrono:

```python
from elasticsearch import AsyncElasticsearch
```

Para operaciones masivas:

```python
from elasticsearch.helpers import bulk
```

## Relación con Elasticsearch

Elasticsearch organiza la información en índices y documentos.

Equivalencia conceptual aproximada:

```text
Elasticsearch index    -> agrupación de documentos
Elasticsearch document -> registro JSON
field                  -> campo del documento
query                  -> consulta de búsqueda
aggregation            -> cálculo resumido sobre documentos
```

La comparación con una base relacional es solo conceptual.

Elasticsearch no está pensado principalmente como una base transaccional clásica, sino como un motor de búsqueda, filtrado y análisis.

## Idea central

La idea principal de `elasticsearch` es crear un cliente y usarlo para comunicarse con un servidor o clúster Elasticsearch.

Flujo típico:

```text
Python -> Elasticsearch client -> Elasticsearch cluster -> índices y documentos
```

Ejemplo mínimo:

```python
from elasticsearch import Elasticsearch

client = Elasticsearch("http://localhost:9200")

response = client.info()

print(response)
```

## Cuándo usar Elasticsearch

Conviene usar Elasticsearch cuando se necesita:

```text
búsqueda de texto
búsqueda tolerante a coincidencias parciales
filtros combinados
autocompletado
análisis de logs
consultas sobre eventos
agregaciones rápidas
monitoreo
exploración de documentos JSON
ranking de resultados
búsqueda por relevancia
```

## Cuándo no usar Elasticsearch

No suele ser la mejor opción cuando se necesita:

```text
transacciones relacionales fuertes
integridad referencial clásica
joins frecuentes
base principal para operaciones bancarias o contables
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

Elasticsearch suele complementar a una base principal, no reemplazarla automáticamente.

## Instalación

Instalación básica:

```bash
python -m pip install elasticsearch
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
elasticsearch==9.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import elasticsearch

print(elasticsearch.__version__)
```

## Conexión básica local

```python
from elasticsearch import Elasticsearch

client = Elasticsearch(
    "http://localhost:9200"
)

response = client.info()

print(response)
```

## Conexión con usuario y contraseña

```python
from elasticsearch import Elasticsearch

client = Elasticsearch(
    "https://localhost:9200",
    basic_auth=("elastic", "password"),
    verify_certs=False
)
```

`verify_certs=False` puede usarse en entornos locales de prueba, pero no debe usarse como práctica general en producción.

## Conexión con API key

```python
from elasticsearch import Elasticsearch

client = Elasticsearch(
    "https://example.es.region.cloud.elastic-cloud.com",
    api_key="api-key-value"
)
```

En proyectos reales, la API key debe venir de variables de entorno.

## Variables de entorno

Menos recomendable:

```python
client = Elasticsearch(
    "https://example.com",
    api_key="secret"
)
```

Más conveniente:

```python
import os

from elasticsearch import Elasticsearch

client = Elasticsearch(
    os.getenv("ELASTICSEARCH_URL"),
    api_key=os.getenv("ELASTICSEARCH_API_KEY")
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
ELASTICSEARCH_URL=http://localhost:9200
ELASTICSEARCH_API_KEY=secret
```

Código:

```python
import os

from dotenv import load_dotenv
from elasticsearch import Elasticsearch

load_dotenv()

client = Elasticsearch(
    os.getenv("ELASTICSEARCH_URL", "http://localhost:9200"),
    api_key=os.getenv("ELASTICSEARCH_API_KEY")
)
```

## Verificar conexión

```python
from elasticsearch import Elasticsearch

client = Elasticsearch("http://localhost:9200")

if client.ping():
    print("Conexión correcta")
else:
    print("No se pudo conectar con Elasticsearch")
```

También puede consultarse información del clúster:

```python
response = client.info()

print(response)
```

## Índice

Un índice agrupa documentos.

Ejemplo conceptual:

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
    document=document
)
```

## Indexar documento

```python
response = client.index(
    index="products",
    id="LAP-001",
    document={
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
    doc={
        "price": 3600,
        "stock": 4
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
    doc={
        "is_active": False
    }
)
```

Consulta de activos:

```python
response = client.search(
    index="products",
    query={
        "term": {
            "is_active": True
        }
    }
)
```

## Búsqueda básica

```python
response = client.search(
    index="products",
    query={
        "match_all": {}
    }
)

for hit in response["hits"]["hits"]:
    print(hit["_source"])
```

## `match_all`

Devuelve documentos sin aplicar filtro.

```python
query = {
    "match_all": {}
}
```

Uso:

```python
client.search(
    index="products",
    query=query
)
```

## Búsqueda por texto

```python
response = client.search(
    index="products",
    query={
        "match": {
            "name": "laptop"
        }
    }
)
```

`match` se usa para búsqueda de texto analizado.

## Búsqueda exacta con `term`

```python
response = client.search(
    index="products",
    query={
        "term": {
            "sku": "LAP-001"
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
    query={
        "range": {
            "price": {
                "gte": 100,
                "lte": 5000
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
    query={
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
    query={
        "match_all": {}
    },
    sort=[
        {
            "price": {
                "order": "desc"
            }
        }
    ]
)
```

## Limitar resultados

```python
response = client.search(
    index="products",
    query={
        "match_all": {}
    },
    size=10
)
```

## Paginación simple

```python
response = client.search(
    index="products",
    query={
        "match_all": {}
    },
    from_=0,
    size=10
)
```

En Python se usa `from_` porque `from` es una palabra reservada del lenguaje.

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
    query={
        "match_all": {}
    },
    source=[
        "sku",
        "name",
        "price"
    ]
)
```

Esto reduce los campos devueltos.

## Mapping

El mapping define tipos de campos y cómo se indexan.

Ejemplo:

```python
client.indices.create(
    index="products",
    mappings={
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
        mappings={
            "properties": {
                "sku": {"type": "keyword"},
                "name": {"type": "text"},
                "price": {"type": "double"},
                "stock": {"type": "integer"},
                "is_active": {"type": "boolean"}
            }
        }
    )
```

## Bulk indexing

Para indexar muchos documentos, conviene usar helpers.

```python
from elasticsearch import Elasticsearch
from elasticsearch.helpers import bulk

client = Elasticsearch("http://localhost:9200")

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
    size=0,
    aggs={
        "by_category": {
            "terms": {
                "field": "category"
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
    size=0,
    aggs={
        "total_stock": {
            "sum": {
                "field": "stock"
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
    size=0,
    aggs={
        "average_price": {
            "avg": {
                "field": "price"
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
    document={
        "event_type": "created",
        "created_at": datetime.now(timezone.utc).isoformat()
    }
)
```

Consulta por rango de fechas:

```python
response = client.search(
    index="events",
    query={
        "range": {
            "created_at": {
                "gte": "2026-01-01",
                "lt": "2027-01-01"
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
    document={
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
    query={
        "term": {
            "is_active": True
        }
    }
)

print(response["count"])
```

## Delete by query

```python
client.delete_by_query(
    index="products",
    query={
        "term": {
            "is_active": False
        }
    }
)
```

Debe usarse con cuidado porque puede eliminar muchos documentos.

## Update by query

```python
client.update_by_query(
    index="products",
    query={
        "term": {
            "category": "old"
        }
    },
    script={
        "source": "ctx._source.category = params.category",
        "params": {
            "category": "new"
        }
    }
)
```

Debe probarse cuidadosamente antes de aplicarse sobre datos importantes.

## Scroll y búsqueda profunda

Para recorrer grandes conjuntos de resultados, existen mecanismos como scroll o alternativas modernas según el caso.

En muchos escenarios nuevos conviene evaluar paginación con `search_after` para búsquedas profundas.

Ejemplo conceptual con `search_after`:

```python
response = client.search(
    index="products",
    query={
        "match_all": {}
    },
    sort=[
        {"sku": "asc"}
    ],
    size=100
)
```

Luego se toma el último valor de `sort` para pedir la siguiente página.

## Manejo de errores

Importaciones frecuentes:

```python
from elasticsearch import ElasticsearchException
from elasticsearch import NotFoundError
from elasticsearch import ConflictError
from elasticsearch import AuthenticationException
from elasticsearch import ConnectionError
```

Ejemplo:

```python
from elasticsearch import ElasticsearchException

try:
    response = client.search(
        index="products",
        query={
            "match_all": {}
        }
    )
except ElasticsearchException as error:
    print("Error de Elasticsearch:", error)
```

## Documento no encontrado

```python
from elasticsearch import NotFoundError

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
from elasticsearch import ConnectionError

try:
    client.info()
except ConnectionError:
    print("No se pudo conectar con Elasticsearch")
```

## API asíncrona

Importación:

```python
from elasticsearch import AsyncElasticsearch
```

Ejemplo:

```python
import asyncio

from elasticsearch import AsyncElasticsearch


async def main():
    client = AsyncElasticsearch(
        "http://localhost:9200"
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
    document={
        "name": "Laptop",
        "price": 3500
    }
)
```

## Buscar de forma asíncrona

```python
response = await client.search(
    index="products",
    query={
        "match": {
            "name": "laptop"
        }
    }
)
```

## Cerrar cliente asíncrono

```python
await client.close()
```

En aplicaciones web asíncronas, el cliente suele crearse al iniciar la aplicación y cerrarse al finalizar.

## Bulk asíncrono

```python
from elasticsearch import AsyncElasticsearch
from elasticsearch.helpers import async_bulk

client = AsyncElasticsearch("http://localhost:9200")

actions = [
    {
        "_index": "products",
        "_id": "LAP-001",
        "_source": {
            "sku": "LAP-001",
            "name": "Laptop",
            "price": 3500
        }
    }
]

success, errors = await async_bulk(
    client,
    actions,
    raise_on_error=False
)
```

## Uso con FastAPI

```python
import os
from contextlib import asynccontextmanager

from elasticsearch import AsyncElasticsearch
from fastapi import FastAPI, HTTPException


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.search_client = AsyncElasticsearch(
        os.getenv("ELASTICSEARCH_URL", "http://localhost:9200"),
        api_key=os.getenv("ELASTICSEARCH_API_KEY")
    )

    yield

    await app.state.search_client.close()


app = FastAPI(lifespan=lifespan)


@app.get("/products/search")
async def search_products(q: str):
    response = await app.state.search_client.search(
        index="products",
        query={
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

from elasticsearch import Elasticsearch
from flask import Flask, request

app = Flask(__name__)

client = Elasticsearch(
    os.getenv("ELASTICSEARCH_URL", "http://localhost:9200"),
    api_key=os.getenv("ELASTICSEARCH_API_KEY")
)


@app.route("/products/search")
def search_products():
    query = request.args.get("q", "")

    response = client.search(
        index="products",
        query={
            "match": {
                "name": query
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

Elasticsearch puede alimentar un `DataFrame`.

```python
import pandas as pd

response = client.search(
    index="products",
    query={
        "match_all": {}
    },
    size=100
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
from elasticsearch.helpers import bulk

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
Elasticsearch                -> búsqueda y análisis
```

Flujo conceptual:

```text
1. Guardar dato principal en base transaccional.
2. Indexar copia útil para búsqueda en Elasticsearch.
3. Consultar Elasticsearch para búsqueda rápida.
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
from elasticsearch import Elasticsearch

load_dotenv()


def create_search_client():
    return Elasticsearch(
        os.getenv("ELASTICSEARCH_URL", "http://localhost:9200"),
        api_key=os.getenv("ELASTICSEARCH_API_KEY")
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
        mappings={
            "properties": {
                "sku": {"type": "keyword"},
                "name": {"type": "text"},
                "category": {"type": "keyword"},
                "price": {"type": "double"},
                "stock": {"type": "integer"},
                "is_active": {"type": "boolean"}
            }
        }
    )


def index_product(client, product):
    client.index(
        index="products",
        id=product["sku"],
        document=product
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
        query={
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
search.py        -> creación del cliente Elasticsearch
repositories.py  -> consultas e indexación
schemas.py       -> validación de entrada
services.py      -> reglas de negocio
main.py          -> aplicación o punto de entrada
```

## Errores comunes

## Usar Elasticsearch como base relacional principal

Elasticsearch es excelente para búsqueda, pero no reemplaza automáticamente una base transaccional.

## No cerrar el cliente asíncrono

Problemático:

```python
client = AsyncElasticsearch(...)
```

sin:

```python
await client.close()
```

## No definir mapping cuando importa la precisión

Si se deja que Elasticsearch infiera tipos, puede crear mappings que no coinciden con lo esperado.

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
size=100000
```

Mejor usar paginación adecuada, scroll o `search_after`, según el caso.

## Concatenar valores en consultas

Problemático:

```python
query = {
    "match": {
        "name": user_input
    }
}
```

Esto no es SQL injection en sentido clásico, pero igual debe validarse entrada cuando afecte lógica, campos, índices o filtros.

## Construir nombres de índices desde entrada libre

Problemático:

```python
client.search(index=user_input, ...)
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
ElasticsearchException
```

## Usar `refresh=True` en exceso

Puede ser útil en pruebas, pero afectar rendimiento en producción.

## Indexar documentos enormes sin controlar campos

Documentos muy grandes pueden afectar rendimiento de indexación, búsqueda y almacenamiento.

Conviene indexar solo lo necesario para búsqueda.

## Buenas prácticas

## Usar variables de entorno

```text
ELASTICSEARCH_URL
ELASTICSEARCH_API_KEY
```

## Definir mappings para índices importantes

```python
client.indices.create(
    index="products",
    mappings={...}
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
from elasticsearch.helpers import bulk
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

## Usar Elasticsearch como buscador complementario

Mantener datos críticos en una fuente principal cuando se requieran transacciones fuertes.

## Ejemplo integrado

```python
import os

from dotenv import load_dotenv
from elasticsearch import Elasticsearch
from elasticsearch import ElasticsearchException
from elasticsearch.helpers import bulk

load_dotenv()

INDEX_NAME = "products"


def create_client():
    return Elasticsearch(
        os.getenv("ELASTICSEARCH_URL", "http://localhost:9200"),
        api_key=os.getenv("ELASTICSEARCH_API_KEY")
    )


def create_index(client):
    if client.indices.exists(index=INDEX_NAME):
        return

    client.indices.create(
        index=INDEX_NAME,
        mappings={
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
    )


def index_product(client, product):
    client.index(
        index=INDEX_NAME,
        id=product["sku"],
        document=product
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
        query={
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
        size=limit
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
        doc={
            "is_active": False
        }
    )


def main():
    client = create_client()

    try:
        if not client.ping():
            raise RuntimeError("No se pudo conectar con Elasticsearch")

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

    except ElasticsearchException as error:
        print("Error de Elasticsearch:", error)


if __name__ == "__main__":
    main()
```

## Ejemplo integrado asíncrono

```python
import asyncio
import os

from dotenv import load_dotenv
from elasticsearch import AsyncElasticsearch
from elasticsearch.helpers import async_bulk

load_dotenv()

INDEX_NAME = "products"


async def create_client():
    return AsyncElasticsearch(
        os.getenv("ELASTICSEARCH_URL", "http://localhost:9200"),
        api_key=os.getenv("ELASTICSEARCH_API_KEY")
    )


async def create_index(client):
    exists = await client.indices.exists(index=INDEX_NAME)

    if exists:
        return

    await client.indices.create(
        index=INDEX_NAME,
        mappings={
            "properties": {
                "sku": {"type": "keyword"},
                "name": {"type": "text"},
                "category": {"type": "keyword"},
                "price": {"type": "double"},
                "stock": {"type": "integer"},
                "is_active": {"type": "boolean"}
            }
        }
    )


async def bulk_index_products(client, products):
    async def generate_actions():
        for product in products:
            yield {
                "_index": INDEX_NAME,
                "_id": product["sku"],
                "_source": product
            }

    return await async_bulk(
        client,
        generate_actions(),
        raise_on_error=False
    )


async def search_products(client, query):
    response = await client.search(
        index=INDEX_NAME,
        query={
            "match": {
                "name": query
            }
        },
        size=10
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

        await bulk_index_products(client, products)

        results = await search_products(client, "laptop")

        for result in results:
            print(result)

    finally:
        await client.close()


asyncio.run(main())
```

## Relación con otras librerías

`elasticsearch` se relaciona especialmente con:

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
API -> Elasticsearch -> resultados buscables
```

## Relación con pandas

Pandas puede alimentar cargas hacia Elasticsearch o recibir resultados de búsqueda para análisis.

## Relación con Pydantic

Pydantic puede validar documentos antes de indexarlos.

```python
product.model_dump()
```

## Relación con bases relacionales

Elasticsearch suele usarse junto con bases relacionales.

```text
PostgreSQL / MySQL -> fuente de verdad
Elasticsearch      -> búsqueda y agregaciones rápidas
```

## Relación con MongoDB

MongoDB almacena documentos como base principal documental.

Elasticsearch indexa documentos para búsqueda avanzada.

Pueden coexistir:

```text
MongoDB        -> almacenamiento principal
Elasticsearch  -> búsqueda textual y filtros rápidos
```

## Orden didáctico interno

```text
1. Propósito de elasticsearch
2. Instalación
3. Conexión con Elasticsearch
4. Índices
5. Documentos
6. index(), get(), update() y delete()
7. search()
8. match_all, match, term, range y bool
9. Ordenamiento, paginación y _source
10. Mapping
11. keyword vs text
12. Bulk indexing
13. Agregaciones
14. Fechas
15. Delete by query y update by query
16. API asíncrona
17. Uso con FastAPI, Flask y pandas
18. Organización recomendada
19. Errores comunes
20. Buenas prácticas
```