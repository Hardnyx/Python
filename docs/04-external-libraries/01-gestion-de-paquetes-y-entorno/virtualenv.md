# `virtualenv`

## Propósito

`virtualenv` permite crear entornos virtuales aislados para proyectos de Python. Un entorno virtual contiene su propio intérprete, sus propios paquetes instalados y su propio espacio de dependencias, separado del Python global del sistema y de otros proyectos.

Su utilidad principal es evitar conflictos entre dependencias.

Por ejemplo, un proyecto puede usar una versión de `pandas` y otro proyecto puede usar una versión distinta sin interferir entre sí.

## Naturaleza de la herramienta

`virtualenv` no es una librería que normalmente se importe en el código Python.

Su uso principal ocurre desde la terminal:

```bash
python -m virtualenv .venv
```

También existe `venv`, que es el módulo incluido en la biblioteca estándar:

```bash
python -m venv .venv
```

En proyectos modernos, muchas veces basta con `venv`. Sin embargo, `virtualenv` sigue siendo útil como herramienta externa, especialmente cuando se requiere compatibilidad más amplia, mayor velocidad de creación o integración con ciertos flujos de trabajo.

## Diferencia entre `venv` y `virtualenv`

## `venv`

`venv` viene incluido con Python.

Se usa así:

```bash
python -m venv .venv
```

No requiere instalación adicional.

## `virtualenv`

`virtualenv` se instala como paquete externo.

Instalación:

```bash
python -m pip install virtualenv
```

Uso:

```bash
python -m virtualenv .venv
```

## Regla práctica

Para la mayoría de proyectos generales, `venv` suele ser suficiente.

`virtualenv` puede preferirse cuando se quiere usar explícitamente la herramienta externa o cuando un flujo de trabajo ya está basado en ella.

## Idea central

Un entorno virtual permite que cada proyecto tenga sus propias dependencias.

Sin entorno virtual, los paquetes pueden instalarse en el Python global del sistema. Eso puede generar conflictos cuando varios proyectos necesitan versiones distintas de las mismas librerías.

Con entorno virtual, cada proyecto mantiene su propio espacio aislado.

## Instalación de `virtualenv`

Como `virtualenv` es una herramienta externa, primero debe instalarse con `pip`.

```bash
python -m pip install virtualenv
```

Verificación:

```bash
python -m virtualenv --version
```

Si el comando muestra una versión, la instalación fue reconocida por el entorno activo.

## Crear un entorno virtual

La convención más común es crear el entorno en una carpeta llamada `.venv`.

```bash
python -m virtualenv .venv
```

Esto crea una carpeta local con los archivos necesarios para aislar el entorno del proyecto.

## Nombre recomendado del entorno

Una convención frecuente es:

```text
.venv
```

Ventajas:

- es corto
- es reconocible
- queda dentro del proyecto
- suele ser detectado por editores como VS Code
- puede excluirse fácilmente de Git

También puede usarse otro nombre:

```bash
python -m virtualenv env
python -m virtualenv venv
```

Sin embargo, `.venv` suele ser una opción clara y moderna.

## Activar el entorno virtual

Después de crearlo, debe activarse para que la terminal use ese entorno.

## Linux, macOS o Git Bash

```bash
source .venv/bin/activate
```

## Windows PowerShell

```powershell
.\.venv\Scripts\Activate.ps1
```

## Windows CMD

```cmd
.venv\Scripts\activate.bat
```

Cuando el entorno está activado, normalmente el prompt muestra algo similar a:

```text
(.venv)
```

## Verificar el entorno activo

## Verificar Python

```bash
python --version
```

## Verificar `pip`

```bash
python -m pip --version
```

## Verificar ruta de Python

En Linux, macOS o Git Bash:

```bash
which python
```

En Windows PowerShell:

```powershell
where python
```

La ruta debería apuntar a la carpeta `.venv`.

## Instalar paquetes dentro del entorno

Una vez activado el entorno, los paquetes instalados con `pip` quedan dentro de ese entorno.

```bash
python -m pip install requests
```

Ejemplo con varias librerías:

```bash
python -m pip install requests pandas openpyxl
```

## Listar paquetes instalados

```bash
python -m pip list
```

Esto muestra los paquetes instalados en el entorno activo.

## Crear `requirements.txt`

Para registrar las dependencias instaladas:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de contenido:

```text
requests==2.32.3
pandas==2.2.2
openpyxl==3.1.5
```

## Instalar dependencias desde `requirements.txt`

En una máquina nueva o después de recrear el entorno:

```bash
python -m pip install -r requirements.txt
```

Este comando instala todas las dependencias listadas.

## Desactivar el entorno

Para salir del entorno virtual:

```bash
deactivate
```

Después de esto, la terminal vuelve a usar el entorno global o el entorno anterior.

## Recrear un entorno virtual

Los entornos virtuales deben considerarse recreables. No deberían contener código del proyecto ni archivos importantes.

Flujo típico:

```bash
rm -rf .venv
python -m virtualenv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

En Windows PowerShell, la eliminación puede hacerse con:

```powershell
Remove-Item -Recurse -Force .venv
```

Luego:

```powershell
python -m virtualenv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

## No subir `.venv` a Git

La carpeta del entorno virtual no debería subirse al repositorio.

Debe agregarse al archivo `.gitignore`:

```gitignore
.venv/
venv/
env/
```

Lo que sí debe subirse es el archivo de dependencias, como:

```text
requirements.txt
```

Así, cualquier persona puede recrear el entorno sin versionar todos los paquetes instalados.

## Relación con `pip`

`virtualenv` crea el entorno.

`pip` instala paquetes dentro de ese entorno.

Flujo típico:

```bash
python -m virtualenv .venv
source .venv/bin/activate
python -m pip install requests pandas
python -m pip freeze > requirements.txt
```

## Relación con editores

Editores como VS Code pueden detectar entornos virtuales ubicados en `.venv`.

En ese caso, conviene seleccionar el intérprete correspondiente al proyecto.

La ruta suele tener una forma similar a:

```text
.venv/bin/python
```

en Linux, macOS o Git Bash.

En Windows:

```text
.venv\Scripts\python.exe
```

## Relación con scripts

Cuando el entorno está activado, los scripts usan los paquetes instalados en ese entorno.

```bash
python script.py
```

Si el entorno no está activado, el script puede fallar con:

```text
ModuleNotFoundError
```

aunque el paquete haya sido instalado en otro entorno.

## Flujo típico recomendado

```bash
python -m pip install virtualenv
python -m virtualenv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install requests pandas openpyxl
python -m pip freeze > requirements.txt
```

En Windows PowerShell:

```powershell
python -m pip install virtualenv
python -m virtualenv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install requests pandas openpyxl
python -m pip freeze > requirements.txt
```

## Estructura típica del proyecto

```text
proyecto/
├─ .venv/
├─ src/
├─ tests/
├─ requirements.txt
├─ .gitignore
└─ README.md
```

La carpeta `.venv/` existe localmente, pero no se versiona.

## Errores comunes

## Instalar paquetes fuera del entorno

Problemático:

```bash
python -m pip install pandas
```

sin haber activado el entorno correcto.

Puede instalarse en otro Python distinto al del proyecto.

Verificación recomendada:

```bash
python -m pip --version
```

## Activar el entorno equivocado

Puede ocurrir cuando existen varios proyectos con varias carpetas `.venv`.

Verificación recomendada:

```bash
which python
```

o en Windows PowerShell:

```powershell
where python
```

## Subir `.venv` al repositorio

Problemático:

```text
.venv/
```

dentro del control de versiones.

Esto aumenta el tamaño del repositorio y vuelve el proyecto menos portable.

## Creer que el entorno virtual reemplaza `requirements.txt`

El entorno virtual contiene los paquetes instalados localmente.

`requirements.txt` registra qué debe instalarse para reconstruirlo.

Ambos cumplen roles distintos.

## Copiar una carpeta `.venv` entre máquinas

No es una práctica recomendable. Los entornos virtuales dependen de rutas, sistema operativo e intérprete base.

La forma adecuada es recrear el entorno:

```bash
python -m virtualenv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

## Usar `pip` sin saber a qué Python pertenece

Problemático:

```bash
pip install requests
```

Forma más clara:

```bash
python -m pip install requests
```

## Buenas prácticas

## Crear un entorno virtual por proyecto

```bash
python -m virtualenv .venv
```

Cada proyecto debería aislar sus dependencias.

## Usar `.venv` como nombre estándar

```text
.venv
```

Es una convención clara y fácil de reconocer.

## Activar el entorno antes de instalar dependencias

```bash
source .venv/bin/activate
```

o en Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

## Usar `python -m pip`

```bash
python -m pip install requests
```

Reduce ambigüedad entre intérpretes y entornos.

## Registrar dependencias

```bash
python -m pip freeze > requirements.txt
```

## Excluir el entorno de Git

Archivo `.gitignore`:

```gitignore
.venv/
```

## Recrear el entorno cuando sea necesario

Un entorno virtual debe poder borrarse y reconstruirse a partir de los archivos del proyecto.

## Ejemplo integrado

Supóngase un proyecto de automatización que usa `requests`, `pandas` y `openpyxl`.

Creación inicial:

```bash
python -m pip install virtualenv
python -m virtualenv .venv
source .venv/bin/activate
python -m pip install requests pandas openpyxl
python -m pip freeze > requirements.txt
```

Estructura esperada:

```text
proyecto/
├─ .venv/
├─ main.py
├─ requirements.txt
└─ .gitignore
```

Contenido de `.gitignore`:

```gitignore
.venv/
```

Preparación del proyecto en otra máquina:

```bash
python -m virtualenv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python main.py
```

## Relación con otras herramientas

`virtualenv` se relaciona especialmente con:

- `pip`, para instalar paquetes dentro del entorno
- `requirements.txt`, para reproducir dependencias
- `python-dotenv`, para cargar variables de entorno del proyecto
- `poetry`, como alternativa más amplia de gestión de proyectos y dependencias
- `venv`, como alternativa incluida en la biblioteca estándar

## Orden didáctico interno

```text
1. Propósito de virtualenv
2. Diferencia entre venv y virtualenv
3. Instalación de virtualenv
4. Creación de entornos
5. Activación y desactivación
6. Instalación de paquetes dentro del entorno
7. requirements.txt
8. Exclusión de .venv en Git
9. Errores comunes
10. Buenas prácticas
```

Las bases de esta separación entre `venv` y `virtualenv` están en la documentación oficial de `venv` y en la documentación de `virtualenv`, que indica que `venv` integra una parte de la funcionalidad de `virtualenv` dentro de la biblioteca estándar. [Documentación](https://docs.python.org/3/library/venv.html)
