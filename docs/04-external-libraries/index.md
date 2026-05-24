# Librerías externas

## Propósito

Las librerías externas amplían las capacidades de Python más allá del núcleo del lenguaje, los elementos integrados y la biblioteca estándar. Permiten trabajar con APIs, scraping, análisis de datos, visualización, archivos especializados, bases de datos, aplicaciones web, herramientas de consola, testing, interfaces, machine learning y deep learning.

## Alcance

Se consideran librerías externas aquellas que normalmente requieren instalación adicional mediante gestores como `pip`.

Ejemplo general:

```bash
pip install nombre-libreria
```

Estas librerías no forman parte directa de la biblioteca estándar de Python. Por eso, su uso habitual requiere un entorno de proyecto configurado correctamente.

## Relación con otras partes de la documentación

**Núcleo de Python**

Describe la sintaxis, funciones, clases, excepciones, iteración, decoradores, context managers y demás estructuras propias del lenguaje.

**Elementos integrados**

Describe tipos, funciones y métodos disponibles sin importar módulos, como `str`, `list`, `dict`, `print()`, `len()` y `open()`.

**Biblioteca estándar**

Describe módulos incluidos con Python, como `math`, `random`, `datetime`, `os`, `pathlib`, `json`, `csv`, `sqlite3` y `tkinter`.

**Librerías externas**

Describe paquetes instalables que amplían Python para tareas prácticas específicas.

## Criterio de selección

No se busca cubrir todas las librerías existentes del ecosistema Python. Se priorizan aquellas que cumplen uno o más de estos criterios:

* uso frecuente en proyectos reales
* utilidad transversal en automatización, datos, web, reportes o productividad
* adopción amplia por la comunidad
* estabilidad razonable
* aplicación práctica en distintos tipos de proyectos

## Estructura interna

```text
04-external-libraries/
├─ index.md
├─ 01-gestion-de-paquetes-y-entorno/
├─ 02-web-apis-y-http/
├─ 03-web-scraping-y-automatizacion-web/
├─ 04-datos-y-analisis/
├─ 05-archivos-y-formatos/
├─ 06-visualizacion/
├─ 07-bases-de-datos/
├─ 08-cli-logs-y-productividad/
├─ 09-testing-calidad-y-desarrollo/
├─ 10-desarrollo-web-y-apis/
├─ 11-interfaces-dashboards-y-aplicaciones/
├─ 12-machine-learning/
└─ 13-deep-learning-y-nlp/
```

## Organización conceptual

## 1. Gestión de paquetes y entorno

Agrupa herramientas relacionadas con instalación, aislamiento de dependencias, configuración de variables de entorno y administración de proyectos.

Librerías y herramientas principales:

```text
pip
virtualenv
python-dotenv
poetry
```

Este bloque debe estudiarse primero porque permite instalar, aislar y administrar las demás librerías externas.

## 2. Web, APIs y HTTP

Agrupa librerías para consumir APIs, realizar solicitudes HTTP, trabajar con respuestas externas y comunicarse con servicios web.

Librerías principales:

```text
requests
httpx
aiohttp
websockets
```

`requests` suele ser una de las primeras librerías externas útiles porque permite traer datos desde internet usando una interfaz simple.

## 3. Web scraping y automatización web

Agrupa herramientas para extraer información desde páginas web o controlar navegadores.

Librerías principales:

```text
beautifulsoup4
lxml
selenium
playwright
scrapy
```

Este bloque depende conceptualmente de conocer HTTP, HTML básico, texto, archivos y estructuras como listas y diccionarios.

## 4. Datos y análisis

Agrupa librerías orientadas a cálculo numérico, análisis tabular, estadística, series de datos y procesamiento eficiente de información estructurada.

Librerías principales:

```text
numpy
pandas
scipy
polars
statsmodels
```

Este bloque es central para análisis de datos, automatización de reportes, finanzas, econometría y procesamiento de información.

## 5. Archivos y formatos

Agrupa librerías para trabajar con formatos especializados como Excel, Word, PowerPoint, PDF e imágenes.

Librerías principales:

```text
openpyxl
xlsxwriter
pyxlsb
python-docx
python-pptx
pypdf
pillow
```

Este bloque es especialmente útil en automatización de oficina, generación de reportes y procesamiento de documentos.

## 6. Visualización

Agrupa librerías para crear gráficos estáticos, interactivos o integrables en reportes y dashboards.

Librerías principales:

```text
matplotlib
seaborn
plotly
bokeh
```

La visualización se estudia después de datos porque normalmente parte de información ya procesada con estructuras como listas, diccionarios, arreglos o dataframes.

## 7. Bases de datos

Agrupa librerías para conectarse a motores de bases de datos relacionales y no relacionales.

Librerías principales:

```text
sqlalchemy
psycopg
pymysql
pymongo
```

Este bloque se apoya en conocimientos de datos tabulares, consultas, registros, diccionarios y estructuras persistentes.

## 8. CLI, logs y productividad

Agrupa herramientas para construir aplicaciones de consola, mejorar la salida en terminal, mostrar progreso y registrar eventos.

Librerías principales:

```text
typer
click
rich
tqdm
loguru
```

Este bloque mejora la calidad práctica de scripts, automatizaciones y herramientas internas.

## 9. Testing, calidad y desarrollo

Agrupa herramientas para pruebas, formato, análisis estático, tipado y control de calidad del código.

Librerías y herramientas principales:

```text
pytest
hypothesis
black
ruff
mypy
pre-commit
```

Este bloque permite pasar de scripts funcionales a proyectos más mantenibles, verificables y consistentes.

## 10. Desarrollo web y APIs

Agrupa frameworks y herramientas para construir servicios web, APIs y aplicaciones backend.

Librerías principales:

```text
fastapi
flask
django
pydantic
uvicorn
```

`FastAPI` y `Flask` son adecuados para iniciar. `Django` corresponde a un framework más amplio. `Pydantic` resulta central para validación de datos, especialmente en proyectos con APIs modernas.

## 11. Interfaces, dashboards y aplicaciones

Agrupa herramientas para construir interfaces gráficas, dashboards, prototipos interactivos y aplicaciones ligeras.

Librerías principales:

```text
customtkinter
pyqt
pyside
streamlit
gradio
```

`tkinter` no aparece en este bloque porque pertenece a la biblioteca estándar. `customtkinter`, en cambio, sí es una librería externa.

## 12. Machine learning

Agrupa librerías de aprendizaje automático clásico, entrenamiento de modelos, evaluación y persistencia de resultados.

Librerías principales:

```text
scikit-learn
xgboost
lightgbm
joblib
```

Este bloque requiere una base previa en datos, arreglos, dataframes, visualización y estadística básica.

## 13. Deep learning y NLP

Agrupa librerías orientadas a redes neuronales, modelos de lenguaje y procesamiento avanzado de texto.

Librerías principales:

```text
torch
tensorflow
transformers
spacy
```

Este bloque es más avanzado y debe estudiarse después de tener bases sólidas en datos, machine learning clásico y estructura general de proyectos.

## Orden de estudio recomendado

El orden recomendado sigue una lógica de dependencias prácticas:

```text
1. Gestión de paquetes y entorno
2. Web, APIs y HTTP
3. Web scraping y automatización web
4. Datos y análisis
5. Archivos y formatos
6. Visualización
7. Bases de datos
8. CLI, logs y productividad
9. Testing, calidad y desarrollo
10. Desarrollo web y APIs
11. Interfaces, dashboards y aplicaciones
12. Machine learning
13. Deep learning y NLP
```

Este orden permite avanzar desde el manejo básico del entorno hasta herramientas más especializadas. Primero se aprende a instalar y configurar paquetes. Luego se aprende a traer datos, procesarlos, almacenarlos, mostrarlos, probar el código y construir aplicaciones más completas.

## Primera capa recomendada

Una primera etapa práctica puede concentrarse en estas herramientas:

```text
pip
virtualenv
python-dotenv
requests
beautifulsoup4
numpy
pandas
openpyxl
matplotlib
sqlalchemy
pytest
rich
tqdm
fastapi
pydantic
streamlit
```

Con esta selección se cubre una parte muy amplia del uso real de Python:

* instalación de paquetes
* manejo de entorno
* consumo de APIs
* scraping básico
* análisis de datos
* automatización de Excel
* visualización
* bases de datos
* pruebas
* salidas de consola
* barras de progreso
* APIs backend
* dashboards simples

## Segunda capa recomendada

Después puede ampliarse con:

```text
httpx
aiohttp
lxml
selenium
playwright
scrapy
scipy
polars
statsmodels
xlsxwriter
pyxlsb
python-docx
python-pptx
pypdf
pillow
seaborn
plotly
bokeh
psycopg
pymysql
pymongo
typer
click
loguru
hypothesis
black
ruff
mypy
pre-commit
flask
django
uvicorn
customtkinter
pyqt
pyside
gradio
scikit-learn
xgboost
lightgbm
joblib
torch
tensorflow
transformers
spacy
```

Estas librerías son importantes, pero no todas resultan necesarias para una primera base general.

## Criterio de documentación por librería

Cada archivo individual debería mantener una estructura homogénea:

* propósito
* instalación
* importación habitual
* alias común, si existe
* objetos principales
* funciones o clases más usadas
* flujo típico de uso
* ejemplos mínimos
* errores comunes
* buenas prácticas
* relación con otras librerías

## Diferencia frente a la biblioteca estándar

Una librería externa debe distinguirse de un módulo estándar.

Ejemplos de biblioteca estándar:

```text
os
pathlib
json
csv
datetime
sqlite3
tkinter
```

Ejemplos de librerías externas:

```text
requests
pandas
numpy
openpyxl
fastapi
pytest
```

La diferencia práctica es que los módulos estándar vienen incluidos con Python, mientras que las librerías externas normalmente se instalan aparte.

## Resultado esperado

Al finalizar este bloque debería quedar claro:

* qué librerías externas son más importantes para uso general
* qué problema resuelve cada grupo
* en qué orden conviene estudiarlas
* cuáles corresponden a una primera capa práctica
* cuáles pueden dejarse para una etapa posterior
* cómo distinguir librerías externas de módulos de biblioteca estándar
* cómo pasar de scripts básicos a proyectos más completos