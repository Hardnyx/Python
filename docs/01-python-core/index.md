# Núcleo de Python

## Propósito

El núcleo de Python reúne los elementos que pertenecen al lenguaje mismo: su modelo de ejecución, su sistema de objetos, la definición y uso de funciones, el manejo de errores, la programación orientada a objetos y otras construcciones fundamentales que no dependen de librerías externas ni de módulos estándar importables.

Esta sección establece la base conceptual sobre la que se apoyan los elementos integrados, la biblioteca estándar y las librerías externas.

## Alcance

Esta parte no se centra en explicar desde cero aspectos elementales como variables, operadores aritméticos, condicionales simples o bucles básicos. El enfoque está puesto en los componentes estructurales del lenguaje que permiten escribir código reutilizable, mantenible y escalable.

Los temas principales son:

1. Modelo de ejecución
2. Objetos, tipos y mutabilidad
3. Funciones
4. Comprensiones, iteradores y generadores
5. Excepciones
6. Programación orientada a objetos
7. Métodos especiales
8. Decoradores y context managers

## Relación con otras partes de la documentación

El núcleo de Python no debe confundirse con los elementos integrados ni con la biblioteca estándar.

**Núcleo de Python**
Corresponde a la sintaxis, semántica y estructuras fundamentales del lenguaje.

**Elementos integrados**
Corresponden a tipos, funciones y herramientas ya disponibles sin `import`, como `str`, `list`, `dict`, `print()`, `len()` u `open()`.

**Biblioteca estándar**
Corresponde a módulos que vienen incluidos con Python, pero que requieren importación explícita, como `math`, `random`, `datetime`, `json` u `os`.

## Estructura interna de la sección

La sección se organiza en los siguientes archivos:

```text
01-python-core/
├─ index.md
├─ 01-modelo-de-ejecucion.md
├─ 02-objetos-tipos-mutabilidad.md
├─ 03-funciones.md
├─ 04-comprensiones-iteradores-generadores.md
├─ 05-excepciones.md
├─ 06-poo.md
├─ 07-metodos-especiales.md
└─ 08-decoradores-context-managers.md
````

## Orden de estudio recomendado

El recorrido sugerido dentro del núcleo del lenguaje es el siguiente:

1. Modelo de ejecución
2. Objetos, tipos y mutabilidad
3. Funciones
4. Comprensiones, iteradores y generadores
5. Excepciones
6. Programación orientada a objetos
7. Métodos especiales
8. Decoradores y context managers

Este orden sigue una progresión natural: primero cómo se ejecuta el código, luego qué tipo de entidades maneja el lenguaje, después cómo se organiza la lógica, cómo se recorren y producen datos, cómo se manejan errores y finalmente cómo se construyen abstracciones más potentes.

## Criterio de documentación

Cada archivo de esta sección debería mantener una estructura homogénea. Cuando corresponda, conviene incluir:

* propósito
* definiciones clave
* reglas del lenguaje asociadas al tema
* formas de uso
* ejemplos representativos
* errores comunes
* buenas prácticas
* relaciones con otros conceptos del lenguaje

## Contenido de cada bloque

### 1. Modelo de ejecución

Debe explicar cómo Python interpreta y ejecuta código.

Temas principales:

* script e intérprete interactivo
* archivos `.py`
* orden de ejecución
* módulos propios
* importaciones entre archivos
* `if __name__ == "__main__"`

### 2. Objetos, tipos y mutabilidad

Debe explicar la base del modelo de objetos de Python.

Temas principales:

* todo es objeto
* identidad, tipo y valor
* mutabilidad e inmutabilidad
* aliasing
* diferencias entre copiar y referenciar
* truthiness
* conversión de tipos

### 3. Funciones

Debe explicar cómo encapsular lógica y construir interfaces reutilizables.

Temas principales:

* definición con `def`
* parámetros y argumentos
* retorno de valores
* parámetros por defecto
* argumentos posicionales y nombrados
* `*args`
* `**kwargs`
* alcance de variables
* closures
* `lambda`

### 4. Comprensiones, iteradores y generadores

Debe explicar mecanismos compactos y eficientes para producir y recorrer datos.

Temas principales:

* list comprehensions
* dict comprehensions
* set comprehensions
* generator expressions
* protocolo de iteración
* `iter()`
* `next()`
* `yield`
* funciones generadoras

### 5. Excepciones

Debe explicar el manejo estructurado de errores.

Temas principales:

* `try`
* `except`
* `else`
* `finally`
* `raise`
* excepciones comunes
* creación de excepciones propias

### 6. Programación orientada a objetos

Debe explicar cómo modelar entidades y comportamientos mediante clases.

Temas principales:

* `class`
* atributos de instancia
* métodos
* `self`
* `__init__`
* atributos de clase
* herencia
* sobrescritura
* `super()`

### 7. Métodos especiales

Debe explicar la integración de objetos personalizados con el comportamiento del lenguaje.

Temas principales:

* `__init__`
* `__str__`
* `__repr__`
* `__len__`
* `__iter__`
* `__next__`
* `__getitem__`
* `__setitem__`
* `__contains__`
* `__call__`

### 8. Decoradores y context managers

Debe explicar mecanismos avanzados de abstracción y control.

Temas principales:

* funciones de orden superior
* decoradores
* cierre sobre funciones
* `with`
* `__enter__`
* `__exit__`

## Resultado esperado de esta sección

Al finalizar esta parte, debería quedar claro:

* cómo se ejecuta un programa en Python
* cómo modela Python los objetos y los tipos
* cómo se organizan funciones y clases
* cómo se controlan flujos de datos y errores
* cómo se construyen abstracciones reutilizables dentro del propio lenguaje