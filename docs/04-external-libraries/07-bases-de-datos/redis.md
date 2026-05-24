# `redis`

## Propósito

`redis` es una librería externa para conectar Python con Redis.

Se utiliza para trabajar con una base de datos en memoria orientada a estructuras de datos, frecuentemente usada como caché, almacén clave-valor, sistema de colas simples, contador, gestor de sesiones, pub/sub, control de expiraciones y almacenamiento temporal de información de alta velocidad.

La librería suele conocerse como `redis-py`, aunque se instala e importa como `redis`.

## Naturaleza de la librería

`redis` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install redis
```

La importación habitual es:

```python
import redis
```

Redis requiere un servidor Redis en ejecución. La librería de Python solo actúa como cliente.

## Relación con Redis

Redis es una base de datos en memoria que trabaja principalmente con claves y valores.

A diferencia de bases relacionales como PostgreSQL, MySQL o SQL Server, Redis no se organiza principalmente alrededor de tablas y filas.

A diferencia de MongoDB, Redis tampoco se centra en documentos flexibles.

Redis se organiza alrededor de estructuras como:

```text
strings
hashes
lists
sets
sorted sets
streams
pub/sub
```

## Idea central

La idea principal de `redis` es crear un cliente y ejecutar comandos contra un servidor Redis.

Flujo típico:

```text
Python -> redis.Redis -> Redis server -> key/value
```

Ejemplo mínimo:

```python
import redis

client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)

client.set("message", "Hola Redis")

value = client.get("message")

print(value)
```

## Cuándo usar Redis

Conviene usar Redis cuando se necesita:

```text
caché rápida
almacenamiento temporal
sesiones
tokens con expiración
contadores
colas simples
locks distribuidos simples
pub/sub
rankings
estructuras clave-valor
datos que deben leerse con muy baja latencia
```

## Cuándo no usar Redis

Redis no siempre es adecuado cuando se necesita:

```text
persistencia relacional compleja
consultas SQL
joins
reportes analíticos grandes
histórico permanente como fuente principal
transacciones complejas entre muchas entidades
modelo documental amplio
```

Para esos casos pueden corresponder:

```text
PostgreSQL
MySQL
SQL Server
MongoDB
DuckDB
SQLite
```

## Instalación

Instalación básica:

```bash
python -m pip install redis
```

Instalación con soporte para `hiredis`:

```bash
python -m pip install "redis[hiredis]"
```

`hiredis` puede mejorar el rendimiento de parseo de respuestas en ciertos escenarios.

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
redis==7.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import redis

print(redis.__version__)
```

## Conexión básica

```python
import redis

client = redis.Redis(
    host="localhost",
    port=6379
)
```

Por defecto, muchas respuestas pueden venir como `bytes`.

Ejemplo:

```python
client.set("name", "Python")

value = client.get("name")

print(value)
```

Salida conceptual:

```text
b'Python'
```

## `decode_responses=True`

Para recibir cadenas de texto en lugar de bytes:

```python
import redis

client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)

client.set("name", "Python")

value = client.get("name")

print(value)
```

Salida conceptual:

```text
Python
```

Este parámetro suele hacer los ejemplos y aplicaciones más legibles cuando se trabaja con texto.

## Conexión con URL

```python
import redis

client = redis.from_url(
    "redis://localhost:6379/0",
    decode_responses=True
)
```

La parte final indica la base lógica de Redis:

```text
/0 -> base 0
/1 -> base 1
```

## Verificar conexión

```python
import redis

client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)

if client.ping():
    print("Conexión correcta")
```

## Variables de entorno

No conviene escribir credenciales directamente en el código.

Menos recomendable:

```python
client = redis.from_url("redis://user:password@host:6379/0")
```

Más conveniente:

```python
import os

import redis

redis_url = os.getenv("REDIS_URL")

client = redis.from_url(
    redis_url,
    decode_responses=True
)
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
REDIS_URL=redis://localhost:6379/0
```

Código:

```python
import os

import redis
from dotenv import load_dotenv

load_dotenv()

redis_url = os.getenv("REDIS_URL", "redis://localhost:6379/0")

client = redis.from_url(
    redis_url,
    decode_responses=True
)
```

## Strings

Los strings son el tipo más básico de Redis.

Permiten almacenar valores simples asociados a una clave.

## Guardar valor

```python
client.set("user:1:name", "Ana")
```

## Leer valor

```python
name = client.get("user:1:name")

print(name)
```

## Verificar existencia

```python
exists = client.exists("user:1:name")

print(exists)
```

## Eliminar clave

```python
client.delete("user:1:name")
```

## Expiración de claves

Redis permite asignar tiempo de vida a una clave.

## `set()` con expiración

```python
client.set(
    "session:abc",
    "user-1",
    ex=3600
)
```

`ex=3600` indica expiración en segundos.

## `expire()`

```python
client.expire("session:abc", 3600)
```

## `ttl()`

```python
ttl = client.ttl("session:abc")

print(ttl)
```

`ttl()` devuelve el tiempo de vida restante en segundos.

## Contadores

Redis puede incrementar valores numéricos.

```python
client.set("counter", 0)

client.incr("counter")
client.incr("counter")

value = client.get("counter")

print(value)
```

## Incrementar por una cantidad

```python
client.incrby("counter", 5)
```

## Decrementar

```python
client.decr("counter")
```

## Hashes

Un hash almacena varios campos dentro de una misma clave.

Es útil para representar objetos simples.

## Crear hash

```python
client.hset(
    "user:1",
    mapping={
        "name": "Ana",
        "email": "ana@example.com",
        "age": "25"
    }
)
```

## Leer un campo

```python
name = client.hget("user:1", "name")

print(name)
```

## Leer todos los campos

```python
user = client.hgetall("user:1")

print(user)
```

## Actualizar campo

```python
client.hset("user:1", "age", "26")
```

## Eliminar campo

```python
client.hdel("user:1", "age")
```

## Lists

Las listas en Redis permiten insertar valores al inicio o al final.

Son útiles para colas simples, historiales o buffers.

## Insertar al final

```python
client.rpush("queue:emails", "email-1")
client.rpush("queue:emails", "email-2")
```

## Extraer del inicio

```python
item = client.lpop("queue:emails")

print(item)
```

## Insertar al inicio

```python
client.lpush("events", "event-1")
```

## Leer rango

```python
items = client.lrange("queue:emails", 0, -1)

print(items)
```

## Longitud de lista

```python
length = client.llen("queue:emails")

print(length)
```

## Sets

Los sets almacenan valores únicos sin orden posicional.

## Agregar elementos

```python
client.sadd("tags", "python", "redis", "database")
```

## Leer miembros

```python
members = client.smembers("tags")

print(members)
```

## Verificar pertenencia

```python
is_member = client.sismember("tags", "python")

print(is_member)
```

## Eliminar elemento

```python
client.srem("tags", "database")
```

## Operaciones de conjuntos

Intersección:

```python
common = client.sinter("tags:a", "tags:b")
```

Unión:

```python
all_tags = client.sunion("tags:a", "tags:b")
```

Diferencia:

```python
difference = client.sdiff("tags:a", "tags:b")
```

## Sorted sets

Los sorted sets almacenan elementos únicos con un puntaje numérico.

Son útiles para rankings, prioridades o ordenamientos.

## Agregar elementos

```python
client.zadd(
    "ranking",
    {
        "Ana": 100,
        "Luis": 80,
        "Marta": 120
    }
)
```

## Leer ranking ascendente

```python
items = client.zrange(
    "ranking",
    0,
    -1,
    withscores=True
)

print(items)
```

## Leer ranking descendente

```python
items = client.zrevrange(
    "ranking",
    0,
    -1,
    withscores=True
)

print(items)
```

## Incrementar puntaje

```python
client.zincrby("ranking", 10, "Luis")
```

## Obtener posición

```python
rank = client.zrevrank("ranking", "Marta")

print(rank)
```

## Pub/Sub

Redis permite publicar mensajes en canales y suscribirse a ellos.

## Publicar mensaje

```python
client.publish("notifications", "Nuevo evento")
```

## Suscribirse a canal

```python
pubsub = client.pubsub()

pubsub.subscribe("notifications")

for message in pubsub.listen():
    print(message)
```

Este patrón puede bloquear la ejecución mientras espera mensajes.

Para aplicaciones reales, suele ejecutarse en un proceso, hilo o tarea separada.

## Streams

Los streams permiten manejar secuencias de eventos.

## Agregar evento

```python
client.xadd(
    "events",
    {
        "type": "created",
        "user_id": "1"
    }
)
```

## Leer eventos

```python
events = client.xrange("events")

print(events)
```

## Leer desde un punto

```python
events = client.xread(
    {"events": "0-0"},
    count=10
)

print(events)
```

Los streams son más avanzados que las listas y pueden servir para flujos de eventos.

## Pipelines

Un pipeline permite enviar varios comandos juntos.

Esto reduce viajes entre Python y Redis.

```python
pipe = client.pipeline()

pipe.set("a", "1")
pipe.set("b", "2")
pipe.get("a")
pipe.get("b")

results = pipe.execute()

print(results)
```

## Pipeline transaccional

Por defecto, los pipelines pueden ejecutarse de forma transaccional.

```python
pipe = client.pipeline(transaction=True)

pipe.incr("counter")
pipe.incr("counter")

results = pipe.execute()

print(results)
```

## Pipeline no transaccional

```python
pipe = client.pipeline(transaction=False)

pipe.set("a", "1")
pipe.set("b", "2")

pipe.execute()
```

## Locks

Redis puede usarse para implementar locks simples.

```python
lock = client.lock(
    "lock:process",
    timeout=30
)

with lock:
    print("Proceso protegido")
```

Los locks distribuidos requieren cuidado. Deben usarse con tiempos de expiración y entendiendo el contexto de concurrencia.

## Connection pool

La librería usa pools de conexión para reutilizar conexiones.

También puede configurarse explícitamente.

```python
pool = redis.ConnectionPool.from_url(
    "redis://localhost:6379/0",
    decode_responses=True
)

client = redis.Redis(connection_pool=pool)
```

Esto es útil en aplicaciones web o procesos con muchas operaciones.

## Uso asíncrono

`redis` también ofrece API asíncrona.

```python
import asyncio

import redis.asyncio as redis


async def main():
    client = redis.Redis(
        host="localhost",
        port=6379,
        decode_responses=True
    )

    await client.set("message", "Hola async")
    value = await client.get("message")

    print(value)

    await client.aclose()


asyncio.run(main())
```

Este enfoque es útil en aplicaciones asíncronas, por ejemplo con FastAPI usando endpoints `async`.

## Uso con FastAPI

Redis suele usarse en FastAPI para caché, sesiones, rate limiting o colas simples.

Ejemplo básico:

```python
import os

import redis
from fastapi import FastAPI

app = FastAPI()

redis_url = os.getenv("REDIS_URL", "redis://localhost:6379/0")

client = redis.from_url(
    redis_url,
    decode_responses=True
)


@app.get("/cache/{key}")
def read_cache(key: str):
    value = client.get(key)

    return {
        "key": key,
        "value": value
    }


@app.post("/cache/{key}")
def write_cache(key: str, value: str):
    client.set(key, value, ex=3600)

    return {
        "key": key,
        "stored": True
    }
```

## Uso con Flask

```python
import os

import redis
from flask import Flask

app = Flask(__name__)

redis_url = os.getenv("REDIS_URL", "redis://localhost:6379/0")

client = redis.from_url(
    redis_url,
    decode_responses=True
)


@app.route("/cache/<key>")
def read_cache(key):
    value = client.get(key)

    return {
        "key": key,
        "value": value
    }
```

## Uso con pandas

Redis no reemplaza a pandas ni a una base analítica, pero puede almacenar resultados calculados.

Ejemplo conceptual:

```python
import json

summary = {
    "total_amount": 3500,
    "count": 10
}

client.set(
    "summary:sales",
    json.dumps(summary),
    ex=3600
)

cached = client.get("summary:sales")

if cached is not None:
    summary = json.loads(cached)
```

## Serialización JSON

Redis almacena bytes o strings. Para guardar estructuras como diccionarios o listas, suele serializarse.

```python
import json

data = {
    "name": "Laptop",
    "price": 3500
}

client.set(
    "product:1",
    json.dumps(data)
)

raw = client.get("product:1")

product = json.loads(raw)

print(product)
```

## Cuidado con tipos

Si `decode_responses=True`, Redis devuelve strings.

Ejemplo:

```python
client.set("stock", 10)

stock = client.get("stock")

print(type(stock))
```

Resultado conceptual:

```text
str
```

Si se necesita número:

```python
stock = int(client.get("stock"))
```

## Caché simple

```python
import json


def get_cached_product(product_id):
    key = f"product:{product_id}"

    cached = client.get(key)

    if cached is None:
        return None

    return json.loads(cached)


def set_cached_product(product_id, product, seconds=3600):
    key = f"product:{product_id}"

    client.set(
        key,
        json.dumps(product),
        ex=seconds
    )
```

## Patrón cache-aside

El patrón cache-aside consiste en:

```text
1. Buscar en caché.
2. Si existe, devolverlo.
3. Si no existe, consultar fuente principal.
4. Guardar resultado en caché.
5. Devolver resultado.
```

Ejemplo:

```python
import json


def get_product(product_id):
    cache_key = f"product:{product_id}"

    cached = client.get(cache_key)

    if cached is not None:
        return json.loads(cached)

    product = load_product_from_database(product_id)

    if product is not None:
        client.set(
            cache_key,
            json.dumps(product),
            ex=3600
        )

    return product
```

`load_product_from_database()` representa una función externa que consulta la base principal.

## Rate limiting simple

Redis puede usarse para limitar frecuencia de solicitudes.

```python
def allow_request(identifier, limit, window_seconds):
    key = f"rate:{identifier}"

    current = client.incr(key)

    if current == 1:
        client.expire(key, window_seconds)

    return current <= limit
```

Uso conceptual:

```python
if not allow_request("user:1", limit=100, window_seconds=60):
    raise ValueError("Límite excedido")
```

Este ejemplo es básico. En producción pueden requerirse algoritmos más precisos.

## Sesiones simples

```python
import json
from uuid import uuid4


def create_session(user_id):
    session_id = str(uuid4())

    client.set(
        f"session:{session_id}",
        json.dumps({"user_id": user_id}),
        ex=3600
    )

    return session_id


def get_session(session_id):
    raw = client.get(f"session:{session_id}")

    if raw is None:
        return None

    return json.loads(raw)
```

## Errores comunes

## Instalar el paquete equivocado

La instalación correcta es:

```bash
python -m pip install redis
```

La importación es:

```python
import redis
```

## No tener servidor Redis activo

La librería puede estar instalada correctamente, pero si el servidor Redis no está ejecutándose, la conexión fallará.

## Olvidar `decode_responses=True`

Sin este parámetro, muchas respuestas pueden llegar como `bytes`.

```python
b'Python'
```

Con:

```python
decode_responses=True
```

se reciben strings.

## Guardar diccionarios directamente

Problemático:

```python
client.set("product:1", {"name": "Laptop"})
```

Mejor:

```python
import json

client.set("product:1", json.dumps({"name": "Laptop"}))
```

## No usar expiración en datos temporales

Si una clave es temporal, conviene usar `ex`.

```python
client.set("token:abc", "value", ex=3600)
```

## Usar Redis como base principal sin evaluar persistencia

Redis puede persistir datos según configuración, pero muchas veces se usa como caché o almacenamiento temporal.

Para datos críticos, conviene evaluar cuidadosamente la arquitectura.

## Usar claves poco claras

Menos claro:

```python
client.set("1", "Ana")
```

Más claro:

```python
client.set("user:1:name", "Ana")
```

## Convertir cursor o listas grandes sin control

Al usar comandos que devuelven muchas claves o muchos valores, conviene tener cuidado con memoria y rendimiento.

Para explorar claves en producción, se prefiere `scan()` frente a comandos que bloqueen o devuelvan todo de golpe.

## Usar `keys()` indiscriminadamente

Problemático en bases grandes:

```python
client.keys("*")
```

Más seguro:

```python
for key in client.scan_iter("user:*"):
    print(key)
```

## Buenas prácticas

## Usar `decode_responses=True` cuando se trabaja con texto

```python
client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)
```

## Usar variables de entorno

```python
REDIS_URL
```

## Definir nombres de claves consistentes

```text
user:1
session:abc
cache:product:10
rate:user:1
```

## Usar expiración en cachés y sesiones

```python
client.set("key", "value", ex=3600)
```

## Serializar estructuras complejas

```python
json.dumps()
json.loads()
```

## Reutilizar cliente o pool

No crear un cliente nuevo para cada operación simple en aplicaciones web.

## Usar pipelines para operaciones agrupadas

```python
pipe = client.pipeline()
```

## Evitar `keys()` en bases grandes

Usar:

```python
scan_iter()
```

## Manejar errores de conexión

```python
from redis.exceptions import RedisError
```

## Separar acceso a Redis en un módulo propio

```text
cache.py
redis_client.py
repositories.py
```

## Ejemplo integrado

```python
import json
import os
from uuid import uuid4

import redis
from dotenv import load_dotenv
from redis.exceptions import RedisError

load_dotenv()


def create_client():
    redis_url = os.getenv("REDIS_URL", "redis://localhost:6379/0")

    return redis.from_url(
        redis_url,
        decode_responses=True
    )


client = create_client()


def set_json(key, value, seconds=3600):
    client.set(
        key,
        json.dumps(value),
        ex=seconds
    )


def get_json(key):
    raw_value = client.get(key)

    if raw_value is None:
        return None

    return json.loads(raw_value)


def create_session(user_id, seconds=3600):
    session_id = str(uuid4())

    session_key = f"session:{session_id}"

    set_json(
        session_key,
        {"user_id": user_id},
        seconds=seconds
    )

    return session_id


def get_session(session_id):
    return get_json(f"session:{session_id}")


def increment_counter(name):
    key = f"counter:{name}"

    return client.incr(key)


def cache_product(product_id, product):
    key = f"cache:product:{product_id}"

    set_json(key, product, seconds=1800)


def get_cached_product(product_id):
    key = f"cache:product:{product_id}"

    return get_json(key)


try:
    client.ping()

    session_id = create_session(user_id=1)

    print("Session:", session_id)
    print("Session data:", get_session(session_id))

    cache_product(
        10,
        {
            "id": 10,
            "name": "Laptop",
            "price": 3500
        }
    )

    print("Cached product:", get_cached_product(10))
    print("Counter:", increment_counter("visits"))

except RedisError as error:
    print("Error de Redis:", error)
```

## Relación con otras librerías

`redis` se relaciona especialmente con:

```text
fastapi
flask
django
celery
rq
python-dotenv
pydantic
json
```

## Relación con Celery

Redis puede usarse como broker o backend de resultados en Celery.

Uso conceptual:

```text
FastAPI / Flask / Django -> Celery -> Redis -> workers
```

## Relación con RQ

RQ es una librería de colas de tareas basada en Redis.

Uso conceptual:

```text
Python app -> Redis Queue -> worker
```

## Relación con FastAPI y Flask

Redis se usa frecuentemente para:

```text
caché
sesiones
tokens temporales
rate limiting
colas simples
pub/sub
```

## Relación con bases relacionales

Redis no reemplaza necesariamente a PostgreSQL, MySQL o SQL Server.

En muchas arquitecturas conviven:

```text
PostgreSQL / MySQL -> fuente principal persistente
Redis              -> caché, sesiones, colas o datos temporales
```

## Orden didáctico interno

```text
1. Propósito de redis
2. Instalación
3. Conexión con Redis
4. Redis como base clave-valor
5. Strings
6. Expiraciones y TTL
7. Contadores
8. Hashes
9. Lists
10. Sets
11. Sorted sets
12. Pub/Sub
13. Streams
14. Pipelines
15. Connection pools
16. Uso asíncrono
17. Caché, sesiones y rate limiting
18. Uso con FastAPI, Flask y pandas
19. Errores comunes
20. Buenas prácticas
```