# `beautifulsoup4`

## Propósito

`beautifulsoup4` es una librería externa para extraer información desde documentos HTML y XML. Se utiliza principalmente en web scraping, limpieza de HTML, extracción de enlaces, lectura de tablas simples, búsqueda de etiquetas y procesamiento de contenido web descargado previamente con librerías como `requests`, `httpx` o `aiohttp`.

## Naturaleza de la librería

`beautifulsoup4` no descarga páginas por sí misma. Su función principal es analizar contenido HTML o XML ya obtenido desde otra fuente.

Flujo típico:

```text
requests / httpx / aiohttp -> HTML -> BeautifulSoup -> extracción de datos
```

Instalación:

```bash
python -m pip install beautifulsoup4
```

Importación habitual:

```python
from bs4 import BeautifulSoup
```

El nombre de instalación es `beautifulsoup4`, pero el paquete se importa como `bs4`.

## Idea central

La idea principal consiste en convertir texto HTML en un objeto navegable.

```python
from bs4 import BeautifulSoup

html = """
<html>
    <body>
        <h1>Título</h1>
        <p class="intro">Texto inicial</p>
    </body>
</html>
"""

soup = BeautifulSoup(html, "html.parser")

print(soup.h1.text)
print(soup.p.text)
```

Salida:

```text
Título
Texto inicial
```

## Instalación

Instalación básica:

```bash
python -m pip install beautifulsoup4
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea en `requirements.txt`:

```text
beautifulsoup4==4.14.3
```

La versión exacta puede variar según el entorno.

## Parsers

Beautiful Soup necesita un parser para interpretar el documento.

Parser incluido en la biblioteca estándar:

```python
"html.parser"
```

Parser externo frecuente:

```python
"lxml"
```

Uso básico:

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, "html.parser")
```

Uso con `lxml`:

```bash
python -m pip install lxml
```

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, "lxml")
```

## Importación habitual

```python
from bs4 import BeautifulSoup
```

No se suele importar así:

```python
import beautifulsoup4
```

El paquete se instala con un nombre y se importa con otro.

## Primer ejemplo completo

```python
from bs4 import BeautifulSoup

html = """
<html>
    <body>
        <h1>Productos</h1>
        <ul>
            <li class="item">Teclado</li>
            <li class="item">Mouse</li>
            <li class="item">Monitor</li>
        </ul>
    </body>
</html>
"""

soup = BeautifulSoup(html, "html.parser")

title = soup.find("h1")
items = soup.find_all("li", class_="item")

print(title.get_text())

for item in items:
    print(item.get_text())
```

Salida:

```text
Productos
Teclado
Mouse
Monitor
```

## Objeto `BeautifulSoup`

El objeto `soup` representa el documento parseado.

```python
soup = BeautifulSoup(html, "html.parser")
```

Desde ese objeto se puede:

- buscar etiquetas
- navegar por el árbol HTML
- extraer texto
- leer atributos
- seleccionar elementos por CSS
- modificar partes del documento
- convertir el resultado nuevamente a texto

## Acceso directo a etiquetas

Beautiful Soup permite acceder directamente a la primera etiqueta encontrada de cierto tipo.

```python
from bs4 import BeautifulSoup

html = "<html><body><h1>Título</h1><p>Texto</p></body></html>"
soup = BeautifulSoup(html, "html.parser")

print(soup.h1)
print(soup.p)
```

Salida:

```text
<h1>Título</h1>
<p>Texto</p>
```

Este estilo es cómodo para exploración, pero en scraping real suele ser más claro usar `find()`, `find_all()` o `select()`.

## `find()`

## Propósito

`find()` busca la primera coincidencia que cumpla el criterio indicado.

```python
from bs4 import BeautifulSoup

html = """
<div>
    <p class="item">Primero</p>
    <p class="item">Segundo</p>
</div>
"""

soup = BeautifulSoup(html, "html.parser")

result = soup.find("p", class_="item")

print(result.get_text())
```

Salida:

```text
Primero
```

## Búsqueda por etiqueta

```python
title = soup.find("h1")
```

## Búsqueda por clase

```python
item = soup.find("p", class_="item")
```

Se usa `class_` porque `class` es palabra reservada en Python.

## Búsqueda por id

```python
main = soup.find("div", id="main")
```

## Búsqueda por atributos

```python
link = soup.find("a", attrs={"data-id": "123"})
```

## Resultado cuando no encuentra

Si `find()` no encuentra coincidencia, devuelve `None`.

```python
result = soup.find("section", id="missing")

if result is None:
    print("No encontrado")
```

## `find_all()`

## Propósito

`find_all()` busca todas las coincidencias que cumplan el criterio indicado.

```python
from bs4 import BeautifulSoup

html = """
<ul>
    <li>Uno</li>
    <li>Dos</li>
    <li>Tres</li>
</ul>
"""

soup = BeautifulSoup(html, "html.parser")

items = soup.find_all("li")

for item in items:
    print(item.get_text())
```

Salida:

```text
Uno
Dos
Tres
```

## Diferencia entre `find()` y `find_all()`

```text
find()     -> primera coincidencia o None
find_all() -> lista de coincidencias
```

Ejemplo:

```python
first_item = soup.find("li")
all_items = soup.find_all("li")
```

## `select()`

## Propósito

`select()` permite buscar elementos usando selectores CSS.

```python
from bs4 import BeautifulSoup

html = """
<div class="products">
    <p class="item">Teclado</p>
    <p class="item">Mouse</p>
</div>
"""

soup = BeautifulSoup(html, "html.parser")

items = soup.select(".products .item")

for item in items:
    print(item.get_text())
```

Salida:

```text
Teclado
Mouse
```

## Selectores frecuentes

## Por etiqueta

```python
soup.select("p")
```

## Por clase

```python
soup.select(".item")
```

## Por id

```python
soup.select("#main")
```

## Por descendencia

```python
soup.select("div p")
```

## Por atributo

```python
soup.select("a[href]")
```

## `select_one()`

`select_one()` devuelve solo la primera coincidencia de un selector CSS.

```python
title = soup.select_one("h1")
```

Si no encuentra coincidencia, devuelve `None`.

## Extraer texto

## `.text`

```python
title = soup.find("h1")
print(title.text)
```

## `get_text()`

```python
title = soup.find("h1")
print(title.get_text())
```

`get_text()` permite opciones adicionales y suele ser más explícito.

## `get_text(strip=True)`

```python
text = title.get_text(strip=True)
```

Elimina espacios sobrantes en los extremos del texto extraído.

## Separador en `get_text()`

```python
text = soup.get_text(separator=" ", strip=True)
```

Esto es útil cuando se extrae texto de un bloque con varias etiquetas internas.

## Extraer atributos

Los atributos HTML se leen como si la etiqueta fuera un diccionario.

```python
from bs4 import BeautifulSoup

html = '<a href="https://example.com" title="Ejemplo">Link</a>'
soup = BeautifulSoup(html, "html.parser")

link = soup.find("a")

print(link["href"])
print(link["title"])
```

Salida:

```text
https://example.com
Ejemplo
```

## Uso seguro con `.get()`

Si el atributo puede no existir, conviene usar `.get()`.

```python
href = link.get("href")
title = link.get("title")
missing = link.get("data-id")
```

Si el atributo no existe, devuelve `None`.

## Navegación básica

## Padre

```python
parent = tag.parent
```

## Hijos

```python
children = tag.children
```

## Descendientes

```python
descendants = tag.descendants
```

## Siguiente hermano

```python
next_item = tag.find_next_sibling()
```

## Hermano anterior

```python
previous_item = tag.find_previous_sibling()
```

## Ejemplo de navegación

```python
from bs4 import BeautifulSoup

html = """
<div>
    <h2>Categoría</h2>
    <p>Descripción</p>
</div>
"""

soup = BeautifulSoup(html, "html.parser")

title = soup.find("h2")
description = title.find_next_sibling("p")

print(description.get_text())
```

Salida:

```text
Descripción
```

## Extracción de enlaces

```python
from bs4 import BeautifulSoup

html = """
<a href="https://example.com/a">A</a>
<a href="https://example.com/b">B</a>
<a>B sin href</a>
"""

soup = BeautifulSoup(html, "html.parser")

links = []

for tag in soup.find_all("a"):
    href = tag.get("href")

    if href is not None:
        links.append(href)

print(links)
```

Salida:

```text
['https://example.com/a', 'https://example.com/b']
```

## Extracción de tablas simples

```python
from bs4 import BeautifulSoup

html = """
<table>
    <tr><th>Nombre</th><th>Edad</th></tr>
    <tr><td>Ana</td><td>20</td></tr>
    <tr><td>Luis</td><td>25</td></tr>
</table>
"""

soup = BeautifulSoup(html, "html.parser")

rows = []

for tr in soup.find_all("tr"):
    cells = [cell.get_text(strip=True) for cell in tr.find_all(["th", "td"])]
    rows.append(cells)

print(rows)
```

Salida:

```text
[['Nombre', 'Edad'], ['Ana', '20'], ['Luis', '25']]
```

## Uso con `requests`

```python
import requests
from bs4 import BeautifulSoup

url = "https://example.com"

response = requests.get(url, timeout=10)
response.raise_for_status()

soup = BeautifulSoup(response.text, "html.parser")

print(soup.title.get_text(strip=True))
```

## Uso con `httpx`

```python
import httpx
from bs4 import BeautifulSoup

url = "https://example.com"

response = httpx.get(url, timeout=10.0)
response.raise_for_status()

soup = BeautifulSoup(response.text, "html.parser")

print(soup.title.get_text(strip=True))
```

## Uso con `aiohttp`

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

    print(soup.title.get_text(strip=True))


asyncio.run(main())
```

## Modificación del árbol

Beautiful Soup también puede modificar el árbol parseado.

## Cambiar texto

```python
from bs4 import BeautifulSoup

html = "<p>Hola</p>"
soup = BeautifulSoup(html, "html.parser")

tag = soup.find("p")
tag.string = "Texto cambiado"

print(soup)
```

Salida:

```html
<p>Texto cambiado</p>
```

## Eliminar una etiqueta

```python
from bs4 import BeautifulSoup

html = "<div><p>Texto</p><script>alert(1)</script></div>"
soup = BeautifulSoup(html, "html.parser")

script = soup.find("script")

if script is not None:
    script.decompose()

print(soup)
```

## Crear listas de resultados

```python
from bs4 import BeautifulSoup

html = """
<div class="product">
    <span class="name">Teclado</span>
    <span class="price">120</span>
</div>
<div class="product">
    <span class="name">Mouse</span>
    <span class="price">50</span>
</div>
"""

soup = BeautifulSoup(html, "html.parser")

products = []

for product in soup.select(".product"):
    name_tag = product.select_one(".name")
    price_tag = product.select_one(".price")

    if name_tag is None or price_tag is None:
        continue

    products.append({
        "name": name_tag.get_text(strip=True),
        "price": float(price_tag.get_text(strip=True))
    })

print(products)
```

Salida:

```text
[{'name': 'Teclado', 'price': 120.0}, {'name': 'Mouse', 'price': 50.0}]
```

## Relación con `pandas`

Después de extraer datos, pueden organizarse en un `DataFrame`.

```python
import pandas as pd
from bs4 import BeautifulSoup

html = """
<div class="product">
    <span class="name">Teclado</span>
    <span class="price">120</span>
</div>
<div class="product">
    <span class="name">Mouse</span>
    <span class="price">50</span>
</div>
"""

soup = BeautifulSoup(html, "html.parser")

records = []

for product in soup.select(".product"):
    records.append({
        "name": product.select_one(".name").get_text(strip=True),
        "price": float(product.select_one(".price").get_text(strip=True))
    })

df = pd.DataFrame(records)

print(df)
```

## Manejo de HTML incompleto

Beautiful Soup suele tolerar HTML mal formado mejor que un parser estricto.

Ejemplo:

```python
from bs4 import BeautifulSoup

html = "<html><body><p>Texto sin cierre"

soup = BeautifulSoup(html, "html.parser")

print(soup.get_text(strip=True))
```

## Errores comunes

## Pensar que Beautiful Soup descarga páginas

Problemático:

```python
soup = BeautifulSoup("https://example.com", "html.parser")
```

Eso parsea la cadena `"https://example.com"` como texto, no descarga la página.

Primero debe descargarse el HTML:

```python
import requests
from bs4 import BeautifulSoup

response = requests.get("https://example.com", timeout=10)
response.raise_for_status()

soup = BeautifulSoup(response.text, "html.parser")
```

## Usar `find()` y no validar `None`

Problemático:

```python
title = soup.find("h1")
print(title.get_text())
```

Si no existe `h1`, `title` será `None`.

Más seguro:

```python
title = soup.find("h1")

if title is not None:
    print(title.get_text(strip=True))
```

## Usar `class` en lugar de `class_`

Problemático:

```python
soup.find("p", class="item")
```

`class` es palabra reservada en Python.

Correcto:

```python
soup.find("p", class_="item")
```

## Acceder a un atributo inexistente con corchetes

Problemático:

```python
href = link["href"]
```

Si el atributo no existe, se genera `KeyError`.

Más seguro:

```python
href = link.get("href")
```

## No revisar el estado HTTP antes de parsear

Problemático:

```python
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")
```

Mejor:

```python
response = requests.get(url, timeout=10)
response.raise_for_status()
soup = BeautifulSoup(response.text, "html.parser")
```

## Confundir `text` con limpieza completa

`get_text(strip=True)` elimina espacios extremos, pero no necesariamente limpia todo el contenido ni normaliza todos los saltos de línea.

## Depender demasiado de selectores frágiles

Selectores muy específicos pueden romperse si cambia la estructura HTML de la página.

Ejemplo frágil:

```python
soup.select("body > div:nth-child(3) > div > p")
```

Mejor buscar atributos más estables cuando existan:

```python
soup.select(".product-card")
```

## Buenas prácticas

## Descargar HTML con una librería HTTP

Beautiful Soup debe recibir HTML, no una URL.

```python
response = requests.get(url, timeout=10)
soup = BeautifulSoup(response.text, "html.parser")
```

## Usar `raise_for_status()`

```python
response.raise_for_status()
```

## Validar resultados antes de acceder

```python
tag = soup.find("h1")

if tag is not None:
    text = tag.get_text(strip=True)
```

## Usar `.get()` para atributos opcionales

```python
href = link.get("href")
```

## Preferir selectores claros

```python
soup.select(".product .name")
```

## Separar extracción en funciones

```python
def get_text_or_none(tag):
    if tag is None:
        return None

    return tag.get_text(strip=True)
```

## Respetar condiciones del sitio

Antes de hacer scraping a una página, debe revisarse si el sitio lo permite, evitar sobrecargar servidores y usar fuentes oficiales o APIs cuando existan.

## Ejemplo integrado

```python
from pathlib import Path

import requests
from bs4 import BeautifulSoup


def fetch_html(url):
    response = requests.get(
        url,
        timeout=10,
        headers={"User-Agent": "python-beautifulsoup-demo"}
    )

    response.raise_for_status()

    return response.text


def extract_links(html):
    soup = BeautifulSoup(html, "html.parser")

    links = []

    for tag in soup.find_all("a"):
        href = tag.get("href")
        text = tag.get_text(strip=True)

        if href is None:
            continue

        links.append({
            "text": text,
            "href": href
        })

    return links


def save_links(links, output_path):
    lines = []

    for link in links:
        lines.append(f"{link['text']} -> {link['href']}")

    output_path.write_text(
        "\n".join(lines),
        encoding="utf-8"
    )


url = "https://example.com"

html = fetch_html(url)
links = extract_links(html)

save_links(
    links,
    Path("links.txt")
)

print("Enlaces guardados correctamente")
```

## Relación con otras librerías

`beautifulsoup4` se relaciona especialmente con:

- `requests`, para descargar HTML de forma síncrona
- `httpx`, como alternativa HTTP síncrona o asíncrona
- `aiohttp`, para descargar HTML de forma asíncrona
- `lxml`, como parser rápido y flexible
- `pandas`, para organizar datos extraídos en tablas
- `csv`, para guardar resultados estructurados
- `pathlib`, para guardar archivos de salida

## Orden didáctico interno

```text
1. Propósito de beautifulsoup4
2. Instalación e importación
3. BeautifulSoup como parser
4. find() y find_all()
5. select() y select_one()
6. Extracción de texto y atributos
7. Navegación por el árbol
8. Uso con requests, httpx o aiohttp
9. Errores comunes
10. Buenas prácticas
```
