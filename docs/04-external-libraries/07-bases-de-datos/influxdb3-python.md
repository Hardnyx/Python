# `influxdb3-python`

## Propósito

`influxdb3-python` es una librería externa para conectar Python con InfluxDB 3.

InfluxDB 3 es una base de datos orientada a series temporales. Se utiliza para almacenar, escribir y consultar datos con marca temporal, como métricas, sensores, eventos, precios, telemetría, logs numéricos y datos de monitoreo.

La librería permite escribir datos en InfluxDB 3 y consultar información usando SQL o InfluxQL.

## Naturaleza de la librería

`influxdb3-python` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install influxdb3-python
```

La importación habitual es:

```python
from influxdb_client_3 import InfluxDBClient3
from influxdb_client_3 import Point
```

Aunque el paquete se instala como:

```text
influxdb3-python
```

se importa desde:

```python
influxdb_client_3
```

## Relación con InfluxDB 3

InfluxDB 3 trabaja con datos temporales y permite consultas mediante SQL e InfluxQL.

Conceptos principales:

```text
database
table
measurement
tag
field
timestamp
point
line protocol
SQL
InfluxQL
Apache Arrow
Flight protocol
```

En InfluxDB 3, el término `database` se usa como contenedor principal de escritura y consulta.

## Diferencia frente a `influxdb-client`

## `influxdb-client`

Se usa principalmente con InfluxDB 2.x.

Importación típica:

```python
from influxdb_client import InfluxDBClient
```

Consulta típica:

```text
Flux
```

## `influxdb3-python`

Se usa con InfluxDB 3.

Importación típica:

```python
from influxdb_client_3 import InfluxDBClient3
```

Consulta típica:

```text
SQL
InfluxQL
```

## Regla práctica

Para InfluxDB 2.x:

```bash
python -m pip install influxdb-client
```

Para InfluxDB 3.x:

```bash
python -m pip install influxdb3-python
```

## Idea central

La idea principal de `influxdb3-python` es crear un cliente, escribir puntos de series temporales y consultar datos con SQL o InfluxQL.

Flujo típico:

```text
Python
-> InfluxDBClient3
-> write() / write_file()
-> query()
-> InfluxDB 3
```

Ejemplo mínimo:

```python
from influxdb_client_3 import InfluxDBClient3, Point

client = InfluxDBClient3(
    host="https://example.influxdata.com",
    token="database-token",
    database="metrics"
)

try:
    point = (
        Point("temperature")
        .tag("location", "office")
        .field("value", 25.3)
    )

    client.write(record=point)

    table = client.query("""
        SELECT *
        FROM temperature
        WHERE time >= now() - INTERVAL '1 hour'
    """)

    print(table)

finally:
    client.close()
```

## Cuándo usar `influxdb3-python`

Conviene usar `influxdb3-python` cuando se necesita:

```text
conectar Python con InfluxDB 3
guardar series temporales
escribir métricas
consultar datos con SQL
consultar datos con InfluxQL
trabajar con Apache Arrow
convertir resultados a pandas
cargar datos desde CSV o Parquet
trabajar con datos de sensores
analizar eventos con timestamp
registrar telemetría
```

## Cuándo no usar `influxdb3-python`

No suele ser la mejor opción cuando se necesita:

```text
conectar con InfluxDB 2.x usando Flux
modelo relacional tradicional
joins complejos de negocio
transacciones relacionales fuertes
integridad referencial clásica
almacenamiento documental flexible
búsqueda textual avanzada
base local simple sin servidor
```

Para esos casos pueden corresponder:

```text
influxdb-client
PostgreSQL
MySQL
SQLite
MongoDB
Elasticsearch
DuckDB
```

## Instalación

Instalación básica:

```bash
python -m pip install influxdb3-python
```

Si se quiere trabajar con DataFrames de pandas, puede ser necesario instalar pandas:

```bash
python -m pip install pandas
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
influxdb3-python==0.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import influxdb_client_3

print(influxdb_client_3.__version__)
```

## Importaciones frecuentes

```python
from influxdb_client_3 import InfluxDBClient3
from influxdb_client_3 import Point
from influxdb_client_3 import WriteOptions
from influxdb_client_3 import write_client_options
```

## Cliente principal

`InfluxDBClient3` es la clase principal para interactuar con InfluxDB 3.

```python
from influxdb_client_3 import InfluxDBClient3

client = InfluxDBClient3(
    host="https://example.influxdata.com",
    token="database-token",
    database="metrics"
)
```

## Parámetros principales

```text
host     -> URL o host del servidor InfluxDB 3
token    -> token de autenticación
database -> base de datos para escribir y consultar
```

Ejemplo:

```python
client = InfluxDBClient3(
    host="https://example.influxdata.com",
    token="database-token",
    database="metrics"
)
```

## Variables de entorno

No conviene escribir tokens directamente en el código.

Menos recomendable:

```python
client = InfluxDBClient3(
    host="https://example.influxdata.com",
    token="database-token",
    database="metrics"
)
```

Más conveniente:

```python
import os

from influxdb_client_3 import InfluxDBClient3

client = InfluxDBClient3(
    host=os.getenv("INFLUXDB3_HOST"),
    token=os.getenv("INFLUXDB3_TOKEN"),
    database=os.getenv("INFLUXDB3_DATABASE")
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
INFLUXDB3_HOST=https://example.influxdata.com
INFLUXDB3_TOKEN=database-token
INFLUXDB3_DATABASE=metrics
```

Código:

```python
import os

from dotenv import load_dotenv
from influxdb_client_3 import InfluxDBClient3

load_dotenv()

client = InfluxDBClient3(
    host=os.getenv("INFLUXDB3_HOST"),
    token=os.getenv("INFLUXDB3_TOKEN"),
    database=os.getenv("INFLUXDB3_DATABASE")
)
```

## Cerrar cliente

```python
client.close()
```

Cerrar el cliente libera recursos asociados a escritura y consulta.

Patrón recomendado:

```python
client = InfluxDBClient3(
    host=host,
    token=token,
    database=database
)

try:
    ...
finally:
    client.close()
```

## Point

`Point` representa una observación temporal.

```python
from influxdb_client_3 import Point

point = (
    Point("temperature")
    .tag("location", "office")
    .field("value", 25.3)
)
```

Partes:

```text
temperature -> measurement o tabla lógica
location    -> tag
value       -> field
25.3        -> valor medido
```

## Measurement

El measurement representa el nombre lógico de la serie o tabla temporal.

Ejemplos:

```text
temperature
cpu
memory
prices
transactions
sensor_readings
api_latency
```

Uso:

```python
Point("temperature")
```

## Tags

Los tags son dimensiones para filtrar o agrupar.

```python
point = (
    Point("temperature")
    .tag("location", "office")
    .tag("device", "sensor-01")
)
```

Ejemplos:

```text
location
device
host
symbol
service
region
portfolio
```

## Fields

Los fields contienen valores medidos.

```python
point = (
    Point("temperature")
    .field("value", 25.3)
)
```

Ejemplos:

```text
value
price
amount
latency
usage
count
volume
```

## Timestamp

Un punto puede tener timestamp explícito.

```python
from datetime import datetime, timezone
from influxdb_client_3 import Point

point = (
    Point("temperature")
    .tag("location", "office")
    .field("value", 25.3)
    .time(datetime.now(timezone.utc))
)
```

Si no se indica timestamp, el servidor puede asignar el tiempo de escritura.

## Escribir un punto

```python
point = (
    Point("temperature")
    .tag("location", "office")
    .field("value", 25.3)
)

client.write(
    record=point
)
```

## Escribir varios puntos

```python
points = [
    (
        Point("temperature")
        .tag("location", "office")
        .field("value", 25.3)
    ),
    (
        Point("temperature")
        .tag("location", "warehouse")
        .field("value", 21.8)
    ),
]

client.write(
    record=points
)
```

## Escribir con precisión temporal

```python
client.write(
    record=points,
    write_precision="s"
)
```

Valores conceptuales frecuentes:

```text
ns -> nanosegundos
us -> microsegundos
ms -> milisegundos
s  -> segundos
```

La precisión debe coincidir con cómo se construyen los timestamps.

## Escribir con diccionario

También puede escribirse usando diccionarios.

```python
point = {
    "measurement": "temperature",
    "tags": {
        "location": "office"
    },
    "fields": {
        "value": 25.3
    }
}

client.write(
    record=point
)
```

## Escribir lista de diccionarios

```python
records = [
    {
        "measurement": "temperature",
        "tags": {
            "location": "office"
        },
        "fields": {
            "value": 25.3
        }
    },
    {
        "measurement": "temperature",
        "tags": {
            "location": "warehouse"
        },
        "fields": {
            "value": 21.8
        }
    }
]

client.write(
    record=records
)
```

## Line protocol

Line protocol es el formato textual para representar puntos.

Ejemplo:

```text
temperature,location=office value=25.3
```

Uso desde Python:

```python
line = "temperature,location=office value=25.3"

client.write(
    record=line
)
```

## Escribir varias líneas

```python
lines = [
    "temperature,location=office value=25.3",
    "temperature,location=warehouse value=21.8",
]

client.write(
    record=lines
)
```

## Escritura síncrona

La escritura síncrona espera a que la operación se complete.

Es la forma más simple para scripts y pruebas.

```python
client = InfluxDBClient3(
    host=host,
    token=token,
    database=database
)

client.write(record=point)
```

## Escritura por lotes

Para cargas más grandes puede configurarse escritura por lotes.

```python
from influxdb_client_3 import InfluxDBClient3
from influxdb_client_3 import WriteOptions
from influxdb_client_3 import write_client_options

write_options = WriteOptions(
    batch_size=500,
    flush_interval=10000
)

client = InfluxDBClient3(
    host=host,
    token=token,
    database=database,
    write_client_options=write_client_options(
        write_options=write_options
    )
)
```

La escritura por lotes agrupa puntos antes de enviarlos.

## Cuándo usar escritura por lotes

Conviene usar escritura por lotes cuando:

```text
se insertan muchos puntos
se cargan archivos grandes
se procesan eventos continuamente
se quiere reducir número de requests
se necesita controlar reintentos
```

Para scripts pequeños, la escritura síncrona suele ser suficiente.

## Escribir archivo

`write_file()` permite cargar datos desde archivos compatibles.

Ejemplo conceptual con CSV:

```python
client.write_file(
    file="data/home-sensor-data.csv",
    timestamp_column="time",
    tag_columns=["room"]
)
```

Este patrón es útil para cargas iniciales o migraciones de datos.

## Escribir desde CSV

```python
client.write_file(
    file="data/metrics.csv",
    timestamp_column="time",
    tag_columns=["location", "device"]
)
```

El archivo debe tener columnas que puedan interpretarse como tiempo, tags y fields.

## Escribir desde pandas

```python
import pandas as pd

df = pd.DataFrame({
    "time": pd.to_datetime([
        "2026-01-01T00:00:00Z",
        "2026-01-01T00:01:00Z"
    ]),
    "location": ["office", "warehouse"],
    "value": [25.3, 21.8]
})
```

Transformación a puntos:

```python
points = []

for row in df.to_dict(orient="records"):
    point = (
        Point("temperature")
        .tag("location", row["location"])
        .field("value", row["value"])
        .time(row["time"])
    )

    points.append(point)

client.write(
    record=points
)
```

## Consultar con SQL

InfluxDB 3 permite consultar con SQL.

```python
table = client.query("""
    SELECT *
    FROM temperature
    WHERE time >= now() - INTERVAL '1 hour'
""")
```

El resultado puede ser una tabla Arrow.

## Consultar columnas específicas

```python
table = client.query("""
    SELECT
        time,
        location,
        value
    FROM temperature
    WHERE time >= now() - INTERVAL '24 hours'
    ORDER BY time
""")
```

## Filtro por tag

```python
table = client.query("""
    SELECT
        time,
        location,
        value
    FROM temperature
    WHERE location = 'office'
      AND time >= now() - INTERVAL '1 hour'
    ORDER BY time
""")
```

## Agregación con SQL

```python
table = client.query("""
    SELECT
        location,
        AVG(value) AS avg_value
    FROM temperature
    WHERE time >= now() - INTERVAL '24 hours'
    GROUP BY location
""")
```

## Límite de resultados

```python
table = client.query("""
    SELECT *
    FROM temperature
    ORDER BY time DESC
    LIMIT 10
""")
```

## Consultar con InfluxQL

También puede consultarse usando InfluxQL.

```python
table = client.query(
    query="""
    SELECT MEAN(value)
    FROM temperature
    WHERE time >= now() - 1h
    GROUP BY location
    """,
    language="influxql"
)
```

## Cuándo usar SQL

SQL suele ser conveniente cuando se necesita:

```text
consultas tabulares
agregaciones familiares
ORDER BY
GROUP BY
LIMIT
integración con herramientas analíticas
```

## Cuándo usar InfluxQL

InfluxQL puede ser conveniente cuando se viene de versiones anteriores de InfluxDB o se trabaja con consultas de series temporales ya escritas en ese lenguaje.

## Resultado como PyArrow Table

`query()` puede devolver una tabla compatible con Apache Arrow.

```python
table = client.query("""
    SELECT *
    FROM temperature
    LIMIT 10
""")

print(table)
```

## Convertir resultado a pandas

```python
df = table.to_pandas()

print(df.head())
```

Este patrón es útil para análisis tabular.

## Convertir resultado a lista de diccionarios

```python
df = table.to_pandas()

rows = df.to_dict(orient="records")

print(rows)
```

## Uso con PyArrow

Como el resultado puede ser una tabla Arrow, se pueden usar operaciones del ecosistema Arrow.

```python
table = client.query("""
    SELECT *
    FROM temperature
    LIMIT 100
""")

print(table.schema)
print(table.num_rows)
```

## Consultar metadatos

El cliente puede usarse para solicitar información del servidor y de la base, según las capacidades disponibles del entorno.

Ejemplo conceptual:

```python
table = client.query("""
    SHOW TABLES
""")
```

La disponibilidad exacta de consultas administrativas puede depender del producto InfluxDB 3 usado.

## Diseño de datos

En series temporales, el diseño se basa en:

```text
measurement
tags
fields
timestamp
cardinalidad
rango temporal
frecuencia de escritura
consultas esperadas
```

## Diseño de measurement

El measurement debe representar una familia de mediciones.

Ejemplos convenientes:

```text
temperature
cpu
memory
price
api_latency
trade_events
```

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
device
host
symbol
service
region
```

No conviene usar como tag valores de cardinalidad excesivamente alta sin evaluar impacto.

Ejemplos riesgosos:

```text
request_id
transaction_id
uuid
unique_user_id
```

## Diseño de fields

Los fields deben contener valores medidos.

Ejemplos:

```text
value
price
amount
latency
cpu_usage
memory_usage
count
```

Si un dato se usa constantemente para filtrar, debe evaluarse si corresponde como tag.

## Cardinalidad

La cardinalidad aumenta cuando existen muchas combinaciones únicas de tags.

Alta cardinalidad puede afectar rendimiento y almacenamiento.

Ejemplos de tags de alta cardinalidad:

```text
id único por evento
uuid
email
request_id
transaction_id
```

Estos valores deben evaluarse con cuidado antes de usarse como tags.

## Uso con fechas

Conviene usar fechas con zona horaria clara.

```python
from datetime import datetime, timezone

point = (
    Point("temperature")
    .tag("location", "office")
    .field("value", 25.3)
    .time(datetime.now(timezone.utc))
)
```

## Uso con datos financieros

Ejemplo conceptual de precios:

```python
point = (
    Point("market_price")
    .tag("symbol", "ABC")
    .tag("currency", "PEN")
    .field("price", 10.25)
)
```

Consulta:

```python
table = client.query("""
    SELECT
        time,
        symbol,
        currency,
        price
    FROM market_price
    WHERE symbol = 'ABC'
    ORDER BY time DESC
    LIMIT 20
""")
```

## Uso con métricas de API

```python
point = (
    Point("api_latency")
    .tag("service", "backend")
    .tag("endpoint", "/products")
    .field("duration_ms", 125.4)
    .field("status_code", 200)
)
```

Consulta:

```python
table = client.query("""
    SELECT
        endpoint,
        AVG(duration_ms) AS avg_duration
    FROM api_latency
    WHERE time >= now() - INTERVAL '1 hour'
    GROUP BY endpoint
""")
```

## Uso con sensores

```python
point = (
    Point("sensor_reading")
    .tag("device", "sensor-01")
    .tag("location", "office")
    .field("temperature", 25.3)
    .field("humidity", 60.1)
)
```

Consulta:

```python
table = client.query("""
    SELECT
        time,
        device,
        location,
        temperature,
        humidity
    FROM sensor_reading
    WHERE time >= now() - INTERVAL '24 hours'
    ORDER BY time
""")
```

## Manejo de errores

La librería puede generar errores por problemas de conexión, autenticación, permisos, escritura o consulta.

Patrón general:

```python
try:
    client.write(record=point)
except Exception as error:
    print("Error al escribir en InfluxDB 3:", error)
```

Consulta:

```python
try:
    table = client.query("""
        SELECT *
        FROM temperature
        LIMIT 10
    """)
except Exception as error:
    print("Error al consultar InfluxDB 3:", error)
```

## Errores frecuentes

```text
host incorrecto
token inválido
database incorrecta
permisos insuficientes
consulta SQL inválida
consulta InfluxQL inválida
archivo CSV mal formado
timestamp inválido
tipo de dato no compatible
problemas de red
```

## Error de token

Si el token es inválido o no tiene permisos, fallarán escrituras o consultas.

Debe revisarse:

```text
INFLUXDB3_TOKEN
database
permisos de lectura
permisos de escritura
entorno correcto
```

## Error de database

Si la base de datos no existe o el token no tiene acceso, las operaciones fallarán.

Debe revisarse:

```text
INFLUXDB3_DATABASE
host
token
permisos
```

## Error de consulta SQL

Una consulta puede fallar por:

```text
nombre de tabla incorrecto
columna inexistente
sintaxis inválida
tipo de dato incompatible
rango temporal mal escrito
```

Conviene probar primero consultas pequeñas.

## Uso con FastAPI

```python
import os
from contextlib import asynccontextmanager

from fastapi import FastAPI
from influxdb_client_3 import InfluxDBClient3, Point


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.influx_client = InfluxDBClient3(
        host=os.getenv("INFLUXDB3_HOST"),
        token=os.getenv("INFLUXDB3_TOKEN"),
        database=os.getenv("INFLUXDB3_DATABASE")
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

    app.state.influx_client.write(
        record=point
    )

    return {
        "stored": True
    }


@app.get("/metrics/temperature")
def read_temperature():
    table = app.state.influx_client.query("""
        SELECT
            time,
            location,
            value
        FROM temperature
        WHERE time >= now() - INTERVAL '1 hour'
        ORDER BY time
    """)

    df = table.to_pandas()

    return df.to_dict(orient="records")
```

## Uso con Pydantic

```python
from pydantic import BaseModel, Field


class TemperatureMetric(BaseModel):
    location: str = Field(min_length=1)
    value: float
```

Uso:

```python
def write_temperature_metric(client, metric: TemperatureMetric):
    point = (
        Point("temperature")
        .tag("location", metric.location)
        .field("value", metric.value)
    )

    client.write(record=point)
```

## Uso con pandas

Consultar a DataFrame:

```python
table = client.query("""
    SELECT *
    FROM temperature
    WHERE time >= now() - INTERVAL '24 hours'
""")

df = table.to_pandas()

print(df.head())
```

Escribir desde DataFrame mediante puntos:

```python
points = []

for row in df.to_dict(orient="records"):
    point = (
        Point("temperature")
        .tag("location", row["location"])
        .field("value", row["value"])
        .time(row["time"])
    )

    points.append(point)

client.write(record=points)
```

## Uso con archivos

Carga desde archivo:

```python
client.write_file(
    file="data/metrics.csv",
    timestamp_column="time",
    tag_columns=["location"]
)
```

Consulta posterior:

```python
table = client.query("""
    SELECT *
    FROM temperature
    ORDER BY time DESC
    LIMIT 10
""")
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
from influxdb_client_3 import InfluxDBClient3

load_dotenv()

INFLUXDB3_HOST = os.getenv("INFLUXDB3_HOST")
INFLUXDB3_TOKEN = os.getenv("INFLUXDB3_TOKEN")
INFLUXDB3_DATABASE = os.getenv("INFLUXDB3_DATABASE")


def create_client():
    return InfluxDBClient3(
        host=INFLUXDB3_HOST,
        token=INFLUXDB3_TOKEN,
        database=INFLUXDB3_DATABASE
    )
```

## `schemas.py`

```python
from pydantic import BaseModel, Field


class TemperatureMetric(BaseModel):
    location: str = Field(min_length=1)
    value: float
```

## `repositories.py`

```python
from influxdb_client_3 import Point


def write_temperature(client, metric):
    point = (
        Point("temperature")
        .tag("location", metric.location)
        .field("value", metric.value)
    )

    client.write(record=point)


def read_temperature(client, start_interval="1 hour"):
    allowed_intervals = {
        "15 minutes": "15 minutes",
        "1 hour": "1 hour",
        "24 hours": "24 hours",
        "7 days": "7 days",
    }

    interval = allowed_intervals.get(start_interval)

    if interval is None:
        raise ValueError("Intervalo no permitido")

    table = client.query(f"""
        SELECT
            time,
            location,
            value
        FROM temperature
        WHERE time >= now() - INTERVAL '{interval}'
        ORDER BY time
    """)

    return table.to_pandas().to_dict(orient="records")
```

## Separación por responsabilidades

```text
database.py      -> creación del cliente
schemas.py       -> validación de entrada
repositories.py  -> escritura y consultas
services.py      -> reglas de negocio
main.py          -> aplicación o script
```

## Diferencia frente a bases relacionales

InfluxDB 3 no debe modelarse como una base relacional clásica.

En vez de pensar principalmente en:

```text
tablas normalizadas
foreign keys
joins de negocio
transacciones relacionales
```

conviene pensar en:

```text
series temporales
mediciones
tags
fields
tiempo
rangos
agregaciones
cardinalidad
```

## Diferencia frente a DuckDB

DuckDB es útil para análisis local de archivos y datos tabulares.

InfluxDB 3 es útil para almacenar y consultar series temporales en un servidor o servicio especializado.

Comparación conceptual:

```text
DuckDB      -> análisis local, SQL sobre archivos y tablas
InfluxDB 3  -> series temporales persistentes, escritura y consulta por tiempo
```

## Diferencia frente a TimescaleDB

TimescaleDB extiende PostgreSQL para series temporales.

InfluxDB 3 es una base especializada en series temporales.

Regla práctica:

```text
SQL relacional + series temporales dentro de PostgreSQL -> TimescaleDB
base especializada para series temporales              -> InfluxDB 3
```

## Diferencia frente a Prometheus

Prometheus se usa mucho para monitoreo y scraping de métricas.

InfluxDB 3 se usa como base de series temporales para escrituras y consultas desde clientes y aplicaciones.

Ambas herramientas pueden coexistir en arquitecturas de monitoreo.

## Errores comunes

## Instalar el paquete correcto pero importar mal

Instalación correcta:

```bash
python -m pip install influxdb3-python
```

Importación correcta:

```python
from influxdb_client_3 import InfluxDBClient3
```

Incorrecto:

```python
import influxdb3_python
```

## Usar `influxdb-client` para InfluxDB 3 sin revisar

Para InfluxDB 3, corresponde revisar `influxdb3-python`.

Para InfluxDB 2.x, corresponde revisar `influxdb-client`.

## No cerrar el cliente

Menos recomendable:

```python
client = InfluxDBClient3(...)
```

sin:

```python
client.close()
```

Más seguro:

```python
try:
    ...
finally:
    client.close()
```

## Guardar token en el código

Problemático:

```python
token = "database-token"
```

Mejor:

```python
token = os.getenv("INFLUXDB3_TOKEN")
```

## Confundir tags y fields

Los tags sirven para dimensiones de consulta y agrupación.

Los fields sirven para valores medidos.

Diseñar mal esta separación puede afectar consultas y rendimiento.

## Usar tags de cardinalidad muy alta

Problemático como tag:

```text
uuid
request_id
transaction_id
email
```

Debe evaluarse antes de usar esos valores como tags.

## Consultar rangos demasiado grandes sin agregación

Puede ser costoso traer demasiados puntos crudos.

Conviene usar:

```text
rangos razonables
agregaciones
LIMIT
filtros por tags
```

## Construir SQL con entrada libre

Problemático:

```python
query = f"""
SELECT *
FROM {user_table}
WHERE time >= now() - INTERVAL '{user_interval}'
"""
```

Debe validarse cualquier tabla, columna o intervalo dinámico contra listas permitidas.

## No validar nombres dinámicos

Los parámetros protegen valores, pero los nombres de tabla y columna requieren validación explícita.

Ejemplo:

```python
allowed_tables = {
    "temperature": "temperature",
    "api_latency": "api_latency",
}
```

## Buenas prácticas

## Usar variables de entorno

```text
INFLUXDB3_HOST
INFLUXDB3_TOKEN
INFLUXDB3_DATABASE
```

## Cerrar el cliente

```python
client.close()
```

## Usar `Point` para construir datos claramente

```python
Point("temperature").tag(...).field(...)
```

## Usar tags para dimensiones consultadas con frecuencia

```text
location
device
host
symbol
service
region
```

## Usar fields para valores medidos

```text
value
price
latency
count
usage
volume
```

## Controlar cardinalidad

Evitar tags con valores únicos por evento si no son necesarios.

## Usar SQL para consultas tabulares

```sql
SELECT ...
FROM ...
WHERE time >= ...
GROUP BY ...
```

## Usar InfluxQL cuando exista compatibilidad o consultas heredadas

```python
client.query(query=..., language="influxql")
```

## Convertir a pandas solo cuando el tamaño sea razonable

```python
df = table.to_pandas()
```

Para resultados grandes, conviene filtrar o agregar antes.

## Usar `write_file()` para cargas desde archivos

```python
client.write_file(...)
```

## Separar consultas en repositorios

```text
repositories.py
```

## Ejemplo integrado

```python
import os
from datetime import datetime, timezone

from dotenv import load_dotenv
from influxdb_client_3 import InfluxDBClient3, Point

load_dotenv()

INFLUXDB3_HOST = os.getenv("INFLUXDB3_HOST")
INFLUXDB3_TOKEN = os.getenv("INFLUXDB3_TOKEN")
INFLUXDB3_DATABASE = os.getenv("INFLUXDB3_DATABASE")


def create_client():
    return InfluxDBClient3(
        host=INFLUXDB3_HOST,
        token=INFLUXDB3_TOKEN,
        database=INFLUXDB3_DATABASE
    )


def write_temperature(client, location, value):
    point = (
        Point("temperature")
        .tag("location", location)
        .field("value", value)
        .time(datetime.now(timezone.utc))
    )

    client.write(record=point)


def read_temperature(client):
    table = client.query("""
        SELECT
            time,
            location,
            value
        FROM temperature
        WHERE time >= now() - INTERVAL '1 hour'
        ORDER BY time
    """)

    return table.to_pandas().to_dict(orient="records")


def read_temperature_summary(client):
    table = client.query("""
        SELECT
            location,
            AVG(value) AS avg_value,
            MIN(value) AS min_value,
            MAX(value) AS max_value
        FROM temperature
        WHERE time >= now() - INTERVAL '24 hours'
        GROUP BY location
        ORDER BY location
    """)

    return table.to_pandas().to_dict(orient="records")


def main():
    client = create_client()

    try:
        write_temperature(
            client,
            location="office",
            value=25.3
        )

        write_temperature(
            client,
            location="warehouse",
            value=21.8
        )

        rows = read_temperature(client)

        for row in rows:
            print(row)

        summary = read_temperature_summary(client)

        for row in summary:
            print(row)

    finally:
        client.close()


if __name__ == "__main__":
    main()
```

## Ejemplo con carga desde CSV

```python
import os

from dotenv import load_dotenv
from influxdb_client_3 import InfluxDBClient3

load_dotenv()


def create_client():
    return InfluxDBClient3(
        host=os.getenv("INFLUXDB3_HOST"),
        token=os.getenv("INFLUXDB3_TOKEN"),
        database=os.getenv("INFLUXDB3_DATABASE")
    )


def load_csv(client, file_path):
    client.write_file(
        file=file_path,
        timestamp_column="time",
        tag_columns=[
            "location",
            "device"
        ]
    )


def main():
    client = create_client()

    try:
        load_csv(
            client,
            file_path="data/metrics.csv"
        )

        table = client.query("""
            SELECT *
            FROM temperature
            ORDER BY time DESC
            LIMIT 10
        """)

        print(table.to_pandas())

    finally:
        client.close()


if __name__ == "__main__":
    main()
```

## Ejemplo con InfluxQL

```python
import os

from dotenv import load_dotenv
from influxdb_client_3 import InfluxDBClient3

load_dotenv()

client = InfluxDBClient3(
    host=os.getenv("INFLUXDB3_HOST"),
    token=os.getenv("INFLUXDB3_TOKEN"),
    database=os.getenv("INFLUXDB3_DATABASE")
)

try:
    table = client.query(
        query="""
        SELECT MEAN(value)
        FROM temperature
        WHERE time >= now() - 1h
        GROUP BY location
        """,
        language="influxql"
    )

    print(table.to_pandas())

finally:
    client.close()
```

## Relación con otras librerías

`influxdb3-python` se relaciona especialmente con:

```text
pandas
pyarrow
python-dotenv
fastapi
pydantic
pytest
```

## Relación con pandas

Los resultados de consulta pueden convertirse a DataFrame.

```python
df = table.to_pandas()
```

También puede transformarse un DataFrame a puntos antes de escribir.

## Relación con PyArrow

El cliente usa datos compatibles con Apache Arrow para consultas.

Esto facilita interoperabilidad con herramientas analíticas modernas.

## Relación con FastAPI

Puede usarse para registrar métricas, eventos temporales o telemetría desde endpoints.

## Relación con Pydantic

Pydantic puede validar mediciones antes de escribirlas en InfluxDB 3.

## Relación con dashboards

InfluxDB 3 puede alimentar tableros y sistemas de monitoreo.

Ejemplos conceptuales:

```text
Grafana
dashboards internos
reportes temporales
alertas
```

## Orden didáctico interno

```text
1. Propósito de influxdb3-python
2. Instalación
3. Diferencia frente a influxdb-client
4. InfluxDBClient3
5. Variables de entorno
6. Point
7. Measurement, tags, fields y timestamp
8. write()
9. Escritura síncrona
10. Escritura por lotes
11. write_file()
12. Line protocol
13. query()
14. SQL
15. InfluxQL
16. PyArrow Table
17. pandas
18. Diseño de series temporales
19. Uso con FastAPI y Pydantic
20. Manejo de errores
21. Errores comunes
22. Buenas prácticas
```