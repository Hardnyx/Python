# `enumerate()`

## Propósito

`enumerate()` permite recorrer un iterable obteniendo simultáneamente un índice y su valor asociado. Es una función integrada especialmente útil cuando se necesita conocer la posición de cada elemento durante una iteración, sin recurrir a patrones menos expresivos como `range(len(...))`.

## Forma general

```python
enumerate(iterable, start=0)
````

## Idea central

`enumerate()` toma un iterable y produce pares de la forma:

```text
(índice, elemento)
```

El índice comienza en `0` por defecto, aunque puede modificarse con el parámetro `start`.

## Argumentos

## `iterable`

Es el objeto que se desea recorrer.

Puede ser, por ejemplo:

* una lista
* una tupla
* una cadena
* un diccionario
* un conjunto
* un rango
* un generador
* cualquier objeto iterable

Ejemplo:

```python id="lfyt14"
nombres = ["Ana", "Luis", "Marta"]

for item in enumerate(nombres):
    print(item)
```

Salida:

```text id="prg79u"
(0, 'Ana')
(1, 'Luis')
(2, 'Marta')
```

## `start`

Indica el valor inicial del índice.

Valor por defecto:

```python id="j9nxv1"
0
```

Ejemplo:

```python id="m96aeb"
nombres = ["Ana", "Luis", "Marta"]

for item in enumerate(nombres, start=1):
    print(item)
```

Salida:

```text id="mg2gxh"
(1, 'Ana')
(2, 'Luis')
(3, 'Marta')
```

## Valor de retorno

`enumerate()` devuelve un objeto de tipo `enumerate`.

```python id="eo87sg"
nombres = ["Ana", "Luis", "Marta"]
resultado = enumerate(nombres)

print(resultado)
print(type(resultado))
```

Salida posible:

```text id="1dqgfp"
<enumerate object at 0x...>
<class 'enumerate'>
```

No devuelve una lista, sino un iterable especializado.

## Comportamiento general

## Uso directo en `for`

La forma más habitual de usar `enumerate()` es dentro de un bucle `for`.

```python id="0eh6y5"
nombres = ["Ana", "Luis", "Marta"]

for indice, nombre in enumerate(nombres):
    print(indice, nombre)
```

Salida:

```text id="nl1qk2"
0 Ana
1 Luis
2 Marta
```

Aquí cada iteración desempaqueta automáticamente el par:

```text
(índice, valor)
```

## Uso con `start`

```python id="h41n56"
nombres = ["Ana", "Luis", "Marta"]

for indice, nombre in enumerate(nombres, start=1):
    print(indice, nombre)
```

Salida:

```text id="0gkxhh"
1 Ana
2 Luis
3 Marta
```

Esto es útil cuando la numeración debe comenzar en 1, por ejemplo en listados visibles para usuarios.

## Conversión a lista

Como el resultado es iterable, puede materializarse con `list()`.

```python id="cqg7wx"
nombres = ["Ana", "Luis", "Marta"]

pares = list(enumerate(nombres))
print(pares)
```

Salida:

```text id="o4ibm5"
[(0, 'Ana'), (1, 'Luis'), (2, 'Marta')]
```

## Uso con distintos iterables

## Listas

```python id="5q0vw1"
valores = [10, 20, 30]

for indice, valor in enumerate(valores):
    print(indice, valor)
```

## Tuplas

```python id="04gke7"
valores = ("a", "b", "c")

for indice, valor in enumerate(valores):
    print(indice, valor)
```

## Cadenas

```python id="86hytn"
texto = "Python"

for indice, caracter in enumerate(texto):
    print(indice, caracter)
```

Salida:

```text id="lh5v1g"
0 P
1 y
2 t
3 h
4 o
5 n
```

## Rangos

```python id="eew4bm"
for indice, valor in enumerate(range(5)):
    print(indice, valor)
```

Salida:

```text id="uozjlwm"
0 0
1 1
2 2
3 3
4 4
```

## Diccionarios

Cuando se itera directamente sobre un diccionario, se recorren sus claves.

```python id="j72k5z"
datos = {"nombre": "Ana", "edad": 20, "ciudad": "Lima"}

for indice, clave in enumerate(datos):
    print(indice, clave)
```

Salida posible:

```text id="jlwmkr"
0 nombre
1 edad
2 ciudad
```

Si se necesitan clave y valor, suele combinarse con `.items()`:

```python id="jlwmjs"
datos = {"nombre": "Ana", "edad": 20}

for indice, (clave, valor) in enumerate(datos.items()):
    print(indice, clave, valor)
```

## Diferencia con `range(len(...))`

Una forma tradicional de obtener índices es:

```python id="tyjlwm"
nombres = ["Ana", "Luis", "Marta"]

for i in range(len(nombres)):
    print(i, nombres[i])
```

Salida:

```text id="jlwmc3"
0 Ana
1 Luis
2 Marta
```

Sin embargo, cuando se necesita índice y valor, `enumerate()` suele ser más claro:

```python id="jlwmd5"
nombres = ["Ana", "Luis", "Marta"]

for i, nombre in enumerate(nombres):
    print(i, nombre)
```

Ventajas de `enumerate()` frente a `range(len(...))`:

* expresa directamente la intención
* evita indexación manual
* reduce posibilidades de error
* resulta más idiomático

## Uso cuando solo interesa el índice

Aunque `enumerate()` se usa principalmente para índice y valor, también puede usarse si el valor no interesa.

```python id="8qjlwm"
nombres = ["Ana", "Luis", "Marta"]

for indice, _ in enumerate(nombres, start=1):
    print(f"Registro {indice}")
```

Aquí el guion bajo `_` indica que el valor existe, pero no se utilizará.

## Uso cuando solo interesa el valor

Si el índice no se necesita, no conviene usar `enumerate()`.

Menos adecuado:

```python id="jlwm0p"
for _, nombre in enumerate(nombres):
    print(nombre)
```

Más claro:

```python id="jlwmw8"
for nombre in nombres:
    print(nombre)
```

## Consumo del objeto `enumerate`

El objeto devuelto por `enumerate()` es iterable y consumible.

```python id="8hjdqg"
nombres = ["Ana", "Luis", "Marta"]
e = enumerate(nombres)

print(next(e))
print(next(e))
```

Salida:

```text id="jlwmvp"
(0, 'Ana')
(1, 'Luis')
```

Si se sigue consumiendo, avanzará hasta agotar el iterable.

## Uso con listas de resultados

`enumerate()` es útil para numerar resultados o reportes.

```python id="zjlwm4"
productos = ["Teclado", "Mouse", "Monitor"]

for numero, producto in enumerate(productos, start=1):
    print(f"{numero}. {producto}")
```

Salida:

```text id="jlwmu2"
1. Teclado
2. Mouse
3. Monitor
```

## Uso con validaciones y búsqueda

Permite identificar la posición de elementos que cumplen una condición.

```python id="jlwmg1"
valores = [5, 8, 13, 21, 34]

for indice, valor in enumerate(valores):
    if valor > 20:
        print("Encontrado en posición:", indice)
```

Salida:

```text id="jlwm8r"
Encontrado en posición: 3
Encontrado en posición: 4
```

## Uso combinado con comprensión de listas

Puede combinarse con comprensiones.

```python id="jlwmli"
nombres = ["Ana", "Luis", "Marta"]

resultado = [f"{i}: {nombre}" for i, nombre in enumerate(nombres)]
print(resultado)
```

Salida:

```text id="jlwm41"
['0: Ana', '1: Luis', '2: Marta']
```

## Casos de uso frecuentes

## Numerar elementos en pantalla

```python id="jlwmhm"
tareas = ["Estudiar", "Practicar", "Repasar"]

for numero, tarea in enumerate(tareas, start=1):
    print(f"{numero}. {tarea}")
```

## Recorrer índice y valor de una secuencia

```python id="6n2t0u"
valores = [100, 200, 300]

for indice, valor in enumerate(valores):
    print(indice, valor)
```

## Identificar posiciones con condición

```python id="abjlwm"
notas = [8, 12, 15, 9, 18]

for indice, nota in enumerate(notas):
    if nota < 11:
        print("Nota desaprobatoria en índice:", indice)
```

## Trabajar con cadenas carácter por carácter

```python id="jlwm7d"
texto = "Hola"

for indice, caracter in enumerate(texto):
    print(indice, caracter)
```

## Errores comunes

## Intentar desempaquetar mal el resultado

Problemático:

```python id="jlwmvd"
nombres = ["Ana", "Luis"]

for indice in enumerate(nombres):
    print(indice, nombre)
```

Aquí `indice` recibe una tupla completa, pero `nombre` no está definido.

Correcto:

```python id="2a62cq"
nombres = ["Ana", "Luis"]

for indice, nombre in enumerate(nombres):
    print(indice, nombre)
```

## Usar `enumerate()` sobre algo no iterable

Problemático:

```python id="0rlnyk"
print(list(enumerate(10)))
```

Esto genera `TypeError`, porque `10` no es iterable.

## Usar `enumerate()` cuando no se necesita índice

Menos claro:

```python id="9u78zl"
for i, nombre in enumerate(nombres):
    print(nombre)
```

Más claro:

```python id="a1qg0m"
for nombre in nombres:
    print(nombre)
```

## Confundir el índice con el valor

Problemático:

```python id="811qpv"
valores = [10, 20, 30]

for indice, valor in enumerate(valores):
    print(valores[indice], valores[valor])
```

Aquí `valor` no representa una posición, sino el contenido real del elemento.

## Suponer que `enumerate()` devuelve lista

Problemático:

```python id="e7rxeu"
resultado = enumerate(["A", "B", "C"])
print(resultado[0])
```

Esto falla porque el objeto `enumerate` no es indexable como una lista común.

Si se necesita indexación, primero debe materializarse:

```python id="kprfbf"
resultado = list(enumerate(["A", "B", "C"]))
print(resultado[0])
```

## Buenas prácticas

## Usar `enumerate()` cuando se necesiten índice y valor al mismo tiempo

```python id="jlwmze"
for indice, valor in enumerate(valores):
    print(indice, valor)
```

## Usar `start=1` cuando se desee numeración humana

```python id="i8jlwm"
for numero, producto in enumerate(productos, start=1):
    print(numero, producto)
```

## Preferir `enumerate()` sobre `range(len(...))` cuando sea posible

Más idiomático:

```python id="jlwm3x"
for i, valor in enumerate(valores):
    print(i, valor)
```

## No usar `enumerate()` si el índice no aporta nada

```python id="jlwm7k"
for valor in valores:
    print(valor)
```

## Usar `_` para valores no utilizados

```python id="jlwmjo"
for indice, _ in enumerate(registros, start=1):
    print(f"Registro {indice}")
```

## Ejemplo integrado

```python id="jlwmjv"
def show_student_report(names, scores):
    print("Reporte de estudiantes")
    print("-" * 30)

    for numero, (name, score) in enumerate(zip(names, scores), start=1):
        status = "Aprobado" if score >= 11 else "Desaprobado"
        print(f"{numero}. {name} -> {score} ({status})")


names = ["Ana", "Luis", "Marta"]
scores = [15, 9, 18]

show_student_report(names, scores)
```

Salida aproximada:

```text id="jlwmdv"
Reporte de estudiantes
------------------------------
1. Ana -> 15 (Aprobado)
2. Luis -> 9 (Desaprobado)
3. Marta -> 18 (Aprobado)
```

## Relación con otros elementos integrados

`enumerate()` se relaciona especialmente con:

* `range()` cuando se trabaja con índices
* `len()` en patrones tradicionales de indexación
* `zip()` cuando se recorren varias secuencias a la vez
* iteración general con `for`
* secuencias como `list`, `tuple` y `str`

## Orden didáctico interno

```text id="10svjlwm"
1. Propósito de enumerate()
2. Forma general
3. Argumentos iterable y start
4. Valor de retorno
5. Uso con for
6. Comparación con range(len(...))
7. Uso con distintos iterables
8. Errores comunes
9. Buenas prácticas
```
