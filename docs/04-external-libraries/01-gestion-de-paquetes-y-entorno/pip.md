# `pip`

## Propósito

`pip` es la herramienta estándar más usada para instalar, actualizar, desinstalar y administrar paquetes externos de Python. Permite incorporar librerías que no forman parte de la biblioteca estándar, como `requests`, `pandas`, `numpy`, `openpyxl`, `fastapi` o `pytest`.

## Naturaleza de la herramienta

`pip` no es una librería que normalmente se importe dentro del código Python.

Su uso principal ocurre desde la terminal:

```bash
pip install requests
```

También puede ejecutarse mediante el intérprete de Python:

```bash
python -m pip install requests
```

Esta segunda forma suele ser más segura porque ayuda a garantizar que `pip` se ejecute asociado al mismo intérprete de Python que se está usando en el proyecto.

## Idea central

El flujo básico con `pip` consiste en:

1. instalar paquetes
2. verificar paquetes instalados
3. actualizar paquetes
4. desinstalar paquetes
5. congelar dependencias en un archivo
6. reinstalar dependencias desde un archivo

## Comando recomendado

La forma más recomendable en proyectos es:

```bash
python -m pip ...
```

Ejemplos:

```bash
python -m pip install requests
python -m pip install pandas
python -m pip list
```

Esto reduce problemas cuando existen varias instalaciones de Python en el mismo equipo.

## Instalación de paquetes

## Instalación básica

```bash
python -m pip install requests
```

Este comando instala el paquete `requests` en el entorno activo.

## Instalación de varios paquetes

```bash
python -m pip install requests pandas openpyxl
```

Permite instalar varias dependencias en una sola instrucción.

## Instalación de una versión específica

```bash
python -m pip install pandas==2.2.0
```

Esto instala exactamente la versión indicada.

## Instalación con versión mínima

```bash
python -m pip install "pandas>=2.0"
```

Esto permite instalar una versión igual o superior a la especificada.

## Instalación con rango de versiones

```bash
python -m pip install "pandas>=2.0,<3.0"
```

Este patrón es útil cuando se quiere permitir actualizaciones dentro de una familia de versiones compatible.

## Actualización de paquetes

Para actualizar un paquete instalado:

```bash
python -m pip install --upgrade requests
```

Forma abreviada:

```bash
python -m pip install -U requests
```

## Desinstalación de paquetes

```bash
python -m pip uninstall requests
```

`pip` pedirá confirmación antes de eliminar el paquete.

Para evitar confirmación interactiva:

```bash
python -m pip uninstall -y requests
```

## Listar paquetes instalados

```bash
python -m pip list
```

Muestra los paquetes instalados en el entorno activo.

Ejemplo de salida:

```text
Package    Version
---------- -------
requests   2.x.x
urllib3    2.x.x
```

## Mostrar información de un paquete

```bash
python -m pip show requests
```

Este comando muestra información como:

- nombre
- versión
- ubicación de instalación
- dependencias requeridas
- paquetes que lo requieren

## Ver paquetes desactualizados

```bash
python -m pip list --outdated
```

Muestra paquetes instalados que tienen versiones más recientes disponibles.

## Archivo `requirements.txt`

El archivo `requirements.txt` permite registrar las dependencias de un proyecto.

Ejemplo:

```text
requests
pandas
openpyxl
python-dotenv
```

También puede incluir versiones específicas:

```text
requests==2.32.3
pandas==2.2.2
openpyxl==3.1.5
python-dotenv==1.0.1
```

## Instalar desde `requirements.txt`

```bash
python -m pip install -r requirements.txt
```

Este comando instala todas las dependencias listadas en el archivo.

## Generar `requirements.txt`

```bash
python -m pip freeze > requirements.txt
```

Este comando guarda las dependencias instaladas en el entorno activo con sus versiones exactas.

## Diferencia entre `pip list` y `pip freeze`

## `pip list`

Muestra paquetes instalados en formato legible.

```bash
python -m pip list
```

## `pip freeze`

Muestra paquetes en formato adecuado para reproducir dependencias.

```bash
python -m pip freeze
```

Ejemplo:

```text
requests==2.32.3
urllib3==2.2.2
```

`pip freeze` suele usarse para crear `requirements.txt`.

## Entorno activo

`pip` instala paquetes en el entorno Python activo.

Esto puede ser:

- instalación global de Python
- entorno virtual
- entorno de Conda
- entorno interno de un editor
- entorno de un contenedor o Codespace

Por eso, antes de instalar paquetes conviene verificar qué Python y qué `pip` están activos.

## Verificar Python activo

```bash
python --version
```

## Verificar ubicación de Python

En Linux, macOS o Git Bash:

```bash
which python
```

En Windows PowerShell:

```powershell
where python
```

## Verificar `pip`

```bash
python -m pip --version
```

La salida muestra la versión de `pip` y la ruta donde está asociado.

## Uso con entornos virtuales

Lo más recomendable es instalar paquetes dentro de un entorno virtual del proyecto.

Flujo típico:

```bash
python -m venv .venv
```

Activación en Linux, macOS o Git Bash:

```bash
source .venv/bin/activate
```

Activación en Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

Luego se instalan dependencias:

```bash
python -m pip install requests pandas
```

## Actualizar `pip`

```bash
python -m pip install --upgrade pip
```

Esto actualiza la propia herramienta `pip` dentro del entorno activo.

## Instalación editable

En proyectos con estructura de paquete, puede usarse instalación editable:

```bash
python -m pip install -e .
```

Esto permite que los cambios en el código fuente del paquete se reflejen sin reinstalarlo manualmente.

## Instalación desde Git

También se pueden instalar paquetes desde repositorios Git.

```bash
python -m pip install git+https://github.com/usuario/repositorio.git
```

Este patrón debe usarse con cuidado, especialmente en proyectos productivos, porque depende del estado del repositorio remoto.

## Caché de `pip`

`pip` puede guardar paquetes descargados en caché para acelerar instalaciones futuras.

Para limpiar caché:

```bash
python -m pip cache purge
```

Para ver información de caché:

```bash
python -m pip cache info
```

## Problemas frecuentes

## Instalar en un Python distinto al que ejecuta el proyecto

Problemático:

```bash
pip install pandas
python script.py
```

Si `pip` y `python` apuntan a entornos distintos, el script puede no encontrar el paquete.

Forma más segura:

```bash
python -m pip install pandas
python script.py
```

## Error `ModuleNotFoundError`

Ejemplo:

```text
ModuleNotFoundError: No module named 'requests'
```

Causas frecuentes:

- el paquete no está instalado
- se instaló en otro entorno
- el entorno virtual no está activado
- el editor está usando otro intérprete

Verificación recomendada:

```bash
python -m pip show requests
python -c "import requests; print(requests.__version__)"
```

## Usar instalación global sin necesidad

Instalar paquetes globalmente puede generar conflictos entre proyectos.

Menos recomendable:

```bash
pip install pandas
```

Más recomendable dentro de proyectos:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install pandas
```

## Congelar demasiadas dependencias accidentales

`pip freeze` guarda todo lo instalado en el entorno activo. Si el entorno tiene paquetes ajenos al proyecto, `requirements.txt` quedará contaminado.

Por eso conviene generar dependencias desde un entorno limpio.

## Instalar paquetes sin registrar dependencia

Problemático:

```bash
python -m pip install openpyxl
```

y no actualizar `requirements.txt`.

Luego otra persona o entorno no sabrá que el proyecto necesita `openpyxl`.

Después de instalar dependencias importantes, conviene actualizar:

```bash
python -m pip freeze > requirements.txt
```

## Buenas prácticas

## Usar `python -m pip`

```bash
python -m pip install requests
```

Reduce ambigüedad entre varias instalaciones de Python.

## Usar entornos virtuales por proyecto

```bash
python -m venv .venv
```

Evita mezclar dependencias de proyectos distintos.

## Mantener un archivo de dependencias

```bash
requirements.txt
```

Permite reproducir el entorno de trabajo.

## Instalar desde `requirements.txt`

```bash
python -m pip install -r requirements.txt
```

Debe ser parte del flujo normal al clonar o preparar un proyecto.

## Evitar instalar paquetes innecesarios

Cada dependencia agrega mantenimiento, peso y posibles conflictos.

## Revisar el entorno activo antes de diagnosticar errores

```bash
python --version
python -m pip --version
```

## Flujo típico recomendado

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install requests pandas openpyxl
python -m pip freeze > requirements.txt
```

En Windows PowerShell, la activación sería:

```powershell
.\.venv\Scripts\Activate.ps1
```

## Ejemplo integrado

Supóngase un proyecto que necesita consumir una API y guardar datos en Excel.

Instalación inicial:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install requests pandas openpyxl
python -m pip freeze > requirements.txt
```

Uso posterior en otra máquina:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Verificación:

```bash
python -c "import requests, pandas, openpyxl; print('Dependencias listas')"
```

## Relación con otras herramientas

`pip` se relaciona especialmente con:

- `venv`, para crear entornos virtuales
- `requirements.txt`, para registrar dependencias
- `python-dotenv`, para configurar variables de entorno
- `poetry`, como alternativa más completa para gestión de dependencias y proyectos
- `pytest`, `requests`, `pandas`, `fastapi` y cualquier librería externa instalable

## Orden didáctico interno

```text
1. Propósito de pip
2. Uso con python -m pip
3. Instalación de paquetes
4. Actualización y desinstalación
5. Listado e inspección de paquetes
6. requirements.txt
7. Relación con entornos virtuales
8. Problemas frecuentes
9. Buenas prácticas
```