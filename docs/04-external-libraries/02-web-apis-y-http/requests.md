# `requests`

## Propósito

`requests` es una librería externa para realizar solicitudes HTTP de forma simple y legible. Se utiliza para consumir APIs, descargar contenido, enviar datos a servidores, trabajar con respuestas JSON, automatizar consultas web y conectar scripts de Python con servicios externos.

## Naturaleza de la librería

`requests` no forma parte de la biblioteca estándar de Python. Debe instalarse antes de usarse.

```bash
python -m pip install requests
```

La importación habitual es:

```python
import requests
```

No suele usarse alias, porque el nombre `requests` ya es claro y breve.

## Idea central

El flujo básico con `requests` consiste en:

1. enviar una solicitud HTTP
2. recibir una respuesta
3. revisar el estado de la respuesta
4. extraer el contenido
5. manejar errores cuando corresponda

Ejemplo mínimo:

```python
import requests

response = requests.get("https://api.example.com/data")

print(response.status_code)
print(response.text)
```

## Conceptos previos necesarios

Para usar `requests` con claridad conviene conocer:

- cadenas de texto
- diccionarios
- listas
- manejo de excepciones
- archivos
- JSON
- URLs
- métodos HTTP básicos

## Instalación

Instalación básica:

```bash
python -m pip install requests
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea en `requirements.txt`:

```text
requests==2.32.3
```

La versión exacta puede variar según el entorno.

## Importación

```python
import requests
```

Ejemplo de verificación:

```python
import requests

print(requests.__version__)
```

## Métodos HTTP principales

`requests` permite usar los métodos HTTP más frecuentes:

```python
requests.get()
requests.post()
requests.put()
requests.patch()
requests.delete()
requests.head()
requests.options()
```

## `GET`

## Propósito

`GET` se usa para solicitar información a un servidor.

Ejemplo:

```python
import requests

url = "https://api.example.com/users"
response = requests.get(url)

print(response.status_code)
print(response.text)
```

## Uso con parámetros de consulta

Los parámetros de consulta pueden pasarse con `params`.

```python
import requests

url = "https://api.example.com/search"

params = {
    "q": "python",
    "page": 1
}

response = requests.get(url, params=params)

print(response.url)
print(response.status_code)
```

`requests` construye la URL final con los parámetros codificados correctamente.

## `POST`

## Propósito

`POST` se usa para enviar datos al servidor, por ejemplo para crear un recurso o enviar información a una API.

## Enviar JSON

```python
import requests

url = "https://api.example.com/users"

payload = {
    "name": "Ana",
    "age": 20
}

response = requests.post(url, json=payload)

print(response.status_code)
print(response.text)
```

Cuando se usa `json=`, `requests` serializa el diccionario a JSON y agrega el encabezado correspondiente.

## Enviar formulario

```python
import requests

url = "https://api.example.com/login"

data = {
    "username": "usuario",
    "password": "clave"
}

response = requests.post(url, data=data)

print(response.status_code)
```

`data=` se usa para formularios o cuerpos codificados como datos de formulario.

## `PUT`

## Propósito

`PUT` suele usarse para reemplazar completamente un recurso existente.

```python
import requests

url = "https://api.example.com/users/1"

payload = {
    "name": "Ana",
    "age": 21
}

response = requests.put(url, json=payload)

print(response.status_code)
```

## `PATCH`

## Propósito

`PATCH` suele usarse para actualizar parcialmente un recurso existente.

```python
import requests

url = "https://api.example.com/users/1"

payload = {
    "age": 21
}

response = requests.patch(url, json=payload)

print(response.status_code)
```

## `DELETE`

## Propósito

`DELETE` se usa para solicitar la eliminación de un recurso.

```python
import requests

url = "https://api.example.com/users/1"

response = requests.delete(url)

print(response.status_code)
```

## Objeto `Response`

Las funciones principales de `requests` devuelven un objeto `Response`.

```python
import requests

response = requests.get("https://api.example.com/data")

print(type(response))
```

Este objeto contiene información como:

- código de estado
- contenido de la respuesta
- encabezados
- URL final
- codificación
- cookies
- tiempo de respuesta
- cuerpo en texto, bytes o JSON

## `status_code`

Indica el código HTTP devuelto por el servidor.

```python
import requests

response = requests.get("https://api.example.com/data")

print(response.status_code)
```

Ejemplos comunes:

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

## `ok`

`response.ok` devuelve `True` si el código de estado indica una respuesta exitosa o aceptable según la lógica de la librería.

```python
import requests

response = requests.get("https://api.example.com/data")

if response.ok:
    print("Solicitud exitosa")
else:
    print("Solicitud con error")
```

## `text`

Devuelve el contenido como texto.

```python
import requests

response = requests.get("https://api.example.com/data")

print(response.text)
```

Es útil para HTML, texto plano o respuestas que deben inspeccionarse como cadena.

## `content`

Devuelve el contenido como bytes.

```python
import requests

response = requests.get("https://api.example.com/file")

content = response.content

print(type(content))
print(len(content))
```

Es útil para archivos, imágenes, PDFs o contenido binario.

## `json()`

Convierte la respuesta JSON a objetos de Python.

```python
import requests

response = requests.get("https://api.example.com/users")

data = response.json()

print(type(data))
print(data)
```

Según la respuesta, el resultado puede ser:

- `dict`
- `list`
- combinaciones anidadas de listas y diccionarios

## Error al usar `json()`

`response.json()` falla si la respuesta no contiene JSON válido.

```python
import requests

response = requests.get("https://example.com")

data = response.json()
```

Esto puede generar una excepción si el contenido es HTML u otro formato.

## `headers`

Permite consultar los encabezados de la respuesta.

```python
import requests

response = requests.get("https://api.example.com/data")

print(response.headers)
print(response.headers.get("Content-Type"))
```

## `url`

Muestra la URL final de la solicitud.

```python
import requests

response = requests.get(
    "https://api.example.com/search",
    params={"q": "python"}
)

print(response.url)
```

## `encoding`

Indica o permite ajustar la codificación usada para interpretar `response.text`.

```python
import requests

response = requests.get("https://api.example.com/data")

print(response.encoding)
```

## `raise_for_status()`

## Propósito

`raise_for_status()` lanza una excepción si la respuesta tiene un código HTTP de error.

```python
import requests

response = requests.get("https://api.example.com/data")
response.raise_for_status()

print(response.text)
```

Esto permite detectar errores HTTP de forma más explícita.

## Uso recomendado

```python
import requests

url = "https://api.example.com/data"

try:
    response = requests.get(url, timeout=10)
    response.raise_for_status()
    data = response.json()
    print(data)
except requests.exceptions.RequestException as error:
    print("Error en la solicitud:", error)
```

## `timeout`

## Propósito

`timeout` define cuánto tiempo esperar antes de considerar que la solicitud falló.

```python
import requests

response = requests.get("https://api.example.com/data", timeout=10)
```

Usar `timeout` es una buena práctica. Sin timeout, un script puede quedarse esperando indefinidamente si el servidor no responde.

## Timeout con conexión y lectura

También puede pasarse una tupla:

```python
import requests

response = requests.get(
    "https://api.example.com/data",
    timeout=(3, 10)
)
```

En esta forma:

- el primer valor corresponde al tiempo máximo para conectar
- el segundo valor corresponde al tiempo máximo para leer la respuesta

## Encabezados

Los encabezados se pasan con `headers`.

```python
import requests

url = "https://api.example.com/data"

headers = {
    "Accept": "application/json",
    "User-Agent": "python-requests-demo"
}

response = requests.get(url, headers=headers)

print(response.status_code)
```

## Autorización con token

Un patrón común consiste en enviar un token en el encabezado `Authorization`.

```python
import requests

url = "https://api.example.com/data"

headers = {
    "Authorization": "Bearer TOKEN",
    "Accept": "application/json"
}

response = requests.get(url, headers=headers, timeout=10)

print(response.status_code)
```

En proyectos reales, el token no debería escribirse directamente en el código. Conviene leerlo desde variables de entorno.

## Uso con `python-dotenv`

Archivo `.env`:

```env
API_URL=https://api.example.com/data
API_TOKEN=token_local
```

Código:

```python
import os
import requests
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

response = requests.get(api_url, headers=headers, timeout=10)
response.raise_for_status()

data = response.json()

print(data)
```

## Parámetros de consulta

Los parámetros de consulta deben pasarse con `params`, no concatenarse manualmente en la URL.

Menos recomendable:

```python
url = "https://api.example.com/search?q=python&page=1"
response = requests.get(url)
```

Más claro:

```python
import requests

url = "https://api.example.com/search"

params = {
    "q": "python",
    "page": 1
}

response = requests.get(url, params=params, timeout=10)

print(response.url)
```

## Envío de datos JSON

Para enviar JSON se usa `json=`.

```python
import requests

url = "https://api.example.com/users"

payload = {
    "name": "Ana",
    "email": "ana@example.com"
}

response = requests.post(url, json=payload, timeout=10)

print(response.status_code)
print(response.text)
```

## Envío de datos de formulario

Para formularios se usa `data=`.

```python
import requests

url = "https://api.example.com/login"

form_data = {
    "username": "usuario",
    "password": "clave"
}

response = requests.post(url, data=form_data, timeout=10)

print(response.status_code)
```

## Descarga de archivos

Para descargar contenido binario puede usarse `response.content`.

```python
import requests
from pathlib import Path

url = "https://example.com/archivo.pdf"
output_path = Path("archivo.pdf")

response = requests.get(url, timeout=30)
response.raise_for_status()

output_path.write_bytes(response.content)

print("Archivo descargado")
```

## Descarga por streaming

Para archivos grandes, conviene usar `stream=True`.

```python
import requests
from pathlib import Path

url = "https://example.com/archivo-grande.zip"
output_path = Path("archivo-grande.zip")

with requests.get(url, stream=True, timeout=30) as response:
    response.raise_for_status()

    with output_path.open("wb") as file:
        for chunk in response.iter_content(chunk_size=8192):
            if chunk:
                file.write(chunk)

print("Descarga completada")
```

## Subida de archivos

Para enviar archivos se usa `files`.

```python
import requests
from pathlib import Path

url = "https://api.example.com/upload"
file_path = Path("reporte.xlsx")

with file_path.open("rb") as file:
    files = {
        "file": file
    }

    response = requests.post(url, files=files, timeout=30)

print(response.status_code)
```

## Sesiones

## Propósito

`requests.Session()` permite reutilizar configuración entre varias solicitudes.

Puede reutilizar:

- encabezados
- cookies
- parámetros comunes
- conexión subyacente

Ejemplo:

```python
import requests

session = requests.Session()

session.headers.update({
    "Accept": "application/json",
    "User-Agent": "python-requests-demo"
})

response_1 = session.get("https://api.example.com/users", timeout=10)
response_2 = session.get("https://api.example.com/products", timeout=10)

print(response_1.status_code)
print(response_2.status_code)
```

## Uso de sesión con `with`

```python
import requests

with requests.Session() as session:
    session.headers.update({"Accept": "application/json"})

    response = session.get("https://api.example.com/data", timeout=10)
    response.raise_for_status()

    data = response.json()
    print(data)
```

## Cookies

`requests` puede manejar cookies, especialmente mediante sesiones.

```python
import requests

with requests.Session() as session:
    response = session.get("https://example.com", timeout=10)
    print(session.cookies)
```

## Excepciones principales

Las excepciones de `requests` se encuentran en `requests.exceptions`.

Ejemplos importantes:

```python
requests.exceptions.RequestException
requests.exceptions.Timeout
requests.exceptions.ConnectionError
requests.exceptions.HTTPError
```

## Manejo general de errores

```python
import requests

url = "https://api.example.com/data"

try:
    response = requests.get(url, timeout=10)
    response.raise_for_status()
    data = response.json()
    print(data)
except requests.exceptions.Timeout:
    print("La solicitud excedió el tiempo máximo de espera")
except requests.exceptions.ConnectionError:
    print("No se pudo conectar con el servidor")
except requests.exceptions.HTTPError as error:
    print("Error HTTP:", error)
except requests.exceptions.RequestException as error:
    print("Error general en la solicitud:", error)
```

## Patrón recomendado para APIs

```python
import requests


def fetch_json(url, params=None, headers=None, timeout=10):
    response = requests.get(
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

## Relación con JSON

`requests` se combina frecuentemente con JSON.

## Respuesta JSON

```python
response = requests.get(url, timeout=10)
data = response.json()
```

## Envío de JSON

```python
response = requests.post(url, json=payload, timeout=10)
```

Cuando se usa `json=`, no es necesario convertir manualmente con `json.dumps()` en la mayoría de casos comunes.

## Relación con archivos

`requests` permite descargar archivos y luego guardarlos.

```python
from pathlib import Path
import requests

response = requests.get("https://example.com/data.csv", timeout=10)
response.raise_for_status()

Path("data.csv").write_text(response.text, encoding="utf-8")
```

Para binarios:

```python
Path("archivo.bin").write_bytes(response.content)
```

## Relación con `pandas`

`requests` puede usarse para obtener datos y luego pasarlos a `pandas`.

```python
import requests
import pandas as pd

url = "https://api.example.com/data"

response = requests.get(url, timeout=10)
response.raise_for_status()

data = response.json()
df = pd.DataFrame(data)

print(df.head())
```

## Relación con scraping

Para scraping básico, `requests` puede obtener HTML y `beautifulsoup4` puede analizarlo.

```python
import requests
from bs4 import BeautifulSoup

url = "https://example.com"

response = requests.get(url, timeout=10)
response.raise_for_status()

soup = BeautifulSoup(response.text, "html.parser")

print(soup.title)
```

## Casos de uso frecuentes

## Consumir una API

```python
import requests

response = requests.get("https://api.example.com/users", timeout=10)
response.raise_for_status()

users = response.json()

print(users)
```

## Enviar datos JSON

```python
import requests

payload = {
    "name": "Ana"
}

response = requests.post(
    "https://api.example.com/users",
    json=payload,
    timeout=10
)

print(response.status_code)
```

## Descargar un archivo

```python
from pathlib import Path
import requests

response = requests.get("https://example.com/report.xlsx", timeout=30)
response.raise_for_status()

Path("report.xlsx").write_bytes(response.content)
```

## Consultar con parámetros

```python
import requests

params = {
    "serie": "inflacion",
    "anio": 2024
}

response = requests.get(
    "https://api.example.com/series",
    params=params,
    timeout=10
)

print(response.url)
```

## Errores comunes

## No usar `timeout`

Problemático:

```python
response = requests.get(url)
```

Si el servidor no responde, el programa puede quedar esperando demasiado tiempo.

Mejor:

```python
response = requests.get(url, timeout=10)
```

## No revisar el código de estado

Problemático:

```python
response = requests.get(url, timeout=10)
data = response.json()
```

Si la respuesta fue `404` o `500`, el programa podría fallar después de forma menos clara.

Mejor:

```python
response = requests.get(url, timeout=10)
response.raise_for_status()
data = response.json()
```

## Usar `json()` sobre una respuesta que no es JSON

Problemático:

```python
data = response.json()
```

si el contenido real es HTML, texto o un archivo.

Conviene revisar:

```python
print(response.headers.get("Content-Type"))
print(response.text[:200])
```

## Concatenar parámetros manualmente

Menos recomendable:

```python
url = "https://api.example.com/search?q=python&page=1"
```

Más claro:

```python
params = {"q": "python", "page": 1}
response = requests.get(url, params=params, timeout=10)
```

## Escribir credenciales directamente en el código

Problemático:

```python
headers = {
    "Authorization": "Bearer clave_real"
}
```

Mejor usar variables de entorno.

## Confundir `data=` con `json=`

`json=` se usa para enviar JSON.

```python
requests.post(url, json=payload)
```

`data=` se usa para formularios u otro cuerpo manual.

```python
requests.post(url, data=form_data)
```

## No cerrar archivos al subirlos

Menos recomendable:

```python
file = open("reporte.xlsx", "rb")
requests.post(url, files={"file": file})
```

Mejor:

```python
with open("reporte.xlsx", "rb") as file:
    requests.post(url, files={"file": file})
```

## Buenas prácticas

## Usar `timeout` en solicitudes externas

```python
requests.get(url, timeout=10)
```

## Usar `raise_for_status()`

```python
response.raise_for_status()
```

Esto permite detectar errores HTTP de forma explícita.

## Pasar parámetros con `params`

```python
requests.get(url, params=params, timeout=10)
```

## Enviar JSON con `json=`

```python
requests.post(url, json=payload, timeout=10)
```

## Leer credenciales desde variables de entorno

```python
api_token = os.getenv("API_TOKEN")
```

## Manejar excepciones de red

```python
except requests.exceptions.RequestException as error:
    print(error)
```

## Usar sesiones cuando se repite configuración

```python
with requests.Session() as session:
    session.headers.update(headers)
    response = session.get(url, timeout=10)
```

## Separar la lógica HTTP en funciones

```python
def fetch_json(url, params=None):
    response = requests.get(url, params=params, timeout=10)
    response.raise_for_status()
    return response.json()
```

## Ejemplo integrado

```python
import os
from pathlib import Path

import requests
from dotenv import load_dotenv


def fetch_json(url, token=None, params=None, timeout=10):
    headers = {
        "Accept": "application/json"
    }

    if token is not None:
        headers["Authorization"] = f"Bearer {token}"

    response = requests.get(
        url,
        params=params,
        headers=headers,
        timeout=timeout
    )

    response.raise_for_status()

    return response.json()


def save_json_text(data, output_path):
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
save_json_text(data, output_path)

print("Respuesta guardada correctamente")
```

## Relación con otras librerías

`requests` se relaciona especialmente con:

- `python-dotenv`, para cargar URLs, tokens y claves de API
- `json`, para entender estructuras JSON
- `pandas`, para convertir respuestas en tablas
- `beautifulsoup4`, para analizar HTML descargado
- `pathlib`, para guardar archivos descargados
- `pytest`, para probar funciones que consumen APIs
- `httpx`, como alternativa moderna con soporte síncrono y asíncrono

## Orden didáctico interno

```text
1. Propósito de requests
2. Instalación e importación
3. Métodos HTTP principales
4. Objeto Response
5. status_code, text, content y json()
6. Parámetros, headers y payloads
7. Timeout y manejo de errores
8. Descarga y subida de archivos
9. Sesiones
10. Buenas prácticas
```