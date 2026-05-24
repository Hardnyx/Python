# `playwright`

## Propósito

`playwright` es una librería externa para automatizar navegadores web desde Python. Se utiliza para interactuar con páginas dinámicas, ejecutar pruebas end-to-end, hacer clic en elementos, llenar formularios, esperar contenido renderizado, capturar pantallas, extraer información de páginas con JavaScript y validar comportamientos reales de una aplicación web.

## Naturaleza de la librería

`playwright` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install playwright
```

Después de instalar el paquete, normalmente también deben instalarse los navegadores administrados por Playwright:

```bash
python -m playwright install
```

Playwright puede automatizar Chromium, Firefox y WebKit. También puede ejecutarse en modo visible o headless, según la configuración. :contentReference[oaicite:1]{index=1}

## Importaciones habituales

Playwright para Python tiene dos APIs principales:

```python
from playwright.sync_api import sync_playwright
```

y:

```python
from playwright.async_api import async_playwright
```

La primera se usa en código síncrono.

La segunda se usa en código asíncrono con `async` y `await`.

## Idea central

La idea principal de Playwright es controlar un navegador real mediante código.

Flujo típico:

```text
Python -> Playwright -> navegador -> página web
```

Esto permite automatizar páginas que no pueden procesarse correctamente solo con `requests`, `httpx`, `beautifulsoup4` o `lxml`, especialmente cuando el contenido depende de JavaScript.

## Cuándo usar Playwright

Conviene usar Playwright cuando se necesita:

- abrir una página en un navegador real
- esperar contenido generado por JavaScript
- hacer clic en botones
- escribir en formularios
- capturar pantallas
- probar interfaces web
- automatizar flujos de usuario
- interactuar con elementos que aparecen dinámicamente
- trabajar con Chromium, Firefox o WebKit desde una API común

## Cuándo no usar Playwright

No conviene usar Playwright si basta con una solicitud HTTP simple.

Para APIs o HTML estático, normalmente es más liviano usar:

```text
requests
httpx
aiohttp
beautifulsoup4
lxml
```

Playwright abre y controla navegadores, por lo que es más pesado que una solicitud HTTP directa.

## Instalación

Instalación del paquete:

```bash
python -m pip install playwright
```

Instalación de navegadores:

```bash
python -m playwright install
```

Instalación de un navegador específico:

```bash
python -m playwright install chromium
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
playwright==1.x.x
```

La versión exacta puede variar según el entorno.

## Primer ejemplo síncrono

```python
from playwright.sync_api import sync_playwright


with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()

    page.goto("https://example.com")

    print(page.title())

    browser.close()
```

Este patrón usa la API síncrona, adecuada para scripts simples y automatizaciones secuenciales. La documentación oficial muestra este flujo con `sync_playwright()`, creación del navegador, nueva página, navegación y cierre del navegador. :contentReference[oaicite:2]{index=2}

## Primer ejemplo asíncrono

```python
import asyncio

from playwright.async_api import async_playwright


async def main():
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page()

        await page.goto("https://example.com")

        print(await page.title())

        await browser.close()


asyncio.run(main())
```

La API asíncrona es más apropiada cuando el proyecto ya usa `asyncio` o cuando se necesita coordinar varias operaciones asíncronas.

## Navegadores disponibles

Playwright puede trabajar con:

```text
chromium
firefox
webkit
```

Ejemplo con Chromium:

```python
browser = p.chromium.launch()
```

Ejemplo con Firefox:

```python
browser = p.firefox.launch()
```

Ejemplo con WebKit:

```python
browser = p.webkit.launch()
```

Playwright también puede trabajar con navegadores de marca como Google Chrome o Microsoft Edge cuando se configura el canal correspondiente. :contentReference[oaicite:3]{index=3}

## Modo headless

Por defecto, Playwright suele ejecutar el navegador en modo headless, es decir, sin ventana visible.

Para abrir el navegador de forma visible:

```python
browser = p.chromium.launch(headless=False)
```

Ejemplo:

```python
from playwright.sync_api import sync_playwright


with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()

    page.goto("https://example.com")

    print(page.title())

    browser.close()
```

## Browser, context y page

Playwright organiza la automatización en varios niveles.

## Browser

Representa el navegador abierto.

```python
browser = p.chromium.launch()
```

## BrowserContext

Representa un contexto aislado dentro del navegador. Puede tener sus propias cookies, almacenamiento, permisos y configuración.

```python
context = browser.new_context()
```

## Page

Representa una pestaña o página dentro del contexto.

```python
page = context.new_page()
```

Ejemplo completo:

```python
from playwright.sync_api import sync_playwright


with sync_playwright() as p:
    browser = p.chromium.launch()
    context = browser.new_context()
    page = context.new_page()

    page.goto("https://example.com")

    print(page.title())

    context.close()
    browser.close()
```

## Navegar a una página

```python
page.goto("https://example.com")
```

## Leer título

```python
title = page.title()
print(title)
```

## Leer URL actual

```python
print(page.url)
```

## Leer contenido HTML

```python
html = page.content()
print(html[:500])
```

## Localizadores

Los localizadores son una pieza central de Playwright. Representan una forma de encontrar elementos en la página y están diseñados para auto-waiting y retry-ability. Esto significa que Playwright puede esperar automáticamente a que el elemento esté disponible antes de interactuar con él. :contentReference[oaicite:4]{index=4}

## `page.locator()`

```python
button = page.locator("button")
button.click()
```

## Buscar por texto

```python
page.get_by_text("Enviar").click()
```

## Buscar por rol

```python
page.get_by_role("button", name="Enviar").click()
```

## Buscar por etiqueta visible

```python
page.get_by_label("Usuario").fill("admin")
```

## Buscar por placeholder

```python
page.get_by_placeholder("Buscar").fill("Python")
```

## Buscar por título

```python
page.get_by_title("Cerrar").click()
```

Playwright recomienda localizadores más semánticos cuando es posible, como rol, texto, label o placeholder, porque suelen expresar mejor la intención de la prueba o automatización. :contentReference[oaicite:5]{index=5}

## Click

```python
page.get_by_role("button", name="Enviar").click()
```

Con CSS selector:

```python
page.locator("button.submit").click()
```

## Escribir texto

```python
page.get_by_label("Usuario").fill("admin")
```

`fill()` reemplaza el contenido del campo.

Para simular escritura tecla por tecla puede usarse:

```python
page.get_by_label("Usuario").press_sequentially("admin")
```

## Presionar teclas

```python
page.get_by_label("Buscar").press("Enter")
```

También puede usarse:

```python
page.keyboard.press("Enter")
```

## Seleccionar opciones

```python
page.locator("select").select_option("valor")
```

## Checkboxes

```python
page.get_by_label("Acepto términos").check()
```

Para desmarcar:

```python
page.get_by_label("Acepto términos").uncheck()
```

## Leer texto

```python
text = page.locator("h1").inner_text()
print(text)
```

Para varios elementos:

```python
items = page.locator(".item").all_inner_texts()
print(items)
```

## Leer atributos

```python
href = page.locator("a").first.get_attribute("href")
print(href)
```

## Esperas automáticas

Una ventaja importante de Playwright es que muchas acciones incluyen esperas automáticas. Por ejemplo, al hacer clic con un locator, Playwright espera condiciones razonables antes de ejecutar la acción.

Esto reduce la necesidad de usar pausas fijas como:

```python
time.sleep(5)
```

Aun así, en algunos casos se pueden usar esperas explícitas.

## Esperar por selector

```python
page.wait_for_selector(".result")
```

## Esperar por URL

```python
page.wait_for_url("**/dashboard")
```

## Esperar carga de red

```python
page.wait_for_load_state("networkidle")
```

## Assertions con `expect`

Playwright incluye aserciones para pruebas. Estas aserciones pueden esperar automáticamente a que una condición se cumpla dentro de un tiempo.

Importación:

```python
from playwright.sync_api import expect
```

Ejemplo:

```python
expect(page.get_by_role("heading")).to_be_visible()
```

Para texto:

```python
expect(page.locator(".status")).to_have_text("Completado")
```

Las aserciones sobre localizadores se usan para validar estado, texto, visibilidad y otras condiciones de elementos. :contentReference[oaicite:6]{index=6}

## Capturas de pantalla

Captura de página completa:

```python
page.screenshot(path="screenshot.png", full_page=True)
```

Captura de un elemento:

```python
page.locator(".card").screenshot(path="card.png")
```

## Descargar archivos

Playwright permite esperar descargas iniciadas por interacción.

```python
with page.expect_download() as download_info:
    page.get_by_text("Descargar").click()

download = download_info.value
download.save_as("archivo_descargado.xlsx")
```

## Subir archivos

```python
page.locator("input[type='file']").set_input_files("archivo.xlsx")
```

## Ejecutar JavaScript

```python
height = page.evaluate("document.body.scrollHeight")
print(height)
```

Con argumentos:

```python
text = page.evaluate(
    "(selector) => document.querySelector(selector).innerText",
    "h1"
)

print(text)
```

## Scroll

```python
page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
```

Scroll hacia un elemento:

```python
page.locator(".target").scroll_into_view_if_needed()
```

## Manejo de múltiples páginas

Algunas acciones abren una nueva pestaña o ventana.

```python
with context.expect_page() as page_info:
    page.get_by_text("Abrir detalle").click()

new_page = page_info.value
new_page.wait_for_load_state()

print(new_page.title())
```

## Manejo de diálogos

```python
page.on("dialog", lambda dialog: dialog.accept())

page.get_by_text("Eliminar").click()
```

Para rechazar:

```python
page.on("dialog", lambda dialog: dialog.dismiss())
```

## Cookies y almacenamiento

Crear contexto con estado guardado:

```python
context = browser.new_context(storage_state="state.json")
```

Guardar estado:

```python
context.storage_state(path="state.json")
```

Esto puede ser útil para conservar sesión entre ejecuciones controladas.

## Modo con viewport

```python
context = browser.new_context(
    viewport={"width": 1366, "height": 768}
)
```

## Emulación móvil

Playwright permite usar parámetros de dispositivos emulados. Su documentación indica soporte para emulación de tablets y móviles. :contentReference[oaicite:7]{index=7}

Ejemplo conceptual:

```python
iphone = p.devices["iPhone 13"]

context = browser.new_context(**iphone)
page = context.new_page()
```

## Uso con Beautiful Soup

Playwright puede renderizar una página y luego entregar el HTML a Beautiful Soup.

```python
from bs4 import BeautifulSoup
from playwright.sync_api import sync_playwright


with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()

    page.goto("https://example.com")

    html = page.content()
    soup = BeautifulSoup(html, "html.parser")

    print(soup.title.get_text(strip=True))

    browser.close()
```

Este patrón es útil cuando el contenido aparece después de ejecutar JavaScript.

## Uso con pandas

Después de extraer registros, pueden organizarse en un `DataFrame`.

```python
import pandas as pd
from playwright.sync_api import sync_playwright


with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()

    page.goto("https://example.com")

    records = []

    for text in page.locator(".product").all_inner_texts():
        records.append({"text": text})

    df = pd.DataFrame(records)

    print(df)

    browser.close()
```

## API asíncrona

Playwright también ofrece API asíncrona.

```python
import asyncio

from playwright.async_api import async_playwright


async def main():
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page()

        await page.goto("https://example.com")
        print(await page.title())

        await browser.close()


asyncio.run(main())
```

## Diferencia entre API síncrona y asíncrona

## API síncrona

```python
from playwright.sync_api import sync_playwright
```

Se usa con scripts tradicionales.

## API asíncrona

```python
from playwright.async_api import async_playwright
```

Se usa con `async`, `await` y `asyncio`.

## Regla práctica

Para automatizaciones simples, la API síncrona suele ser suficiente.

Para proyectos que ya usan programación asíncrona, conviene usar la API asíncrona.

## Uso con pytest

Playwright puede integrarse con pruebas end-to-end. Para pruebas en Python, suele usarse junto con `pytest`.

Instalación frecuente:

```bash
python -m pip install pytest-playwright
python -m playwright install
```

Ejemplo conceptual:

```python
from playwright.sync_api import Page, expect


def test_title(page: Page):
    page.goto("https://example.com")

    expect(page).to_have_title("Example Domain")
```

## Comparación con Selenium

## Selenium

- muy extendido
- amplio soporte histórico
- usa WebDriver
- adecuado para automatización general de navegadores
- depende de estrategias de espera explícitas en muchos casos

## Playwright

- API moderna
- buen auto-waiting
- soporte directo para Chromium, Firefox y WebKit
- locators expresivos
- buena integración con pruebas end-to-end
- API síncrona y asíncrona en Python

## Regla práctica

Para proyectos nuevos de automatización o testing web, Playwright suele resultar muy cómodo por sus locators y auto-waiting.

Selenium sigue siendo importante por compatibilidad, trayectoria e integración con ecosistemas existentes.

## Errores comunes

## Instalar el paquete pero no los navegadores

Problemático:

```bash
python -m pip install playwright
```

y luego ejecutar el script sin haber corrido:

```bash
python -m playwright install
```

La instalación de navegadores es necesaria en configuraciones habituales. :contentReference[oaicite:8]{index=8}

## No cerrar el navegador

Problemático:

```python
browser = p.chromium.launch()
page = browser.new_page()
page.goto("https://example.com")
```

Mejor:

```python
browser = p.chromium.launch()

try:
    page = browser.new_page()
    page.goto("https://example.com")
finally:
    browser.close()
```

## Usar selectores demasiado frágiles

Problemático:

```python
page.locator("body > div:nth-child(3) > div > button").click()
```

Mejor usar localizadores más semánticos cuando existan:

```python
page.get_by_role("button", name="Enviar").click()
```

## Usar `time.sleep()` como primera opción

Menos recomendable:

```python
time.sleep(5)
```

Mejor apoyarse en locators, auto-waiting o esperas explícitas de Playwright.

## Confundir API síncrona y asíncrona

Problemático:

```python
from playwright.async_api import async_playwright

page.goto("https://example.com")
```

En API asíncrona se debe usar:

```python
await page.goto("https://example.com")
```

## Usar Playwright cuando basta con una API

Si el sitio expone una API JSON, suele ser mejor consumir la API con `requests` o `httpx`.

## Buenas prácticas

## Instalar navegadores después de instalar Playwright

```bash
python -m playwright install
```

## Usar locators semánticos cuando sea posible

```python
page.get_by_role("button", name="Enviar")
page.get_by_label("Correo")
page.get_by_text("Confirmar")
```

## Cerrar navegador o contexto

```python
browser.close()
```

o:

```python
context.close()
```

## Separar navegación, interacción y extracción

```python
page.goto(url)
page.get_by_role("button", name="Buscar").click()
records = extract_records(page)
```

## Evitar esperas fijas

Preferir locators, aserciones o esperas explícitas.

## Usar contextos para aislar sesiones

```python
context = browser.new_context()
```

## Usar `wss`, HTTPS y credenciales con cuidado

Las credenciales y tokens deben manejarse mediante variables de entorno cuando corresponda.

## Ejemplo integrado

```python
from pathlib import Path

from playwright.sync_api import sync_playwright


def extract_page_summary(page):
    title = page.title()
    heading = page.locator("h1").first.inner_text()

    return {
        "title": title,
        "heading": heading,
        "url": page.url
    }


def save_summary(summary, output_path):
    lines = [
        f"Title: {summary['title']}",
        f"Heading: {summary['heading']}",
        f"URL: {summary['url']}"
    ]

    output_path.write_text(
        "\n".join(lines),
        encoding="utf-8"
    )


with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)

    try:
        page = browser.new_page()
        page.goto("https://example.com")

        summary = extract_page_summary(page)

        page.screenshot(
            path="example.png",
            full_page=True
        )

        save_summary(
            summary,
            Path("playwright-summary.txt")
        )

        print("Resumen guardado correctamente")

    finally:
        browser.close()
```

## Relación con otras librerías

`playwright` se relaciona especialmente con:

- `selenium`, como alternativa de automatización de navegador
- `beautifulsoup4`, para analizar HTML renderizado
- `lxml`, para extracción estructurada
- `pandas`, para organizar datos extraídos
- `pytest`, para pruebas end-to-end
- `python-dotenv`, para cargar credenciales, rutas o URLs
- `requests` y `httpx`, como alternativas más livianas cuando no se necesita navegador real

## Orden didáctico interno

```text
1. Propósito de playwright
2. Instalación del paquete y navegadores
3. API síncrona y asíncrona
4. Browser, context y page
5. Navegación básica
6. Locators
7. Clicks, formularios y texto
8. Esperas automáticas y assertions
9. Capturas, descargas y subida de archivos
10. Uso con Beautiful Soup o pandas
11. Comparación con Selenium
12. Errores comunes
13. Buenas prácticas
```