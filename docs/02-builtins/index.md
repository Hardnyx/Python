# Elementos integrados

## Propósito

Los elementos integrados reúnen los tipos, funciones y herramientas que están disponibles en Python sin necesidad de importar módulos. Constituyen el primer nivel operativo del lenguaje después de su núcleo conceptual y permiten trabajar con datos, iteración, entrada y salida, conversión, evaluación, exploración e interacción básica con archivos.

## Alcance

Esta sección no corresponde a la biblioteca estándar importable. Aquí se documenta lo que ya está presente desde el inicio del programa, sin `import`.

El bloque se organiza en tres grandes grupos:

1. Funciones integradas
2. Tipos integrados
3. Métodos de tipos integrados

## Relación con otras partes de la documentación

**Núcleo de Python**

Describe la sintaxis, el modelo de ejecución, funciones, clases, excepciones, iteración, decoradores y context managers como parte del lenguaje.

**Elementos integrados**

Describe las herramientas disponibles sin importar módulos, como `print()`, `len()`, `open()`, `str`, `list`, `dict` y sus métodos.

**Biblioteca estándar**

Describe módulos incluidos con Python que requieren importación explícita, como `math`, `random`, `datetime`, `json` u `os`.

## Estructura interna de la sección

```text
02-builtins/
├─ index.md
├─ 01-funciones-integradas/
│  ├─ index.md
│  ├─ 01-salida-e-interaccion/
│  │  ├─ print.md
│  │  └─ input.md
│  ├─ 02-archivos-y-recursos/
│  │  └─ open.md
│  ├─ 03-inspeccion-y-tipos/
│  │  ├─ type.md
│  │  ├─ isinstance.md
│  │  ├─ dir.md
│  │  ├─ help.md
│  │  ├─ id.md
│  │  └─ callable.md
│  ├─ 04-iteracion-y-recorrido/
│  │  ├─ range.md
│  │  ├─ enumerate.md
│  │  ├─ zip.md
│  │  ├─ iter.md
│  │  ├─ next.md
│  │  ├─ reversed.md
│  │  └─ slice.md
│  ├─ 05-agregacion-y-orden/
│  │  ├─ len.md
│  │  ├─ sorted.md
│  │  ├─ sum.md
│  │  ├─ min.md
│  │  ├─ max.md
│  │  ├─ all.md
│  │  └─ any.md
│  ├─ 06-calculo-y-representacion/
│  │  ├─ abs.md
│  │  ├─ round.md
│  │  ├─ pow.md
│  │  ├─ divmod.md
│  │  ├─ repr.md
│  │  ├─ format.md
│  │  ├─ hash.md
│  │  ├─ ascii.md
│  │  ├─ chr.md
│  │  ├─ ord.md
│  │  ├─ bin.md
│  │  ├─ oct.md
│  │  └─ hex.md
│  ├─ 07-transformacion-funcional/
│  │  ├─ map.md
│  │  └─ filter.md
│  ├─ 08-atributos-y-entorno/
│  │  ├─ hasattr.md
│  │  ├─ getattr.md
│  │  ├─ setattr.md
│  │  ├─ delattr.md
│  │  ├─ vars.md
│  │  ├─ globals.md
│  │  └─ locals.md
│  ├─ 09-ejecucion-dinamica/
│  │  ├─ eval.md
│  │  ├─ exec.md
│  │  ├─ compile.md
│  │  ├─ breakpoint.md
│  │  └─ __import__.md
│  └─ 10-iteracion-asincrona/
│     ├─ aiter.md
│     └─ anext.md
│
├─ 02-tipos-integrados/
│  ├─ index.md
│  ├─ int.md
│  ├─ float.md
│  ├─ bool.md
│  ├─ str.md
│  ├─ list.md
│  ├─ tuple.md
│  ├─ dict.md
│  ├─ set.md
│  ├─ bytes.md
│  └─ bytearray.md
│
└─ 03-metodos-de-tipos/
   ├─ index.md
   ├─ str/
   ├─ list/
   ├─ dict/
   ├─ tuple/
   ├─ set/
   └─ bytes/
````

## Qué se entiende por función integrada

Una función integrada es una función que puede usarse directamente, sin importar ningún módulo.

Ejemplos:

```python
print("Hola")
print(len([1, 2, 3]))
print(type("Python"))
```

Estas funciones forman parte del entorno básico del lenguaje.

## Qué se entiende por tipo integrado

Un tipo integrado es un tipo disponible directamente en Python sin importación previa.

Ejemplos:

```python
numero = 10
texto = "Python"
valores = [1, 2, 3]
datos = {"nombre": "Ana"}
```

Aquí aparecen tipos integrados como:

```text
int
str
list
dict
```

## Qué se entiende por métodos de tipos integrados

Son métodos definidos sobre instancias de tipos incorporados.

Ejemplos:

```python
texto = "python"
print(texto.upper())

valores = [1, 2, 3]
valores.append(4)

datos = {"nombre": "Ana"}
print(datos.keys())
```

Estos métodos no se documentan como funciones sueltas, sino como parte del comportamiento propio de cada tipo.

## Organización conceptual de la sección

## 1. Funciones integradas

Este bloque reúne funciones generales del lenguaje y se organiza por tipo de operación.

La subdivisión se realiza del siguiente modo:

### Salida e interacción

Incluye funciones orientadas a la comunicación directa con el usuario.

```python
print
input
```

### Archivos y recursos

Incluye funciones orientadas a la apertura y manejo inicial de recursos externos.

```python
open
```

### Inspección y tipos

Incluye funciones orientadas a examinar objetos, tipos, atributos y documentación disponible.

```python
type
isinstance
dir
help
id
callable
```

### Iteración y recorrido

Incluye funciones ligadas a secuencias, índices, emparejamiento de iterables y control del avance de la iteración.

```python
range
enumerate
zip
iter
next
reversed
slice
```

### Agregación y orden

Incluye funciones para medir, resumir, comparar, ordenar o evaluar colecciones.

```python
len
sorted
sum
min
max
all
any
```

### Cálculo y representación

Incluye funciones orientadas a cálculo básico, representación textual y transformación de valores numéricos o simbólicos.

```python
abs
round
pow
divmod
repr
format
hash
ascii
chr
ord
bin
oct
hex
```

### Transformación funcional

Incluye funciones que aplican transformaciones o filtrados sobre iterables.

```python
map
filter
```

### Atributos y entorno

Incluye funciones relacionadas con atributos, namespaces y exploración del entorno de ejecución.

```python
hasattr
getattr
setattr
delattr
vars
globals
locals
```

### Ejecución dinámica

Incluye funciones ligadas a evaluación, compilación, importación dinámica y depuración.

```python
eval
exec
compile
breakpoint
__import__
```

### Iteración asíncrona

Incluye funciones integradas orientadas a iterables asíncronos.

```python
aiter
anext
```

Dentro de este bloque, conviene comenzar por las funciones más transversales y frecuentes, dejando preparadas las restantes para su documentación posterior.

Las funciones iniciales más importantes de este bloque son:

```python
print
input
open
type
isinstance
dir
help
len
range
enumerate
zip
iter
next
sorted
sum
min
max
all
any
abs
round
```

Estas funciones cubren una parte sustancial del uso cotidiano de Python.

## 2. Tipos integrados

Este bloque reúne los tipos fundamentales sobre los que se apoya casi todo el lenguaje.

Los principales tipos iniciales documentados son:

```python
int
float
bool
str
list
tuple
dict
set
bytes
bytearray
```

Dentro de este grupo, algunos son escalares, otros son secuencias, otros son mapeos o colecciones sin orden posicional.

## 3. Métodos de tipos integrados

Este bloque documenta el comportamiento específico de los tipos a través de sus métodos públicos.

La organización se realiza por tipo:

* métodos de `str`
* métodos de `list`
* métodos de `dict`
* métodos de `tuple`
* métodos de `set`
* métodos de `bytes`

Este bloque es especialmente importante porque una gran parte del trabajo real en Python se realiza mediante métodos de objetos integrados.

## Orden de estudio recomendado

El recorrido sugerido es el siguiente:

1. Funciones integradas
2. Tipos integrados
3. Métodos de tipos integrados

Dentro de funciones integradas, conviene empezar por las más transversales y frecuentes.

Dentro de tipos integrados, conviene empezar por los tipos más usados en programación general.

Dentro de métodos, conviene estudiar primero los asociados a texto y colecciones mutables.

## Criterio de documentación

Cada archivo de esta sección debería mantener una estructura homogénea. Cuando corresponda, conviene incluir:

* propósito
* forma general de uso
* argumentos
* valor de retorno
* reglas de comportamiento
* ejemplos
* errores comunes
* buenas prácticas
* relaciones con otros elementos integrados

## Bloque de tipos integrados

Los tipos integrados se abordarán con énfasis en:

* naturaleza del tipo
* mutabilidad
* forma de creación
* operaciones habituales
* relación con truthiness
* uso idiomático
* errores frecuentes

## Bloque de métodos

Los métodos se documentarán por tipo y no en una lista única general. Esto permite preservar el contexto semántico de cada operación.

Por ejemplo:

* `str.upper()` pertenece al trabajo con texto
* `list.append()` pertenece al trabajo con listas mutables
* `dict.get()` pertenece al trabajo con mapeos
* `set.add()` pertenece al trabajo con conjuntos

## Resultado esperado de esta sección

Al finalizar esta parte debería quedar claro:

* qué herramientas ya están disponibles sin importar módulos
* qué tipos forman la base operativa del lenguaje
* cómo se usan las funciones integradas más frecuentes
* cómo se aprovechan los métodos de los tipos integrados para trabajar con texto, listas, diccionarios, conjuntos y bytes
