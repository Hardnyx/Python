# `scrapy`

## Propósito

`scrapy` es un framework externo para crawling y web scraping. Se utiliza para recorrer sitios web, seguir enlaces, enviar solicitudes, procesar respuestas, extraer datos estructurados y exportar resultados en formatos como JSON, CSV o XML.

A diferencia de combinar manualmente `requests` con `beautifulsoup4`, Scrapy está diseñado para proyectos de scraping más estructurados, con spiders, configuración centralizada, pipelines, middlewares y exportación integrada de datos.

## Naturaleza de la librería

`scrapy` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install scrapy
```

La importación principal en spiders suele ser:

```python
import scrapy
```

Además, Scrapy incluye una herramienta de línea de comandos:

```bash
scrapy
```

Esta herramienta permite crear proyectos, ejecutar spiders, abrir una shell interactiva y exportar datos.

## Idea central

La idea principal de Scrapy consiste en definir spiders.

Un spider es una clase que indica:

- qué URLs visitar
- cómo procesar las respuestas
- qué datos extraer
- qué enlaces seguir
- qué elementos devolver

Flujo general:

```text
spider -> requests -> responses -> parse() -> items -> pipelines / exports
```

## Cuándo usar Scrapy

Scrapy conviene cuando se necesita:

- extraer datos de muchas páginas
- seguir enlaces automáticamente
- estructurar un proyecto de scraping
- exportar datos en JSON, CSV o XML
- aplicar reglas de configuración
- reutilizar spiders
- manejar pipelines de limpieza o validación
- escalar más allá de un script simple

## Cuándo no usar Scrapy

Scrapy puede ser demasiado para tareas pequeñas.

Si solo se necesita descargar una página y extraer dos o tres elementos, puede bastar con:

```text
requests
beautifulsoup4
lxml
```

Si la página depende fuertemente de JavaScript, puede ser necesario usar:

```text
selenium
playwright
```

o combinar Scrapy con herramientas capaces de renderizar contenido dinámico.

## Instalación

Instalación básica:

```bash
python -m pip install scrapy
```

Verificación:

```bash
scrapy version
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
Scrapy==2.x.x
```

La versión exacta puede variar según el entorno.

## Crear un proyecto

Scrapy permite crear una estructura base de proyecto con:

```bash
scrapy startproject mi_scraper
```

Esto genera una estructura similar a:

```text
mi_scraper/
├─ scrapy.cfg
└─ mi_scraper/
   ├─ __init__.py
   ├─ items.py
   ├─ middlewares.py
   ├─ pipelines.py
   ├─ settings.py
   └─ spiders/
      └─ __init__.py
```

## Archivos principales del proyecto

## `scrapy.cfg`

Archivo de configuración principal del proyecto.

## `settings.py`

Contiene configuraciones del proyecto, como:

- user agent
- delays
- pipelines activos
- concurrencia
- reglas de robots.txt
- exportaciones
- middlewares

## `items.py`

Permite definir estructuras de datos para los elementos extraídos.

## `pipelines.py`

Permite procesar, limpiar, validar o guardar los datos extraídos.

## `middlewares.py`

Permite intervenir en el flujo de solicitudes y respuestas.

## `spiders/`

Carpeta donde se definen los spiders.

## Spider básico

Un spider es una clase que hereda de `scrapy.Spider`.

```python
import scrapy


class ExampleSpider(scrapy.Spider):
    name = "example"
    start_urls = [
        "https://example.com"
    ]

    def parse(self, response):
        title = response.css("title::text").get()

        yield {
            "title": title
        }
```

Elementos importantes:

- `name`: nombre con el que se ejecuta el spider
- `start_urls`: URLs iniciales
- `parse()`: método que procesa cada respuesta
- `yield`: devuelve datos o nuevas solicitudes

## Ejecutar un spider

Desde la raíz del proyecto:

```bash
scrapy crawl example
```

Para exportar resultados:

```bash
scrapy crawl example -O output.json
```

También puede exportarse a CSV:

```bash
scrapy crawl example -O output.csv
```

## `name`

Cada spider debe tener un nombre único dentro del proyecto.

```python
name = "products"
```

Se usa para ejecutarlo:

```bash
scrapy crawl products
```

## `start_urls`

`start_urls` contiene las URLs iniciales que Scrapy visitará.

```python
start_urls = [
    "https://example.com/products"
]
```

Scrapy enviará solicitudes a esas URLs y procesará las respuestas con `parse()`.

## `parse()`

`parse()` es el método principal de extracción.

```python
def parse(self, response):
    title = response.css("title::text").get()

    yield {
        "title": title
    }
```

El parámetro `response` representa la respuesta recibida desde la página.

## `response`

El objeto `response` permite acceder al contenido descargado.

Usos frecuentes:

```python
response.url
response.status
response.text
response.body
response.css()
response.xpath()
```

## Selectores

Scrapy permite extraer datos mediante selectores CSS o XPath.

## CSS selectors

```python
response.css("title::text").get()
```

## XPath

```python
response.xpath("//title/text()").get()
```

Ambos estilos son válidos. CSS suele ser más simple para empezar. XPath puede ser más potente para búsquedas estructurales.

## `.get()`

Devuelve la primera coincidencia.

```python
title = response.css("title::text").get()
```

Si no encuentra nada, devuelve `None`.

## `.getall()`

Devuelve todas las coincidencias como lista.

```python
links = response.css("a::attr(href)").getall()
```

## `.attrib`

Permite acceder a atributos de un elemento seleccionado.

```python
href = response.css("a").attrib["href"]
```

En muchos casos es más seguro usar selectores de atributo:

```python
href = response.css("a::attr(href)").get()
```

## Extraer texto

```python
title = response.css("h1::text").get()
```

Para múltiples textos:

```python
items = response.css(".item::text").getall()
```

## Extraer atributos

```python
links = response.css("a::attr(href)").getall()
```

Ejemplo:

```python
def parse(self, response):
    for href in response.css("a::attr(href)").getall():
        yield {
            "href": href
        }
```

## Extraer bloques repetidos

```python
import scrapy


class ProductsSpider(scrapy.Spider):
    name = "products"
    start_urls = [
        "https://example.com/products"
    ]

    def parse(self, response):
        for product in response.css(".product"):
            yield {
                "name": product.css(".name::text").get(),
                "price": product.css(".price::text").get()
            }
```

## Seguir enlaces

Scrapy puede generar nuevas solicitudes con `response.follow()`.

```python
def parse(self, response):
    for href in response.css("a::attr(href)").getall():
        yield response.follow(href, callback=self.parse_detail)
```

Luego se define el callback:

```python
def parse_detail(self, response):
    yield {
        "url": response.url,
        "title": response.css("title::text").get()
    }
```

## Spider con páginas de detalle

```python
import scrapy


class ProductsSpider(scrapy.Spider):
    name = "products"
    start_urls = [
        "https://example.com/products"
    ]

    def parse(self, response):
        for href in response.css(".product a::attr(href)").getall():
            yield response.follow(
                href,
                callback=self.parse_product
            )

    def parse_product(self, response):
        yield {
            "url": response.url,
            "name": response.css("h1::text").get(),
            "price": response.css(".price::text").get()
        }
```

## Paginación

Un patrón común es extraer datos y luego seguir el enlace de la página siguiente.

```python
def parse(self, response):
    for product in response.css(".product"):
        yield {
            "name": product.css(".name::text").get(),
            "price": product.css(".price::text").get()
        }

    next_page = response.css("a.next::attr(href)").get()

    if next_page is not None:
        yield response.follow(next_page, callback=self.parse)
```

## Items

Los items permiten definir una estructura formal para los datos extraídos.

Archivo `items.py`:

```python
import scrapy


class ProductItem(scrapy.Item):
    name = scrapy.Field()
    price = scrapy.Field()
    url = scrapy.Field()
```

Uso en un spider:

```python
from mi_scraper.items import ProductItem


def parse_product(self, response):
    item = ProductItem()

    item["name"] = response.css("h1::text").get()
    item["price"] = response.css(".price::text").get()
    item["url"] = response.url

    yield item
```

## Diccionarios vs items

Para proyectos simples, devolver diccionarios es suficiente:

```python
yield {
    "name": name,
    "price": price
}
```

Para proyectos más estructurados, los items ayudan a definir campos esperados:

```python
item = ProductItem()
item["name"] = name
item["price"] = price
yield item
```

## Item pipelines

Los pipelines procesan los datos después de ser extraídos.

Usos frecuentes:

- limpiar texto
- convertir precios
- validar campos obligatorios
- eliminar duplicados
- guardar en base de datos
- enriquecer datos

Archivo `pipelines.py`:

```python
class CleanProductPipeline:
    def process_item(self, item, spider):
        if item.get("name") is not None:
            item["name"] = item["name"].strip()

        if item.get("price") is not None:
            item["price"] = item["price"].strip()

        return item
```

Activación en `settings.py`:

```python
ITEM_PIPELINES = {
    "mi_scraper.pipelines.CleanProductPipeline": 300,
}
```

## Orden de pipelines

El número indica el orden de ejecución.

```python
ITEM_PIPELINES = {
    "mi_scraper.pipelines.CleanProductPipeline": 300,
    "mi_scraper.pipelines.SaveProductPipeline": 400,
}
```

Los valores menores se ejecutan antes.

## Exportación de datos

Scrapy puede exportar datos desde la terminal.

## JSON

```bash
scrapy crawl products -O products.json
```

## CSV

```bash
scrapy crawl products -O products.csv
```

## JSON Lines

```bash
scrapy crawl products -O products.jl
```

`-O` sobrescribe el archivo si ya existe.

También existe `-o`, que puede agregar o escribir según el formato y contexto.

## Scrapy shell

Scrapy shell permite probar selectores de forma interactiva.

```bash
scrapy shell "https://example.com"
```

Dentro de la shell:

```python
response.css("title::text").get()
response.css("a::attr(href)").getall()
response.xpath("//h1/text()").get()
```

Es útil antes de escribir el spider final.

## Settings importantes

Algunas configuraciones frecuentes en `settings.py` son:

```python
ROBOTSTXT_OBEY = True
DOWNLOAD_DELAY = 1
CONCURRENT_REQUESTS = 8
USER_AGENT = "mi-scraper (+https://example.com/contact)"
```

## `ROBOTSTXT_OBEY`

Define si Scrapy debe respetar `robots.txt`.

```python
ROBOTSTXT_OBEY = True
```

## `DOWNLOAD_DELAY`

Agrega una pausa entre solicitudes.

```python
DOWNLOAD_DELAY = 1
```

Esto reduce la carga sobre el servidor.

## `CONCURRENT_REQUESTS`

Controla cuántas solicitudes pueden ejecutarse en paralelo.

```python
CONCURRENT_REQUESTS = 8
```

## `USER_AGENT`

Permite identificar el cliente.

```python
USER_AGENT = "mi-scraper (+https://example.com/contact)"
```

## Headers personalizados

Puede pasarse metadata o headers en solicitudes específicas.

```python
yield scrapy.Request(
    url="https://example.com",
    callback=self.parse,
    headers={
        "User-Agent": "mi-scraper"
    }
)
```

## Pasar datos entre callbacks

Puede usarse `meta`.

```python
def parse(self, response):
    for href in response.css(".product a::attr(href)").getall():
        yield response.follow(
            href,
            callback=self.parse_product,
            meta={"category": "tecnologia"}
        )


def parse_product(self, response):
    category = response.meta["category"]

    yield {
        "category": category,
        "name": response.css("h1::text").get()
    }
```

## Limpieza de datos

Ejemplo de función auxiliar:

```python
def clean_text(value):
    if value is None:
        return None

    return value.strip()
```

Uso:

```python
yield {
    "name": clean_text(response.css("h1::text").get()),
    "price": clean_text(response.css(".price::text").get())
}
```

## Conversión de datos

Ejemplo:

```python
def parse_price(value):
    if value is None:
        return None

    cleaned = value.replace("S/", "").replace(",", "").strip()

    return float(cleaned)
```

Uso:

```python
yield {
    "price": parse_price(response.css(".price::text").get())
}
```

## Middleware

Los middlewares permiten intervenir en solicitudes y respuestas.

Usos frecuentes:

- modificar headers
- manejar proxies
- controlar user agents
- procesar errores
- ajustar comportamiento de descarga

En una documentación inicial, conviene entender primero spiders, selectors, items, pipelines y settings antes de profundizar en middlewares.

## Diferencia frente a Beautiful Soup

## Beautiful Soup

- procesa HTML ya descargado
- es simple y flexible
- útil para scripts pequeños
- no gestiona crawling por sí mismo

## Scrapy

- descarga páginas
- sigue enlaces
- ejecuta spiders
- gestiona concurrencia
- exporta datos
- permite pipelines y configuración centralizada

## Regla práctica

Para una página simple, `requests` + `beautifulsoup4` puede ser suficiente.

Para crawling estructurado, múltiples páginas, paginación, pipelines y exportaciones, Scrapy suele ser más apropiado.

## Diferencia frente a Selenium o Playwright

## Selenium y Playwright

- controlan navegadores reales
- ejecutan JavaScript
- interactúan con botones, formularios y eventos
- son más pesados

## Scrapy

- trabaja principalmente con solicitudes HTTP
- no renderiza JavaScript como un navegador
- es más eficiente para crawling estructurado
- necesita HTML disponible en la respuesta

Si el contenido solo aparece después de ejecutar JavaScript, Scrapy por sí solo puede no ser suficiente.

## Casos de uso frecuentes

## Extraer listados

```python
def parse(self, response):
    for product in response.css(".product"):
        yield {
            "name": product.css(".name::text").get(),
            "price": product.css(".price::text").get()
        }
```

## Seguir páginas de detalle

```python
yield response.follow(
    href,
    callback=self.parse_product
)
```

## Exportar resultados a CSV

```bash
scrapy crawl products -O products.csv
```

## Probar selectores

```bash
scrapy shell "https://example.com"
```

## Aplicar limpieza con pipelines

```python
class CleanPipeline:
    def process_item(self, item, spider):
        ...
        return item
```

## Errores comunes

## Usar Scrapy para una tarea demasiado pequeña

Para extraer un solo dato de una página estática, puede ser excesivo crear todo un proyecto Scrapy.

## No probar selectores antes de escribir el spider

Menos conveniente:

```python
response.css(".product .name::text").get()
```

sin haberlo probado.

Mejor:

```bash
scrapy shell "https://example.com"
```

y luego probar:

```python
response.css(".product .name::text").getall()
```

## Suponer que Scrapy ejecuta JavaScript

Scrapy no se comporta como un navegador completo. Si el contenido depende de JavaScript, puede no aparecer en `response.text`.

## No controlar paginación

Extraer solo la primera página puede dejar incompleto el dataset.

## No validar valores `None`

Problemático:

```python
name = response.css("h1::text").get().strip()
```

Si no se encuentra el `h1`, se genera error.

Más seguro:

```python
name = response.css("h1::text").get()

if name is not None:
    name = name.strip()
```

## Sobrecargar servidores

No configurar delays, concurrencia o reglas de crawling puede generar un comportamiento agresivo.

## No respetar condiciones del sitio

Antes de hacer scraping debe revisarse si el sitio lo permite, preferir APIs oficiales cuando existan y evitar sobrecargar servidores.

## Buenas prácticas

## Probar selectores en Scrapy shell

```bash
scrapy shell "https://example.com"
```

## Usar `response.follow()` para enlaces relativos

```python
yield response.follow(href, callback=self.parse_detail)
```

## Validar datos antes de limpiarlos

```python
value = response.css(".price::text").get()

if value is not None:
    value = value.strip()
```

## Separar extracción y limpieza

```python
def clean_text(value):
    ...
```

## Usar pipelines para limpieza repetida

```python
ITEM_PIPELINES = {
    "mi_scraper.pipelines.CleanPipeline": 300,
}
```

## Controlar velocidad de scraping

```python
DOWNLOAD_DELAY = 1
CONCURRENT_REQUESTS = 8
```

## Identificar el scraper con un user agent claro

```python
USER_AGENT = "mi-scraper (+https://example.com/contact)"
```

## Preferir APIs oficiales cuando existan

Si el sitio ofrece una API estable, suele ser mejor usarla que extraer HTML.

## Ejemplo integrado

Archivo:

```text
mi_scraper/spiders/products.py
```

Código:

```python
import scrapy


def clean_text(value):
    if value is None:
        return None

    return value.strip()


def parse_price(value):
    if value is None:
        return None

    cleaned = (
        value
        .replace("S/", "")
        .replace(",", "")
        .strip()
    )

    try:
        return float(cleaned)
    except ValueError:
        return None


class ProductsSpider(scrapy.Spider):
    name = "products"

    start_urls = [
        "https://example.com/products"
    ]

    def parse(self, response):
        for product in response.css(".product"):
            href = product.css("a::attr(href)").get()

            if href is None:
                continue

            yield response.follow(
                href,
                callback=self.parse_product
            )

        next_page = response.css("a.next::attr(href)").get()

        if next_page is not None:
            yield response.follow(
                next_page,
                callback=self.parse
            )

    def parse_product(self, response):
        yield {
            "url": response.url,
            "name": clean_text(response.css("h1::text").get()),
            "price": parse_price(response.css(".price::text").get()),
            "description": clean_text(response.css(".description::text").get())
        }
```

Ejecución:

```bash
scrapy crawl products -O products.csv
```

## Relación con otras librerías

`scrapy` se relaciona especialmente con:

- `beautifulsoup4`, como alternativa más simple para HTML ya descargado
- `lxml`, porque Scrapy puede trabajar con selectores XPath y parsing estructurado
- `requests` y `httpx`, como alternativas más simples para solicitudes puntuales
- `selenium` y `playwright`, cuando se necesita navegador real o JavaScript
- `pandas`, para analizar resultados exportados
- `python-dotenv`, para cargar configuración, rutas o credenciales
- `pytest`, para probar funciones auxiliares de limpieza o parsing

## Orden didáctico interno

```text
1. Propósito de scrapy
2. Instalación y creación de proyecto
3. Estructura del proyecto
4. Spiders
5. Response y selectors
6. CSS y XPath
7. Seguir enlaces y paginación
8. Items y pipelines
9. Exportación de datos
10. Settings
11. Scrapy shell
12. Errores comunes
13. Buenas prácticas
```