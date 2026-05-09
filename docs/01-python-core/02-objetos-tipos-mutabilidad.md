# Objetos, tipos y mutabilidad

## Propósito

Python se basa en un modelo de objetos. Comprender cómo se relacionan objeto, tipo, valor, identidad y mutabilidad permite explicar comportamientos fundamentales del lenguaje, como la asignación, el paso de argumentos, la modificación de estructuras de datos y las diferencias entre copiar, referenciar y transformar valores.

## Alcance

Este tema no se limita a enumerar tipos básicos, sino a explicar cómo Python representa y manipula los valores en memoria y cómo esa representación afecta el comportamiento del programa.

## Ideas fundamentales

Los principios centrales de este bloque son los siguientes:

1. Todo valor en Python es un objeto.
2. Todo objeto tiene tipo, identidad y valor.
3. Algunos objetos son mutables y otros son inmutables.
4. Una variable no contiene el objeto, sino una referencia a él.
5. Asignar una variable no siempre crea una copia.
6. Modificar un objeto mutable puede afectar a todas las referencias que apuntan a él.
7. La igualdad y la identidad no significan lo mismo.

## Qué es un objeto

Un objeto es una entidad que tiene:

- un tipo
- una identidad
- un valor

Ejemplo:

```python
numero = 10
texto = "Python"
valores = [1, 2, 3]
````

En los tres casos, `10`, `"Python"` y `[1, 2, 3]` son objetos.

## Tipo

El tipo indica qué clase de objeto es y qué operaciones admite.

```python
print(type(10))
print(type("Python"))
print(type([1, 2, 3]))
```

Salida:

```text
<class 'int'>
<class 'str'>
<class 'list'>
```

## Valor

El valor es el contenido lógico del objeto.

Ejemplos:

```python
numero = 10
texto = "Python"
datos = [1, 2, 3]
```

Valores:

```text
10
"Python"
[1, 2, 3]
```

## Identidad

La identidad distingue un objeto de cualquier otro, aunque tengan el mismo contenido.

La función `id()` permite observar esa identidad.

```python
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a))
print(id(b))
```

Las identidades serán distintas, porque son objetos diferentes.

## Relación entre tipo, valor e identidad

Dos objetos pueden:

* tener el mismo valor
* tener el mismo tipo
* tener distinta identidad

Ejemplo:

```python
a = [1, 2, 3]
b = [1, 2, 3]

print(type(a) == type(b))
print(a == b)
print(a is b)
```

Salida:

```text
True
True
False
```

Interpretación:

* ambos son listas
* ambas listas contienen los mismos valores
* no son el mismo objeto

## Variables y referencias

En Python, una variable actúa como un nombre que referencia un objeto.

```python
x = [1, 2, 3]
```

Aquí, `x` no contiene físicamente la lista. `x` referencia el objeto lista.

Si otra variable apunta al mismo objeto:

```python
x = [1, 2, 3]
y = x
```

entonces ambas referencias apuntan al mismo objeto.

```python
print(x is y)
print(id(x))
print(id(y))
```

Salida:

```text
True
<misma identidad>
<misma identidad>
```

## Asignación

La asignación enlaza un nombre con un objeto.

```python
x = 10
```

Luego:

```python
x = 20
```

La variable `x` deja de referenciar el objeto anterior y pasa a referenciar otro.

Esto no implica que el valor `10` haya sido modificado. Solo cambió la referencia almacenada en `x`.

## Reasignación

```python
x = "hola"
print(x)

x = [1, 2, 3]
print(x)
```

Salida:

```text
hola
[1, 2, 3]
```

La variable puede referenciar objetos de distinto tipo a lo largo del tiempo.

## Mutabilidad e inmutabilidad

La mutabilidad indica si el contenido interno de un objeto puede modificarse después de su creación.

## Objetos mutables

Un objeto mutable puede cambiar sin dejar de ser el mismo objeto.

Ejemplos típicos:

* `list`
* `dict`
* `set`
* `bytearray`

Ejemplo:

```python
valores = [1, 2, 3]
print(id(valores))

valores.append(4)

print(valores)
print(id(valores))
```

Salida:

```text
[1, 2, 3, 4]
<mismo id antes y después>
```

La lista cambió, pero siguió siendo el mismo objeto.

## Objetos inmutables

Un objeto inmutable no puede modificarse internamente después de crearse.

Ejemplos típicos:

* `int`
* `float`
* `bool`
* `str`
* `tuple`
* `bytes`
* `frozenset`

Ejemplo:

```python
texto = "hola"
print(id(texto))

texto = texto.upper()

print(texto)
print(id(texto))
```

Salida:

```text
HOLA
<id distinto>
```

No se modificó el objeto original. Se creó otro objeto y la variable pasó a referenciarlo.

## Resumen de mutabilidad

| Tipo        | Mutable |
| ----------- | ------: |
| `int`       |      No |
| `float`     |      No |
| `bool`      |      No |
| `str`       |      No |
| `tuple`     |      No |
| `bytes`     |      No |
| `frozenset` |      No |
| `list`      |      Sí |
| `dict`      |      Sí |
| `set`       |      Sí |
| `bytearray` |      Sí |

## Igualdad e identidad

## Igualdad: `==`

Compara valores.

```python
a = [1, 2, 3]
b = [1, 2, 3]

print(a == b)
```

Salida:

```text
True
```

## Identidad: `is`

Compara si ambas referencias apuntan al mismo objeto.

```python
a = [1, 2, 3]
b = [1, 2, 3]

print(a is b)
```

Salida:

```text
False
```

## Ejemplo comparativo

```python
a = [1, 2, 3]
b = a
c = [1, 2, 3]

print(a == b)
print(a is b)

print(a == c)
print(a is c)
```

Salida:

```text
True
True
True
False
```

## Cuándo usar `==` y cuándo usar `is`

Regla general:

* `==` para comparar contenido
* `is` para comparar identidad

Uso correcto de `is` con `None`:

```python
valor = None

if valor is None:
    print("No hay valor")
```

No conviene usar `is` para comparar números, cadenas o listas por contenido.

## Aliasing

Aliasing ocurre cuando dos o más nombres apuntan al mismo objeto mutable.

```python
a = [1, 2, 3]
b = a

b.append(4)

print(a)
print(b)
```

Salida:

```text
[1, 2, 3, 4]
[1, 2, 3, 4]
```

La modificación hecha a través de `b` también afecta a `a`, porque ambas variables apuntan a la misma lista.

## Copia y referencia

## Referencia compartida

```python
a = [1, 2, 3]
b = a
```

No se crea una copia. Solo se crea otra referencia al mismo objeto.

## Copia superficial

Una copia superficial crea un nuevo contenedor, pero los elementos internos siguen siendo referencias a los mismos objetos.

```python
a = [[1, 2], [3, 4]]
b = a.copy()

print(a is b)
print(a[0] is b[0])
```

Salida:

```text
False
True
```

La lista externa es nueva, pero las listas internas son compartidas.

## Copia profunda

Una copia profunda crea nuevos objetos también para los elementos internos.

```python
import copy

a = [[1, 2], [3, 4]]
b = copy.deepcopy(a)

print(a is b)
print(a[0] is b[0])
```

Salida:

```text
False
False
```

La diferencia entre copia superficial y profunda será desarrollada con más detalle en el módulo `copy`.

## Mutabilidad dentro de objetos inmutables

Un objeto inmutable puede contener referencias a objetos mutables.

Ejemplo:

```python
datos = ([1, 2], [3, 4])
```

La tupla es inmutable, pero las listas dentro de ella sí pueden modificarse.

```python
datos[0].append(99)
print(datos)
```

Salida:

```text
([1, 2, 99], [3, 4])
```

No se modificó la estructura de la tupla, pero sí el contenido de un objeto mutable referenciado por ella.

## Paso de argumentos a funciones

Python pasa referencias a objetos. Esto tiene efectos distintos según el objeto sea mutable o inmutable.

## Ejemplo con objeto inmutable

```python
def incrementar(numero):
    numero = numero + 1
    print("Dentro de la función:", numero)

x = 10
incrementar(x)
print("Fuera de la función:", x)
```

Salida:

```text
Dentro de la función: 11
Fuera de la función: 10
```

La operación crea un nuevo objeto entero dentro de la función. La variable externa no cambia.

## Ejemplo con objeto mutable

```python
def agregar_elemento(lista):
    lista.append(4)
    print("Dentro de la función:", lista)

valores = [1, 2, 3]
agregar_elemento(valores)
print("Fuera de la función:", valores)
```

Salida:

```text
Dentro de la función: [1, 2, 3, 4]
Fuera de la función: [1, 2, 3, 4]
```

La función modifica el objeto original.

## Reasignación local de un objeto mutable

```python
def reemplazar_lista(lista):
    lista = [100, 200, 300]
    print("Dentro de la función:", lista)

valores = [1, 2, 3]
reemplazar_lista(valores)
print("Fuera de la función:", valores)
```

Salida:

```text
Dentro de la función: [100, 200, 300]
Fuera de la función: [1, 2, 3]
```

Aquí no se modificó el objeto original. Solo se reasignó la variable local `lista`.

## Truthiness

Python permite evaluar objetos en contextos booleanos.

En general:

* objetos vacíos se evalúan como `False`
* objetos no vacíos se evalúan como `True`

Ejemplos que se evalúan como `False`:

```python
False
None
0
0.0
""
[]
{}
set()
tuple()
```

Ejemplo:

```python
texto = ""

if texto:
    print("Tiene contenido")
else:
    print("Está vacío")
```

Salida:

```text
Está vacío
```

## Conversión de tipos

Python permite construir un objeto de otro tipo a partir de un valor dado.

```python
print(int("10"))
print(float("3.14"))
print(str(25))
print(list("abc"))
print(tuple([1, 2, 3]))
print(set([1, 2, 2, 3]))
```

Salida:

```text
10
3.14
25
['a', 'b', 'c']
(1, 2, 3)
{1, 2, 3}
```

La conversión no siempre es válida.

```python
print(int("hola"))
```

Error típico:

```text
ValueError
```

## Inmutabilidad de cadenas

Las cadenas son inmutables. Sus métodos devuelven nuevos objetos.

```python
texto = "python"
nuevo_texto = texto.upper()

print(texto)
print(nuevo_texto)
```

Salida:

```text
python
PYTHON
```

El objeto original no cambia.

## Mutabilidad de listas

Las listas sí pueden modificarse en el mismo objeto.

```python
valores = [1, 2, 3]
valores.append(4)

print(valores)
```

Salida:

```text
[1, 2, 3, 4]
```

## Tuplas y listas

La diferencia entre lista y tupla no es solo sintáctica. También es conceptual.

* `list` es mutable
* `tuple` es inmutable

```python
lista = [1, 2, 3]
tupla = (1, 2, 3)
```

La lista puede cambiar:

```python
lista.append(4)
```

La tupla no admite modificación directa:

```python
tupla[0] = 10
```

Error típico:

```text
TypeError
```

## Diccionarios y conjuntos

Tanto `dict` como `set` son mutables.

```python
datos = {"nombre": "Ana"}
datos["edad"] = 20

print(datos)
```

Salida:

```text
{'nombre': 'Ana', 'edad': 20}
```

```python
valores = {1, 2, 3}
valores.add(4)

print(valores)
```

## Objetos hashables

Un objeto hashable puede usarse como clave de diccionario o como elemento de un conjunto.

En general, los objetos inmutables suelen ser hashables, aunque no siempre. Los objetos mutables normalmente no lo son.

Ejemplo válido:

```python
datos = {
    "nombre": "Ana",
    10: "entero",
    (1, 2): "tupla"
}

print(datos)
```

Ejemplo inválido:

```python
datos = {
    [1, 2]: "lista"
}
```

Error típico:

```text
TypeError: unhashable type: 'list'
```

## Errores comunes

## Confundir igualdad con identidad

Problemático:

```python
a = [1, 2, 3]
b = [1, 2, 3]

print(a is b)
```

Esto no compara contenido, sino identidad.

Correcto para comparar contenido:

```python
print(a == b)
```

## Modificar un objeto compartido sin advertir el aliasing

```python
a = [1, 2, 3]
b = a

b.append(4)

print(a)
```

La modificación afecta también a `a`.

## Suponer que una asignación crea copia

```python
a = [1, 2, 3]
b = a
```

Aquí no hay copia. Ambas variables apuntan al mismo objeto.

## Suponer que métodos de cadenas modifican el texto original

```python
texto = "python"
texto.upper()

print(texto)
```

Salida:

```text
python
```

Debe reasignarse:

```python
texto = texto.upper()
```

## Usar objetos mutables como claves de diccionario

```python
datos = {
    [1, 2]: "valor"
}
```

Esto genera error porque las listas no son hashables.

## Buenas prácticas

## Usar `==` para comparar valores

```python
if lista_1 == lista_2:
    print("Mismo contenido")
```

## Usar `is` para `None`

```python
if resultado is None:
    print("Sin resultado")
```

## Copiar explícitamente cuando no se quiera compartir un objeto mutable

```python
a = [1, 2, 3]
b = a.copy()
```

## Tener cuidado al modificar argumentos mutables dentro de funciones

```python
def procesar(lista):
    copia = lista.copy()
    copia.append(99)
    return copia
```

## Distinguir entre modificar y reasignar

Modificar:

```python
lista.append(4)
```

Reasignar:

```python
lista = [1, 2, 3, 4]
```

Ambas operaciones tienen efectos distintos sobre las referencias existentes.

## Ejemplo integrado

```python
def add_item_safely(items, item):
    copied_items = items.copy()
    copied_items.append(item)
    return copied_items


original_items = [1, 2, 3]
shared_reference = original_items
independent_copy = original_items.copy()

shared_reference.append(4)
result = add_item_safely(original_items, 5)

print("Original:", original_items)
print("Shared reference:", shared_reference)
print("Independent copy:", independent_copy)
print("Returned copy:", result)

print("Mismo contenido:", original_items == shared_reference)
print("Mismo objeto:", original_items is shared_reference)
print("Copia independiente:", original_items is independent_copy)
```

## Orden didáctico interno

```text
1. Objeto, tipo, valor e identidad
2. Variables como referencias
3. Asignación y reasignación
4. Mutabilidad e inmutabilidad
5. Igualdad e identidad
6. Aliasing
7. Copias y referencias compartidas
8. Paso de argumentos a funciones
9. Truthiness
10. Conversión de tipos
11. Hashabilidad
12. Errores comunes y buenas prácticas
```