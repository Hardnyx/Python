# Tipos integrados

## Propósito

Los tipos integrados constituyen la base operativa de Python. Permiten representar números, texto, colecciones, datos binarios y valores lógicos sin necesidad de importar módulos. Comprender su naturaleza, mutabilidad, formas de creación y operaciones principales es indispensable para escribir código correcto, idiomático y mantenible.

## Alcance

Esta sección documenta los tipos integrados de uso más frecuente dentro de un recorrido práctico y general. El foco inicial está puesto en los tipos que aparecen constantemente en programación cotidiana:

```python id="t8y2cm"
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
````

Cada tipo se abordará con énfasis en:

* propósito
* naturaleza del tipo
* mutabilidad
* formas de creación
* operaciones habituales
* reglas de comportamiento
* truthiness
* errores comunes
* buenas prácticas

## Relación con otras partes de la documentación

**Núcleo de Python**

Describe el modelo de objetos, mutabilidad, funciones, clases, iteración y demás estructuras del lenguaje.

**Funciones integradas**

Describe herramientas disponibles sin `import`, como `print()`, `len()`, `type()`, `range()` u `open()`.

**Tipos integrados**

Describe los objetos fundamentales sobre los que operan esas funciones y el resto del lenguaje.

**Métodos de tipos integrados**

Describe los métodos públicos de cada tipo, como `str.upper()`, `list.append()` o `dict.get()`.

## Estructura interna de la sección

```text
02-tipos-integrados/
├─ index.md
├─ int.md
├─ float.md
├─ bool.md
├─ str.md
├─ list.md
├─ tuple.md
├─ dict.md
├─ set.md
├─ bytes.md
└─ bytearray.md
```

## Qué se entiende por tipo integrado

Un tipo integrado es un tipo disponible directamente en Python sin importación previa.

Ejemplos:

```python id="m4k7zp"
numero = 10
precio = 3.14
activo = True
texto = "Python"
valores = [1, 2, 3]
datos = {"nombre": "Ana"}
```

Aquí aparecen tipos integrados como:

```text id="t1c6ny"
int
float
bool
str
list
dict
```

## Clasificación general

Los tipos integrados iniciales pueden organizarse de forma práctica en cuatro grupos.

## 1. Tipos numéricos y lógicos

Incluyen valores usados para cálculo, comparación y control de flujo.

```python id="e2w9jf"
int
float
bool
```

### `int`

Representa enteros de precisión arbitraria.

Ejemplos:

```python id="yg6jv1"
10
-5
0
1000000
```

### `float`

Representa números reales en punto flotante.

Ejemplos:

```python id="f7u3qd"
3.14
-0.5
0.0
2.5e3
```

### `bool`

Representa verdad lógica.

Ejemplos:

```python id="q9r2hk"
True
False
```

## 2. Tipos textuales

Representan secuencias de caracteres.

```python id="e6m8dp"
str
```

### `str`

Representa texto Unicode.

Ejemplos:

```python id="j3d1nx"
"Hola"
"Python"
"123"
""
```

## 3. Tipos de colección

Permiten agrupar varios valores.

```python id="x4u7bl"
list
tuple
dict
set
```

### `list`

Secuencia mutable y ordenada.

```python id="c9k5mv"
[1, 2, 3]
["Ana", "Luis"]
[]
```

### `tuple`

Secuencia inmutable y ordenada.

```python id="a2p8rz"
(1, 2, 3)
("Ana", "Luis")
()
```

### `dict`

Mapa clave-valor mutable.

```python id="d5v3qy"
{"nombre": "Ana", "edad": 20}
{}
```

### `set`

Colección mutable sin duplicados y sin orden posicional.

```python id="r8j1wc"
{1, 2, 3}
set()
```

## 4. Tipos binarios

Representan datos binarios o secuencias de bytes.

```python id="m7z4ht"
bytes
bytearray
```

### `bytes`

Secuencia inmutable de bytes.

```python id="m1w6sk"
b"abc"
bytes([65, 66, 67])
```

### `bytearray`

Secuencia mutable de bytes.

```python id="z6q2pe"
bytearray(b"abc")
bytearray([65, 66, 67])
```

## Criterios principales de estudio

Cada tipo se estudiará atendiendo a ciertas propiedades comunes.

## Naturaleza del contenido

Un tipo puede representar:

* un valor escalar
* una secuencia
* una colección
* un mapa
* un bloque binario

## Mutabilidad

Un tipo puede ser mutable o inmutable.

### Inmutables

```python id="k8n3yv"
int
float
bool
str
tuple
bytes
```

### Mutables

```python id="u4m9qx"
list
dict
set
bytearray
```

La mutabilidad afecta:

* asignación
* copias
* paso de argumentos
* aliasing
* diseño de funciones y clases

## Orden y acceso posicional

Algunos tipos preservan orden posicional:

```python id="d9t5kf"
str
list
tuple
bytes
bytearray
```

Otros no tienen acceso posicional estándar:

```python id="b2v7wr"
dict
set
```

En `dict`, la noción central no es la posición sino la clave.

## Duplicados

Algunos tipos permiten repetición de valores:

```python id="x1q6tm"
list
tuple
str
bytes
bytearray
```

Otros no la permiten en su estructura lógica:

```python id="h7m4zd"
set
```

## Relación con truthiness

Todos los tipos integrados participan en evaluación booleana.

Regla general:

* valores vacíos o cero tienden a evaluarse como `False`
* valores no vacíos o distintos de cero tienden a evaluarse como `True`

Ejemplos:

```python id="t5w1pn"
print(bool(0))
print(bool(0.0))
print(bool(""))
print(bool([]))
print(bool({}))
print(bool(set()))
```

Salida:

```text id="r3k8mv"
False
False
False
False
False
False
```

Ejemplos verdaderos:

```python id="g6p2sy"
print(bool(1))
print(bool("Hola"))
print(bool([1]))
print(bool({"a": 1}))
```

Salida:

```text id="q4m7xn"
True
True
True
True
```

## Creación y construcción

Cada tipo integrado puede aparecer:

* como literal
* como resultado de una expresión
* como conversión desde otro tipo

Ejemplos de literales:

```python id="h1z6qc"
10
3.14
True
"Hola"
[1, 2, 3]
(1, 2, 3)
{"a": 1}
{1, 2, 3}
b"abc"
```

Ejemplos de construcción explícita:

```python id="r9u4kb"
int("10")
float("3.14")
str(25)
list("abc")
tuple([1, 2, 3])
dict([("a", 1), ("b", 2)])
set([1, 2, 2, 3])
bytes("abc", encoding="utf-8")
bytearray(b"abc")
```

## Diferencia entre tipo y valor

Cada valor pertenece a un tipo.

```python id="y2m8vd"
valor = [1, 2, 3]

print(valor)
print(type(valor))
```

Salida:

```text id="n6w1kp"
[1, 2, 3]
<class 'list'>
```

La sección de tipos integrados se ocupa de describir qué significa pertenecer a cada uno de estos tipos y cómo se comportan.

## Relación con funciones integradas

Muchos built-ins operan directamente sobre tipos integrados.

Ejemplos:

```python id="n4x7jq"
print(len("Python"))
print(len([1, 2, 3]))
print(type({"a": 1}))
print(sorted([3, 1, 2]))
print(sum([1, 2, 3]))
```

Por eso, comprender los tipos integrados permite entender mejor por qué ciertas funciones operan de una forma sobre un tipo y de otra forma sobre otro.

## Relación con métodos de tipos

Cada tipo tiene su propia interfaz de métodos.

Ejemplos:

```python id="v7k2yt"
texto = "python"
print(texto.upper())

valores = [1, 2, 3]
valores.append(4)

datos = {"nombre": "Ana"}
print(datos.get("nombre"))
```

Estas operaciones no se documentan aquí en detalle, sino en la sección de métodos de tipos integrados.

## Orden de estudio recomendado

El recorrido sugerido es el siguiente:

1. `int`
2. `float`
3. `bool`
4. `str`
5. `list`
6. `tuple`
7. `dict`
8. `set`
9. `bytes`
10. `bytearray`

Este orden va desde los tipos más básicos y frecuentes hasta colecciones y tipos binarios más especializados.

## Justificación del orden

### `int`, `float`, `bool`

Conviene empezar por los tipos más simples y frecuentes en expresiones, validaciones y control de flujo.

### `str`

El texto aparece constantemente en entrada, salida, validación, archivos y transformación de datos.

### `list` y `tuple`

Ambos introducen secuencias y permiten explicar orden, indexación, mutabilidad e inmutabilidad.

### `dict`

Es esencial en modelado de datos, configuración, registros y estructuras clave-valor.

### `set`

Introduce operaciones de pertenencia, unicidad y teoría básica de conjuntos.

### `bytes` y `bytearray`

Se dejan para el final porque son más especializados y dependen de comprender primero texto, secuencias y mutabilidad.

## Resultado esperado de esta sección

Al finalizar esta parte debería quedar claro:

* qué tipos integrados forman la base operativa de Python
* cómo se clasifican
* cuáles son mutables y cuáles no
* qué diferencias estructurales existen entre secuencias, mapas y conjuntos
* cómo se crean y convierten valores
* cómo se relacionan los tipos con funciones integradas y métodos