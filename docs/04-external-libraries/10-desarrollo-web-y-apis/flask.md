El siguiente archivo debería ser:

```text
docs/04-external-libraries/07-desarrollo-web-y-apis/flask.md
```

Base factual principal: Flask es un framework web WSGI ligero para Python, pensado para empezar rápido y poder escalar hacia aplicaciones complejas. Su documentación oficial muestra el patrón `app = Flask(__name__)`, rutas con `@app.route()`, renderizado con plantillas, acceso a `request`, respuestas JSON, blueprints y testing con cliente de prueba. ([Flask Documentation][1])

Contenido propuesto:

````markdown
# `flask`

## Propósito

`flask` es una librería externa para crear aplicaciones web y APIs en Python. Se utiliza para construir sitios web, servicios HTTP, endpoints REST, paneles internos, prototipos, aplicaciones pequeñas y proyectos web que requieren flexibilidad en su estructura.

A diferencia de frameworks más estructurados, Flask ofrece un núcleo ligero y permite agregar extensiones según las necesidades del proyecto.

## Naturaleza de la librería

`flask` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install flask
```

La importación principal es:

```python
from flask import Flask
```

También se usan con frecuencia:

```python
from flask import request
from flask import jsonify
from flask import render_template
from flask import redirect
from flask import url_for
from flask import abort
from flask import Blueprint
```

## Idea central

La idea principal de Flask es crear una aplicación web mediante una instancia de `Flask` y registrar rutas que ejecutan funciones de Python.

Ejemplo mínimo:

```python
from flask import Flask

app = Flask(__name__)


@app.route("/")
def index():
    return {"message": "Hello, Flask"}
```

Cada función asociada a una ruta se llama función de vista.

## Instalación

Instalación básica:

```bash
python -m pip install flask
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
Flask==3.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
from flask import Flask
```

Verificación:

```python
import flask

print(flask.__version__)
```

## Crear una aplicación

```python
from flask import Flask

app = Flask(__name__)
```

El argumento `__name__` ayuda a Flask a ubicar recursos relacionados con la aplicación, como plantillas y archivos estáticos.

## Primer endpoint

```python
from flask import Flask

app = Flask(__name__)


@app.route("/")
def index():
    return {
        "status": "ok",
        "message": "API funcionando"
    }
```

Este endpoint responde a solicitudes sobre la ruta raíz `/`.

## Ejecutar una aplicación

Si el archivo se llama:

```text
app.py
```

puede ejecutarse en desarrollo con:

```bash
flask --app app run --debug
```

También puede usarse:

```bash
python -m flask --app app run --debug
```

Interpretación:

```text
--app app  -> indica el módulo app.py
run        -> ejecuta el servidor de desarrollo
--debug    -> activa modo de desarrollo y recarga automática
```

## Ejecutar directamente desde Python

También puede agregarse un bloque de ejecución directa:

```python
from flask import Flask

app = Flask(__name__)


@app.route("/")
def index():
    return {"status": "ok"}


if __name__ == "__main__":
    app.run(debug=True)
```

Ejecución:

```bash
python app.py
```

En proyectos reales, suele preferirse usar el comando `flask` o un servidor WSGI en producción.

## Servidor de desarrollo

El servidor incluido en Flask es útil para desarrollo local.

No debe usarse como servidor de producción.

Para producción suelen usarse servidores WSGI como:

```text
gunicorn
waitress
uWSGI
```

según el entorno de despliegue.

## Rutas

Las rutas se definen con decoradores.

```python
@app.route("/")
def index():
    return "Página principal"
```

Otra ruta:

```python
@app.route("/about")
def about():
    return "Acerca de"
```

## Métodos HTTP

Por defecto, una ruta acepta `GET`.

Para aceptar otros métodos se usa `methods`.

```python
@app.route("/items", methods=["GET"])
def read_items():
    return []


@app.route("/items", methods=["POST"])
def create_item():
    return {"message": "Item creado"}
```

## Ruta con varios métodos

```python
@app.route("/items", methods=["GET", "POST"])
def items():
    if request.method == "POST":
        return {"message": "Item creado"}

    return {"items": []}
```

Para esto debe importarse `request`:

```python
from flask import request
```

## Métodos HTTP frecuentes

```text
GET     -> consultar información
POST    -> crear o enviar información
PUT     -> reemplazar un recurso
PATCH   -> actualizar parcialmente un recurso
DELETE  -> eliminar un recurso
```

## Parámetros de ruta

Los parámetros de ruta se escriben entre `< >`.

```python
@app.route("/items/<item_id>")
def read_item(item_id):
    return {"item_id": item_id}
```

Ejemplo de solicitud:

```text
/items/ABC123
```

Resultado:

```json
{
  "item_id": "ABC123"
}
```

## Convertidores de ruta

Flask permite especificar tipos en la ruta.

```python
@app.route("/items/<int:item_id>")
def read_item(item_id):
    return {"item_id": item_id}
```

Convertidores frecuentes:

```text
string
int
float
path
uuid
```

## Parámetro entero

```python
@app.route("/clients/<int:client_id>")
def read_client(client_id):
    return {"client_id": client_id}
```

Si se envía un valor que no cumple el tipo, la ruta no coincide.

## Parámetro tipo path

```python
@app.route("/files/<path:file_path>")
def read_file(file_path):
    return {"file_path": file_path}
```

Este convertidor permite capturar rutas con barras internas.

## Query parameters

Los query parameters se leen desde `request.args`.

```python
from flask import Flask, request

app = Flask(__name__)


@app.route("/search")
def search():
    query = request.args.get("q")
    page = request.args.get("page", default=1, type=int)

    return {
        "query": query,
        "page": page
    }
```

Ejemplo:

```text
/search?q=python&page=2
```

## Leer query parameter obligatorio

```python
name = request.args["name"]
```

Si no existe, se genera error.

## Leer query parameter opcional

```python
name = request.args.get("name")
```

Si no existe, devuelve `None`.

## Valor por defecto y conversión

```python
limit = request.args.get("limit", default=10, type=int)
```

## JSON de entrada

Para leer JSON enviado en una solicitud se usa `request.get_json()`.

```python
from flask import Flask, request

app = Flask(__name__)


@app.route("/items", methods=["POST"])
def create_item():
    data = request.get_json()

    return {
        "received": data
    }
```

Ejemplo de cuerpo JSON:

```json
{
  "name": "Laptop",
  "price": 3500
}
```

## Validar JSON manualmente

Flask no valida modelos automáticamente como FastAPI. La validación debe hacerse manualmente o con librerías externas.

```python
@app.route("/items", methods=["POST"])
def create_item():
    data = request.get_json()

    if not data:
        return {"error": "Cuerpo JSON requerido"}, 400

    if "name" not in data:
        return {"error": "El campo name es obligatorio"}, 400

    if "price" not in data:
        return {"error": "El campo price es obligatorio"}, 400

    return {
        "name": data["name"],
        "price": data["price"]
    }, 201
```

## Respuestas JSON

En versiones modernas de Flask, devolver un diccionario permite generar una respuesta JSON.

```python
@app.route("/status")
def status():
    return {"status": "ok"}
```

También puede usarse `jsonify`.

```python
from flask import jsonify


@app.route("/status")
def status():
    return jsonify(status="ok")
```

## Código de estado

Puede devolverse una tupla con cuerpo y código HTTP.

```python
@app.route("/items", methods=["POST"])
def create_item():
    return {"message": "Item creado"}, 201
```

También puede devolverse:

```python
return {"error": "No encontrado"}, 404
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
500 Internal Server Error
```

## Errores HTTP con `abort`

`abort()` permite detener la ejecución y devolver un error HTTP.

```python
from flask import abort


@app.route("/items/<int:item_id>")
def read_item(item_id):
    if item_id <= 0:
        abort(404)

    return {"item_id": item_id}
```

## Error con mensaje JSON

Para controlar mejor la respuesta de error, puede devolverse una tupla.

```python
@app.route("/items/<int:item_id>")
def read_item(item_id):
    if item_id <= 0:
        return {"error": "Item no encontrado"}, 404

    return {"item_id": item_id}
```

## Manejadores de error

Flask permite definir manejadores de error.

```python
@app.errorhandler(404)
def not_found(error):
    return {"error": "Recurso no encontrado"}, 404
```

Para error 500:

```python
@app.errorhandler(500)
def internal_error(error):
    return {"error": "Error interno del servidor"}, 500
```

## Formularios HTML

Flask puede leer formularios enviados como `application/x-www-form-urlencoded` o `multipart/form-data`.

```python
from flask import request


@app.route("/login", methods=["POST"])
def login():
    username = request.form.get("username")
    password = request.form.get("password")

    return {
        "username": username,
        "password_received": password is not None
    }
```

## Subida de archivos

```python
from pathlib import Path

from flask import Flask, request

app = Flask(__name__)


@app.route("/upload", methods=["POST"])
def upload_file():
    uploaded_file = request.files.get("file")

    if uploaded_file is None:
        return {"error": "Archivo requerido"}, 400

    output_dir = Path("uploads")
    output_dir.mkdir(exist_ok=True)

    output_path = output_dir / uploaded_file.filename
    uploaded_file.save(output_path)

    return {
        "filename": uploaded_file.filename
    }
```

En proyectos reales conviene validar nombre, extensión, tamaño y tipo de archivo.

## Descargar archivos

```python
from flask import send_file


@app.route("/download")
def download_file():
    return send_file(
        "report.xlsx",
        as_attachment=True,
        download_name="report.xlsx"
    )
```

## Plantillas HTML

Flask se integra con Jinja para renderizar plantillas HTML.

Estructura típica:

```text
project/
├─ app.py
└─ templates/
   └─ index.html
```

Archivo:

```text
templates/index.html
```

Contenido:

```html
<!doctype html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <title>{{ title }}</title>
</head>
<body>
    <h1>{{ title }}</h1>
    <p>{{ message }}</p>
</body>
</html>
```

Código Flask:

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route("/")
def index():
    return render_template(
        "index.html",
        title="Inicio",
        message="Página generada con Flask"
    )
```

## Variables en plantillas

```html
<h1>{{ title }}</h1>
<p>{{ message }}</p>
```

## Condicionales en plantillas

```html
{% if user %}
    <p>Hola, {{ user }}</p>
{% else %}
    <p>Usuario no identificado</p>
{% endif %}
```

## Bucles en plantillas

```html
<ul>
{% for product in products %}
    <li>{{ product.name }} - {{ product.price }}</li>
{% endfor %}
</ul>
```

## Archivos estáticos

Los archivos estáticos suelen ir en la carpeta `static`.

Estructura:

```text
project/
├─ app.py
├─ templates/
│  └─ index.html
└─ static/
   ├─ css/
   │  └─ styles.css
   └─ images/
      └─ logo.png
```

En una plantilla:

```html
<link rel="stylesheet" href="{{ url_for('static', filename='css/styles.css') }}">
```

Imagen:

```html
<img src="{{ url_for('static', filename='images/logo.png') }}" alt="Logo">
```

## `url_for`

`url_for()` genera URLs a partir del nombre de una función de vista.

```python
from flask import url_for

url = url_for("index")
```

En plantillas:

```html
<a href="{{ url_for('index') }}">Inicio</a>
```

Esto es más mantenible que escribir rutas manualmente.

## Redirecciones

```python
from flask import redirect, url_for


@app.route("/old")
def old_page():
    return redirect(url_for("index"))
```

## Cookies

Leer cookie:

```python
from flask import request


@app.route("/cookies")
def read_cookie():
    session_id = request.cookies.get("session_id")

    return {"session_id": session_id}
```

Crear cookie:

```python
from flask import make_response


@app.route("/set-cookie")
def set_cookie():
    response = make_response({"status": "cookie creada"})
    response.set_cookie("session_id", "abc123")

    return response
```

## Sesiones

Flask permite usar sesiones firmadas mediante cookies.

```python
from flask import Flask, session

app = Flask(__name__)
app.secret_key = "clave-de-desarrollo"


@app.route("/set-session")
def set_session():
    session["user"] = "Ana"
    return {"status": "session creada"}


@app.route("/get-session")
def get_session():
    return {"user": session.get("user")}
```

En proyectos reales, `secret_key` debe leerse desde variables de entorno.

## Configuración

Se puede configurar la aplicación mediante `app.config`.

```python
app.config["DEBUG"] = True
app.config["UPLOAD_FOLDER"] = "uploads"
```

## Variables de entorno

```python
import os

from flask import Flask

app = Flask(__name__)

app.config["SECRET_KEY"] = os.getenv("SECRET_KEY", "dev-secret")
```

## Uso con `python-dotenv`

Instalación:

```bash
python -m pip install python-dotenv
```

Archivo `.env`:

```env
SECRET_KEY=clave_local
DATABASE_URL=sqlite:///app.db
```

Código:

```python
import os

from dotenv import load_dotenv
from flask import Flask

load_dotenv()

app = Flask(__name__)

app.config["SECRET_KEY"] = os.getenv("SECRET_KEY")
app.config["DATABASE_URL"] = os.getenv("DATABASE_URL")
```

## Blueprints

Los blueprints permiten organizar rutas por módulos.

Estructura:

```text
app/
├─ __init__.py
├─ routes/
│  ├─ __init__.py
│  └─ products.py
└─ templates/
```

Archivo:

```text
app/routes/products.py
```

Código:

```python
from flask import Blueprint

products_bp = Blueprint(
    "products",
    __name__,
    url_prefix="/products"
)


@products_bp.route("/")
def read_products():
    return [
        {"id": 1, "name": "Laptop"},
        {"id": 2, "name": "Mouse"}
    ]
```

Archivo:

```text
app/__init__.py
```

Código:

```python
from flask import Flask

from app.routes.products import products_bp


def create_app():
    app = Flask(__name__)

    app.register_blueprint(products_bp)

    return app
```

## Application factory

El patrón application factory consiste en crear la aplicación dentro de una función.

```python
from flask import Flask


def create_app():
    app = Flask(__name__)

    @app.route("/")
    def index():
        return {"status": "ok"}

    return app
```

Este patrón facilita:

```text
testing
configuración por entorno
registro de blueprints
inicialización de extensiones
estructura modular
```

## Ejecutar application factory

Si el archivo contiene `create_app`, puede ejecutarse con:

```bash
flask --app app run --debug
```

Si el paquete se llama `app`, Flask puede detectar la factory en muchos casos.

También puede indicarse explícitamente:

```bash
flask --app "app:create_app" run --debug
```

## Testing

Flask incluye un cliente de prueba.

```python
from app import create_app


def test_index():
    app = create_app()
    client = app.test_client()

    response = client.get("/")

    assert response.status_code == 200
    assert response.get_json()["status"] == "ok"
```

## Testing de POST JSON

```python
def test_create_item():
    app = create_app()
    client = app.test_client()

    response = client.post(
        "/items",
        json={
            "name": "Laptop",
            "price": 3500
        }
    )

    assert response.status_code == 201
```

## Uso con pytest

Instalación:

```bash
python -m pip install pytest
```

Ejecución:

```bash
pytest
```

Estructura:

```text
tests/
└─ test_app.py
```

## Uso con pandas

Flask puede exponer datos procesados con pandas.

```python
import pandas as pd
from flask import Flask

app = Flask(__name__)


@app.route("/summary")
def summary():
    df = pd.DataFrame({
        "product": ["Laptop", "Mouse"],
        "amount": [3500, 800]
    })

    total_amount = df["amount"].sum()

    return {
        "total_amount": float(total_amount)
    }
```

## Generar archivo y descargarlo

```python
from pathlib import Path

import pandas as pd
from flask import Flask, send_file

app = Flask(__name__)


@app.route("/report")
def report():
    output_path = Path("report.xlsx")

    df = pd.DataFrame({
        "product": ["Laptop", "Mouse"],
        "amount": [3500, 800]
    })

    df.to_excel(output_path, index=False)

    return send_file(
        output_path,
        as_attachment=True,
        download_name="report.xlsx"
    )
```

## Uso con bases de datos

Flask suele combinarse con extensiones o librerías externas para bases de datos.

Opciones frecuentes:

```text
SQLAlchemy
Flask-SQLAlchemy
sqlite3
psycopg
pymysql
```

Flask no impone una capa de base de datos por defecto.

## Uso con SQLAlchemy

Ejemplo conceptual:

```python
from sqlalchemy import create_engine, text

engine = create_engine("sqlite:///app.db")


@app.route("/clients")
def read_clients():
    with engine.connect() as connection:
        rows = connection.execute(
            text("SELECT id, name FROM clients")
        ).mappings().all()

    return [dict(row) for row in rows]
```

## Uso con frontend

Un frontend puede consumir Flask mediante solicitudes HTTP.

Flujo típico:

```text
HTML / JavaScript / React / Vue -> fetch() -> Flask -> JSON
```

Ejemplo conceptual de respuesta:

```python
@app.route("/api/status")
def api_status():
    return {"status": "ok"}
```

## CORS

Cuando un frontend está en otro dominio o puerto, puede requerirse CORS.

Instalación frecuente:

```bash
python -m pip install flask-cors
```

Uso:

```python
from flask import Flask
from flask_cors import CORS

app = Flask(__name__)

CORS(app)
```

En producción conviene restringir orígenes permitidos.

## Estructura mínima

```text
project/
├─ app.py
└─ requirements.txt
```

Archivo:

```text
app.py
```

Código:

```python
from flask import Flask

app = Flask(__name__)


@app.route("/")
def index():
    return {
        "status": "ok",
        "service": "Flask API"
    }


@app.route("/health")
def health_check():
    return {"status": "ok"}
```

Ejecución:

```bash
flask --app app run --debug
```

## Estructura modular

```text
project/
├─ app/
│  ├─ __init__.py
│  ├─ routes/
│  │  ├─ __init__.py
│  │  └─ products.py
│  ├─ services/
│  │  ├─ __init__.py
│  │  └─ product_service.py
│  ├─ templates/
│  └─ static/
├─ tests/
│  └─ test_products.py
└─ requirements.txt
```

## Separación por responsabilidades

```text
__init__.py   -> create_app() y registro de blueprints
routes/       -> endpoints HTTP
services/     -> lógica de negocio
templates/    -> HTML
static/       -> CSS, JS, imágenes
tests/        -> pruebas automatizadas
```

## Comparación con FastAPI

## Flask

- framework ligero y flexible
- muy usado para sitios web y APIs simples
- no impone validación automática de modelos
- usa Jinja para plantillas HTML
- se apoya en extensiones para muchas funciones
- tradicionalmente basado en WSGI

## FastAPI

- orientado a APIs modernas
- usa type hints intensivamente
- valida datos con Pydantic
- genera documentación OpenAPI automáticamente
- soporta programación asíncrona con ASGI
- resulta muy cómodo para APIs JSON tipadas

## Regla práctica

Para APIs modernas con validación fuerte y documentación automática, FastAPI suele ser más directo.

Para aplicaciones web ligeras, prototipos, HTML con plantillas y máxima flexibilidad, Flask sigue siendo una opción muy sólida.

## Errores comunes

## Olvidar crear `app`

Problemático:

```python
@app.route("/")
def index():
    return "Hola"
```

Correcto:

```python
app = Flask(__name__)


@app.route("/")
def index():
    return "Hola"
```

## Ejecutar mal el módulo

Si el archivo es:

```text
app.py
```

el comando es:

```bash
flask --app app run --debug
```

No:

```bash
flask --app app.py run
```

## No importar `request`

Problemático:

```python
@app.route("/search")
def search():
    q = request.args.get("q")
    return {"q": q}
```

Falta:

```python
from flask import request
```

## No validar JSON de entrada

Problemático:

```python
data = request.get_json()
name = data["name"]
```

Si el cuerpo no existe o falta el campo, puede fallar.

Más seguro:

```python
data = request.get_json() or {}

if "name" not in data:
    return {"error": "name requerido"}, 400
```

## Escribir secretos directamente en el código

Problemático:

```python
app.secret_key = "clave_real"
```

Mejor:

```python
app.secret_key = os.getenv("SECRET_KEY")
```

## Guardar archivos subidos sin validar nombre

Problemático:

```python
uploaded_file.save(uploaded_file.filename)
```

Conviene validar nombre, extensión y ubicación de guardado.

## Poner toda la aplicación en un solo archivo

Para prototipos está bien. Para proyectos medianos, conviene usar blueprints y separar lógica.

## Usar servidor de desarrollo en producción

El servidor de desarrollo no debe usarse como servidor final de producción.

## Devolver HTML cuando se espera JSON

En APIs, los errores deberían mantener un formato de respuesta consistente.

Ejemplo:

```python
return {"error": "No encontrado"}, 404
```

## Buenas prácticas

## Mantener rutas simples

```python
@app.route("/health")
def health_check():
    return {"status": "ok"}
```

## Usar códigos HTTP correctos

```python
return {"message": "Creado"}, 201
```

## Validar entradas

```python
data = request.get_json() or {}
```

## Usar blueprints en proyectos medianos

```python
app.register_blueprint(products_bp)
```

## Usar application factory

```python
def create_app():
    app = Flask(__name__)
    return app
```

## Separar lógica de negocio

```text
routes -> reciben HTTP
services -> ejecutan lógica
```

## Usar variables de entorno

```python
os.getenv("SECRET_KEY")
```

## Escribir pruebas

```python
client = app.test_client()
```

## Usar `url_for` en plantillas

```html
<a href="{{ url_for('index') }}">Inicio</a>
```

## No usar el servidor de desarrollo en producción

Para producción, usar un servidor WSGI adecuado al entorno.

## Ejemplo integrado

```python
import os
from pathlib import Path

import pandas as pd
from dotenv import load_dotenv
from flask import Flask, request, send_file


def create_app():
    load_dotenv()

    app = Flask(__name__)
    app.config["SECRET_KEY"] = os.getenv("SECRET_KEY", "dev-secret")

    products = [
        {"id": 1, "name": "Laptop", "price": 3500.0},
        {"id": 2, "name": "Mouse", "price": 80.0}
    ]

    @app.route("/")
    def index():
        return {
            "status": "ok",
            "service": "Products API"
        }

    @app.route("/health")
    def health_check():
        return {"status": "ok"}

    @app.route("/products", methods=["GET"])
    def read_products():
        return products

    @app.route("/products/<int:product_id>", methods=["GET"])
    def read_product(product_id):
        for product in products:
            if product["id"] == product_id:
                return product

        return {"error": "Producto no encontrado"}, 404

    @app.route("/products", methods=["POST"])
    def create_product():
        data = request.get_json() or {}

        required_fields = ["name", "price"]

        for field in required_fields:
            if field not in data:
                return {"error": f"Campo requerido: {field}"}, 400

        new_product = {
            "id": len(products) + 1,
            "name": str(data["name"]),
            "price": float(data["price"])
        }

        products.append(new_product)

        return new_product, 201

    @app.route("/products/report", methods=["GET"])
    def download_products_report():
        output_path = Path("products_report.xlsx")

        df = pd.DataFrame(products)
        df.to_excel(output_path, index=False)

        return send_file(
            output_path,
            as_attachment=True,
            download_name="products_report.xlsx"
        )

    return app
```

Archivo de ejecución alternativo:

```python
from app import create_app

app = create_app()


if __name__ == "__main__":
    app.run(debug=True)
```

## Relación con otras librerías

`flask` se relaciona especialmente con:

- `werkzeug`, por componentes WSGI, request, response y servidor de desarrollo
- `jinja2`, por el sistema de plantillas HTML
- `click`, por comandos de línea de comandos
- `python-dotenv`, para cargar variables de entorno
- `flask-cors`, para configurar CORS
- `flask-sqlalchemy`, para integración con SQLAlchemy
- `sqlalchemy`, para acceso a bases de datos
- `pytest`, para pruebas automatizadas
- `pandas`, cuando la aplicación expone o genera datos tabulares
- `openpyxl`, cuando se generan reportes Excel desde endpoints
- `requests` o `httpx`, cuando se consumen APIs externas

## Orden didáctico interno

```text
1. Propósito de flask
2. Instalación e importación
3. Crear app con Flask()
4. Primer endpoint
5. Ejecución con flask run
6. Rutas y métodos HTTP
7. Parámetros de ruta
8. Query parameters con request.args
9. JSON de entrada y salida
10. Códigos de estado y errores
11. Plantillas con Jinja
12. Archivos estáticos
13. Formularios y archivos
14. Redirecciones, cookies y sesiones
15. Configuración y variables de entorno
16. Blueprints
17. Application factory
18. Testing
19. Estructura de proyecto
20. Comparación con FastAPI
21. Errores comunes
22. Buenas prácticas
```