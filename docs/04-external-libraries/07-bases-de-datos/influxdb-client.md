# `influxdb-client`

## Propósito

`influxdb-client` es una librería externa para conectar Python con InfluxDB.

InfluxDB es una base de datos orientada a series temporales. Se utiliza para guardar, consultar y analizar datos que evolucionan en el tiempo, como métricas, sensores, eventos, logs, precios, monitoreo de sistemas, telemetría y datos financieros con marca temporal.

La librería permite escribir puntos de datos, consultar series temporales, trabajar con buckets, organizaciones, tokens, line protocol y consultas Flux.

## Naturaleza de la librería

`influxdb-client` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install influxdb-client
```

La importación habitual es:

```python
import influxdb_client
```

También se importan clases específicas:

```python
from influxdb_client import InfluxDBClient
from influxdb_client import Point
from influxdb_client.client.write_api import SYNCHRONOUS
```

## Relación con InfluxDB

InfluxDB está diseñado para datos temporales.

Conceptos principales:

```text
bucket
organization
token
measurement
tag
field
timestamp
point
line protocol
```

Equivalencia conceptual aproximada:

```text
bucket      -> contenedor de datos
measurement -> grupo lógico de mediciones
tag         -> metadato indexado
field       -> valor medido
timestamp   -> momento del dato
point       -> observación temporal
```

## InfluxDB 2.x e InfluxDB 3.x

`influxdb-client` se usa principalmente con InfluxDB 2.x.

Para InfluxDB 3.x existe otra librería:

```bash
python -m pip install influxdb3-python
```

Importación de InfluxDB 3:

```python
from influxdb_client_3 import InfluxDBClient3
```

Por tanto, antes de elegir cliente, debe revisarse qué versión de InfluxDB se usará.

## Idea central

La idea principal de `influxdb-client` es crear un cliente, escribir puntos temporales y consultarlos luego con Flux.

Flujo típico:

```text
Python -> InfluxDBClient -> write_api / query_api -> InfluxDB
```

Ejemplo mínimo:

```python
from influxdb_client import InfluxDBClient, Point
from influxdb_client.client.write_api import SYNCHRONOUS

client = InfluxDBClient(
    url="http://localhost:8086",
    token="my-token",
    org="my-org"
)

try:
    write_api = client.write_api(write_options=SYNCHRONOUS)

    point = (
        Point("temperature")
        .tag("location", "office")
        .field("value", 25.3)
    )

    write_api.write(
        bucket="my-bucket",
        org="my-org",
        record=point
    )
finally:
    client.close()
```

## Cuándo usar InfluxDB

Conviene usar InfluxDB cuando se necesita:

```text
guardar series temporales
registrar métricas
monitorear sistemas
almacenar datos de sensores
analizar eventos con timestamp
consultar datos por rangos de tiempo
calcular medias móviles
agrupar datos por ventanas temporales
trabajar con telemetría
analizar logs numéricos o métricas
```

## Cuándo no usar InfluxDB

No suele ser la mejor opción cuando se necesita:

```text
modelo relacional clásico
joins frecuentes
transacciones complejas
integridad referencial fuerte
datos sin componente temporal importante
almacenamiento documental flexible
búsqueda textual avanzada
grafos de relaciones
```

Para esos casos pueden corresponder:

```text
PostgreSQL
MySQL
SQLite
MongoDB
Elasticsearch
Neo4j
DuckDB
```

## Instalación

Instalación básica:

```bash
python -m pip install influxdb-client
```

Instalación con soporte adicional para pandas:

```bash
python -m pip install "influxdb-client[extras]"
```

Instalación con soporte asíncrono:

```bash
python -m pip install "influxdb-client[async]"
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
influxdb-client==1.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import influxdb_client

print(influxdb_client.__version__)
```

## Conexión básica

```python
from influxdb_client import InfluxDBClient

client = InfluxDBClient(
    url="http://localhost:8086",
    token="my-token",
    org="my-org"
)

client.close()
```

## Parámetros principales

```text
url   -> dirección de InfluxDB
token -> token de autenticación
org   -> organización
bucket -> contenedor donde se escriben o consultan datos
```

Ejemplo:

```python
url = "http://localhost:8086"
token = "my-token"
org = "my-org"
bucket = "metrics"
```

## Variables de entorno

No conviene escribir tokens directamente en el código.

Menos recomendable:

```python
client = InfluxDBClient(
    url="http://localhost:8086",
    token="my-token",
    org="my-org"
)
```

Más conveniente:

```python
import os

from influxdb_client import InfluxDBClient

client = InfluxDBClient(
    url=os.getenv("INFLUXDB_URL"),
    token=os.getenv("INFLUXDB_TOKEN"),
    org=os.getenv("INFLUXDB_ORG")
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
INFLUXDB_URL=http://localhost:8086
INFLUXDB_TOKEN=my-token
INFLUXDB_ORG=my-org
INFLUXDB_BUCKET=metrics
```

Código:

```python
import os

from dotenv import load_dotenv
from influxdb_client import InfluxDBClient

load_dotenv()

client = InfluxDBClient(
    url=os.getenv("INFLUXDB_URL"),
    token=os.getenv("INFLUXDB_TOKEN"),
    org=os.getenv("INFLUXDB_ORG")
)
```

## Cerrar cliente

```python
client.close()
```

Cerrar el cliente libera recursos de conexión.

Patrón recomendado:

```python
client = InfluxDBClient(
    url=url,
    token=token,
    org=org
)

try:
    ...
finally:
    client.close()
```

## Verificar conexión

```python
health = client.health()

print(health.status)
```

También puede usarse para validar que el servidor responde antes de ejecutar operaciones principales.

## Point

Un `Point` representa una observación temporal.

```python
from influxdb_client import Point

point = (
    Point("temperature")
    .tag("location", "office")
    .field("value", 25.3)
)
```

Partes:

```text
temperature -> measurement
location    -> tag
value       -> field
25.3        -> valor medido
```

## Measurement

El measurement agrupa datos del mismo tipo.

Ejemplos:

```text
temperature
cpu
memory
price
transaction_metric
sensor_reading
```

Uso:

```python
Point("temperature")
```

## Tags

Los tags son metadatos indexados.

```python
Point("temperature").tag("location", "office")
```

Ejemplos:

```text
location
host
device
region
symbol
portfolio
```

Los tags son útiles para filtrar y agrupar.

## Fields

Los fields contienen los valores medidos.

```python
Point("temperature").field("value", 25.3)
```

Ejemplos:

```text
value
price
volume
usage
latency
count
```

## Timestamp

Un punto puede llevar timestamp explícito.

```python
from datetime import datetime, timezone
from influxdb_client import Point

point = (
    Point("temperature")
    .tag("location", "office")
    .field("value", 25.3)
    .time(datetime.now(timezone.utc))
)
```

Si no se indica tiempo, InfluxDB puede asignar el tiempo de escritura.

## Escribir datos

```python
from influxdb_client.client.write_api import SYNCHRONOUS

write_api = client.write_api(
    write_options=SYNCHRONOUS
)

write_api.write(
    bucket=bucket,
    org=org,
    record=point
)
```

## Escritura síncrona

```python
write_api = client.write_api(
    write_options=SYNCHRONOUS
)
```

Este modo espera a que la escritura termine.

Es útil para scripts, pruebas y ejemplos simples.

## Escritura por lotes

La escritura por lotes permite enviar varios puntos.

```python
points = [
    Point("temperature")
    .tag("location", "office")
    .field("value", 25.3),
    Point("temperature")
    .tag("location", "warehouse")
    .field("value", 21.8),
]

write_api.write(
    bucket=bucket,
    org=org,
    record=points
)
```

## Escritura con diccionarios

También puede escribirse usando estructuras compatibles.

```python
data = {
    "measurement": "temperature",
    "tags": {
        "location": "office"
    },
    "fields": {
        "value": 25.3
    }
}

write_api.write(
    bucket=bucket,
    org=org,
    record=data
)
```

## Line protocol

Line protocol es el formato textual de InfluxDB para escribir datos.

Ejemplo conceptual:

```text
temperature,location=office value=25.3
```

Uso desde Python:

```python
line = "temperature,location=office value=25.3"

write_api.write(
    bucket=bucket,
    org=org,
    record=line
)
```

## Escribir múltiples líneas

```python
lines = [
    "temperature,location=office value=25.3",
    "temperature,location=warehouse value=21.8",
]

write_api.write(
    bucket=bucket,
    org=org,
    record=lines
)
```

## Diseño de measurements

Los measurements deben representar una familia de mediciones.

Ejemplos:

```text
cpu
memory
temperature
prices
trades
api_latency
```

No conviene crear un measurement distinto para cada entidad si esa entidad puede ser un tag.

Menos conveniente:

```text
temperature_office
temperature_warehouse
```

Más conveniente:

```text
measurement: temperature
tag: location=office
tag: location=warehouse
```

## Diseño de tags

Los tags deben usarse para dimensiones de consulta frecuente.

Ejemplos:

```text
location
host
device
symbol
service
region
```

No conviene guardar como tag valores de cardinalidad excesivamente alta sin evaluar el impacto.

Ejemplos riesgosos:

```text
request_id
unique_user_id
transaction_id
uuid
```

## Diseño de fields

Los fields deben contener valores medidos.

Ejemplos:

```text
temperature
price
amount
latency
cpu_usage
memory_usage
count
```

Los fields no se indexan de la misma forma que los tags.

Por eso, si un dato se usará constantemente para filtrar, puede convenir que sea tag.

## Consultar datos con Flux

Para consultar se usa `query_api`.

```python
query_api = client.query_api()
```

Consulta básica:

```python
query = """
from(bucket: "metrics")
    |> range(start: -1h)
    |> filter(fn: (r) => r._measurement == "temperature")
"""

tables = query_api.query(
    query=query,
    org=org
)

for table in tables:
    for record in table.records:
        print(record.get_time(), record.get_value())
```

## `from()`

Indica el bucket.

```flux
from(bucket: "metrics")
```

## `range()`

Define el rango temporal.

```flux
|> range(start: -1h)
```

Ejemplos:

```text
-15m
-1h
-24h
-7d
```

## `filter()`

Filtra registros.

```flux
|> filter(fn: (r) => r._measurement == "temperature")
```

Filtrar por tag:

```flux
|> filter(fn: (r) => r.location == "office")
```

Filtrar por field:

```flux
|> filter(fn: (r) => r._field == "value")
```

## Consultar último valor

```python
query = """
from(bucket: "metrics")
    |> range(start: -24h)
    |> filter(fn: (r) => r._measurement == "temperature")
    |> filter(fn: (r) => r.location == "office")
    |> last()
"""
```

## Consultar promedio

```python
query = """
from(bucket: "metrics")
    |> range(start: -24h)
    |> filter(fn: (r) => r._measurement == "temperature")
    |> filter(fn: (r) => r._field == "value")
    |> mean()
"""
```

## Agrupar por ventanas temporales

```python
query = """
from(bucket: "metrics")
    |> range(start: -24h)
    |> filter(fn: (r) => r._measurement == "temperature")
    |> aggregateWindow(every: 1h, fn: mean, createEmpty: false)
"""
```

Este patrón es común para resumir series temporales.

## Convertir resultado a lista de diccionarios

```python
def query_to_dicts(tables):
    rows = []

    for table in tables:
        for record in table.records:
            rows.append({
                "time": record.get_time(),
                "measurement": record.get_measurement(),
                "field": record.get_field(),
                "value": record.get_value(),
                "location": record.values.get("location")
            })

    return rows
```

Uso:

```python
tables = query_api.query(
    query=query,
    org=org
)

rows = query_to_dicts(tables)
```

## Consultar como DataFrame

Con dependencias adicionales, puede consultarse como DataFrame.

```python
query_api = client.query_api()

df = query_api.query_data_frame(
    """
    from(bucket: "metrics")
        |> range(start: -24h)
        |> filter(fn: (r) => r._measurement == "temperature")
    """
)

print(df.head())
```

Este enfoque es útil para análisis con pandas.

## Escribir desde pandas

```python
import pandas as pd

df = pd.DataFrame({
    "location": ["office", "warehouse"],
    "value": [25.3, 21.8]
})
```

Puede transformarse a puntos:

```python
points = []

for row in df.to_dict(orient="records"):
    point = (
        Point("temperature")
        .tag("location", row["location"])
        .field("value", row["value"])
    )

    points.append(point)

write_api.write(
    bucket=bucket,
    org=org,
    record=points
)
```

## Buckets

Un bucket es donde se guardan los datos.

Ejemplos:

```text
metrics
logs
prices
sensors
events
```

El bucket puede tener políticas de retención según la configuración de InfluxDB.

## Crear bucket

La creación de buckets se puede hacer con APIs de administración.

```python
buckets_api = client.buckets_api()

bucket = buckets_api.create_bucket(
    bucket_name="metrics",
    org=org
)
```

En muchos proyectos, los buckets se crean desde la interfaz o configuración de InfluxDB y el código solo escribe y consulta.

## Listar buckets

```python
buckets_api = client.buckets_api()

buckets = buckets_api.find_buckets().buckets

for bucket_item in buckets:
    print(bucket_item.name)
```

## Eliminar bucket

```python
buckets_api = client.buckets_api()

bucket_item = buckets_api.find_bucket_by_name("metrics")

if bucket_item is not None:
    buckets_api.delete_bucket(bucket_item)
```

Debe usarse con cuidado porque puede eliminar datos.

## Retención

La retención define cuánto tiempo se conservan los datos.

Ejemplos conceptuales:

```text
7 días
30 días
1 año
sin expiración automática
```

En series temporales, la retención es importante porque los datos pueden crecer rápidamente.

## Uso con FastAPI

```python
import os
from contextlib import asynccontextmanager

from fastapi import FastAPI
from influxdb_client import InfluxDBClient, Point
from influxdb_client.client.write_api import SYNCHRONOUS


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.influx_client = InfluxDBClient(
        url=os.getenv("INFLUXDB_URL"),
        token=os.getenv("INFLUXDB_TOKEN"),
        org=os.getenv("INFLUXDB_ORG")
    )

    app.state.write_api = app.state.influx_client.write_api(
        write_options=SYNCHRONOUS
    )

    yield

    app.state.influx_client.close()


app = FastAPI(lifespan=lifespan)


@app.post("/metrics/temperature")
def write_temperature(location: str, value: float):
    point = (
        Point("temperature")
        .tag("location", location)
        .field("value", value)
    )

    app.state.write_api.write(
        bucket=os.getenv("INFLUXDB_BUCKET"),
        org=os.getenv("INFLUXDB_ORG"),
        record=point
    )

    return {
        "stored": True
    }


@app.get("/metrics/temperature")
def read_temperature():
    query = f"""
    from(bucket: "{os.getenv("INFLUXDB_BUCKET")}")
        |> range(start: -1h)
        |> filter(fn: (r) => r._measurement == "temperature")
    """

    tables = app.state.influx_client.query_api().query(
        query=query,
        org=os.getenv("INFLUXDB_ORG")
    )

    rows = []

    for table in tables:
        for record in table.records:
            rows.append({
                "time": record.get_time(),
                "value": record.get_value(),
                "location": record.values.get("location")
            })

    return rows
```

## Uso con scripts

```python
import os
from datetime import datetime, timezone

from dotenv import load_dotenv
from influxdb_client import InfluxDBClient, Point
from influxdb_client.client.write_api import SYNCHRONOUS

load_dotenv()

url = os.getenv("INFLUXDB_URL")
token = os.getenv("INFLUXDB_TOKEN")
org = os.getenv("INFLUXDB_ORG")
bucket = os.getenv("INFLUXDB_BUCKET")

client = InfluxDBClient(
    url=url,
    token=token,
    org=org
)

try:
    write_api = client.write_api(
        write_options=SYNCHRONOUS
    )

    point = (
        Point("temperature")
        .tag("location", "office")
        .field("value", 25.3)
        .time(datetime.now(timezone.utc))
    )

    write_api.write(
        bucket=bucket,
        org=org,
        record=point
    )

    query = f"""
    from(bucket: "{bucket}")
        |> range(start: -1h)
        |> filter(fn: (r) => r._measurement == "temperature")
    """

    tables = client.query_api().query(
        query=query,
        org=org
    )

    for table in tables:
        for record in table.records:
            print(
                record.get_time(),
                record.values.get("location"),
                record.get_value()
            )

finally:
    client.close()
```

## API asíncrona

Con instalación compatible, puede usarse cliente asíncrono.

```python
from influxdb_client.client.influxdb_client_async import InfluxDBClientAsync
```

Ejemplo conceptual:

```python
import asyncio

from influxdb_client import Point
from influxdb_client.client.influxdb_client_async import InfluxDBClientAsync


async def main():
    client = InfluxDBClientAsync(
        url="http://localhost:8086",
        token="my-token",
        org="my-org"
    )

    try:
        write_api = client.write_api()

        point = (
            Point("temperature")
            .tag("location", "office")
            .field("value", 25.3)
        )

        await write_api.write(
            bucket="metrics",
            org="my-org",
            record=point
        )

    finally:
        await client.close()


asyncio.run(main())
```

La API asíncrona debe usarse cuando la aplicación ya trabaja con `asyncio`.

## Manejo de errores

Importaciones frecuentes:

```python
from influxdb_client.rest import ApiException
```

Ejemplo:

```python
from influxdb_client.rest import ApiException

try:
    write_api.write(
        bucket=bucket,
        org=org,
        record=point
    )
except ApiException as error:
    print("Error de InfluxDB:", error)
```

## Errores frecuentes

```text
ApiException
errores de autenticación
bucket inexistente
organización incorrecta
token inválido
URL incorrecta
consulta Flux inválida
problemas de conexión
```

## Error de token

Si el token es inválido o no tiene permisos, las operaciones pueden fallar.

Conviene revisar:

```text
INFLUXDB_TOKEN
permisos de lectura
permisos de escritura
bucket correcto
org correcta
```

## Error de bucket

Si el bucket no existe, la escritura o consulta puede fallar.

Debe revisarse:

```text
nombre exacto del bucket
organización
permisos del token
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
from influxdb_client import InfluxDBClient
from influxdb_client.client.write_api import SYNCHRONOUS

load_dotenv()

INFLUXDB_ORG = os.getenv("INFLUXDB_ORG")
INFLUXDB_BUCKET = os.getenv("INFLUXDB_BUCKET")


def create_client():
    return InfluxDBClient(
        url=os.getenv("INFLUXDB_URL"),
        token=os.getenv("INFLUXDB_TOKEN"),
        org=INFLUXDB_ORG
    )


def create_write_api(client):
    return client.write_api(
        write_options=SYNCHRONOUS
    )
```

## `repositories.py`

```python
from influxdb_client import Point

from .database import INFLUXDB_BUCKET
from .database import INFLUXDB_ORG


def write_temperature(write_api, location, value):
    point = (
        Point("temperature")
        .tag("location", location)
        .field("value", value)
    )

    write_api.write(
        bucket=INFLUXDB_BUCKET,
        org=INFLUXDB_ORG,
        record=point
    )


def read_temperature(client, start="-1h"):
    query = f"""
    from(bucket: "{INFLUXDB_BUCKET}")
        |> range(start: {start})
        |> filter(fn: (r) => r._measurement == "temperature")
    """

    tables = client.query_api().query(
        query=query,
        org=INFLUXDB_ORG
    )

    rows = []

    for table in tables:
        for record in table.records:
            rows.append({
                "time": record.get_time(),
                "location": record.values.get("location"),
                "field": record.get_field(),
                "value": record.get_value()
            })

    return rows
```

## Separación por responsabilidades

```text
database.py      -> cliente, org y bucket
repositories.py  -> escrituras y consultas Flux
schemas.py       -> validación de entrada
services.py      -> reglas de negocio
main.py          -> aplicación o script
```

## Diferencia frente a bases relacionales

InfluxDB no se modela como una base relacional clásica.

En vez de pensar en:

```text
tablas
filas
joins
foreign keys
```

conviene pensar en:

```text
mediciones
tags
fields
tiempo
rangos temporales
ventanas de agregación
retención
```

## Diferencia frente a Prometheus

Prometheus se usa frecuentemente para monitoreo y scraping de métricas.

InfluxDB puede recibir datos desde distintos clientes y se usa como base de series temporales de propósito más general.

Ambas herramientas pueden aparecer en arquitecturas de monitoreo, pero tienen modelos y ecosistemas distintos.

## Diferencia frente a TimescaleDB

TimescaleDB extiende PostgreSQL para series temporales.

InfluxDB es una base especializada en series temporales.

Regla práctica:

```text
si se necesita SQL relacional + series temporales -> TimescaleDB puede ser conveniente
si se necesita base especializada en métricas y series temporales -> InfluxDB puede ser conveniente
```

## Errores comunes

## Usar el cliente incorrecto para la versión de InfluxDB

Para InfluxDB 2.x:

```bash
python -m pip install influxdb-client
```

Para InfluxDB 3.x:

```bash
python -m pip install influxdb3-python
```

## No cerrar el cliente

Menos recomendable:

```python
client = InfluxDBClient(...)
```

sin:

```python
client.close()
```

## Guardar token en el código

Problemático:

```python
token = "my-token"
```

Mejor:

```python
token = os.getenv("INFLUXDB_TOKEN")
```

## Confundir tags y fields

Los tags sirven para filtrar y agrupar eficientemente.

Los fields sirven para valores medidos.

Diseñar mal esta separación puede dificultar consultas.

## Usar tags de cardinalidad demasiado alta

Valores únicos por evento, como `request_id` o `transaction_id`, pueden generar alta cardinalidad.

Debe evaluarse antes de usarlos como tags.

## No indicar bucket u organización correctos

Las operaciones suelen requerir:

```text
bucket
org
token
```

Los tres deben corresponder al mismo entorno.

## Construir consultas Flux con entrada libre

Problemático:

```python
query = f"""
from(bucket: "{bucket}")
    |> range(start: {user_input})
"""
```

Si hay entrada del usuario, debe validarse antes de insertarse en la consulta.

## Usar InfluxDB como base transaccional principal

InfluxDB está orientado a series temporales, no a transacciones relacionales complejas.

## Consultar rangos demasiado grandes sin agregación

Puede ser costoso traer muchos puntos crudos.

Conviene usar:

```text
aggregateWindow()
rangos razonables
filtros por measurement y tags
```

## Buenas prácticas

## Usar variables de entorno

```text
INFLUXDB_URL
INFLUXDB_TOKEN
INFLUXDB_ORG
INFLUXDB_BUCKET
```

## Cerrar el cliente

```python
client.close()
```

## Usar tags para dimensiones frecuentes

```text
host
location
symbol
service
region
```

## Usar fields para valores medidos

```text
value
price
count
latency
usage
```

## Controlar cardinalidad

Evitar tags con demasiados valores únicos.

## Usar escritura por lotes para múltiples puntos

```python
write_api.write(record=points)
```

## Usar ventanas de agregación en consultas amplias

```flux
aggregateWindow(every: 1h, fn: mean)
```

## Separar consultas en repositorios

```text
repositories.py
```

## Validar entrada usada en Flux

Especialmente rangos, buckets, measurements y tags dinámicos.

## Ejemplo integrado

```python
import os
from datetime import datetime, timezone

from dotenv import load_dotenv
from influxdb_client import InfluxDBClient, Point
from influxdb_client.client.write_api import SYNCHRONOUS
from influxdb_client.rest import ApiException

load_dotenv()

INFLUXDB_URL = os.getenv("INFLUXDB_URL", "http://localhost:8086")
INFLUXDB_TOKEN = os.getenv("INFLUXDB_TOKEN")
INFLUXDB_ORG = os.getenv("INFLUXDB_ORG")
INFLUXDB_BUCKET = os.getenv("INFLUXDB_BUCKET", "metrics")


def create_client():
    return InfluxDBClient(
        url=INFLUXDB_URL,
        token=INFLUXDB_TOKEN,
        org=INFLUXDB_ORG
    )


def write_temperature(write_api, location, value):
    point = (
        Point("temperature")
        .tag("location", location)
        .field("value", value)
        .time(datetime.now(timezone.utc))
    )

    write_api.write(
        bucket=INFLUXDB_BUCKET,
        org=INFLUXDB_ORG,
        record=point
    )


def read_temperature(client, start="-1h"):
    query = f"""
    from(bucket: "{INFLUXDB_BUCKET}")
        |> range(start: {start})
        |> filter(fn: (r) => r._measurement == "temperature")
        |> filter(fn: (r) => r._field == "value")
    """

    tables = client.query_api().query(
        query=query,
        org=INFLUXDB_ORG
    )

    rows = []

    for table in tables:
        for record in table.records:
            rows.append({
                "time": record.get_time(),
                "location": record.values.get("location"),
                "value": record.get_value()
            })

    return rows


def main():
    client = create_client()

    try:
        health = client.health()

        print("Estado:", health.status)

        write_api = client.write_api(
            write_options=SYNCHRONOUS
        )

        write_temperature(
            write_api,
            location="office",
            value=25.3
        )

        write_temperature(
            write_api,
            location="warehouse",
            value=21.8
        )

        rows = read_temperature(client)

        for row in rows:
            print(
                row["time"],
                row["location"],
                row["value"]
            )

    except ApiException as error:
        print("Error de InfluxDB:", error)
    finally:
        client.close()


if __name__ == "__main__":
    main()
```

## Relación con otras librerías

`influxdb-client` se relaciona especialmente con:

```text
pandas
fastapi
python-dotenv
pydantic
pytest
asyncio
```

## Relación con pandas

Puede consultarse información como DataFrame y también transformar DataFrames en puntos para escritura.

## Relación con FastAPI

Puede usarse para registrar métricas, telemetría o eventos temporales desde endpoints.

## Relación con Pydantic

Pydantic puede validar mediciones antes de escribirlas.

```python
from pydantic import BaseModel, Field


class TemperatureMetric(BaseModel):
    location: str = Field(min_length=1)
    value: float
```

## Relación con bases relacionales

InfluxDB puede coexistir con PostgreSQL, MySQL o SQL Server.

Patrón frecuente:

```text
base relacional -> entidades y transacciones principales
InfluxDB        -> métricas, eventos y series temporales
```

## Relación con dashboards

InfluxDB suele usarse junto con herramientas de visualización y monitoreo.

Ejemplos conceptuales:

```text
Grafana
dashboards internos
alertas
reportes temporales
```

## Orden didáctico interno

```text
1. Propósito de influxdb-client
2. Instalación
3. InfluxDB 2.x frente a InfluxDB 3.x
4. InfluxDBClient
5. Variables de entorno
6. Point
7. Measurement, tags, fields y timestamp
8. write_api
9. Escritura síncrona
10. Escritura por lotes
11. Line protocol
12. query_api
13. Consultas Flux
14. range(), filter(), last(), mean() y aggregateWindow()
15. DataFrames
16. Buckets y retención
17. Uso con FastAPI
18. API asíncrona
19. Manejo de errores
20. Errores comunes
21. Buenas prácticas
```