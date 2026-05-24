El siguiente archivo debería ser:

```text id="0kz5vi"
docs/04-external-libraries/02-web-apis-y-http/httpx.md
```

Base factual principal: `HTTPX` es un cliente HTTP para Python con API síncrona y asíncrona, soporte para HTTP/1.1 y HTTP/2, timeouts estrictos por defecto y una API ampliamente compatible con `requests`. ([Httpx][1])

Contenido propuesto:

````markdown id="xyoqmc"
# `httpx`

## Propósito

`httpx` es una librería externa para realizar solicitudes HTTP en Python. Se utiliza para consumir APIs, enviar datos a servicios externos, descargar contenido, trabajar con respuestas JSON y ejecutar solicitudes tanto de forma síncrona como asíncrona.

Su principal diferencia frente a `requests` es que `httpx` ofrece una API muy similar, pero añade soporte nativo para programación asíncrona y HTTP/2.

## Naturaleza de la librería

`httpx` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install httpx
```

La importación habitual es:

```python
import httpx
```

No suele usarse alias, porque `httpx` ya es un nombre breve y reconocible.

## Idea central

El flujo básico con `httpx` consiste en:

1. enviar una solicitud HTTP
2. recibir una respuesta
3. revisar el código de estado
4. extraer texto, bytes o JSON
5. manejar errores
6. usar cliente síncrono o asíncrono según el contexto

Ejemplo mínimo:

```python
import httpx

response = httpx.get("https://api.example.com/data")

print(response.status_code)
print(response.text)
```

## Relación con `requests`

`httpx` se parece mucho a `requests` en su uso básico.

Ejemplo con `requests`:

```python
import requests

response = requests.get("https://api.example.com/data")
```

Ejemplo con `httpx`:

```python
import httpx

response = httpx.get("https://api.example.com/data")
```

La diferencia aparece con más claridad cuando se necesita:

- soporte asíncrono
- clientes reutilizables
- HTTP/2
- timeouts más explícitos
- integración con aplicaciones modernas como APIs asíncronas

## Instalación

Instalación básica:

```bash
python -m pip install httpx
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea en `requirements.txt`:

```text
httpx==0.27.0
```

La versión exacta puede variar según el entorno.

## Importación

```python
import httpx
```

Verificación:

```python
import httpx

print(httpx.__version__)
```

## Métodos HTTP principales

`httpx` permite usar los métodos HTTP más frecuentes:

```python
httpx.get()
httpx.post()
httpx.put()
httpx.patch()
httpx.delete()
httpx.head()
httpx.options()
```

## `GET`

## Propósito

`GET` se usa para solicitar información a un servidor.

```python
import httpx

url = "https://api.example.com/users"

response = httpx.get(url)

print(response.status_code)
print(response.text)
```

## Uso con parámetros de consulta

Los parámetros deben pasarse con `params`.

```python
import httpx

url = "https://api.example.com/search"

params = {
    "q": "python",
    "page": 1
}

response = httpx.get(url, params=params)

print(response.url)
print(response.status_code)
```

## `POST`

## Propósito

`POST` se usa para enviar datos al servidor.

## Enviar JSON

```python
import httpx

url = "https://api.example.com/users"

payload = {
    "name": "Ana",
    "age": 20
}

response = httpx.post(url, json=payload)

print(response.status_code)
print(response.text)
```

Cuando se usa `json=`, `httpx` serializa el diccionario a JSON y envía el cuerpo correspondiente.

## Enviar formulario

```python
import httpx

url = "https://api.example.com/login"

data = {
    "username": "usuario",
    "password": "clave"
}

response = httpx.post(url, data=data)

print(response.status_code)
```

## `PUT`

```python
import httpx

url = "https://api.example.com/users/1"

payload = {
    "name": "Ana",
    "age": 21
}

response = httpx.put(url, json=payload)

print(response.status_code)
```

## `PATCH`

```python
import httpx

url = "https://api.example.com/users/1"

payload = {
    "age": 21
}

response = httpx.patch(url, json=payload)

print(response.status_code)
```

## `DELETE`

```python
import httpx

url = "https://api.example.com/users/1"

response = httpx.delete(url)

print(response.status_code)
```

## Objeto `Response`

Las solicitudes devuelven un objeto `Response`.

```python
import httpx

response = httpx.get("https://api.example.com/data")

print(type(response))
```

Este objeto contiene información como:

- código de estado
- contenido textual
- contenido binario
- encabezados
- URL final
- cookies
- codificación
- respuesta JSON

## `status_code`

```python
import httpx

response = httpx.get("https://api.example.com/data")

print(response.status_code)
```

Códigos frecuentes:

```text
200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
500 Internal Server Error
```

## `text`

Devuelve la respuesta como texto.

```python
import httpx

response = httpx.get("https://api.example.com/data")

print(response.text)
```

## `content`

Devuelve la respuesta como bytes.

```python
import httpx

response = httpx.get("https://api.example.com/file")

content = response.content

print(type(content))
print(len(content))
```

## `json()`

Convierte una respuesta JSON a objetos de Python.

```python
import httpx

response = httpx.get("https://api.example.com/users")

data = response.json()

print(type(data))
print(data)
```

El resultado puede ser:

- `dict`
- `list`
- estructuras anidadas de listas y diccionarios

## `headers`

```python
import httpx

response = httpx.get("https://api.example.com/data")

print(response.headers)
print(response.headers.get("content-type"))
```

## `url`

```python
import httpx

response = httpx.get(
    "https://api.example.com/search",
    params={"q": "python"}
)

print(response.url)
```

## `raise_for_status()`

`raise_for_status()` lanza una excepción cuando la respuesta tiene un código HTTP de error.

```python
import httpx

response = httpx.get("https://api.example.com/data")
response.raise_for_status()

print(response.text)
```

Patrón recomendado:

```python
import httpx

url = "https://api.example.com/data"

try:
    response = httpx.get(url, timeout=10.0)
    response.raise_for_status()
    data = response.json()
    print(data)
except httpx.HTTPError as error:
    print("Error HTTP:", error)
```

## Timeouts

`httpx` usa timeouts como parte normal de su diseño. Aun así, conviene declararlos explícitamente para que el código sea claro.

```python
import httpx

response = httpx.get("https://api.example.com/data", timeout=10.0)
```

## Timeout detallado

También puede configurarse un objeto `Timeout`.

```python
import httpx

timeout = httpx.Timeout(
    timeout=10.0,
    connect=3.0
)

response = httpx.get(
    "https://api.example.com/data",
    timeout=timeout
)

print(response.status_code)
```

## Encabezados

Los encabezados se pasan con `headers`.

```python
import httpx

url = "https://api.example.com/data"

headers = {
    "Accept": "application/json",
    "User-Agent": "python-httpx-demo"
}

response = httpx.get(url, headers=headers, timeout=10.0)

print(response.status_code)
```

## Autorización con token

```python
import httpx

url = "https://api.example.com/data"

headers = {
    "Authorization": "Bearer TOKEN",
    "Accept": "application/json"
}

response = httpx.get(url, headers=headers, timeout=10.0)

print(response.status_code)
```

En proyectos reales, el token debería leerse desde variables de entorno y no escribirse directamente en el código.

## Uso con `python-dotenv`

Archivo `.env`:

```env
API_URL=https://api.example.com/data
API_TOKEN=token_local
```

Código:

```python
import os

import httpx
from dotenv import load_dotenv

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

response = httpx.get(api_url, headers=headers, timeout=10.0)
response.raise_for_status()

data = response.json()

print(data)
```

## Cliente síncrono

## Propósito

`httpx.Client` permite reutilizar configuración entre varias solicitudes.

Puede reutilizar:

- encabezados
- cookies
- conexión subyacente
- configuración base
- timeouts
- autenticación

Ejemplo:

```python
import httpx

with httpx.Client(
    base_url="https://api.example.com",
    timeout=10.0,
    headers={"Accept": "application/json"}
) as client:
    response_1 = client.get("/users")
    response_2 = client.get("/products")

    print(response_1.status_code)
    print(response_2.status_code)
```

## Ventaja de `Client`

Usar `Client` suele ser mejor cuando se harán varias solicitudes al mismo servicio.

Menos conveniente:

```python
import httpx

response_1 = httpx.get("https://api.example.com/users")
response_2 = httpx.get("https://api.example.com/products")
```

Más organizado:

```python
import httpx

with httpx.Client(base_url="https://api.example.com") as client:
    response_1 = client.get("/users")
    response_2 = client.get("/products")
```

## Cliente asíncrono

## Propósito

`httpx.AsyncClient` permite realizar solicitudes HTTP usando `async` y `await`.

Esto es útil en aplicaciones asíncronas, como servicios web modernos, crawlers o tareas donde se realizan muchas operaciones de red.

## Ejemplo básico

```python
import asyncio

import httpx


async def main():
    async with httpx.AsyncClient() as client:
        response = await client.get("https://api.example.com/data")
        response.raise_for_status()

        data = response.json()
        print(data)


asyncio.run(main())
```

## Diferencia entre cliente síncrono y asíncrono

## `Client`

```python
with httpx.Client() as client:
    response = client.get(url)
```

Se usa en código síncrono tradicional.

## `AsyncClient`

```python
async with httpx.AsyncClient() as client:
    response = await client.get(url)
```

Se usa dentro de funciones `async`.

## Solicitudes concurrentes

Una ventaja de `AsyncClient` es que permite ejecutar varias solicitudes de red de forma concurrente.

```python
import asyncio

import httpx


async def fetch(client, url):
    response = await client.get(url)
    response.raise_for_status()
    return response.json()


async def main():
    urls = [
        "https://api.example.com/data/1",
        "https://api.example.com/data/2",
        "https://api.example.com/data/3"
    ]

    async with httpx.AsyncClient(timeout=10.0) as client:
        results = await asyncio.gather(
            *(fetch(client, url) for url in urls)
        )

    print(results)


asyncio.run(main())
```

## Uso en FastAPI

En aplicaciones asíncronas, como APIs construidas con FastAPI, suele ser más apropiado usar `AsyncClient` para llamadas HTTP externas.

```python
import httpx
from fastapi import FastAPI

app = FastAPI()


@app.get("/external-data")
async def external_data():
    async with httpx.AsyncClient(timeout=10.0) as client:
        response = await client.get("https://api.example.com/data")
        response.raise_for_status()
        return response.json()
```

## HTTP/2

`httpx` puede trabajar con HTTP/2 cuando se instala y configura el soporte correspondiente.

Instalación con soporte HTTP/2:

```bash
python -m pip install "httpx[http2]"
```

Uso:

```python
import httpx

with httpx.Client(http2=True) as client:
    response = client.get("https://api.example.com/data")
    print(response.http_version)
```

## Descarga de archivos

Para archivos pequeños o medianos puede usarse `response.content`.

```python
from pathlib import Path

import httpx

url = "https://example.com/archivo.pdf"
output_path = Path("archivo.pdf")

response = httpx.get(url, timeout=30.0)
response.raise_for_status()

output_path.write_bytes(response.content)

print("Archivo descargado")
```

## Streaming de respuestas

Para archivos grandes o respuestas largas, conviene usar streaming.

```python
from pathlib import Path

import httpx

url = "https://example.com/archivo-grande.zip"
output_path = Path("archivo-grande.zip")

with httpx.stream("GET", url, timeout=30.0) as response:
    response.raise_for_status()

    with output_path.open("wb") as file:
        for chunk in response.iter_bytes():
            file.write(chunk)

print("Descarga completada")
```

## Streaming asíncrono

```python
from pathlib import Path

import httpx
import asyncio


async def download_file(url, output_path):
    async with httpx.AsyncClient(timeout=30.0) as client:
        async with client.stream("GET", url) as response:
            response.raise_for_status()

            with output_path.open("wb") as file:
                async for chunk in response.aiter_bytes():
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
from pathlib import Path

import httpx

url = "https://api.example.com/upload"
file_path = Path("reporte.xlsx")

with file_path.open("rb") as file:
    files = {
        "file": file
    }

    response = httpx.post(url, files=files, timeout=30.0)

response.raise_for_status()

print(response.status_code)
```

## Excepciones principales

Las excepciones principales se encuentran dentro de `httpx`.

Ejemplos frecuentes:

```python
httpx.HTTPError
httpx.RequestError
httpx.HTTPStatusError
httpx.TimeoutException
httpx.ConnectError
```

## Manejo general de errores

```python
import httpx

url = "https://api.example.com/data"

try:
    response = httpx.get(url, timeout=10.0)
    response.raise_for_status()
    data = response.json()
    print(data)
except httpx.TimeoutException:
    print("La solicitud excedió el tiempo máximo de espera")
except httpx.HTTPStatusError as error:
    print("Error de estado HTTP:", error)
except httpx.RequestError as error:
    print("Error de conexión o solicitud:", error)
except httpx.HTTPError as error:
    print("Error HTTP general:", error)
```

## Patrón recomendado para APIs

```python
import httpx


def fetch_json(url, params=None, headers=None, timeout=10.0):
    response = httpx.get(
        url,
        params=params,
        headers=headers,
        timeout=timeout
    )

    response.raise_for_status()

    return response.json()


data = fetch_json(
    "https://api.example.com/data",
    params={"page": 1}
)

print(data)
```

## Patrón recomendado con `Client`

```python
import httpx


def fetch_user(client, user_id):
    response = client.get(f"/users/{user_id}")
    response.raise_for_status()
    return response.json()


with httpx.Client(
    base_url="https://api.example.com",
    timeout=10.0,
    headers={"Accept": "application/json"}
) as client:
    user = fetch_user(client, 1)

print(user)
```

## Patrón recomendado con `AsyncClient`

```python
import asyncio

import httpx


async def fetch_user(client, user_id):
    response = await client.get(f"/users/{user_id}")
    response.raise_for_status()
    return response.json()


async def main():
    async with httpx.AsyncClient(
        base_url="https://api.example.com",
        timeout=10.0,
        headers={"Accept": "application/json"}
    ) as client:
        user = await fetch_user(client, 1)

    print(user)


asyncio.run(main())
```

## Relación con JSON

## Leer JSON

```python
response = httpx.get(url, timeout=10.0)
data = response.json()
```

## Enviar JSON

```python
response = httpx.post(url, json=payload, timeout=10.0)
```

## Relación con archivos

```python
from pathlib import Path

import httpx

response = httpx.get("https://example.com/data.csv", timeout=10.0)
response.raise_for_status()

Path("data.csv").write_text(response.text, encoding="utf-8")
```

Para binarios:

```python
Path("archivo.bin").write_bytes(response.content)
```

## Relación con `pandas`

```python
import httpx
import pandas as pd

url = "https://api.example.com/data"

response = httpx.get(url, timeout=10.0)
response.raise_for_status()

data = response.json()
df = pd.DataFrame(data)

print(df.head())
```

## Relación con scraping

```python
import httpx
from bs4 import BeautifulSoup

url = "https://example.com"

response = httpx.get(url, timeout=10.0)
response.raise_for_status()

soup = BeautifulSoup(response.text, "html.parser")

print(soup.title)
```

## Casos de uso frecuentes

## Consumir una API

```python
import httpx

response = httpx.get("https://api.example.com/users", timeout=10.0)
response.raise_for_status()

users = response.json()

print(users)
```

## Enviar datos JSON

```python
import httpx

payload = {
    "name": "Ana"
}

response = httpx.post(
    "https://api.example.com/users",
    json=payload,
    timeout=10.0
)

response.raise_for_status()

print(response.status_code)
```

## Realizar varias solicitudes al mismo servidor

```python
import httpx

with httpx.Client(base_url="https://api.example.com", timeout=10.0) as client:
    users = client.get("/users")
    products = client.get("/products")

    print(users.status_code)
    print(products.status_code)
```

## Realizar solicitudes asíncronas

```python
import asyncio

import httpx


async def main():
    async with httpx.AsyncClient(timeout=10.0) as client:
        response = await client.get("https://api.example.com/data")
        response.raise_for_status()
        print(response.json())


asyncio.run(main())
```

## Errores comunes

## Usar `AsyncClient` sin `await`

Problemático:

```python
import httpx

async def main():
    async with httpx.AsyncClient() as client:
        response = client.get("https://api.example.com/data")
```

Correcto:

```python
response = await client.get("https://api.example.com/data")
```

## Usar `AsyncClient` fuera de una función `async`

Problemático:

```python
async with httpx.AsyncClient() as client:
    response = await client.get(url)
```

Debe ejecutarse dentro de una función `async`.

## No cerrar clientes

Menos recomendable:

```python
client = httpx.Client()
response = client.get(url)
```

Más seguro:

```python
with httpx.Client() as client:
    response = client.get(url)
```

En asíncrono:

```python
async with httpx.AsyncClient() as client:
    response = await client.get(url)
```

## No usar `raise_for_status()`

Problemático:

```python
response = httpx.get(url)
data = response.json()
```

Mejor:

```python
response = httpx.get(url, timeout=10.0)
response.raise_for_status()
data = response.json()
```

## Confundir `data=` con `json=`

`json=` se usa para enviar JSON.

```python
httpx.post(url, json=payload)
```

`data=` se usa para formularios u otro cuerpo manual.

```python
httpx.post(url, data=form_data)
```

## Intentar usar `response.json()` sobre contenido no JSON

Conviene revisar el encabezado:

```python
print(response.headers.get("content-type"))
```

## Buenas prácticas

## Usar `timeout`

```python
httpx.get(url, timeout=10.0)
```

## Usar `raise_for_status()`

```python
response.raise_for_status()
```

## Usar `Client` para varias solicitudes al mismo servicio

```python
with httpx.Client(base_url="https://api.example.com") as client:
    response = client.get("/data")
```

## Usar `AsyncClient` en código asíncrono

```python
async with httpx.AsyncClient() as client:
    response = await client.get(url)
```

## Usar `params` para query strings

```python
httpx.get(url, params={"q": "python"})
```

## Usar `json` para cuerpos JSON

```python
httpx.post(url, json=payload)
```

## Leer secretos desde variables de entorno

```python
api_token = os.getenv("API_TOKEN")
```

## Separar lógica HTTP en funciones

```python
def fetch_json(url):
    response = httpx.get(url, timeout=10.0)
    response.raise_for_status()
    return response.json()
```

## Ejemplo integrado síncrono

```python
import os
from pathlib import Path

import httpx
from dotenv import load_dotenv


def fetch_json(url, token=None, params=None, timeout=10.0):
    headers = {
        "Accept": "application/json"
    }

    if token is not None:
        headers["Authorization"] = f"Bearer {token}"

    response = httpx.get(
        url,
        params=params,
        headers=headers,
        timeout=timeout
    )

    response.raise_for_status()

    return response.json()


def save_response(data, output_path):
    output_path.write_text(
        str(data),
        encoding="utf-8"
    )


load_dotenv()

api_url = os.getenv("API_URL")
api_token = os.getenv("API_TOKEN")

if api_url is None:
    raise ValueError("Falta configurar API_URL")

data = fetch_json(
    api_url,
    token=api_token,
    params={"page": 1}
)

output_path = Path("response.txt")
save_response(data, output_path)

print("Respuesta guardada correctamente")
```

## Ejemplo integrado asíncrono

```python
import asyncio
import os
from pathlib import Path

import httpx
from dotenv import load_dotenv


async def fetch_json(client, url, token=None, params=None):
    headers = {
        "Accept": "application/json"
    }

    if token is not None:
        headers["Authorization"] = f"Bearer {token}"

    response = await client.get(
        url,
        params=params,
        headers=headers
    )

    response.raise_for_status()

    return response.json()


async def main():
    load_dotenv()

    api_url = os.getenv("API_URL")
    api_token = os.getenv("API_TOKEN")

    if api_url is None:
        raise ValueError("Falta configurar API_URL")

    async with httpx.AsyncClient(timeout=10.0) as client:
        data = await fetch_json(
            client,
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

`httpx` se relaciona especialmente con:

- `requests`, como alternativa más tradicional y síncrona
- `aiohttp`, como alternativa asíncrona más orientada a clientes y servidores HTTP
- `python-dotenv`, para cargar URLs y tokens
- `FastAPI`, por su utilidad dentro de aplicaciones asíncronas
- `pandas`, para convertir respuestas en estructuras tabulares
- `beautifulsoup4`, para analizar HTML descargado
- `pytest`, para probar clientes HTTP y servicios

## Orden didáctico interno

```text
1. Propósito de httpx
2. Instalación e importación
3. Relación con requests
4. Métodos HTTP principales
5. Objeto Response
6. Parámetros, headers y payloads
7. Client síncrono
8. AsyncClient
9. HTTP/2
10. Streaming
11. Manejo de errores
12. Buenas prácticas
```
````

[1]: https://www.python-httpx.org/?utm_source=chatgpt.com "HTTPX"
