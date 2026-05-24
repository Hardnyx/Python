# `aiohttp`

## Propósito

`aiohttp` es una librería externa para trabajar con HTTP de forma asíncrona usando `asyncio`. Puede utilizarse como cliente HTTP para consumir APIs, descargar contenido y realizar múltiples solicitudes concurrentes. También permite construir servidores HTTP, aunque en esta sección se prioriza su uso como cliente.

## Naturaleza de la librería

`aiohttp` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install aiohttp
```

La importación habitual es:

```python
import aiohttp
```

Normalmente se usa junto con `asyncio`:

```python
import asyncio
import aiohttp
```

## Idea central

El flujo básico con `aiohttp` consiste en:

1. definir una función asíncrona
2. crear una sesión HTTP con `aiohttp.ClientSession`
3. realizar solicitudes con `await` y `async with`
4. leer la respuesta de forma asíncrona
5. cerrar correctamente la sesión

Ejemplo mínimo:

```python
import asyncio

import aiohttp


async def main():
    async with aiohttp.ClientSession() as session:
        async with session.get("https://api.example.com/data") as response:
            print(response.status)
            text = await response.text()
            print(text)


asyncio.run(main())
```

## Relación con `requests` y `httpx`

`requests` es una librería síncrona.

```python
import requests

response = requests.get("https://api.example.com/data")
print(response.text)
```

`aiohttp` es asíncrona.

```python
import aiohttp

async with aiohttp.ClientSession() as session:
    async with session.get("https://api.example.com/data") as response:
        text = await response.text()
```

`httpx` puede trabajar tanto en modo síncrono como asíncrono.

`aiohttp` tiene sentido especialmente cuando el proyecto ya trabaja con `asyncio` o cuando se necesitan muchas operaciones de red concurrentes.

## Instalación

Instalación básica:

```bash
python -m pip install aiohttp
```

Instalación con dependencias opcionales de mejora de rendimiento:

```bash
python -m pip install "aiohttp[speedups]"
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea en `requirements.txt`:

```text
aiohttp==3.13.5
```

La versión exacta puede variar según el entorno.

## Importación

```python
import aiohttp
import asyncio
```

Verificación:

```python
import aiohttp

print(aiohttp.__version__)
```

## `ClientSession`

## Propósito

`aiohttp.ClientSession` representa una sesión HTTP reutilizable.

Una sesión permite manejar:

- conexiones
- cookies
- encabezados comunes
- configuración compartida
- solicitudes al mismo servicio

La sesión debe cerrarse correctamente. Por eso se usa normalmente con `async with`.

```python
import aiohttp


async with aiohttp.ClientSession() as session:
    ...
```

## No crear una sesión por cada solicitud

Menos recomendable:

```python
import aiohttp


async def fetch(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()
```

Más recomendable cuando se harán varias solicitudes:

```python
import aiohttp


async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()


async with aiohttp.ClientSession() as session:
    html = await fetch(session, "https://example.com")
```

La sesión debería reutilizarse para un grupo de solicitudes relacionadas.

## Métodos HTTP principales

Una sesión permite usar métodos HTTP como:

```python
session.get()
session.post()
session.put()
session.patch()
session.delete()
session.head()
session.options()
```

Todos deben usarse dentro de una función asíncrona.

## `GET`

## Propósito

`GET` se usa para solicitar información a un servidor.

```python
import asyncio

import aiohttp


async def main():
    async with aiohttp.ClientSession() as session:
        async with session.get("https://api.example.com/users") as response:
            print(response.status)
            text = await response.text()
            print(text)


asyncio.run(main())
```

## Parámetros de consulta

Los parámetros de consulta se pasan con `params`.

```python
import asyncio

import aiohttp


async def main():
    url = "https://api.example.com/search"

    params = {
        "q": "python",
        "page": 1
    }

    async with aiohttp.ClientSession() as session:
        async with session.get(url, params=params) as response:
            print(response.url)
            print(response.status)


asyncio.run(main())
```

## `POST`

## Propósito

`POST` se usa para enviar datos al servidor.

## Enviar JSON

```python
import asyncio

import aiohttp


async def main():
    url = "https://api.example.com/users"

    payload = {
        "name": "Ana",
        "age": 20
    }

    async with aiohttp.ClientSession() as session:
        async with session.post(url, json=payload) as response:
            print(response.status)
            print(await response.text())


asyncio.run(main())
```

## Enviar formulario

```python
import asyncio

import aiohttp


async def main():
    url = "https://api.example.com/login"

    data = {
        "username": "usuario",
        "password": "clave"
    }

    async with aiohttp.ClientSession() as session:
        async with session.post(url, data=data) as response:
            print(response.status)


asyncio.run(main())
```

## Otros métodos HTTP

```python
async with session.put(url, json=payload) as response:
    ...

async with session.patch(url, json=payload) as response:
    ...

async with session.delete(url) as response:
    ...
```

## Objeto `ClientResponse`

Las solicitudes con `aiohttp` devuelven un objeto de respuesta, generalmente llamado `response`.

Ese objeto permite consultar:

- estado HTTP
- encabezados
- URL final
- cookies
- contenido como texto
- contenido como bytes
- contenido como JSON

## `status`

En `aiohttp`, el código de estado se consulta con `response.status`.

```python
async with session.get(url) as response:
    print(response.status)
```

## `headers`

```python
async with session.get(url) as response:
    print(response.headers)
    print(response.headers.get("content-type"))
```

## `url`

```python
async with session.get(url, params={"q": "python"}) as response:
    print(response.url)
```

## Leer texto con `text()`

En `aiohttp`, la lectura del cuerpo es asíncrona. Por eso se usa `await`.

```python
async with session.get(url) as response:
    text = await response.text()
    print(text)
```

## Leer bytes con `read()`

```python
async with session.get(url) as response:
    content = await response.read()
    print(type(content))
```

`read()` devuelve bytes.

## Leer JSON con `json()`

```python
async with session.get(url) as response:
    data = await response.json()
    print(data)
```

El resultado suele ser un diccionario, una lista o estructuras anidadas equivalentes.

## Revisión del estado HTTP

`aiohttp` no usa `raise_for_status()` exactamente igual que `requests`, pero el objeto de respuesta dispone de esa operación.

```python
async with session.get(url) as response:
    response.raise_for_status()
    data = await response.json()
```

También puede revisarse manualmente:

```python
async with session.get(url) as response:
    if response.status >= 400:
        raise RuntimeError(f"Error HTTP: {response.status}")

    data = await response.json()
```

## Timeouts

Es recomendable definir límites de espera para evitar que una solicitud quede bloqueada demasiado tiempo.

```python
import aiohttp

timeout = aiohttp.ClientTimeout(total=10)

async with aiohttp.ClientSession(timeout=timeout) as session:
    async with session.get(url) as response:
        print(response.status)
```

También puede pasarse timeout por solicitud.

```python
timeout = aiohttp.ClientTimeout(total=10)

async with session.get(url, timeout=timeout) as response:
    ...
```

## Encabezados

Los encabezados se pasan con `headers`.

```python
headers = {
    "Accept": "application/json",
    "User-Agent": "aiohttp-demo"
}

async with aiohttp.ClientSession(headers=headers) as session:
    async with session.get(url) as response:
        print(response.status)
```

También pueden pasarse por solicitud:

```python
async with session.get(url, headers=headers) as response:
    ...
```

## Autorización con token

```python
headers = {
    "Authorization": "Bearer TOKEN",
    "Accept": "application/json"
}

async with aiohttp.ClientSession(headers=headers) as session:
    async with session.get(url) as response:
        response.raise_for_status()
        data = await response.json()
```

En proyectos reales, el token debería leerse desde variables de entorno.

## Uso con `python-dotenv`

Archivo `.env`:

```env
API_URL=https://api.example.com/data
API_TOKEN=token_local
```

Código:

```python
import asyncio
import os

import aiohttp
from dotenv import load_dotenv


async def main():
    load_dotenv()

    api_url = os.getenv("API_URL")
    api_token = os.getenv("API_TOKEN")

    if api_url is None:
        raise ValueError("Falta configurar API_URL")

    if api_token is None:
        raise ValueError("Falta configurar API_TOKEN")

    headers = {
        "Authorization": f"Bearer {api_token}",
        "Accept": "application/json"
    }

    timeout = aiohttp.ClientTimeout(total=10)

    async with aiohttp.ClientSession(headers=headers, timeout=timeout) as session:
        async with session.get(api_url) as response:
            response.raise_for_status()
            data = await response.json()
            print(data)


asyncio.run(main())
```

## Solicitudes concurrentes

Una ventaja importante de `aiohttp` es realizar varias solicitudes de red de forma concurrente con `asyncio.gather()`.

```python
import asyncio

import aiohttp


async def fetch_json(session, url):
    async with session.get(url) as response:
        response.raise_for_status()
        return await response.json()


async def main():
    urls = [
        "https://api.example.com/data/1",
        "https://api.example.com/data/2",
        "https://api.example.com/data/3"
    ]

    timeout = aiohttp.ClientTimeout(total=10)

    async with aiohttp.ClientSession(timeout=timeout) as session:
        results = await asyncio.gather(
            *(fetch_json(session, url) for url in urls)
        )

    print(results)


asyncio.run(main())
```

## Control básico de concurrencia

Cuando hay muchas solicitudes, puede ser conveniente limitar la concurrencia con `asyncio.Semaphore`.

```python
import asyncio

import aiohttp


async def fetch_text(session, url, semaphore):
    async with semaphore:
        async with session.get(url) as response:
            response.raise_for_status()
            return await response.text()


async def main():
    urls = [
        "https://example.com",
        "https://example.org",
        "https://example.net"
    ]

    semaphore = asyncio.Semaphore(5)

    async with aiohttp.ClientSession() as session:
        results = await asyncio.gather(
            *(fetch_text(session, url, semaphore) for url in urls)
        )

    print(results)


asyncio.run(main())
```

## Descarga de archivos

## Archivo completo en memoria

```python
import asyncio
from pathlib import Path

import aiohttp


async def download_file(url, output_path):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            response.raise_for_status()
            content = await response.read()

    output_path.write_bytes(content)


asyncio.run(
    download_file(
        "https://example.com/archivo.pdf",
        Path("archivo.pdf")
    )
)
```

## Descarga por bloques

Para archivos grandes, conviene escribir por bloques.

```python
import asyncio
from pathlib import Path

import aiohttp


async def download_file(url, output_path):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            response.raise_for_status()

            with output_path.open("wb") as file:
                async for chunk in response.content.iter_chunked(8192):
                    file.write(chunk)


asyncio.run(
    download_file(
        "https://example.com/archivo-grande.zip",
        Path("archivo-grande.zip")
    )
)
```

## Subida de archivos

```python
import asyncio
from pathlib import Path

import aiohttp


async def upload_file(url, file_path):
    data = aiohttp.FormData()

    with file_path.open("rb") as file:
        data.add_field(
            "file",
            file,
            filename=file_path.name,
            content_type="application/octet-stream"
        )

        async with aiohttp.ClientSession() as session:
            async with session.post(url, data=data) as response:
                response.raise_for_status()
                return await response.text()


asyncio.run(
    upload_file(
        "https://api.example.com/upload",
        Path("reporte.xlsx")
    )
)
```

## Cookies

`ClientSession` puede manejar cookies entre solicitudes.

```python
async with aiohttp.ClientSession() as session:
    async with session.get("https://example.com") as response:
        print(response.cookies)
```

## `base_url`

Si se hacen varias solicitudes al mismo servidor, se puede usar una URL base.

```python
import asyncio

import aiohttp


async def main():
    async with aiohttp.ClientSession("https://api.example.com") as session:
        async with session.get("/users") as response:
            print(response.status)

        async with session.get("/products") as response:
            print(response.status)


asyncio.run(main())
```

## Excepciones frecuentes

Al trabajar con red pueden aparecer errores como:

- problemas de conexión
- timeouts
- errores HTTP
- errores de decodificación JSON
- respuestas inesperadas

Ejemplo de manejo general:

```python
import asyncio

import aiohttp


async def main():
    url = "https://api.example.com/data"

    try:
        timeout = aiohttp.ClientTimeout(total=10)

        async with aiohttp.ClientSession(timeout=timeout) as session:
            async with session.get(url) as response:
                response.raise_for_status()
                data = await response.json()
                print(data)

    except aiohttp.ClientResponseError as error:
        print("Error de respuesta HTTP:", error)
    except aiohttp.ClientConnectionError as error:
        print("Error de conexión:", error)
    except asyncio.TimeoutError:
        print("La solicitud excedió el tiempo máximo")
    except aiohttp.ClientError as error:
        print("Error general de aiohttp:", error)


asyncio.run(main())
```

## Patrón recomendado para APIs

```python
import aiohttp


async def fetch_json(session, url, params=None):
    async with session.get(url, params=params) as response:
        response.raise_for_status()
        return await response.json()
```

Uso:

```python
import asyncio

import aiohttp


async def main():
    timeout = aiohttp.ClientTimeout(total=10)

    async with aiohttp.ClientSession(timeout=timeout) as session:
        data = await fetch_json(
            session,
            "https://api.example.com/data",
            params={"page": 1}
        )

    print(data)


asyncio.run(main())
```

## Relación con JSON

## Leer JSON

```python
async with session.get(url) as response:
    data = await response.json()
```

## Enviar JSON

```python
async with session.post(url, json=payload) as response:
    ...
```

## Relación con archivos

Para texto:

```python
from pathlib import Path

text = await response.text()
Path("response.txt").write_text(text, encoding="utf-8")
```

Para binarios:

```python
content = await response.read()
Path("archivo.bin").write_bytes(content)
```

## Relación con scraping

`aiohttp` puede descargar HTML y `beautifulsoup4` puede analizarlo.

```python
import asyncio

import aiohttp
from bs4 import BeautifulSoup


async def main():
    url = "https://example.com"

    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            response.raise_for_status()
            html = await response.text()

    soup = BeautifulSoup(html, "html.parser")

    print(soup.title)


asyncio.run(main())
```

## Casos de uso frecuentes

## Consumir una API asíncronamente

```python
async with aiohttp.ClientSession() as session:
    async with session.get(url) as response:
        response.raise_for_status()
        data = await response.json()
```

## Descargar varias URLs concurrentemente

```python
results = await asyncio.gather(
    *(fetch_json(session, url) for url in urls)
)
```

## Descargar archivos grandes por bloques

```python
async for chunk in response.content.iter_chunked(8192):
    file.write(chunk)
```

## Usar una sesión por aplicación o grupo de solicitudes

```python
async with aiohttp.ClientSession() as session:
    ...
```

## Errores comunes

## Usar `aiohttp` fuera de una función `async`

Problemático:

```python
async with aiohttp.ClientSession() as session:
    ...
```

Debe estar dentro de una función `async`.

## Olvidar `await`

Problemático:

```python
data = response.json()
```

Correcto:

```python
data = await response.json()
```

También aplica a:

```python
await response.text()
await response.read()
```

## Crear una sesión por cada request

Menos recomendable cuando hay múltiples solicitudes:

```python
async def fetch(url):
    async with aiohttp.ClientSession() as session:
        ...
```

Mejor reutilizar una sesión para varias solicitudes relacionadas.

## No cerrar la sesión

Debe usarse `async with aiohttp.ClientSession()` para asegurar el cierre correcto.

## No revisar el estado HTTP

Problemático:

```python
data = await response.json()
```

Mejor:

```python
response.raise_for_status()
data = await response.json()
```

## No definir timeout

En solicitudes externas conviene definir límites de espera.

```python
timeout = aiohttp.ClientTimeout(total=10)
```

## Confundir `response.status` con `response.status_code`

En `requests` y `httpx` suele usarse:

```python
response.status_code
```

En `aiohttp` se usa:

```python
response.status
```

## Buenas prácticas

## Usar `ClientSession`

```python
async with aiohttp.ClientSession() as session:
    ...
```

## Reutilizar la sesión

Una sesión por aplicación o por grupo de solicitudes suele ser mejor que una sesión por cada request.

## Usar `await` al leer el cuerpo

```python
text = await response.text()
data = await response.json()
content = await response.read()
```

## Usar `raise_for_status()`

```python
response.raise_for_status()
```

## Definir timeouts

```python
timeout = aiohttp.ClientTimeout(total=10)
```

## Limitar concurrencia cuando hay muchas solicitudes

```python
semaphore = asyncio.Semaphore(5)
```

## Leer credenciales desde variables de entorno

```python
api_token = os.getenv("API_TOKEN")
```

## Separar lógica HTTP en funciones

```python
async def fetch_json(session, url):
    async with session.get(url) as response:
        response.raise_for_status()
        return await response.json()
```

## Ejemplo integrado

```python
import asyncio
import os
from pathlib import Path

import aiohttp
from dotenv import load_dotenv


async def fetch_json(session, url, token=None, params=None):
    headers = {
        "Accept": "application/json"
    }

    if token is not None:
        headers["Authorization"] = f"Bearer {token}"

    async with session.get(
        url,
        params=params,
        headers=headers
    ) as response:
        response.raise_for_status()
        return await response.json()


async def main():
    load_dotenv()

    api_url = os.getenv("API_URL")
    api_token = os.getenv("API_TOKEN")

    if api_url is None:
        raise ValueError("Falta configurar API_URL")

    timeout = aiohttp.ClientTimeout(total=10)

    async with aiohttp.ClientSession(timeout=timeout) as session:
        data = await fetch_json(
            session,
            api_url,
            token=api_token,
            params={"page": 1}
        )

    Path("response.txt").write_text(
        str(data),
        encoding="utf-8"
    )

    print("Respuesta guardada correctamente")


asyncio.run(main())
```

## Relación con otras librerías

`aiohttp` se relaciona especialmente con:

- `asyncio`, porque funciona sobre programación asíncrona
- `requests`, como alternativa síncrona más simple
- `httpx`, como alternativa con modo síncrono y asíncrono
- `python-dotenv`, para cargar URLs y tokens
- `beautifulsoup4`, para analizar HTML descargado
- `pandas`, para convertir respuestas JSON en estructuras tabulares
- `FastAPI`, cuando una aplicación asíncrona necesita consultar servicios externos

## Orden didáctico interno

```text
1. Propósito de aiohttp
2. Instalación e importación
3. Relación con requests y httpx
4. ClientSession
5. Métodos HTTP principales
6. Lectura de respuestas con await
7. Timeouts, headers y tokens
8. Solicitudes concurrentes
9. Descarga y subida de archivos
10. Manejo de errores
11. Buenas prácticas
```