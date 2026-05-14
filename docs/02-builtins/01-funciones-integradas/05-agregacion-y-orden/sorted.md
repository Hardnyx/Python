# `sorted()`

## Propósito

`sorted()` devuelve una nueva lista con los elementos de un iterable ordenados. Es una función integrada fundamental para ordenar números, cadenas, tuplas, diccionarios y objetos personalizados sin modificar el iterable original.

## Forma general

```python
sorted(iterable, *, key=None, reverse=False)
````

## Idea central

`sorted()` toma un iterable, obtiene sus elementos, los ordena y devuelve una lista nueva.

Esto implica dos ideas importantes:

1. puede recibir distintos tipos de iterables
2. siempre devuelve una lista

## Argumentos

## `iterable`

Es el objeto cuyos elementos se desean ordenar.

Puede ser, por ejemplo:

* una lista
* una tupla
* una cadena
* un conjunto
* un diccionario
* un rango
* un generador

Ejemplo:

```python id="8wa5qz"
print(sorted([3, 1, 2]))
print(sorted((3, 1, 2)))
print(sorted("python"))
```

Salida:

```text id="f3m6sj"
[1, 2, 3]
[1, 2, 3]
['h', 'n', 'o', 'p', 't', 'y']
```

## `key`

Es una función que se aplica a cada elemento para obtener el criterio de ordenación.

Valor por defecto:

```python id="xt8m2w"
None
```

Si `key` no se especifica, los elementos se comparan directamente.

Ejemplo:

```python id="ud2zwu"
palabras = ["uva", "manzana", "kiwi"]

resultado = sorted(palabras, key=len)
print(resultado)
```

Salida:

```text id="hr7mcp"
['uva', 'kiwi', 'manzana']
```

Aquí no se ordena alfabéticamente, sino por longitud.

## `reverse`

Indica si el orden debe invertirse.

Valor por defecto:

```python id="rhm3bm"
False
```

Ejemplo:

```python id="uwdoel"
print(sorted([3, 1, 2], reverse=True))
```

Salida:

```text id="m0s4ck"
[3, 2, 1]
```

## Valor de retorno

`sorted()` siempre devuelve una lista nueva.

```python id="pxi9jc"
resultado = sorted((3, 1, 2))

print(resultado)
print(type(resultado))
```

Salida:

```text id="n9shvb"
[1, 2, 3]
<class 'list'>
```

Aunque el iterable original sea una tupla, cadena o rango, el resultado sigue siendo una lista.

## Comportamiento general

## Orden ascendente por defecto

```python id="v9tq1s"
print(sorted([5, 2, 9, 1]))
```

Salida:

```text id="d1xj7m"
[1, 2, 5, 9]
```

## Orden descendente con `reverse=True`

```python id="oq5jp8"
print(sorted([5, 2, 9, 1], reverse=True))
```

Salida:

```text id="s9m4rd"
[9, 5, 2, 1]
```

## Orden de cadenas

Por defecto, las cadenas se ordenan lexicográficamente.

```python id="p0w2nf"
print(sorted(["banana", "apple", "cherry"]))
```

Salida:

```text id="q2v4lj"
['apple', 'banana', 'cherry']
```

## Orden de caracteres en una cadena

```python id="q1g8ne"
print(sorted("python"))
```

Salida:

```text id="b7x0wh"
['h', 'n', 'o', 'p', 't', 'y']
```

## Orden de tuplas

Las tuplas se comparan elemento por elemento de izquierda a derecha.

```python id="jlwm8t"
datos = [(2, "b"), (1, "c"), (1, "a")]
print(sorted(datos))
```

Salida:

```text id="7tflr5"
[(1, 'a'), (1, 'c'), (2, 'b')]
```

Primero se compara el primer elemento de cada tupla. Si hay empate, se compara el segundo.

## Uso de `key`

## Ordenar por longitud

```python id="jlwm0n"
palabras = ["uva", "manzana", "kiwi", "pera"]
print(sorted(palabras, key=len))
```

Salida:

```text id="5wmb6p"
['uva', 'kiwi', 'pera', 'manzana']
```

## Ordenar ignorando mayúsculas y minúsculas

```python id="jlwmn3"
palabras = ["Ana", "ana", "Luis", "luis"]
print(sorted(palabras, key=str.lower))
```

Salida posible:

```text id="ylvrbd"
['Ana', 'ana', 'Luis', 'luis']
```

## Ordenar por un campo de una tupla

```python id="jlwm1v"
personas = [("Ana", 20), ("Luis", 18), ("Marta", 25)]
print(sorted(personas, key=lambda persona: persona[1]))
```

Salida:

```text id="jlwmfi"
[('Luis', 18), ('Ana', 20), ('Marta', 25)]
```

Aquí se ordena por edad.

## Ordenar diccionarios mediante sus claves o valores

Cuando se usa `sorted()` directamente sobre un diccionario, se ordenan sus claves.

```python id="jlwmly"
datos = {"b": 2, "a": 1, "c": 3}
print(sorted(datos))
```

Salida:

```text id="jlwmj3"
['a', 'b', 'c']
```

Si se quieren ordenar pares clave-valor:

```python id="jlwm7g"
datos = {"b": 2, "a": 1, "c": 3}

print(sorted(datos.items()))
print(sorted(datos.items(), key=lambda par: par[1]))
```

Salida:

```text id="k4b8gz"
[('a', 1), ('b', 2), ('c', 3)]
[('a', 1), ('b', 2), ('c', 3)]
```

En el segundo caso, el criterio es el valor del diccionario.

## Ordenar conjuntos

```python id="jlwmx1"
valores = {5, 2, 9, 1}
print(sorted(valores))
```

Salida:

```text id="q4j3ta"
[1, 2, 5, 9]
```

Aunque el conjunto no preserve orden, `sorted()` produce una lista ordenada con sus elementos.

## Ordenar rangos

```python id="jlwmwp"
print(sorted(range(5)))
print(sorted(range(5), reverse=True))
```

Salida:

```text id="jlwm88"
[0, 1, 2, 3, 4]
[4, 3, 2, 1, 0]
```

## Relación con `list.sort()`

Es importante distinguir `sorted()` de `list.sort()`.

## `sorted()`

* funciona con cualquier iterable
* devuelve una nueva lista
* no modifica el objeto original

```python id="jlwmw4"
valores = [3, 1, 2]
resultado = sorted(valores)

print(valores)
print(resultado)
```

Salida:

```text id="j2hk06"
[3, 1, 2]
[1, 2, 3]
```

## `list.sort()`

* solo funciona con listas
* modifica la lista original
* no devuelve una nueva lista útil

```python id="0xtu6q"
valores = [3, 1, 2]
resultado = valores.sort()

print(valores)
print(resultado)
```

Salida:

```text id="xx2f0e"
[1, 2, 3]
None
```

## Cuándo usar `sorted()` y cuándo `list.sort()`

Conviene usar `sorted()` cuando:

* el origen no es una lista
* se quiere conservar el objeto original
* se desea una expresión que produzca una lista ordenada

Conviene usar `list.sort()` cuando:

* ya se tiene una lista
* se quiere modificarla directamente
* no se necesita conservar el orden original

## Estabilidad de la ordenación

La ordenación de Python es estable. Esto significa que, si dos elementos tienen el mismo criterio de orden, conservan su orden relativo original.

Ejemplo:

```python id="jlwm24"
datos = [("Ana", 20), ("Luis", 20), ("Marta", 18)]
resultado = sorted(datos, key=lambda persona: persona[1])

print(resultado)
```

Salida:

```text id="ec63m8"
[('Marta', 18), ('Ana', 20), ('Luis', 20)]
```

`Ana` aparece antes que `Luis` porque ambos tenían el mismo criterio `20` y ese era también su orden original.

## Casos de uso frecuentes

## Ordenar números

```python id="jjlwmq"
numeros = [8, 3, 5, 1]
print(sorted(numeros))
```

## Ordenar en sentido inverso

```python id="pf45bd"
numeros = [8, 3, 5, 1]
print(sorted(numeros, reverse=True))
```

## Ordenar palabras por longitud

```python id="1cx9lu"
palabras = ["uva", "manzana", "kiwi", "pera"]
print(sorted(palabras, key=len))
```

## Ordenar registros por campo

```python id="dpfjlwm"
empleados = [("Ana", 2500), ("Luis", 1800), ("Marta", 3200)]
print(sorted(empleados, key=lambda empleado: empleado[1]))
```

## Ordenar ignorando mayúsculas

```python id="jlwmq0"
nombres = ["ana", "Luis", "marta", "Ana"]
print(sorted(nombres, key=str.lower))
```

## Errores comunes

## Suponer que `sorted()` modifica el objeto original

Problemático:

```python id="jlwmk8"
valores = [3, 1, 2]
sorted(valores)

print(valores)
```

Salida:

```text id="jlwm2k"
[3, 1, 2]
```

El original no cambia.

## Esperar que `sorted()` devuelva el mismo tipo del iterable

Problemático:

```python id="wl3ljy"
resultado = sorted((3, 1, 2))
print(type(resultado))
```

Salida:

```text id="jlwmd1"
<class 'list'>
```

El resultado siempre es lista.

## Usar `key` como valor en lugar de función

Problemático:

```python id="zmtjlwm"
palabras = ["uva", "manzana"]
print(sorted(palabras, key=len()))
```

Esto falla porque `key` debe recibir una función, no el resultado de ejecutarla.

Correcto:

```python id="7qjlwm"
print(sorted(palabras, key=len))
```

## Intentar ordenar elementos no comparables entre sí sin `key`

Problemático:

```python id="jlwm0m"
print(sorted([1, "a", 3]))
```

Esto puede producir `TypeError` porque no existe una comparación natural entre enteros y cadenas en este contexto.

## Confundir `sorted()` con `sort()`

Problemático:

```python id="jlwm3j"
valores = [3, 1, 2]
resultado = valores.sort()

print(resultado)
```

Salida:

```text id="qv1ymw"
None
```

## Buenas prácticas

## Usar `sorted()` cuando se quiera preservar el original

```python id="jlwm05"
ordenados = sorted(valores)
```

## Usar `key` para expresar el criterio de orden de forma clara

```python id="pqbjlwm"
ordenados = sorted(personas, key=lambda persona: persona[1])
```

## Usar funciones ya existentes cuando sea posible

```python id="jlwm6k"
sorted(palabras, key=len)
sorted(nombres, key=str.lower)
```

## Usar `reverse=True` solo cuando se necesite orden inverso real

```python id="jlwm8m"
sorted(numeros, reverse=True)
```

## Preferir claridad en criterios complejos

Si el criterio de orden es difícil de entender en una sola expresión, conviene extraerlo a una función con nombre.

```python id="p0xh7s"
def employee_salary(employee):
    return employee[1]

ordenados = sorted(empleados, key=employee_salary)
```

## Ejemplo integrado

```python id="jlwm4m"
def show_ranking(students):
    ordered_students = sorted(
        students,
        key=lambda student: student[1],
        reverse=True
    )

    print("Ranking")
    print("-" * 30)

    for position, (name, score) in enumerate(ordered_students, start=1):
        print(f"{position}. {name} -> {score}")


students = [
    ("Ana", 18),
    ("Luis", 12),
    ("Marta", 20),
    ("Carlos", 15)
]

show_ranking(students)
```

Salida aproximada:

```text id="8h3xqv"
Ranking
------------------------------
1. Marta -> 20
2. Ana -> 18
3. Carlos -> 15
4. Luis -> 12
```

## Relación con otros elementos integrados

`sorted()` se relaciona especialmente con:

* `list.sort()` para ordenación en listas
* `min()` y `max()` como operaciones de comparación extrema
* `enumerate()` cuando se numera el resultado ordenado
* `zip()` al ordenar estructuras paralelas o registros combinados
* funciones y lambdas usadas como criterio en `key`

## Orden didáctico interno

```text
1. Propósito de sorted()
2. Forma general
3. Argumentos iterable, key y reverse
4. Orden ascendente y descendente
5. Uso con números, cadenas, tuplas y diccionarios
6. Uso de key
7. Diferencia entre sorted() y list.sort()
8. Estabilidad de la ordenación
9. Errores comunes
10. Buenas prácticas
```