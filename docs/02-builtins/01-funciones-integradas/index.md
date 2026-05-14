# Funciones integradas

## Propósito

Las funciones integradas son funciones disponibles desde el inicio de un programa en Python, sin necesidad de importar módulos. Permiten resolver tareas fundamentales de salida, inspección, conversión, agregación, ordenamiento, iteración, entrada y manejo básico de archivos.

## Alcance

Esta sección documenta las funciones integradas más frecuentes dentro de un recorrido práctico y general. No busca cubrir absolutamente todas las funciones built-in desde el inicio, sino organizar primero las que aparecen con mayor frecuencia en programación cotidiana, aprendizaje estructurado y desarrollo general.

## Relación con otras partes de la documentación

**Núcleo de Python**

Describe la sintaxis y las estructuras fundamentales del lenguaje, como funciones, clases, excepciones, iteración y context managers.

**Funciones integradas**

Describe funciones ya disponibles sin `import`, como `print()`, `len()`, `type()`, `range()` u `open()`.

**Tipos integrados**

Describe tipos como `str`, `list`, `dict` o `set`.

**Biblioteca estándar**

Describe módulos que sí requieren importación, como `math`, `random`, `datetime` o `json`.

## Idea general

Una función integrada puede:

- mostrar información
- consultar propiedades de un objeto
- convertir un valor
- recorrer datos
- resumir una colección
- ordenar elementos
- abrir archivos
- interactuar con el entorno inmediato de ejecución

Ejemplos:

```python
print("Hola")
print(len([1, 2, 3]))
print(type("Python"))
````

## Estructura del bloque

La documentación práctica inicial de funciones integradas se organiza en los siguientes archivos:

```text
01-funciones-integradas/
├─ index.md
├─ print.md
├─ len.md
├─ type.md
├─ isinstance.md
├─ range.md
├─ enumerate.md
├─ zip.md
├─ sorted.md
├─ sum.md
├─ min.md
├─ max.md
├─ abs.md
├─ round.md
├─ open.md
├─ input.md
├─ dir.md
└─ help.md
```

## Clasificación funcional

Para estudiarlas con mayor claridad, las funciones integradas se agrupan por propósito.

## 1. Salida e interacción básica

```python
print
input
```

Estas funciones permiten comunicar resultados o recibir entrada directa del usuario.

## 2. Inspección e introspección

```python
type
isinstance
dir
help
```

Estas funciones permiten explorar objetos, tipos, atributos y documentación.

## 3. Longitud, agregación y orden

```python
len
sum
min
max
sorted
abs
round
```

Estas funciones permiten medir, resumir, comparar y transformar valores numéricos o colecciones.

## 4. Iteración y recorrido estructurado

```python
range
enumerate
zip
```

Estas funciones permiten construir recorridos, índices y combinaciones de secuencias.

## 5. Entrada y salida con archivos

```python
open
```

`open()` merece un tratamiento especial porque conecta los built-ins con el trabajo real sobre archivos.

## Criterio de selección

Las funciones documentadas primero en este bloque cubren una parte muy amplia del trabajo cotidiano en Python, especialmente en:

* scripts
* automatización
* procesamiento de texto
* manejo de listas y diccionarios
* depuración inicial
* interacción con archivos
* transformación básica de datos

## Lista inicial de funciones documentadas

```python
print
len
type
isinstance
range
enumerate
zip
sorted
sum
min
max
abs
round
open
input
dir
help
```

## Funciones que pueden incorporarse después

Más adelante, esta sección puede ampliarse con otras funciones integradas relevantes, por ejemplo:

```python
all
any
map
filter
iter
next
id
callable
hasattr
getattr
setattr
delattr
vars
globals
locals
reversed
slice
pow
divmod
```

Estas funciones son importantes, pero no necesitan aparecer necesariamente en la primera capa de documentación.

## Forma de documentación de cada función

Cada archivo individual debería mantener, cuando corresponda, una estructura como esta:

* propósito
* forma general
* argumentos
* valor de retorno
* reglas importantes de comportamiento
* ejemplos
* errores comunes
* buenas prácticas
* relación con otras funciones similares

## Orden de estudio recomendado

El recorrido sugerido dentro de funciones integradas es el siguiente:

1. `print()`
2. `len()`
3. `type()`
4. `isinstance()`
5. `range()`
6. `enumerate()`
7. `zip()`
8. `sorted()`
9. `sum()`
10. `min()`
11. `max()`
12. `abs()`
13. `round()`
14. `input()`
15. `open()`
16. `dir()`
17. `help()`

Este orden parte de funciones simples y transversales, luego pasa a recorrido y agregación, y finalmente aborda entrada, archivos e introspección.

## Observaciones generales

## 1. No todas las funciones integradas son equivalentes en frecuencia de uso

Algunas aparecen constantemente:

```python
print
len
range
type
open
```

Otras son más situacionales:

```python
help
dir
```

## 2. Una función integrada puede trabajar con muchos tipos distintos

Ejemplo:

```python
print(len("Python"))
print(len([1, 2, 3]))
print(len({"a": 1, "b": 2}))
```

Salida:

```text
6
3
2
```

La misma función puede tener sentido sobre diferentes tipos, siempre que el objeto soporte la operación correspondiente.

## 3. Varias funciones integradas se apoyan en métodos especiales

Por ejemplo:

* `len(obj)` depende de `obj.__len__()`
* `print(obj)` usa representación textual del objeto
* `sorted(obj)` depende de iteración y comparación
* `bool(obj)` depende de verdad lógica

Esto conecta directamente esta sección con el bloque de métodos especiales y con los tipos integrados.

## 4. Algunas funciones integradas devuelven objetos iterables o resultados perezosos

Ejemplo:

```python
r = range(5)
print(r)
```

`range()` no produce una lista literal, sino un objeto especializado.

Esto será relevante al documentar `range()`, `zip()`, `enumerate()`, `iter()` y `next()`.

## Resultado esperado de esta subsección

Al finalizar este bloque debería quedar claro:

* qué funciones pueden usarse sin importación
* cómo se agrupan por propósito
* qué funciones son centrales en una práctica general de Python
* cómo se relacionan con tipos, iteración, archivos e introspección
