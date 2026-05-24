# `lxml`

## Propósito

`lxml` es una librería externa para procesar documentos XML y HTML de forma rápida y estructurada. Se utiliza para parsear contenido, navegar árboles de etiquetas, extraer información, ejecutar consultas XPath, transformar XML con XSLT y trabajar con documentos donde se requiere más control que con parsers simples.

## Naturaleza de la librería

`lxml` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install lxml
```

Importaciones frecuentes:

```python
from lxml import etree
from lxml import html
```

`etree` se usa principalmente para XML.

`html` se usa principalmente para HTML.

## Idea central

La idea principal consiste en convertir un documento XML o HTML en un árbol de elementos.

Ese árbol permite:

- acceder a etiquetas
- leer atributos
- extraer texto
- recorrer nodos
- buscar elementos
- aplicar XPath
- modificar contenido
- serializar el resultado nuevamente

Ejemplo mínimo:

```python
from lxml import html

content = """
<html>
    <body>
        <h1>Productos</h1>
        <p class="item">Teclado</p>
    </body>
</html>
"""

tree = html.fromstring(content)

title = tree.xpath("//h1/text()")

print(title)
```

Salida:

```text
['Productos']
```

## Instalación

Instalación básica:

```bash
python -m pip install lxml
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea en `requirements.txt`:

```text
lxml==5.3.0
```

La versión exacta puede variar según el entorno.

## Importación

Para XML:

```python
from lxml import etree
```

Para HTML:

```python
from lxml import html
```

Verificación básica:

```python
import lxml

print(lxml.__version__)
```

## Relación con XML y HTML

`lxml` puede trabajar con XML y HTML, pero conviene distinguirlos.

## XML

XML es más estricto. Las etiquetas deben estar correctamente cerradas y el documento debe cumplir reglas de estructura más rígidas.

Ejemplo:

```xml
<root>
    <item id="1">Teclado</item>
</root>
```

## HTML

HTML suele ser más tolerante. En páginas reales puede haber etiquetas incompletas, atributos variados o estructuras menos limpias.

Ejemplo:

```html
<html>
    <body>
        <h1>Productos</h1>
        <p>Teclado</p>
    </body>
</html>
```

Para XML se suele usar:

```python
from lxml import etree
```

Para HTML se suele usar:

```python
from lxml import html
```

## Parseo de HTML

## `html.fromstring()`

Convierte una cadena HTML en un árbol de elementos.

```python
from lxml import html

content = """
<html>
    <body>
        <h1>Productos</h1>
        <p>Teclado</p>
    </body>
</html>
"""

tree = html.fromstring(content)

print(tree.xpath("//h1/text()"))
```

Salida:

```text
['Productos']
```

## Parseo de XML

## `etree.fromstring()`

Convierte una cadena XML en un elemento raíz.

```python
from lxml import etree

content = """
<root>
    <item id="1">Teclado</item>
    <item id="2">Mouse</item>
</root>
"""

root = etree.fromstring(content)

print(root.tag)
```

Salida:

```text
root
```

## Diferencia entre `html.fromstring()` y `etree.fromstring()`

`html.fromstring()` está orientado a documentos HTML.

`etree.fromstring()` está orientado a XML.

Ejemplo:

```python
from lxml import etree, html

html_tree = html.fromstring("<html><body><p>Texto</p></body></html>")
xml_root = etree.fromstring("<root><item>Texto</item></root>")

print(html_tree.tag)
print(xml_root.tag)
```

## Elementos

Los nodos principales de un árbol se representan como elementos.

Un elemento puede tener:

- nombre de etiqueta
- texto
- atributos
- hijos
- padre
- hermanos

Ejemplo:

```python
from lxml import etree

root = etree.fromstring("""
<root>
    <item id="1">Teclado</item>
</root>
""")

item = root.find("item")

print(item.tag)
print(item.text)
print(item.get("id"))
```

Salida:

```text
item
Teclado
1
```

## Atributos

Los atributos se leen con `.get()`.

```python
from lxml import etree

root = etree.fromstring("""
<root>
    <item id="1" category="peripheral">Teclado</item>
</root>
""")

item = root.find("item")

print(item.get("id"))
print(item.get("category"))
```

Salida:

```text
1
peripheral
```

También puede accederse al diccionario de atributos:

```python
print(item.attrib)
```

## Texto

El contenido textual directo de un elemento se obtiene con `.text`.

```python
from lxml import etree

root = etree.fromstring("<root><item>Teclado</item></root>")

item = root.find("item")

print(item.text)
```

Salida:

```text
Teclado
```

## Texto completo

Para obtener el texto acumulado de un elemento y sus descendientes puede usarse:

```python
text = element.text_content()
```

Esto es común en árboles HTML creados con `lxml.html`.

Ejemplo:

```python
from lxml import html

tree = html.fromstring("""
<div>
    <p>Hola <strong>Python</strong></p>
</div>
""")

print(tree.text_content())
```

Salida aproximada:

```text
Hola Python
```

## Búsqueda con `find()`

`find()` devuelve el primer hijo que coincide con la ruta indicada.

```python
from lxml import etree

root = etree.fromstring("""
<root>
    <item>Teclado</item>
    <item>Mouse</item>
</root>
""")

item = root.find("item")

print(item.text)
```

Salida:

```text
Teclado
```

## Búsqueda con `findall()`

`findall()` devuelve una lista de coincidencias.

```python
from lxml import etree

root = etree.fromstring("""
<root>
    <item>Teclado</item>
    <item>Mouse</item>
</root>
""")

items = root.findall("item")

for item in items:
    print(item.text)
```

Salida:

```text
Teclado
Mouse
```

## Búsqueda con `iter()`

`iter()` permite recorrer elementos por etiqueta.

```python
from lxml import etree

root = etree.fromstring("""
<root>
    <category>
        <item>Teclado</item>
    </category>
    <category>
        <item>Mouse</item>
    </category>
</root>
""")

for item in root.iter("item"):
    print(item.text)
```

Salida:

```text
Teclado
Mouse
```

## XPath

## Propósito

XPath permite buscar nodos dentro de documentos XML o HTML mediante expresiones de ruta.

Es una de las razones principales para usar `lxml`.

Ejemplo:

```python
from lxml import html

content = """
<html>
    <body>
        <div class="product">
            <span class="name">Teclado</span>
            <span class="price">120</span>
        </div>
    </body>
</html>
"""

tree = html.fromstring(content)

names = tree.xpath("//span[@class='name']/text()")

print(names)
```

Salida:

```text
['Teclado']
```

## Expresiones XPath frecuentes

## Todas las etiquetas de un tipo

```python
tree.xpath("//p")
```

## Texto de etiquetas

```python
tree.xpath("//p/text()")
```

## Buscar por atributo

```python
tree.xpath("//div[@class='product']")
```

## Extraer atributo

```python
tree.xpath("//a/@href")
```

## Buscar dentro de un nodo

```python
product.xpath(".//span[@class='name']/text()")
```

El punto inicial indica búsqueda relativa al nodo actual.

## XPath absoluto y relativo

## XPath absoluto

```python
tree.xpath("/html/body/div")
```

Depende de la estructura completa del documento.

## XPath relativo

```python
node.xpath(".//span")
```

Busca dentro del nodo actual.

En scraping, las rutas relativas suelen ser más resistentes cuando se trabaja sobre bloques repetidos.

## Extracción de enlaces

```python
from lxml import html

content = """
<html>
    <body>
        <a href="https://example.com/a">A</a>
        <a href="https://example.com/b">B</a>
        <a>Sin enlace</a>
    </body>
</html>
"""

tree = html.fromstring(content)

links = tree.xpath("//a/@href")

print(links)
```

Salida:

```text
['https://example.com/a', 'https://example.com/b']
```

## Extracción de bloques repetidos

```python
from lxml import html

content = """
<div class="product">
    <span class="name">Teclado</span>
    <span class="price">120</span>
</div>
<div class="product">
    <span class="name">Mouse</span>
    <span class="price">50</span>
</div>
"""

tree = html.fromstring(content)

products = []

for product in tree.xpath("//div[@class='product']"):
    name = product.xpath(".//span[@class='name']/text()")
    price = product.xpath(".//span[@class='price']/text()")

    if not name or not price:
        continue

    products.append({
        "name": name[0],
        "price": float(price[0])
    })

print(products)
```

Salida:

```text
[{'name': 'Teclado', 'price': 120.0}, {'name': 'Mouse', 'price': 50.0}]
```

## Uso con `requests`

```python
import requests
from lxml import html

url = "https://example.com"

response = requests.get(url, timeout=10)
response.raise_for_status()

tree = html.fromstring(response.text)

title = tree.xpath("//title/text()")

print(title)
```

## Uso con `httpx`

```python
import httpx
from lxml import html

url = "https://example.com"

response = httpx.get(url, timeout=10.0)
response.raise_for_status()

tree = html.fromstring(response.text)

title = tree.xpath("//title/text()")

print(title)
```

## Uso con `aiohttp`

```python
import asyncio

import aiohttp
from lxml import html


async def main():
    url = "https://example.com"

    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            response.raise_for_status()
            content = await response.text()

    tree = html.fromstring(content)

    print(tree.xpath("//title/text()"))


asyncio.run(main())
```

## Serialización

## Convertir elemento a bytes

```python
from lxml import etree

root = etree.Element("root")
item = etree.SubElement(root, "item")
item.text = "Teclado"

content = etree.tostring(root)

print(content)
```

Salida:

```text
b'<root><item>Teclado</item></root>'
```

## Serialización con formato legible

```python
content = etree.tostring(
    root,
    pretty_print=True,
    encoding="unicode"
)

print(content)
```

Salida aproximada:

```xml
<root>
  <item>Teclado</item>
</root>
```

## Crear XML

```python
from lxml import etree

root = etree.Element("products")

item = etree.SubElement(root, "product")
item.set("id", "1")
item.text = "Teclado"

print(
    etree.tostring(
        root,
        pretty_print=True,
        encoding="unicode"
    )
)
```

Salida:

```xml
<products>
  <product id="1">Teclado</product>
</products>
```

## Modificar elementos

```python
from lxml import etree

root = etree.fromstring("""
<products>
    <product id="1">Teclado</product>
</products>
""")

product = root.find("product")
product.text = "Mouse"
product.set("id", "2")

print(etree.tostring(root, encoding="unicode"))
```

Salida:

```xml
<products>
    <product id="2">Mouse</product>
</products>
```

## Eliminar elementos

```python
from lxml import etree

root = etree.fromstring("""
<products>
    <product>Teclado</product>
    <product>Mouse</product>
</products>
""")

first_product = root.find("product")
root.remove(first_product)

print(etree.tostring(root, encoding="unicode"))
```

## Namespaces en XML

En XML, algunos documentos usan namespaces.

Ejemplo:

```xml
<root xmlns:f="https://example.com/fields">
    <f:item>Teclado</f:item>
</root>
```

Para buscar con XPath, debe declararse el namespace.

```python
from lxml import etree

root = etree.fromstring("""
<root xmlns:f="https://example.com/fields">
    <f:item>Teclado</f:item>
</root>
""")

namespaces = {
    "f": "https://example.com/fields"
}

items = root.xpath("//f:item/text()", namespaces=namespaces)

print(items)
```

Salida:

```text
['Teclado']
```

## Validación XML

`lxml` puede validar XML con esquemas como DTD o XML Schema.

Ese uso es más avanzado y se suele reservar para proyectos donde se necesita verificar formalmente la estructura de documentos XML.

Ejemplo conceptual:

```python
from lxml import etree

schema_doc = etree.parse("schema.xsd")
schema = etree.XMLSchema(schema_doc)

xml_doc = etree.parse("document.xml")

schema.assertValid(xml_doc)
```

## XSLT

`lxml` permite aplicar transformaciones XSLT sobre XML.

Este uso es avanzado y aparece en flujos donde un documento XML debe transformarse en otro formato XML, HTML o texto.

Ejemplo conceptual:

```python
from lxml import etree

xml = etree.parse("input.xml")
xslt = etree.parse("transform.xsl")

transform = etree.XSLT(xslt)
result = transform(xml)

print(str(result))
```

## `lxml` como parser de Beautiful Soup

`lxml` puede usarse como parser dentro de Beautiful Soup.

```python
from bs4 import BeautifulSoup

html_content = """
<html>
    <body>
        <p>Texto</p>
    </body>
</html>
"""

soup = BeautifulSoup(html_content, "lxml")

print(soup.p.get_text())
```

Para esto, deben estar instalados ambos paquetes:

```bash
python -m pip install beautifulsoup4 lxml
```

## Diferencia entre `lxml` y `beautifulsoup4`

## `beautifulsoup4`

- más amigable para scraping inicial
- tolerante con HTML irregular
- API simple para buscar etiquetas
- suele usarse con `find()`, `find_all()` y `select()`

## `lxml`

- más rápido en muchos escenarios
- más potente para XPath
- más fuerte en XML
- permite validación y XSLT
- exige más cuidado con rutas y estructura

## Regla práctica

Para scraping simple de HTML, `beautifulsoup4` suele ser más accesible.

Para XPath, XML, validación, XSLT o extracción más estructurada, `lxml` es una opción más potente.

## Errores comunes

## Usar `etree.fromstring()` con texto Unicode que declara encoding

Problemático en algunos casos:

```python
from lxml import etree

xml = '<?xml version="1.0" encoding="UTF-8"?><root></root>'
root = etree.fromstring(xml)
```

Puede ser preferible trabajar con bytes cuando el XML declara codificación:

```python
root = etree.fromstring(xml.encode("utf-8"))
```

## No validar listas devueltas por XPath

Problemático:

```python
title = tree.xpath("//h1/text()")[0]
```

Si no existe `h1`, se genera `IndexError`.

Más seguro:

```python
titles = tree.xpath("//h1/text()")

if titles:
    title = titles[0]
else:
    title = None
```

## Usar XPath absoluto demasiado frágil

Problemático:

```python
tree.xpath("/html/body/div[3]/section/div[2]/p/text()")
```

Puede romperse si cambia la estructura.

Suele ser mejor buscar atributos estables:

```python
tree.xpath("//div[@class='product']//span[@class='name']/text()")
```

## Confundir atributo con texto

Texto:

```python
tree.xpath("//a/text()")
```

Atributo:

```python
tree.xpath("//a/@href")
```

## No revisar el estado HTTP antes de parsear

Problemático:

```python
response = requests.get(url)
tree = html.fromstring(response.text)
```

Mejor:

```python
response = requests.get(url, timeout=10)
response.raise_for_status()
tree = html.fromstring(response.text)
```

## Usar `lxml` para páginas renderizadas por JavaScript

`lxml` procesa HTML recibido. Si el contenido aparece solo después de ejecutar JavaScript en el navegador, `lxml` no lo verá directamente.

En esos casos pueden ser necesarias herramientas como:

```text
selenium
playwright
```

## Buenas prácticas

## Usar `html.fromstring()` para HTML

```python
tree = html.fromstring(content)
```

## Usar `etree.fromstring()` para XML

```python
root = etree.fromstring(content)
```

## Preferir XPath relativo dentro de bloques repetidos

```python
product.xpath(".//span[@class='name']/text()")
```

## Validar resultados antes de acceder por índice

```python
values = tree.xpath("//h1/text()")

if values:
    title = values[0]
```

## Usar `response.raise_for_status()` antes de parsear contenido remoto

```python
response.raise_for_status()
```

## Separar descarga y extracción

```python
html_content = fetch_html(url)
records = extract_records(html_content)
```

## Usar `lxml` cuando se requiera XPath o procesamiento XML avanzado

No siempre es necesario para scraping simple, pero es muy útil cuando se necesita precisión estructural.

## Ejemplo integrado

```python
from pathlib import Path

import requests
from lxml import html


def fetch_html(url):
    response = requests.get(
        url,
        timeout=10,
        headers={"User-Agent": "python-lxml-demo"}
    )

    response.raise_for_status()

    return response.text


def extract_links(content):
    tree = html.fromstring(content)

    records = []

    for link in tree.xpath("//a[@href]"):
        text = link.text_content().strip()
        href = link.get("href")

        records.append({
            "text": text,
            "href": href
        })

    return records


def save_links(records, output_path):
    lines = []

    for record in records:
        lines.append(f"{record['text']} -> {record['href']}")

    output_path.write_text(
        "\n".join(lines),
        encoding="utf-8"
    )


url = "https://example.com"

content = fetch_html(url)
links = extract_links(content)

save_links(
    links,
    Path("links.txt")
)

print("Enlaces guardados correctamente")
```

## Relación con otras librerías

`lxml` se relaciona especialmente con:

- `beautifulsoup4`, como parser o alternativa para extracción
- `requests`, para descargar HTML o XML
- `httpx`, como alternativa HTTP síncrona o asíncrona
- `aiohttp`, para descargas asíncronas
- `pandas`, para organizar datos extraídos
- `selenium` y `playwright`, cuando la página depende de JavaScript
- `xml.etree.ElementTree`, como alternativa incluida en la biblioteca estándar para XML básico

## Orden didáctico interno

```text
1. Propósito de lxml
2. Instalación e importación
3. Diferencia entre XML y HTML
4. Parseo con html.fromstring() y etree.fromstring()
5. Elementos, texto y atributos
6. find(), findall() e iter()
7. XPath
8. Serialización y modificación
9. Uso con requests, httpx o aiohttp
10. Relación con Beautiful Soup
11. Errores comunes
12. Buenas prácticas
```
