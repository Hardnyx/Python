# `selenium`

## Propósito

`selenium` es una librería externa para automatizar navegadores web desde Python. Se utiliza para interactuar con páginas como si se estuviera usando un navegador real: abrir sitios, hacer clic, escribir en campos, enviar formularios, leer contenido renderizado, ejecutar JavaScript y automatizar pruebas o tareas web repetitivas.

## Naturaleza de la librería

`selenium` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install selenium
```

La importación habitual es:

```python
from selenium import webdriver
```

También se importan clases auxiliares como:

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
```

## Idea central

La idea principal de Selenium es controlar un navegador real mediante WebDriver.

Flujo típico:

```text
Python -> Selenium WebDriver -> navegador -> página web
```

Esto permite automatizar páginas que requieren interacción real del navegador, especialmente cuando el contenido se genera con JavaScript o cuando no basta con descargar HTML usando `requests`.

## Cuándo usar Selenium

Selenium conviene cuando se necesita:

- interactuar con páginas dinámicas
- hacer clic en botones
- llenar formularios
- esperar elementos que cargan con JavaScript
- automatizar pruebas de interfaz
- capturar comportamiento real del navegador
- trabajar con sitios donde `requests` no ve el contenido final

## Cuándo no usar Selenium

Selenium no debería ser la primera opción si basta con una solicitud HTTP simple.

Para una API o una página estática, normalmente conviene usar:

```text
requests
httpx
aiohttp
beautifulsoup4
lxml
```

Selenium es más pesado porque abre y controla un navegador real.

## Instalación

Instalación básica:

```bash
python -m pip install selenium
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
selenium==4.x.x
```

La versión exacta puede variar según el entorno.

## Importación básica

```python
from selenium import webdriver
```

Verificación:

```python
import selenium

print(selenium.__version__)
```

## Primer ejemplo con Chrome

```python
from selenium import webdriver

driver = webdriver.Chrome()

driver.get("https://example.com")

print(driver.title)

driver.quit()
```

## Cierre del navegador

Debe cerrarse el navegador al terminar.

```python
driver.quit()
```

`quit()` cierra la sesión completa del navegador.

## Uso con `try` y `finally`

```python
from selenium import webdriver

driver = webdriver.Chrome()

try:
    driver.get("https://example.com")
    print(driver.title)
finally:
    driver.quit()
```

Este patrón asegura que el navegador se cierre aunque ocurra un error.

## Uso con Firefox

```python
from selenium import webdriver

driver = webdriver.Firefox()

try:
    driver.get("https://example.com")
    print(driver.title)
finally:
    driver.quit()
```

## Selenium Manager

En versiones modernas de Selenium, Selenium Manager ayuda a gestionar automáticamente drivers de navegador en configuraciones estándar.

Esto reduce la necesidad de descargar manualmente ejecutables como `chromedriver` o `geckodriver` para casos comunes.

Aun así, pueden existir escenarios donde se requiera configuración manual, por ejemplo:

- entornos corporativos restringidos
- versiones específicas de navegador
- drivers en rutas personalizadas
- servidores remotos
- contenedores especiales

## WebDriver

`webdriver` es el componente que permite controlar el navegador.

Ejemplo:

```python
from selenium import webdriver

driver = webdriver.Chrome()
```

El objeto `driver` representa la sesión del navegador.

Con él se puede:

- abrir páginas
- buscar elementos
- leer contenido
- hacer clic
- escribir texto
- ejecutar JavaScript
- navegar hacia atrás o adelante
- cerrar el navegador

## Abrir una página

```python
from selenium import webdriver

driver = webdriver.Chrome()

try:
    driver.get("https://example.com")
finally:
    driver.quit()
```

## Leer título

```python
print(driver.title)
```

## Leer URL actual

```python
print(driver.current_url)
```

## Leer HTML de la página

```python
html = driver.page_source
print(html[:500])
```

## Localización de elementos

Para buscar elementos se usa normalmente `find_element()` o `find_elements()` junto con `By`.

Importación:

```python
from selenium.webdriver.common.by import By
```

## `find_element()`

Devuelve el primer elemento que coincide.

```python
element = driver.find_element(By.TAG_NAME, "h1")
print(element.text)
```

Si no encuentra el elemento, lanza una excepción.

## `find_elements()`

Devuelve una lista de elementos.

```python
items = driver.find_elements(By.TAG_NAME, "a")

for item in items:
    print(item.text)
```

Si no encuentra coincidencias, devuelve una lista vacía.

## Estrategias de búsqueda

Las estrategias más comunes son:

```python
By.ID
By.NAME
By.CLASS_NAME
By.TAG_NAME
By.CSS_SELECTOR
By.XPATH
By.LINK_TEXT
By.PARTIAL_LINK_TEXT
```

## Buscar por id

```python
element = driver.find_element(By.ID, "main")
```

## Buscar por name

```python
field = driver.find_element(By.NAME, "q")
```

## Buscar por clase

```python
items = driver.find_elements(By.CLASS_NAME, "item")
```

## Buscar por etiqueta

```python
links = driver.find_elements(By.TAG_NAME, "a")
```

## Buscar con CSS selector

```python
button = driver.find_element(By.CSS_SELECTOR, "button.primary")
```

## Buscar con XPath

```python
button = driver.find_element(By.XPATH, "//button[text()='Enviar']")
```

## CSS selector vs XPath

## CSS selector

Suele ser claro para buscar por clases, ids, atributos y estructura visual.

Ejemplos:

```python
".product"
"#main"
"a[href]"
"div.product span.name"
```

## XPath

Puede ser útil cuando se necesita buscar por texto o por relaciones más específicas.

Ejemplos:

```python
"//button[text()='Enviar']"
"//div[@class='product']//span"
```

## Regla práctica

Para scraping o automatización simple, suele ser conveniente empezar con CSS selectors.

XPath se reserva para casos donde CSS no expresa bien la búsqueda.

## Leer texto de un elemento

```python
element = driver.find_element(By.TAG_NAME, "h1")

print(element.text)
```

## Leer atributos

```python
link = driver.find_element(By.TAG_NAME, "a")

href = link.get_attribute("href")

print(href)
```

## Hacer clic

```python
button = driver.find_element(By.CSS_SELECTOR, "button")
button.click()
```

## Escribir texto

```python
field = driver.find_element(By.NAME, "q")
field.send_keys("Python")
```

## Enviar teclas especiales

```python
from selenium.webdriver.common.keys import Keys

field = driver.find_element(By.NAME, "q")
field.send_keys("Python")
field.send_keys(Keys.ENTER)
```

## Limpiar un campo

```python
field = driver.find_element(By.NAME, "q")
field.clear()
field.send_keys("Nuevo texto")
```

## Formularios

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome()

try:
    driver.get("https://example.com/search")

    field = driver.find_element(By.NAME, "q")
    field.send_keys("Python")
    field.send_keys(Keys.ENTER)

    print(driver.title)
finally:
    driver.quit()
```

## Esperas

Las esperas son fundamentales en Selenium porque muchas páginas cargan contenido de forma dinámica.

Hay dos tipos frecuentes:

```text
esperas implícitas
esperas explícitas
```

## Espera implícita

Una espera implícita indica al driver que espere cierto tiempo al intentar encontrar elementos.

```python
driver.implicitly_wait(10)
```

Puede ser útil, pero no siempre ofrece el control más claro.

## Espera explícita

La espera explícita permite esperar una condición concreta.

Importaciones:

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
```

Ejemplo:

```python
wait = WebDriverWait(driver, 10)

element = wait.until(
    EC.presence_of_element_located((By.CSS_SELECTOR, ".content"))
)
```

## Condiciones esperadas frecuentes

Algunas condiciones comunes son:

```python
EC.presence_of_element_located
EC.visibility_of_element_located
EC.element_to_be_clickable
EC.text_to_be_present_in_element
EC.url_contains
EC.title_contains
```

## Esperar un elemento visible

```python
wait = WebDriverWait(driver, 10)

element = wait.until(
    EC.visibility_of_element_located((By.CSS_SELECTOR, ".result"))
)

print(element.text)
```

## Esperar un botón clickeable

```python
wait = WebDriverWait(driver, 10)

button = wait.until(
    EC.element_to_be_clickable((By.CSS_SELECTOR, "button.submit"))
)

button.click()
```

## Evitar `time.sleep()` como solución principal

Menos recomendable:

```python
import time

time.sleep(5)
element = driver.find_element(By.CSS_SELECTOR, ".result")
```

Más claro:

```python
wait = WebDriverWait(driver, 10)

element = wait.until(
    EC.visibility_of_element_located((By.CSS_SELECTOR, ".result"))
)
```

La espera explícita espera solo hasta que se cumpla la condición o hasta que expire el tiempo máximo.

## Modo headless

El modo headless permite ejecutar el navegador sin ventana visible.

Ejemplo con Chrome:

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

options = Options()
options.add_argument("--headless=new")

driver = webdriver.Chrome(options=options)

try:
    driver.get("https://example.com")
    print(driver.title)
finally:
    driver.quit()
```

Este modo es útil en automatizaciones, servidores y pipelines.

## Opciones del navegador

Se pueden configurar opciones antes de crear el driver.

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

options = Options()
options.add_argument("--window-size=1366,768")

driver = webdriver.Chrome(options=options)
```

## Capturas de pantalla

```python
driver.save_screenshot("screenshot.png")
```

Ejemplo:

```python
from selenium import webdriver

driver = webdriver.Chrome()

try:
    driver.get("https://example.com")
    driver.save_screenshot("example.png")
finally:
    driver.quit()
```

## Ejecutar JavaScript

```python
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
```

También puede devolver valores:

```python
height = driver.execute_script("return document.body.scrollHeight;")
print(height)
```

## Scroll

```python
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
```

Scroll hacia un elemento:

```python
element = driver.find_element(By.CSS_SELECTOR, ".target")

driver.execute_script(
    "arguments[0].scrollIntoView();",
    element
)
```

## Ventanas y pestañas

Selenium permite trabajar con múltiples ventanas o pestañas.

```python
print(driver.window_handles)
print(driver.current_window_handle)
```

Cambiar a otra ventana:

```python
driver.switch_to.window(driver.window_handles[-1])
```

## Alertas

```python
alert = driver.switch_to.alert

print(alert.text)

alert.accept()
```

También puede cancelarse:

```python
alert.dismiss()
```

## Selectores en formularios

Para elementos `<select>`, Selenium ofrece `Select`.

```python
from selenium.webdriver.support.ui import Select

select_element = driver.find_element(By.TAG_NAME, "select")
select = Select(select_element)

select.select_by_visible_text("Opción 1")
```

## ActionChains

`ActionChains` permite acciones más elaboradas, como mover el mouse, hacer doble clic o arrastrar.

```python
from selenium.webdriver.common.action_chains import ActionChains

element = driver.find_element(By.CSS_SELECTOR, ".target")

actions = ActionChains(driver)
actions.move_to_element(element).perform()
```

## Cookies

Leer cookies:

```python
cookies = driver.get_cookies()
print(cookies)
```

Agregar cookie:

```python
driver.add_cookie({
    "name": "session",
    "value": "abc123"
})
```

## Uso con Beautiful Soup

Selenium puede renderizar la página y Beautiful Soup puede analizar el HTML resultante.

```python
from bs4 import BeautifulSoup
from selenium import webdriver

driver = webdriver.Chrome()

try:
    driver.get("https://example.com")

    html = driver.page_source
    soup = BeautifulSoup(html, "html.parser")

    print(soup.title.get_text(strip=True))
finally:
    driver.quit()
```

Este patrón es útil cuando se necesita que el navegador ejecute JavaScript antes de extraer el HTML.

## Uso con pandas

Después de extraer registros, pueden organizarse en un `DataFrame`.

```python
import pandas as pd
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()

try:
    driver.get("https://example.com")

    records = []

    items = driver.find_elements(By.CSS_SELECTOR, ".product")

    for item in items:
        records.append({
            "text": item.text
        })

    df = pd.DataFrame(records)

    print(df)
finally:
    driver.quit()
```

## Patrón recomendado de extracción

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC


def create_driver():
    return webdriver.Chrome()


def get_products(driver, url):
    driver.get(url)

    wait = WebDriverWait(driver, 10)

    wait.until(
        EC.presence_of_all_elements_located((By.CSS_SELECTOR, ".product"))
    )

    products = []

    for element in driver.find_elements(By.CSS_SELECTOR, ".product"):
        products.append(element.text)

    return products


driver = create_driver()

try:
    products = get_products(driver, "https://example.com")
    print(products)
finally:
    driver.quit()
```

## Excepciones frecuentes

Algunas excepciones comunes son:

```python
NoSuchElementException
TimeoutException
ElementClickInterceptedException
ElementNotInteractableException
StaleElementReferenceException
WebDriverException
```

Importación frecuente:

```python
from selenium.common.exceptions import (
    NoSuchElementException,
    TimeoutException,
    WebDriverException,
)
```

## Manejo de errores

```python
from selenium import webdriver
from selenium.common.exceptions import TimeoutException, WebDriverException
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()

try:
    driver.get("https://example.com")

    wait = WebDriverWait(driver, 10)

    element = wait.until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, ".content"))
    )

    print(element.text)

except TimeoutException:
    print("No apareció el elemento dentro del tiempo esperado")

except WebDriverException as error:
    print("Error de WebDriver:", error)

finally:
    driver.quit()
```

## Errores comunes

## Usar Selenium cuando basta con `requests`

Problemático:

```python
driver.get("https://api.example.com/data")
```

Si se trata de una API JSON, normalmente corresponde usar `requests` o `httpx`.

## No cerrar el navegador

Problemático:

```python
driver = webdriver.Chrome()
driver.get("https://example.com")
```

Mejor:

```python
driver = webdriver.Chrome()

try:
    driver.get("https://example.com")
finally:
    driver.quit()
```

## Usar `time.sleep()` en exceso

Problemático:

```python
time.sleep(10)
```

Mejor usar esperas explícitas con `WebDriverWait`.

## Buscar elementos antes de que existan

Problemático:

```python
element = driver.find_element(By.CSS_SELECTOR, ".result")
```

en páginas dinámicas.

Mejor:

```python
element = wait.until(
    EC.visibility_of_element_located((By.CSS_SELECTOR, ".result"))
)
```

## Usar selectores demasiado frágiles

Problemático:

```python
driver.find_element(By.XPATH, "/html/body/div[3]/div[2]/button")
```

Puede romperse si cambia la estructura.

Mejor buscar atributos más estables:

```python
driver.find_element(By.CSS_SELECTOR, "button[data-testid='submit']")
```

## Ignorar cambios del DOM

Un elemento puede quedar obsoleto si la página se actualiza.

Esto puede generar `StaleElementReferenceException`.

En esos casos, suele ser necesario volver a localizar el elemento.

## Escribir credenciales directamente en el código

Problemático:

```python
field.send_keys("clave_real")
```

Mejor leer credenciales desde variables de entorno cuando sea necesario.

## Buenas prácticas

## Usar Selenium solo cuando se necesita navegador real

Para HTML estático o APIs, convienen herramientas más livianas.

## Cerrar siempre el navegador

```python
finally:
    driver.quit()
```

## Usar esperas explícitas

```python
WebDriverWait(driver, 10)
```

## Preferir selectores estables

Ejemplos:

```python
By.ID
By.CSS_SELECTOR
By.NAME
```

cuando usen atributos estables.

## Separar navegación, espera y extracción

```python
driver.get(url)
wait.until(...)
extract_data(...)
```

## Mantener la extracción en funciones

```python
def extract_products(driver):
    ...
```

## Usar modo headless para automatizaciones

```python
options.add_argument("--headless=new")
```

## No automatizar acciones innecesariamente agresivas

Debe evitarse sobrecargar sitios, evadir controles o incumplir condiciones de uso. Cuando exista una API oficial, suele ser preferible usarla.

## Ejemplo integrado

```python
from pathlib import Path

from selenium import webdriver
from selenium.common.exceptions import TimeoutException, WebDriverException
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC


def create_driver():
    options = webdriver.ChromeOptions()
    options.add_argument("--headless=new")
    options.add_argument("--window-size=1366,768")

    return webdriver.Chrome(options=options)


def fetch_page_title(driver, url):
    driver.get(url)

    wait = WebDriverWait(driver, 10)

    wait.until(
        EC.presence_of_element_located((By.TAG_NAME, "body"))
    )

    return driver.title


def save_result(title, output_path):
    output_path.write_text(
        f"Título: {title}",
        encoding="utf-8"
    )


driver = create_driver()

try:
    title = fetch_page_title(driver, "https://example.com")

    save_result(
        title,
        Path("selenium-result.txt")
    )

    print("Resultado guardado correctamente")

except TimeoutException:
    print("La página no cargó dentro del tiempo esperado")

except WebDriverException as error:
    print("Error de WebDriver:", error)

finally:
    driver.quit()
```

## Relación con otras librerías

`selenium` se relaciona especialmente con:

- `beautifulsoup4`, para analizar HTML renderizado
- `lxml`, como alternativa de análisis estructurado
- `pandas`, para tabular datos extraídos
- `python-dotenv`, para cargar credenciales o rutas
- `pytest`, para pruebas automatizadas
- `playwright`, como alternativa moderna para automatización de navegador
- `requests` y `httpx`, como alternativas más livianas cuando no se necesita navegador real

## Orden didáctico interno

```text
1. Propósito de selenium
2. Instalación e importación
3. WebDriver
4. Abrir y cerrar navegador
5. Localización de elementos
6. Clicks, texto y formularios
7. Esperas explícitas
8. Modo headless
9. JavaScript, scroll y capturas
10. Uso con Beautiful Soup o pandas
11. Errores comunes
12. Buenas prácticas
```