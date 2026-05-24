# `fastapi`

## Propósito

`fastapi` es una librería externa para crear APIs web en Python. Se utiliza para construir servicios HTTP, endpoints REST, backends para aplicaciones web, microservicios, APIs internas, servicios de datos, integraciones con frontend y aplicaciones que necesitan validación automática de entradas y documentación interactiva.

Su principal fortaleza es combinar rutas web, type hints, validación de datos, serialización, documentación automática y soporte para programación asíncrona.

## Naturaleza de la librería

`fastapi` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install "fastapi[standard]"
```

La importación principal es:

```python
from fastapi import FastAPI
```

También suele usarse con:

```python
from pydantic import BaseModel
```

y con un servidor ASGI como Uvicorn.

## Idea central

La idea principal de FastAPI es definir una aplicación web mediante una instancia de `FastAPI` y declarar rutas mediante decoradores.

Ejemplo mínimo:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"message": "Hello, FastAPI"}
```

Cada función decorada representa una operación disponible en la API.

## Instalación

Instalación recomendada para uso general:

```bash
python -m pip install "fastapi[standard]"
```

Esto instala FastAPI junto con dependencias útiles para desarrollo y ejecución.

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
fastapi==0.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
from fastapi import FastAPI
```

Verificación:

```python
import fastapi

print(fastapi.__version__)
```

## Crear una aplicación

```python
from fastapi import FastAPI

app = FastAPI()
```

El objeto `app` representa la aplicación web.

Sobre ese objeto se registran rutas, middlewares, routers, dependencias y configuración general.

## Primer endpoint

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {
        "status": "ok",
        "message": "API funcionando"
    }
```

Este endpoint responde a solicitudes `GET` sobre la ruta raíz `/`.

## Ejecutar una aplicación

Si el archivo se llama:

```text
main.py
```

puede ejecutarse con:

```bash
fastapi dev main.py
```

También puede ejecutarse con Uvicorn:

```bash
uvicorn main:app --reload
```

Interpretación:

```text
main -> archivo main.py
app  -> variable app dentro de main.py
--reload -> recarga automática en desarrollo
```

## Documentación automática

FastAPI genera documentación interactiva automáticamente.

Rutas frecuentes:

```text
/docs
/redoc
/openapi.json
```

## `/docs`

Muestra una interfaz interactiva basada en Swagger UI.

```text
http://127.0.0.1:8000/docs
```

## `/redoc`

Muestra documentación alternativa basada en ReDoc.

```text
http://127.0.0.1:8000/redoc
```

## `/openapi.json`

Expone el esquema OpenAPI generado por la aplicación.

```text
http://127.0.0.1:8000/openapi.json
```

## Métodos HTTP principales

FastAPI permite registrar rutas para distintos métodos HTTP.

```python
@app.get("/items")
def read_items():
    ...

@app.post("/items")
def create_item():
    ...

@app.put("/items/{item_id}")
def update_item(item_id: int):
    ...

@app.patch("/items/{item_id}")
def partial_update_item(item_id: int):
    ...

@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    ...
```

## `GET`

Se usa para consultar información.

```python
@app.get("/items")
def read_items():
    return [
        {"id": 1, "name": "Item A"},
        {"id": 2, "name": "Item B"}
    ]
```

## `POST`

Se usa para crear o enviar información.

```python
@app.post("/items")
def create_item():
    return {"message": "Item creado"}
```

## `PUT`

Se usa para reemplazar un recurso.

```python
@app.put("/items/{item_id}")
def update_item(item_id: int):
    return {"item_id": item_id, "message": "Item actualizado"}
```

## `PATCH`

Se usa para actualizar parcialmente un recurso.

```python
@app.patch("/items/{item_id}")
def partial_update_item(item_id: int):
    return {"item_id": item_id, "message": "Item actualizado parcialmente"}
```

## `DELETE`

Se usa para eliminar un recurso.

```python
@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    return {"item_id": item_id, "message": "Item eliminado"}
```

## Parámetros de ruta

Los parámetros de ruta se declaran dentro de llaves.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

Si se llama a:

```text
/items/10
```

FastAPI pasa `10` como argumento `item_id`.

## Validación de parámetros de ruta

Los type hints permiten validar y convertir valores.

```python
@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

Si se envía un valor no convertible a entero:

```text
/items/abc
```

FastAPI devuelve un error de validación.

## Query parameters

Los query parameters se declaran como argumentos que no están en la ruta.

```python
@app.get("/items")
def read_items(skip: int = 0, limit: int = 10):
    return {
        "skip": skip,
        "limit": limit
    }
```

Ejemplo de solicitud:

```text
/items?skip=10&limit=5
```

## Query parameters opcionales

```python
@app.get("/search")
def search_items(q: str | None = None):
    return {"query": q}
```

Si no se envía `q`, el valor será `None`.

## Parámetros con restricciones

FastAPI permite usar `Query` para agregar validaciones.

```python
from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items")
def read_items(
    limit: int = Query(default=10, ge=1, le=100)
):
    return {"limit": limit}
```

Interpretación:

```text
ge=1   -> mayor o igual que 1
le=100 -> menor o igual que 100
```

## Cuerpo de solicitud

Para recibir JSON en el cuerpo de una solicitud se suele usar un modelo Pydantic.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    price: float
    is_active: bool = True


@app.post("/items")
def create_item(item: Item):
    return item
```

Ejemplo de cuerpo JSON:

```json
{
  "name": "Laptop",
  "price": 3500,
  "is_active": true
}
```

## Modelos Pydantic

Los modelos Pydantic definen la estructura esperada de los datos.

```python
from pydantic import BaseModel


class Client(BaseModel):
    document_id: str
    name: str
    age: int | None = None
```

FastAPI usa estos modelos para:

```text
validar entradas
convertir tipos
generar documentación
serializar respuestas
mostrar esquemas en OpenAPI
```

## Campos opcionales

```python
class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
```

`description` puede enviarse como texto o puede omitirse.

## Valores por defecto

```python
class Item(BaseModel):
    name: str
    price: float
    is_active: bool = True
```

Si `is_active` no se envía, toma el valor `True`.

## Cuerpo, ruta y query juntos

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    price: float


@app.put("/items/{item_id}")
def update_item(
    item_id: int,
    item: Item,
    notify: bool = False
):
    return {
        "item_id": item_id,
        "item": item,
        "notify": notify
    }
```

Ejemplo:

```text
PUT /items/10?notify=true
```

Con cuerpo:

```json
{
  "name": "Laptop",
  "price": 3500
}
```

## Modelo de respuesta

`response_model` permite declarar la estructura de salida.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class ItemResponse(BaseModel):
    id: int
    name: str
    price: float


@app.get("/items/{item_id}", response_model=ItemResponse)
def read_item(item_id: int):
    return {
        "id": item_id,
        "name": "Laptop",
        "price": 3500,
        "internal_code": "ABC"
    }
```

Aunque la función devuelve `internal_code`, la respuesta final se ajusta al modelo declarado.

## Status codes

Puede indicarse el código HTTP de respuesta.

```python
from fastapi import FastAPI, status

app = FastAPI()


@app.post("/items", status_code=status.HTTP_201_CREATED)
def create_item():
    return {"message": "Item creado"}
```

También puede usarse directamente:

```python
@app.post("/items", status_code=201)
def create_item():
    return {"message": "Item creado"}
```

## Errores HTTP

Para devolver errores controlados se usa `HTTPException`.

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()


@app.get("/items/{item_id}")
def read_item(item_id: int):
    if item_id <= 0:
        raise HTTPException(
            status_code=400,
            detail="item_id debe ser mayor que cero"
        )

    return {"item_id": item_id}
```

## Códigos frecuentes

```text
200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
422 Unprocessable Entity
500 Internal Server Error
```

## Respuestas personalizadas

FastAPI permite devolver distintos tipos de respuesta.

```python
from fastapi.responses import JSONResponse


@app.get("/custom")
def custom_response():
    return JSONResponse(
        status_code=200,
        content={"message": "Respuesta personalizada"}
    )
```

## `async` y `def`

FastAPI permite definir endpoints síncronos o asíncronos.

## Endpoint síncrono

```python
@app.get("/sync")
def read_sync():
    return {"mode": "sync"}
```

## Endpoint asíncrono

```python
@app.get("/async")
async def read_async():
    return {"mode": "async"}
```

## Regla práctica

Usar `async def` cuando dentro del endpoint se llamen operaciones asíncronas, por ejemplo:

```text
consultas HTTP asíncronas con httpx.AsyncClient
drivers async de base de datos
operaciones concurrentes no bloqueantes
```

Usar `def` es suficiente para lógica normal, cálculos simples o llamadas bloqueantes tradicionales.

## Rutas con tags

Los tags ayudan a organizar la documentación.

```python
@app.get("/items", tags=["items"])
def read_items():
    return []
```

## Descripción de una ruta

```python
@app.get(
    "/items/{item_id}",
    tags=["items"],
    summary="Obtener item",
    description="Devuelve la información de un item por identificador."
)
def read_item(item_id: int):
    return {"item_id": item_id}
```

## Configuración de la aplicación

```python
app = FastAPI(
    title="Mi API",
    description="API de ejemplo construida con FastAPI",
    version="0.1.0"
)
```

Esta información aparece en la documentación automática.

## APIRouter

`APIRouter` permite organizar rutas por módulos.

Ejemplo de archivo:

```text
app/routers/items.py
```

Código:

```python
from fastapi import APIRouter

router = APIRouter(
    prefix="/items",
    tags=["items"]
)


@router.get("/")
def read_items():
    return []
```

En el archivo principal:

```python
from fastapi import FastAPI
from app.routers import items

app = FastAPI()

app.include_router(items.router)
```

## Estructura recomendada básica

```text
app/
├─ main.py
├─ routers/
│  ├─ __init__.py
│  └─ items.py
├─ schemas/
│  ├─ __init__.py
│  └─ item.py
├─ services/
│  ├─ __init__.py
│  └─ item_service.py
└─ core/
   ├─ __init__.py
   └─ config.py
```

## Separación por responsabilidades

```text
main.py       -> instancia de app y registro de routers
routers/      -> endpoints HTTP
schemas/      -> modelos Pydantic
services/     -> lógica de negocio
core/         -> configuración general
```

## Dependencias

FastAPI permite declarar dependencias con `Depends`.

```python
from fastapi import Depends, FastAPI

app = FastAPI()


def get_settings():
    return {
        "app_name": "Mi API"
    }


@app.get("/settings")
def read_settings(settings: dict = Depends(get_settings)):
    return settings
```

## Uso típico de dependencias

Las dependencias son útiles para:

```text
configuración
conexiones a base de datos
autenticación
validación compartida
servicios reutilizables
parámetros comunes
```

## Dependencia para paginación

```python
from fastapi import Depends, FastAPI, Query

app = FastAPI()


def pagination_params(
    skip: int = Query(default=0, ge=0),
    limit: int = Query(default=10, ge=1, le=100)
):
    return {
        "skip": skip,
        "limit": limit
    }


@app.get("/items")
def read_items(pagination: dict = Depends(pagination_params)):
    return pagination
```

## Variables de entorno

FastAPI suele combinarse con variables de entorno para configurar URLs, credenciales o parámetros de ejecución.

Ejemplo con `python-dotenv`:

```python
import os

from dotenv import load_dotenv
from fastapi import FastAPI

load_dotenv()

app = FastAPI()

database_url = os.getenv("DATABASE_URL")


@app.get("/config")
def read_config():
    return {
        "database_configured": database_url is not None
    }
```

## CORS

Cuando un frontend en otro origen necesita consumir la API, puede configurarse CORS con `CORSMiddleware`.

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost:3000",
    "http://127.0.0.1:3000"
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"]
)
```

## Middleware

Un middleware se ejecuta antes y después del procesamiento de una solicitud.

```python
import time

from fastapi import FastAPI, Request

app = FastAPI()


@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    start_time = time.perf_counter()

    response = await call_next(request)

    process_time = time.perf_counter() - start_time
    response.headers["X-Process-Time"] = str(process_time)

    return response
```

Usos frecuentes:

```text
medición de tiempos
logging de solicitudes
headers comunes
control de CORS
manejo transversal de request y response
```

## Background tasks

Las background tasks permiten ejecutar tareas después de devolver una respuesta.

```python
from fastapi import BackgroundTasks, FastAPI

app = FastAPI()


def write_log(message: str):
    with open("log.txt", "a", encoding="utf-8") as file:
        file.write(message + "\n")


@app.post("/send")
def send_message(
    message: str,
    background_tasks: BackgroundTasks
):
    background_tasks.add_task(write_log, message)

    return {"status": "accepted"}
```

Este patrón sirve para tareas ligeras que no requieren bloquear la respuesta.

## Archivos estáticos

FastAPI puede servir archivos estáticos usando `StaticFiles`.

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles

app = FastAPI()

app.mount(
    "/static",
    StaticFiles(directory="static"),
    name="static"
)
```

Esto permite servir archivos desde la carpeta:

```text
static/
```

## Subida de archivos

Para recibir archivos se usan `File` y `UploadFile`.

```python
from fastapi import FastAPI, File, UploadFile

app = FastAPI()


@app.post("/upload")
async def upload_file(file: UploadFile = File(...)):
    content = await file.read()

    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "size": len(content)
    }
```

## Descargar archivos

```python
from fastapi import FastAPI
from fastapi.responses import FileResponse

app = FastAPI()


@app.get("/download")
def download_file():
    return FileResponse(
        path="report.xlsx",
        filename="report.xlsx",
        media_type="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
    )
```

## Headers

```python
from fastapi import FastAPI, Header

app = FastAPI()


@app.get("/headers")
def read_headers(user_agent: str | None = Header(default=None)):
    return {"user_agent": user_agent}
```

## Cookies

```python
from fastapi import Cookie, FastAPI

app = FastAPI()


@app.get("/cookies")
def read_cookie(session_id: str | None = Cookie(default=None)):
    return {"session_id": session_id}
```

## Seguridad básica con API Key

```python
from fastapi import Depends, FastAPI, Header, HTTPException

app = FastAPI()


def verify_api_key(x_api_key: str | None = Header(default=None)):
    if x_api_key != "secret":
        raise HTTPException(
            status_code=401,
            detail="API key inválida"
        )

    return x_api_key


@app.get("/protected")
def read_protected(api_key: str = Depends(verify_api_key)):
    return {"status": "authorized"}
```

En proyectos reales, las claves no deberían escribirse directamente en el código.

## Testing

FastAPI puede probarse con `TestClient`.

```python
from fastapi import FastAPI
from fastapi.testclient import TestClient

app = FastAPI()


@app.get("/")
def read_root():
    return {"status": "ok"}


client = TestClient(app)


def test_read_root():
    response = client.get("/")

    assert response.status_code == 200
    assert response.json() == {"status": "ok"}
```

## Pruebas con endpoints POST

```python
from fastapi import FastAPI
from fastapi.testclient import TestClient
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    price: float


@app.post("/items")
def create_item(item: Item):
    return item


client = TestClient(app)


def test_create_item():
    response = client.post(
        "/items",
        json={
            "name": "Laptop",
            "price": 3500
        }
    )

    assert response.status_code == 200
    assert response.json()["name"] == "Laptop"
```

## Uso con pandas

FastAPI puede exponer resultados procesados con pandas.

```python
import pandas as pd
from fastapi import FastAPI

app = FastAPI()


@app.get("/summary")
def read_summary():
    df = pd.DataFrame({
        "product": ["Laptop", "Mouse"],
        "amount": [3500, 800]
    })

    total_amount = df["amount"].sum()

    return {
        "total_amount": float(total_amount)
    }
```

## Uso con archivos

FastAPI puede generar archivos y devolverlos como descarga.

```python
from pathlib import Path

import pandas as pd
from fastapi import FastAPI
from fastapi.responses import FileResponse

app = FastAPI()


@app.get("/report")
def create_report():
    output_path = Path("report.xlsx")

    df = pd.DataFrame({
        "product": ["Laptop", "Mouse"],
        "amount": [3500, 800]
    })

    df.to_excel(output_path, index=False)

    return FileResponse(
        path=output_path,
        filename="report.xlsx"
    )
```

## Uso con bases de datos

FastAPI suele combinarse con librerías de base de datos como:

```text
SQLAlchemy
SQLModel
asyncpg
psycopg
sqlite3
```

Una estructura común separa:

```text
router -> endpoint HTTP
service -> lógica de negocio
repository -> acceso a datos
schema -> entrada y salida
```

## Uso con frontend

Un frontend puede consumir FastAPI mediante solicitudes HTTP.

Ejemplo conceptual:

```text
React / Vue / Angular / HTML -> fetch() -> FastAPI -> JSON
```

En estos casos CORS suele ser relevante durante desarrollo.

## Uso con Docker

FastAPI puede ejecutarse dentro de contenedores.

Ejemplo conceptual de comando:

```bash
fastapi run app/main.py
```

También puede usarse Uvicorn:

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

## Proyecto mínimo

Estructura:

```text
app/
└─ main.py
```

Archivo:

```text
app/main.py
```

Código:

```python
from fastapi import FastAPI

app = FastAPI(
    title="Example API",
    version="0.1.0"
)


@app.get("/")
def read_root():
    return {
        "status": "ok",
        "service": "Example API"
    }


@app.get("/health")
def health_check():
    return {"status": "ok"}
```

Ejecución:

```bash
fastapi dev app/main.py
```

o:

```bash
uvicorn app.main:app --reload
```

## Proyecto con router

Estructura:

```text
app/
├─ main.py
└─ routers/
   ├─ __init__.py
   └─ items.py
```

Archivo:

```text
app/routers/items.py
```

Código:

```python
from fastapi import APIRouter
from pydantic import BaseModel

router = APIRouter(
    prefix="/items",
    tags=["items"]
)


class Item(BaseModel):
    id: int
    name: str
    price: float


items = [
    Item(id=1, name="Laptop", price=3500),
    Item(id=2, name="Mouse", price=80)
]


@router.get("/", response_model=list[Item])
def read_items():
    return items


@router.get("/{item_id}", response_model=Item)
def read_item(item_id: int):
    for item in items:
        if item.id == item_id:
            return item

    raise ValueError("Item no encontrado")
```

Archivo:

```text
app/main.py
```

Código:

```python
from fastapi import FastAPI

from app.routers import items

app = FastAPI(
    title="Example API",
    version="0.1.0"
)

app.include_router(items.router)


@app.get("/")
def read_root():
    return {"status": "ok"}
```

## Mejor manejo de error en routers

En endpoints reales conviene usar `HTTPException` para errores HTTP.

```python
from fastapi import HTTPException


@router.get("/{item_id}", response_model=Item)
def read_item(item_id: int):
    for item in items:
        if item.id == item_id:
            return item

    raise HTTPException(
        status_code=404,
        detail="Item no encontrado"
    )
```

## Errores comunes

## Olvidar crear la instancia `app`

Problemático:

```python
from fastapi import FastAPI


@app.get("/")
def read_root():
    return {"status": "ok"}
```

Correcto:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"status": "ok"}
```

## Ejecutar mal el módulo con Uvicorn

Si la estructura es:

```text
app/main.py
```

y la variable se llama `app`, el comando es:

```bash
uvicorn app.main:app --reload
```

No:

```bash
uvicorn main.py
```

## Confundir parámetro de ruta con query parameter

Ruta:

```python
@app.get("/items/{item_id}")
def read_item(item_id: int):
    ...
```

Query parameter:

```python
@app.get("/items")
def read_items(limit: int = 10):
    ...
```

## Usar modelos Pydantic como si fueran diccionarios siempre

```python
def create_item(item: Item):
    return item["name"]
```

Más claro:

```python
def create_item(item: Item):
    return item.name
```

Si se necesita diccionario:

```python
item.model_dump()
```

## Devolver errores con `ValueError` en endpoints

Problemático:

```python
raise ValueError("No encontrado")
```

Mejor:

```python
raise HTTPException(
    status_code=404,
    detail="No encontrado"
)
```

## Escribir secretos directamente en el código

Problemático:

```python
API_KEY = "clave_real"
```

Mejor:

```python
import os

api_key = os.getenv("API_KEY")
```

## No separar routers en proyectos medianos

Poner todos los endpoints en `main.py` puede volverse difícil de mantener.

Conviene separar por dominio:

```text
routers/items.py
routers/users.py
routers/reports.py
```

## Usar `async def` con operaciones bloqueantes pesadas

Un endpoint `async def` no convierte automáticamente una operación bloqueante en no bloqueante.

Si se llama una función bloqueante dentro de `async def`, puede afectar el rendimiento.

## No validar CORS en frontend

Cuando un frontend en otro puerto no puede consumir la API, puede faltar configuración CORS.

## No usar modelos de respuesta

Sin `response_model`, la API puede devolver campos internos que no deberían exponerse.

## Buenas prácticas

## Usar type hints

```python
def read_item(item_id: int):
    ...
```

## Usar modelos Pydantic para entradas y salidas

```python
class ItemCreate(BaseModel):
    name: str
    price: float
```

## Usar `response_model`

```python
@app.get("/items/{item_id}", response_model=ItemResponse)
```

## Separar rutas con `APIRouter`

```python
app.include_router(items.router)
```

## Separar lógica de negocio

```text
endpoint -> valida HTTP y llama servicio
service  -> contiene reglas de negocio
```

## Manejar errores con `HTTPException`

```python
raise HTTPException(status_code=404, detail="No encontrado")
```

## Usar variables de entorno

```python
os.getenv("DATABASE_URL")
```

## Definir endpoints de salud

```python
@app.get("/health")
def health_check():
    return {"status": "ok"}
```

## Probar endpoints

```python
from fastapi.testclient import TestClient
```

## Mantener documentación clara

```python
app = FastAPI(
    title="Example API",
    version="0.1.0"
)
```

## Ejemplo integrado

```python
from fastapi import FastAPI, HTTPException, Query, status
from pydantic import BaseModel, Field


class ProductCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)


class ProductResponse(BaseModel):
    id: int
    name: str
    price: float
    stock: int


app = FastAPI(
    title="Products API",
    description="API de ejemplo para gestionar productos.",
    version="0.1.0"
)

products: list[ProductResponse] = [
    ProductResponse(id=1, name="Laptop", price=3500, stock=5),
    ProductResponse(id=2, name="Mouse", price=80, stock=20)
]


@app.get("/")
def read_root():
    return {
        "status": "ok",
        "service": "Products API"
    }


@app.get("/health")
def health_check():
    return {"status": "ok"}


@app.get("/products", response_model=list[ProductResponse])
def read_products(
    limit: int = Query(default=10, ge=1, le=100)
):
    return products[:limit]


@app.get("/products/{product_id}", response_model=ProductResponse)
def read_product(product_id: int):
    for product in products:
        if product.id == product_id:
            return product

    raise HTTPException(
        status_code=404,
        detail="Producto no encontrado"
    )


@app.post(
    "/products",
    response_model=ProductResponse,
    status_code=status.HTTP_201_CREATED
)
def create_product(product: ProductCreate):
    new_product = ProductResponse(
        id=len(products) + 1,
        name=product.name,
        price=product.price,
        stock=product.stock
    )

    products.append(new_product)

    return new_product
```

Ejecución:

```bash
fastapi dev main.py
```

Prueba en navegador:

```text
http://127.0.0.1:8000/docs
```

## Relación con otras librerías

`fastapi` se relaciona especialmente con:

- `pydantic`, para validación y serialización de datos
- `starlette`, como base ASGI y componentes web
- `uvicorn`, como servidor ASGI frecuente para ejecutar la aplicación
- `httpx`, para clientes HTTP y pruebas
- `pytest`, para pruebas automatizadas
- `python-dotenv`, para cargar variables de entorno
- `sqlalchemy` y `sqlmodel`, para bases de datos
- `alembic`, para migraciones de base de datos
- `pandas`, cuando la API expone datos procesados
- `openpyxl`, cuando se generan archivos Excel desde endpoints

## Orden didáctico interno

```text
1. Propósito de fastapi
2. Instalación e importación
3. Crear app con FastAPI()
4. Primer endpoint
5. Ejecución con fastapi dev o uvicorn
6. Documentación automática
7. Métodos HTTP
8. Path parameters y query parameters
9. Cuerpos con Pydantic
10. Response models y status codes
11. HTTPException
12. async def y def
13. APIRouter
14. Depends y dependencias
15. CORS, middleware y background tasks
16. Archivos, headers y cookies
17. Testing
18. Estructura de proyecto
19. Errores comunes
20. Buenas prácticas
```