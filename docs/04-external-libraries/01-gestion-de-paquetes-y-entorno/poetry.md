# `poetry`

## Propósito

`poetry` es una herramienta para gestionar dependencias, entornos, empaquetado y configuración de proyectos Python. Permite declarar las librerías que necesita un proyecto, resolver versiones compatibles, crear un archivo de bloqueo reproducible y ejecutar comandos dentro del entorno administrado por el proyecto.

## Naturaleza de la herramienta

`poetry` no es una librería que normalmente se importe dentro del código Python.

Su uso principal ocurre desde la terminal:

```bash
poetry install
poetry add requests
poetry run python main.py
```

A diferencia de `pip`, que instala paquetes directamente en un entorno, `poetry` administra el proyecto completo mediante archivos como:

```text
pyproject.toml
poetry.lock
```

## Idea central

El flujo básico con `poetry` consiste en:

1. crear o inicializar un proyecto
2. declarar dependencias
3. resolver versiones compatibles
4. generar o actualizar `poetry.lock`
5. instalar dependencias reproducibles
6. ejecutar comandos dentro del entorno del proyecto
7. empaquetar o distribuir el proyecto si corresponde

## Instalación

Una forma habitual de instalar Poetry es mediante `pipx`, porque se trata de una aplicación de línea de comandos que conviene mantener aislada de los entornos de los proyectos.

```bash
pipx install poetry
```

Después de instalarlo, puede verificarse con:

```bash
poetry --version
```

También puede instalarse de otras formas según el sistema operativo o flujo de trabajo, pero la idea práctica es que `poetry` quede disponible como comando global.

## Ver comandos disponibles

```bash
poetry list
```

Muestra los comandos disponibles de Poetry.

Para ayuda sobre un comando específico:

```bash
poetry help install
```

## Crear un nuevo proyecto

```bash
poetry new mi-proyecto
```

Esto crea una estructura base de proyecto.

Ejemplo de estructura:

```text
mi-proyecto/
├─ pyproject.toml
├─ README.md
├─ mi_proyecto/
│  └─ __init__.py
└─ tests/
   └─ __init__.py
```

## Inicializar Poetry en un proyecto existente

Si el proyecto ya existe, puede inicializarse con:

```bash
poetry init
```

Este comando guía la creación de `pyproject.toml`.

## `pyproject.toml`

`pyproject.toml` es el archivo central de configuración del proyecto.

Contiene información como:

- nombre del proyecto
- versión
- descripción
- autores
- versión compatible de Python
- dependencias principales
- dependencias de desarrollo
- configuración de herramientas

Ejemplo simplificado:

```toml
[project]
name = "mi-proyecto"
version = "0.1.0"
description = "Proyecto de ejemplo"
requires-python = ">=3.11"
dependencies = [
    "requests>=2.32.0,<3.0.0"
]
```

## `poetry.lock`

`poetry.lock` registra las versiones exactas resueltas para las dependencias y subdependencias del proyecto.

Su función principal es mejorar la reproducibilidad.

Si dos personas instalan el proyecto con el mismo `poetry.lock`, deberían obtener las mismas versiones de dependencias.

## Instalar dependencias del proyecto

```bash
poetry install
```

Este comando lee `pyproject.toml`, resuelve dependencias si es necesario e instala los paquetes correspondientes.

Si existe `poetry.lock`, Poetry usa las versiones exactas registradas ahí.

Si no existe `poetry.lock`, Poetry lo crea después de resolver dependencias.

## Instalar solo dependencias

En algunos proyectos no se quiere instalar el paquete raíz, sino solo sus dependencias.

```bash
poetry install --no-root
```

Esto es útil en scripts, automatizaciones o proyectos que no están pensados como paquete instalable.

## Sincronizar entorno con `poetry.lock`

```bash
poetry sync
```

Este comando busca que el entorno quede alineado con el archivo `poetry.lock`.

A diferencia de una instalación simple, puede eliminar paquetes que no estén registrados en el lockfile.

## Agregar dependencias

```bash
poetry add requests
```

Esto agrega `requests` al proyecto, actualiza `pyproject.toml`, resuelve versiones y actualiza `poetry.lock`.

## Agregar varias dependencias

```bash
poetry add requests pandas openpyxl
```

## Agregar dependencia con restricción de versión

```bash
poetry add "pandas>=2.0,<3.0"
```

## Agregar dependencias de desarrollo

Las dependencias de desarrollo se usan para herramientas necesarias durante el desarrollo, pero no necesariamente para ejecutar la aplicación.

Ejemplos:

```bash
poetry add --group dev pytest
poetry add --group dev ruff
poetry add --group dev black
```

Estas herramientas pueden quedar separadas de las dependencias principales.

## Eliminar dependencias

```bash
poetry remove requests
```

Esto elimina la dependencia del proyecto y actualiza los archivos de configuración correspondientes.

## Actualizar dependencias

```bash
poetry update
```

Actualiza las dependencias dentro de las restricciones definidas en `pyproject.toml`.

Para actualizar una dependencia concreta:

```bash
poetry update requests
```

## Bloquear dependencias sin instalar

```bash
poetry lock
```

Este comando actualiza o genera `poetry.lock` sin instalar necesariamente los paquetes.

## Ejecutar comandos dentro del entorno

```bash
poetry run python main.py
```

También se puede ejecutar una herramienta instalada en el entorno:

```bash
poetry run pytest
poetry run ruff check .
```

Esto asegura que el comando se ejecute usando el entorno administrado por Poetry.

## Activación del entorno

Poetry puede crear y administrar un entorno virtual propio. En muchos casos no es necesario activarlo manualmente si se usa:

```bash
poetry run ...
```

Sin embargo, también puede obtenerse información sobre el entorno con:

```bash
poetry env info
```

Y listar entornos asociados:

```bash
poetry env list
```

## Configurar entorno dentro del proyecto

Por defecto, Poetry puede crear entornos en una ubicación de caché. Para proyectos donde se prefiere tener el entorno dentro de la carpeta del proyecto, puede configurarse:

```bash
poetry config virtualenvs.in-project true
```

Con esto, Poetry puede crear una carpeta `.venv` dentro del proyecto.

## Diferencia entre `pip` y `poetry`

## `pip`

`pip` instala paquetes en el entorno activo.

Ejemplo:

```bash
python -m pip install requests
```

## `poetry`

`poetry` administra el proyecto, sus dependencias, su lockfile y su entorno.

Ejemplo:

```bash
poetry add requests
poetry install
poetry run python main.py
```

## Regla práctica

`pip` es suficiente para proyectos simples o flujos basados en `requirements.txt`.

`poetry` es más conveniente cuando se quiere una gestión más estructurada del proyecto, con dependencias declaradas, lockfile, grupos de dependencias y empaquetado.

## Diferencia entre `requirements.txt` y `pyproject.toml`

## `requirements.txt`

Suele listar paquetes a instalar.

Ejemplo:

```text
requests==2.32.3
pandas==2.2.2
```

## `pyproject.toml`

Define configuración más amplia del proyecto, incluyendo dependencias, metadatos y herramientas.

Ejemplo:

```toml
[project]
dependencies = [
    "requests>=2.32.0,<3.0.0",
    "pandas>=2.2.0,<3.0.0"
]
```

## Diferencia entre `pyproject.toml` y `poetry.lock`

## `pyproject.toml`

Declara restricciones y configuración del proyecto.

Ejemplo:

```toml
dependencies = [
    "requests>=2.32.0,<3.0.0"
]
```

## `poetry.lock`

Registra versiones exactas resueltas.

Ejemplo conceptual:

```text
requests 2.32.3
urllib3 2.2.2
certifi ...
```

## Regla práctica

`pyproject.toml` dice qué se necesita.

`poetry.lock` dice exactamente qué se instaló.

## Commit de archivos

En aplicaciones, normalmente se versionan ambos archivos:

```text
pyproject.toml
poetry.lock
```

Esto mejora la reproducibilidad entre máquinas y entornos.

La carpeta del entorno virtual, si existe, no debe versionarse.

Archivo `.gitignore`:

```gitignore
.venv/
__pycache__/
```

## Modo paquete y modo no paquete

Poetry puede usarse para proyectos empaquetables o solo para gestión de dependencias.

En proyectos que no buscan crear un paquete instalable, puede usarse el modo no paquete:

```toml
[tool.poetry]
package-mode = false
```

Esto resulta útil para scripts, automatizaciones, análisis o proyectos internos donde no se necesita publicar una librería.

## Flujo típico para proyecto nuevo

```bash
poetry new mi-proyecto
cd mi-proyecto
poetry add requests pandas
poetry add --group dev pytest ruff
poetry install
poetry run python -m mi_proyecto
```

## Flujo típico para proyecto existente

```bash
cd proyecto-existente
poetry init
poetry add requests pandas openpyxl
poetry add --group dev pytest
poetry install
```

## Flujo típico al clonar un proyecto con Poetry

```bash
git clone repositorio
cd repositorio
poetry install
```

Si el proyecto usa el modo no paquete o no se quiere instalar el paquete raíz:

```bash
poetry install --no-root
```

## Flujo típico de ejecución

```bash
poetry run python main.py
```

Para pruebas:

```bash
poetry run pytest
```

Para linting:

```bash
poetry run ruff check .
```

## Flujo típico de actualización

Actualizar una dependencia concreta:

```bash
poetry update requests
```

Actualizar todas las dependencias permitidas por las restricciones:

```bash
poetry update
```

## Estructura típica de proyecto

```text
proyecto/
├─ pyproject.toml
├─ poetry.lock
├─ README.md
├─ src/
│  └─ paquete/
│     └─ __init__.py
├─ tests/
└─ .gitignore
```

Para proyectos más simples:

```text
proyecto/
├─ pyproject.toml
├─ poetry.lock
├─ main.py
├─ .env
├─ .gitignore
└─ README.md
```

## Uso con `python-dotenv`

Poetry administra dependencias, pero no reemplaza la configuración mediante variables de entorno.

Ejemplo:

```bash
poetry add python-dotenv
```

Luego:

```python
import os
from dotenv import load_dotenv

load_dotenv()

api_key = os.getenv("API_KEY")
```

## Uso con `pytest`

```bash
poetry add --group dev pytest
poetry run pytest
```

Esto mantiene `pytest` como dependencia de desarrollo.

## Uso con `FastAPI`

```bash
poetry add fastapi uvicorn
```

Ejecución:

```bash
poetry run uvicorn app.main:app --reload
```

## Uso con análisis de datos

```bash
poetry add pandas openpyxl matplotlib
```

Ejecución:

```bash
poetry run python main.py
```

## Errores comunes

## Mezclar `pip install` y `poetry add` sin criterio

Problemático:

```bash
python -m pip install requests
```

dentro de un proyecto administrado por Poetry, sin actualizar `pyproject.toml`.

Esto puede dejar el entorno funcionando localmente, pero el proyecto no registrará correctamente la dependencia.

Forma más coherente:

```bash
poetry add requests
```

## Borrar `poetry.lock` sin intención

Eliminar `poetry.lock` obliga a resolver dependencias nuevamente.

Esto puede cambiar versiones y producir resultados distintos.

## No versionar `poetry.lock` en una aplicación

En aplicaciones, no versionar `poetry.lock` reduce la reproducibilidad.

## Ejecutar Python fuera del entorno de Poetry

Problemático:

```bash
python main.py
```

si el entorno activo no es el de Poetry.

Forma más segura:

```bash
poetry run python main.py
```

## Confundir `poetry install` con `poetry update`

`poetry install` instala según el lockfile existente cuando está disponible.

`poetry update` busca versiones nuevas permitidas por las restricciones y actualiza el lockfile.

## Usar Poetry para scripts mínimos sin necesidad

Para scripts muy pequeños, `pip` y `venv` pueden ser suficientes.

Poetry tiene más sentido cuando el proyecto necesita estructura, reproducibilidad o mantenimiento.

## Buenas prácticas

## Usar `poetry add` para registrar dependencias

```bash
poetry add requests
```

## Separar dependencias de desarrollo

```bash
poetry add --group dev pytest ruff
```

## Versionar `pyproject.toml` y `poetry.lock`

```text
pyproject.toml
poetry.lock
```

## No versionar el entorno virtual

```gitignore
.venv/
```

## Usar `poetry run` para ejecutar comandos del proyecto

```bash
poetry run python main.py
```

## Preferir `poetry install` al preparar un proyecto clonado

```bash
poetry install
```

## Usar restricciones de versión razonables

```bash
poetry add "pandas>=2.0,<3.0"
```

## Ejemplo integrado

Creación de un proyecto simple de automatización:

```bash
mkdir reporte-automatizado
cd reporte-automatizado
poetry init
poetry add requests pandas openpyxl python-dotenv
poetry add --group dev pytest ruff
```

Estructura:

```text
reporte-automatizado/
├─ pyproject.toml
├─ poetry.lock
├─ main.py
├─ .env
├─ .gitignore
└─ README.md
```

`.gitignore`:

```gitignore
.venv/
.env
__pycache__/
```

Ejecución:

```bash
poetry run python main.py
```

Pruebas:

```bash
poetry run pytest
```

## Relación con otras herramientas

`poetry` se relaciona especialmente con:

- `pip`, porque ambos trabajan con paquetes, aunque con distinto alcance
- `virtualenv` o `venv`, porque Poetry administra entornos virtuales
- `python-dotenv`, para configuración mediante variables de entorno
- `pytest`, `ruff`, `black` y `mypy`, como herramientas de desarrollo
- `FastAPI`, `pandas`, `requests` y otras librerías externas como dependencias administradas
- `pyproject.toml`, como archivo central de configuración moderna en Python

## Orden didáctico interno

```text
1. Propósito de poetry
2. Diferencia frente a pip
3. pyproject.toml y poetry.lock
4. Creación e inicialización de proyectos
5. Instalación de dependencias
6. Agregar, eliminar y actualizar paquetes
7. Uso de poetry run
8. Grupos de dependencias
9. Errores comunes
10. Buenas prácticas
```
[Documentación](https://python-poetry.org/docs/)