# Documentación de Python

## Propósito

Esta documentación organiza Python en cuatro grandes bloques:

1. Núcleo del lenguaje.
2. Elementos integrados.
3. Biblioteca estándar.
4. Librerías externas.

La separación permite distinguir con claridad qué pertenece a la sintaxis y al modelo del lenguaje, qué herramientas están disponibles sin importar nada, qué módulos vienen incluidos con Python y qué librerías deben instalarse de forma externa.

## Estructura general

**1. Núcleo de Python**

Contiene los fundamentos del lenguaje:

- modelo de ejecución
- objetos, tipos y mutabilidad
- funciones
- comprensiones, iteradores y generadores
- excepciones
- programación orientada a objetos
- métodos especiales
- decoradores y context managers

**2. Elementos integrados**

Contiene los tipos, funciones y métodos disponibles sin importar módulos:

- funciones integradas
- tipos integrados
- métodos de `str`, `list`, `dict`, `tuple`, `set`, `bytes` y otros tipos base

**3. Biblioteca estándar**

Contiene los módulos incluidos con Python que se importan explícitamente.

La documentación práctica se centra en los módulos que cubren la gran mayoría de usos frecuentes:

- matemática, aleatoriedad y estadística básica
- fechas y tiempo
- rutas, archivos y sistema
- formatos de datos y texto
- estructuras y utilidades funcionales
- persistencia local
- sistema, terminal y procesos
- calidad y herramientas de desarrollo
- interfaz gráfica estándar

**4. Librerías externas**

Contiene paquetes instalables con `pip`, organizados por dominio de uso:

- cálculo científico
- análisis de datos
- visualización
- automatización de oficina
- APIs y scraping
- bases de datos
- machine learning
- deep learning e inteligencia artificial
- desarrollo web y backend
- dashboards
- interfaces gráficas externas
- herramientas de consola
- librerías especializadas

## Criterio de organización

La documentación no sigue un orden alfabético. Sigue un orden conceptual y didáctico.

Primero se documenta el lenguaje.

Después se documentan los elementos integrados.

Luego se documenta la biblioteca estándar.

Finalmente se documentan las librerías externas.

Este orden evita mezclar sintaxis, funciones integradas, módulos estándar y paquetes de terceros dentro de una sola estructura indiferenciada.

## Recorrido recomendado

El recorrido sugerido es el siguiente:

1. Núcleo de Python
2. Elementos integrados
3. Biblioteca estándar
4. Librerías externas

Dentro de cada bloque, los temas se agrupan por afinidad funcional y por dependencia conceptual.

## Alcance de la documentación

La documentación no pretende cubrir absolutamente todo el ecosistema de Python en un solo nivel de detalle.

El foco principal está en:

- los conceptos fundamentales del lenguaje
- las herramientas integradas de uso frecuente
- los módulos estándar más útiles en la práctica
- las librerías externas más relevantes por dominio

Los componentes demasiado internos, históricos o de uso muy especializado pueden mencionarse, pero no constituyen el eje principal del recorrido.

## Bloques principales del proyecto

La estructura del proyecto está organizada de la siguiente forma:

```text
docs/
├─ 01-python-core/
├─ 02-builtins/
├─ 03-standard-library/
└─ 04-external-libraries/