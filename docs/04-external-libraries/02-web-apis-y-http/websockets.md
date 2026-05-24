Base factual principal: `websockets` es una librería para construir clientes y servidores WebSocket en Python, con foco en corrección, simplicidad, robustez y rendimiento. Su implementación principal está construida sobre `asyncio`, aunque también existen implementaciones sobre `threading` y Sans-I/O. ([websockets][1])

Contenido propuesto:

````markdown id="gmonwg"
# `websockets`

## Propósito

`websockets` es una librería externa para construir clientes y servidores WebSocket en Python. Se utiliza cuando una aplicación necesita comunicación bidireccional y persistente entre cliente y servidor, por ejemplo en chats, paneles en tiempo real, notificaciones, monitoreo, colaboración en vivo o transmisión continua de eventos.

## Naturaleza de la librería

`websockets` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install websockets
```

La importación habitual es:

```python
import websockets
```

Normalmente se usa junto con `asyncio`:

```python
import asyncio
import websockets
```

## Idea central

HTTP tradicional funciona como un intercambio de solicitud y respuesta:

```text
cliente solicita -> servidor responde
```

WebSocket permite mantener una conexión abierta para que ambos lados puedan enviar mensajes mientras la conexión siga activa:

```text
cliente <-> servidor
```

Esto es útil cuando no basta con consultar una API ocasionalmente y se necesita comunicación continua.

## Relación con HTTP

Una conexión WebSocket comienza con un handshake HTTP. Si el servidor acepta la actualización, la comunicación cambia a WebSocket. Desde ese momento, cliente y servidor pueden enviarse mensajes por la misma conexión abierta.

Esto diferencia WebSocket de un patrón HTTP común, donde cada solicitud y respuesta se tratan como intercambios separados.

## Instalación

Instalación básica:

```bash
python -m pip install websockets
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea en `requirements.txt`:

```text
websockets==16.0
```

La versión exacta puede variar según el entorno.

## Importación

```python
import websockets
```

Con `asyncio`:

```python
import asyncio
import websockets
```

Verificación:

```python
import websockets

print(websockets.__version__)
```

## Cliente WebSocket básico

## Propósito

Un cliente WebSocket se conecta a un servidor WebSocket y puede enviar o recibir mensajes mientras la conexión esté abierta.

Ejemplo básico:

```python
import asyncio

import websockets


async def main():
    uri = "ws://localhost:8765"

    async with websockets.connect(uri) as websocket:
        await websocket.send("Hola")
        response = await websocket.recv()

        print(response)


asyncio.run(main())
```

## `websockets.connect()`

`websockets.connect()` abre una conexión WebSocket hacia una URL.

La URL normalmente empieza con:

```text
ws://
```

o con:

```text
wss://
```

`ws://` se usa para conexiones no cifradas.

`wss://` se usa para conexiones cifradas sobre TLS.

## Enviar mensajes

Para enviar mensajes se usa `send()`.

```python
await websocket.send("Hola")
```

También pueden enviarse bytes:

```python
await websocket.send(b"datos")
```

## Recibir mensajes

Para recibir mensajes se usa `recv()`.

```python
message = await websocket.recv()
```

El resultado puede ser texto o bytes, según lo que envíe el otro extremo.

## Servidor WebSocket básico

## Propósito

Un servidor WebSocket acepta conexiones entrantes y maneja mensajes enviados por los clientes.

La documentación oficial describe que el servidor crea una conexión por cliente, realiza el handshake de apertura y delega el manejo a una corrutina `handler`. :contentReference[oaicite:1]{index=1}

Ejemplo básico de servidor echo:

```python
import asyncio

from websockets.asyncio.server import serve


async def echo(websocket):
    async for message in websocket:
        await websocket.send(message)


async def main():
    async with serve(echo, "localhost", 8765):
        await asyncio.Future()


asyncio.run(main())
```

Este servidor responde con el mismo mensaje que recibe.

## `serve()`

`serve()` crea un servidor WebSocket que escucha en un host y puerto.

```python
async with serve(handler, "localhost", 8765):
    ...
```

El `handler` es la función asíncrona que se ejecuta por cada conexión.

## Handler

El handler recibe el objeto de conexión.

```python
async def handler(websocket):
    ...
```

Dentro del handler se puede:

- recibir mensajes
- enviar mensajes
- iterar sobre mensajes entrantes
- manejar cierre de conexión
- aplicar lógica de negocio

## Iteración sobre mensajes

Una forma común de procesar mensajes entrantes es:

```python
async for message in websocket:
    ...
```

Ejemplo:

```python
async def handler(websocket):
    async for message in websocket:
        print("Mensaje recibido:", message)
```

## Cliente y servidor juntos

Para probar localmente, puede ejecutarse primero el servidor:

```python
import asyncio

from websockets.asyncio.server import serve


async def echo(websocket):
    async for message in websocket:
        await websocket.send(f"Eco: {message}")


async def main():
    async with serve(echo, "localhost", 8765):
        await asyncio.Future()


asyncio.run(main())
```

Luego, en otro proceso, ejecutar el cliente:

```python
import asyncio

import websockets


async def main():
    uri = "ws://localhost:8765"

    async with websockets.connect(uri) as websocket:
        await websocket.send("Hola servidor")

        response = await websocket.recv()
        print(response)


asyncio.run(main())
```

## Conexión persistente

Una conexión WebSocket permanece abierta hasta que una de las partes la cierra o se produce un error.

Eso permite flujos como:

```python
async with websockets.connect(uri) as websocket:
    await websocket.send("mensaje 1")
    await websocket.send("mensaje 2")

    response_1 = await websocket.recv()
    response_2 = await websocket.recv()
```

## Bucle de recepción

Un cliente puede quedarse escuchando mensajes:

```python
import asyncio

import websockets


async def main():
    uri = "ws://localhost:8765"

    async with websockets.connect(uri) as websocket:
        async for message in websocket:
            print("Mensaje recibido:", message)


asyncio.run(main())
```

Este patrón se usa cuando el servidor envía eventos de forma continua.

## Enviar mensajes repetidos

```python
import asyncio

import websockets


async def main():
    uri = "ws://localhost:8765"

    async with websockets.connect(uri) as websocket:
        for number in range(3):
            message = f"Mensaje {number}"
            await websocket.send(message)

            response = await websocket.recv()
            print(response)


asyncio.run(main())
```

## Mensajes de texto y bytes

WebSocket puede transportar mensajes de texto o binarios.

Texto:

```python
await websocket.send("Hola")
```

Bytes:

```python
await websocket.send(b"\x01\x02\x03")
```

Recepción:

```python
message = await websocket.recv()
print(type(message))
```

El tipo dependerá del mensaje recibido.

## Serialización con JSON

Cuando se necesita enviar estructuras como diccionarios o listas, es común serializar a JSON.

```python
import asyncio
import json

import websockets


async def main():
    uri = "ws://localhost:8765"

    payload = {
        "event": "ping",
        "value": 1
    }

    async with websockets.connect(uri) as websocket:
        await websocket.send(json.dumps(payload))

        response = await websocket.recv()
        data = json.loads(response)

        print(data)


asyncio.run(main())
```

## Servidor con JSON

```python
import asyncio
import json

from websockets.asyncio.server import serve


async def handler(websocket):
    async for message in websocket:
        data = json.loads(message)

        response = {
            "received": data
        }

        await websocket.send(json.dumps(response))


async def main():
    async with serve(handler, "localhost", 8765):
        await asyncio.Future()


asyncio.run(main())
```

## Múltiples clientes

Un servidor WebSocket puede aceptar múltiples clientes.

Ejemplo conceptual:

```python
connected_clients = set()


async def handler(websocket):
    connected_clients.add(websocket)

    try:
        async for message in websocket:
            for client in connected_clients:
                await client.send(message)
    finally:
        connected_clients.remove(websocket)
```

Este patrón permite enviar mensajes a varios clientes conectados.

## Broadcast básico

```python
import asyncio

from websockets.asyncio.server import serve


connected_clients = set()


async def handler(websocket):
    connected_clients.add(websocket)

    try:
        async for message in websocket:
            disconnected = set()

            for client in connected_clients:
                try:
                    await client.send(message)
                except Exception:
                    disconnected.add(client)

            connected_clients.difference_update(disconnected)

    finally:
        connected_clients.discard(websocket)


async def main():
    async with serve(handler, "localhost", 8765):
        await asyncio.Future()


asyncio.run(main())
```

## Cierre de conexión

Las conexiones WebSocket pueden cerrarse normalmente o por error.

En código cliente:

```python
async with websockets.connect(uri) as websocket:
    await websocket.send("Hola")
```

Al salir del bloque `async with`, la conexión se cierra correctamente.

## Manejo básico de errores

```python
import asyncio

import websockets


async def main():
    uri = "ws://localhost:8765"

    try:
        async with websockets.connect(uri) as websocket:
            await websocket.send("Hola")
            response = await websocket.recv()
            print(response)

    except websockets.exceptions.ConnectionClosed:
        print("La conexión fue cerrada")
    except OSError as error:
        print("Error de conexión:", error)


asyncio.run(main())
```

## Timeouts

Puede usarse `asyncio.wait_for()` para limitar una operación de recepción.

```python
import asyncio

import websockets


async def main():
    uri = "ws://localhost:8765"

    async with websockets.connect(uri) as websocket:
        await websocket.send("Hola")

        response = await asyncio.wait_for(
            websocket.recv(),
            timeout=10
        )

        print(response)


asyncio.run(main())
```

## Ping y keepalive

Las conexiones WebSocket de larga duración pueden necesitar mecanismos de keepalive para detectar conexiones rotas o mantenerlas activas.

`websockets` incluye soporte para ping y pong como parte del manejo de conexiones.

En proyectos reales, los parámetros relacionados con ping deben definirse de acuerdo con las necesidades de red y del servidor.

## `ws://` y `wss://`

## `ws://`

Se usa para conexiones WebSocket sin cifrado.

```text
ws://localhost:8765
```

## `wss://`

Se usa para conexiones WebSocket cifradas.

```text
wss://example.com/socket
```

En producción normalmente se prefiere `wss://`, especialmente si se transmiten datos sensibles.

## Diferencia frente a HTTP tradicional

## HTTP tradicional

- patrón solicitud-respuesta
- cada consulta suele ser independiente
- adecuado para APIs REST
- se usa con `requests`, `httpx` o `aiohttp`

## WebSocket

- conexión persistente
- comunicación bidireccional
- adecuado para eventos en tiempo real
- permite que el servidor envíe mensajes sin esperar una solicitud nueva

## Relación con `asyncio`

`websockets` se usa principalmente con corrutinas, `async` y `await`.

Ejemplo:

```python
async def main():
    ...
```

Ejecución:

```python
asyncio.run(main())
```

Esto significa que conviene entender antes:

- funciones asíncronas
- `await`
- `async with`
- `async for`
- event loop
- tareas concurrentes

## Uso con FastAPI

FastAPI tiene soporte propio para endpoints WebSocket. Aun así, `websockets` puede ser útil para construir clientes WebSocket, pruebas, servidores independientes o herramientas de comunicación en tiempo real.

Ejemplo conceptual de cliente:

```python
import asyncio

import websockets


async def main():
    uri = "ws://localhost:8000/ws"

    async with websockets.connect(uri) as websocket:
        await websocket.send("Hola")
        print(await websocket.recv())


asyncio.run(main())
```

## Casos de uso frecuentes

## Chat

Un servidor recibe mensajes de clientes y los distribuye a otros clientes conectados.

## Paneles en tiempo real

Un servidor envía actualizaciones de métricas, eventos o estados.

## Notificaciones

El servidor informa cambios sin que el cliente consulte repetidamente.

## Monitoreo

El cliente recibe eventos continuos de un sistema observado.

## Juegos o colaboración en vivo

Los participantes intercambian eventos frecuentes por una conexión persistente.

## Errores comunes

## Usar `websockets` para una API REST común

Si solo se necesita hacer solicitudes puntuales, normalmente basta con:

```python
requests
httpx
aiohttp
```

WebSocket tiene sentido cuando se necesita conexión persistente y bidireccional.

## Olvidar `await`

Problemático:

```python
websocket.send("Hola")
```

Correcto:

```python
await websocket.send("Hola")
```

También aplica a:

```python
await websocket.recv()
```

## Usar código asíncrono fuera de una función `async`

Problemático:

```python
async with websockets.connect(uri) as websocket:
    ...
```

Debe estar dentro de una función `async`.

## No cerrar conexiones

Conviene usar `async with` para asegurar el cierre correcto:

```python
async with websockets.connect(uri) as websocket:
    ...
```

## No manejar desconexiones

En conexiones persistentes, el otro extremo puede cerrar la conexión. Conviene manejar ese caso.

```python
except websockets.exceptions.ConnectionClosed:
    ...
```

## No serializar estructuras complejas

No se debe enviar directamente un diccionario con `send()` esperando que se convierta automáticamente.

Problemático:

```python
await websocket.send({"event": "ping"})
```

Mejor:

```python
await websocket.send(json.dumps({"event": "ping"}))
```

## Buenas prácticas

## Usar WebSocket solo cuando se necesite comunicación persistente

Para solicitudes simples, suele ser mejor usar HTTP tradicional.

## Usar `async with`

```python
async with websockets.connect(uri) as websocket:
    ...
```

## Separar mensajes en funciones

```python
async def send_json(websocket, data):
    await websocket.send(json.dumps(data))
```

## Serializar estructuras con JSON

```python
await websocket.send(json.dumps(payload))
```

## Manejar desconexiones

```python
except websockets.exceptions.ConnectionClosed:
    ...
```

## Usar `wss://` en producción

```text
wss://example.com/socket
```

## Controlar timeouts cuando se espera respuesta

```python
response = await asyncio.wait_for(websocket.recv(), timeout=10)
```

## Ejemplo integrado

```python
import asyncio
import json

import websockets


async def send_json(websocket, data):
    await websocket.send(json.dumps(data))


async def receive_json(websocket):
    message = await websocket.recv()
    return json.loads(message)


async def main():
    uri = "ws://localhost:8765"

    payload = {
        "event": "ping",
        "value": 1
    }

    try:
        async with websockets.connect(uri) as websocket:
            await send_json(websocket, payload)

            response = await asyncio.wait_for(
                receive_json(websocket),
                timeout=10
            )

            print("Respuesta:", response)

    except asyncio.TimeoutError:
        print("No llegó respuesta dentro del tiempo esperado")
    except websockets.exceptions.ConnectionClosed:
        print("La conexión fue cerrada")
    except OSError as error:
        print("Error de conexión:", error)


asyncio.run(main())
```

## Relación con otras librerías

`websockets` se relaciona especialmente con:

- `asyncio`, porque su uso principal se basa en corrutinas
- `json`, para enviar estructuras como texto serializado
- `httpx` y `aiohttp`, porque también trabajan con comunicación web
- `FastAPI`, cuando se construyen aplicaciones con endpoints WebSocket
- `pytest`, para probar comportamiento de clientes o servidores
- `python-dotenv`, para cargar URLs o configuración de conexión

## Orden didáctico interno

```text
1. Propósito de websockets
2. Instalación e importación
3. Diferencia entre HTTP y WebSocket
4. Cliente básico
5. Servidor básico
6. Envío y recepción de mensajes
7. JSON sobre WebSocket
8. Múltiples clientes y broadcast
9. Manejo de errores y desconexiones
10. Buenas prácticas
```