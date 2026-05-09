# Comprensiones, iteradores y generadores

## Propósito

Las comprensiones, los iteradores y los generadores permiten producir, transformar y recorrer datos de forma más expresiva y eficiente. Este bloque conecta la sintaxis compacta de construcción de colecciones con el protocolo de iteración del lenguaje y con la producción diferida de valores.

## Alcance

El tema incluye:

- comprensiones de listas, conjuntos y diccionarios
- expresiones generadoras
- diferencia entre iterable e iterador
- uso de `iter()` y `next()`
- funciones generadoras con `yield`
- consumo parcial y perezoso de datos
- buenas prácticas de uso

No se trata de repetir el uso elemental de `for`, sino de explicar cómo Python construye y recorre secuencias de manera estructurada.

## Ideas fundamentales

1. Una comprensión es una forma compacta de construir una colección a partir de otra.
2. Un iterable es un objeto que puede recorrerse.
3. Un iterador es un objeto que entrega elementos uno a uno.
4. `iter()` obtiene un iterador a partir de un iterable.
5. `next()` consume el siguiente elemento de un iterador.
6. Un generador produce valores bajo demanda, sin construir toda la secuencia en memoria.
7. `yield` transforma una función en una función generadora.
8. Las expresiones generadoras y las funciones generadoras permiten procesamiento perezoso.

## Comprensiones

Las comprensiones permiten construir nuevas colecciones a partir de un iterable.

## Comprensión de lista

Forma general:

```python
[expresión for elemento in iterable]
````

Ejemplo:

```python id="0w2w50"
cuadrados = [x ** 2 for x in range(5)]
print(cuadrados)
```

Salida:

```text id="nq8y1s"
[0, 1, 4, 9, 16]
```

Equivale conceptualmente a:

```python id="ue08qf"
cuadrados = []

for x in range(5):
    cuadrados.append(x ** 2)

print(cuadrados)
```

## Comprensión de lista con condición

Forma general:

```python
[expresión for elemento in iterable if condición]
```

Ejemplo:

```python id="rq14g9"
pares = [x for x in range(10) if x % 2 == 0]
print(pares)
```

Salida:

```text id="wdn7dr"
[0, 2, 4, 6, 8]
```

## Expresión condicional dentro de la comprensión

Puede aplicarse una decisión sobre cada elemento.

Forma general:

```python
[valor_si_true if condición else valor_si_false for elemento in iterable]
```

Ejemplo:

```python id="u38svz"
etiquetas = ["par" if x % 2 == 0 else "impar" for x in range(5)]
print(etiquetas)
```

Salida:

```text id="serot7"
['par', 'impar', 'par', 'impar', 'par']
```

## Comprensión de conjunto

Forma general:

```python
{expresión for elemento in iterable}
```

Ejemplo:

```python id="tzjkhy"
valores = {x % 3 for x in range(10)}
print(valores)
```

Salida posible:

```text id="zvt1b2"
{0, 1, 2}
```

Se usa cuando se desea construir un conjunto, normalmente para eliminar duplicados o trabajar con pertenencia.

## Comprensión de diccionario

Forma general:

```python
{clave: valor for elemento in iterable}
```

Ejemplo:

```python id="kf5w7y"
cuadrados = {x: x ** 2 for x in range(5)}
print(cuadrados)
```

Salida:

```text id="ocdzj4"
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

## Comprensiones anidadas

Una comprensión puede incluir más de un `for`.

Ejemplo:

```python id="n53s4f"
pares = [(x, y) for x in [1, 2] for y in [10, 20]]
print(pares)
```

Salida:

```text id="2e4qwc"
[(1, 10), (1, 20), (2, 10), (2, 20)]
```

El orden sigue la misma lógica que los bucles anidados:

```python id="mef4wo"
pares = []

for x in [1, 2]:
    for y in [10, 20]:
        pares.append((x, y))

print(pares)
```

## Cuándo conviene usar comprensiones

Las comprensiones convienen cuando:

* la transformación es breve y clara
* la colección resultante debe construirse completa
* la lógica cabe razonablemente en una sola expresión

No conviene usarlas cuando la lógica es demasiado extensa o difícil de leer.

Problemático:

```python id="j7lyiw"
resultado = [x ** 2 if x % 2 == 0 else x ** 3 if x > 5 else x - 1 for x in range(20)]
```

En casos así, suele ser preferible usar un bucle explícito o una función auxiliar.

## Iterable e iterador

## Iterable

Un iterable es un objeto que puede recorrerse elemento por elemento.

Ejemplos comunes:

* `str`
* `list`
* `tuple`
* `dict`
* `set`
* `range`
* generadores
* archivos abiertos

Ejemplo:

```python id="g5wjkv"
for letra in "Python":
    print(letra)
```

La cadena es iterable.

## Iterador

Un iterador es un objeto que entrega elementos uno a uno y recuerda su posición actual.

Un iterador debe implementar el protocolo de iteración:

* `__iter__()`
* `__next__()`

Ejemplo con `iter()`:

```python id="nhhymd"
numeros = [10, 20, 30]
it = iter(numeros)

print(it)
```

## `iter(obj)`

Devuelve un iterador a partir de un iterable.

```python id="qq3qwa"
numeros = [10, 20, 30]
it = iter(numeros)

print(next(it))
print(next(it))
print(next(it))
```

Salida:

```text id="gubg2a"
10
20
30
```

## `next(it)`

Devuelve el siguiente elemento del iterador.

Cuando no quedan más elementos, genera `StopIteration`.

```python id="8pq3sa"
numeros = [10, 20]
it = iter(numeros)

print(next(it))
print(next(it))
print(next(it))
```

Error típico:

```text id="d5sxqv"
StopIteration
```

## Relación entre iterable e iterador

No todo iterable es un iterador, pero todo iterador es iterable.

Ejemplo:

```python id="nt0bql"
numeros = [1, 2, 3]
it = iter(numeros)

print(iter(it) is it)
```

Salida:

```text id="ok6gkx"
True
```

## Consumo de iteradores

Un iterador se consume a medida que se avanza sobre él.

```python id="rtu2rm"
numeros = [1, 2, 3]
it = iter(numeros)

print(next(it))
print(list(it))
print(list(it))
```

Salida:

```text id="nhyn50"
1
[2, 3]
[]
```

Después de consumirse, ya no vuelve a empezar automáticamente.

## `for` e iteración interna

El bucle `for` usa internamente `iter()` y `next()`.

```python id="x227rq"
for x in [10, 20, 30]:
    print(x)
```

Conceptualmente equivale a:

```python id="yandp4"
it = iter([10, 20, 30])

while True:
    try:
        x = next(it)
        print(x)
    except StopIteration:
        break
```

## Expresiones generadoras

Una expresión generadora tiene una sintaxis similar a una comprensión de lista, pero usa paréntesis y no construye todos los valores de inmediato.

Forma general:

```python
(expresión for elemento in iterable)
```

Ejemplo:

```python id="5unrv9"
gen = (x ** 2 for x in range(5))
print(gen)
```

Salida posible:

```text id="k4xjxq"
<generator object ...>
```

Para consumirlo:

```python id="xf5znx"
gen = (x ** 2 for x in range(5))

print(next(gen))
print(next(gen))
print(list(gen))
```

Salida:

```text id="6mgs4i"
0
1
[4, 9, 16]
```

## Diferencia entre comprensión de lista y expresión generadora

| Construcción         | Sintaxis           | Crea la colección completa | Uso de memoria |
| -------------------- | ------------------ | -------------------------: | -------------: |
| Comprensión de lista | `[x for x in ...]` |                         Sí |          Mayor |
| Expresión generadora | `(x for x in ...)` |                         No |          Menor |

Ejemplo comparativo:

```python id="r0dbld"
lista = [x ** 2 for x in range(5)]
gen = (x ** 2 for x in range(5))

print(lista)
print(gen)
```

Salida:

```text id="f3ldw5"
[0, 1, 4, 9, 16]
<generator object ...>
```

## Cuándo conviene usar expresiones generadoras

Convienen cuando:

* no hace falta materializar todos los datos al mismo tiempo
* se trabaja con muchos elementos
* el resultado será consumido una sola vez
* se encadenan operaciones de forma eficiente

Ejemplo:

```python id="cbm2fu"
total = sum(x ** 2 for x in range(1000))
print(total)
```

En este caso no hace falta construir una lista intermedia.

## Funciones generadoras

Una función generadora es una función que usa `yield` en lugar de `return` para producir valores gradualmente.

Ejemplo:

```python id="rp8my0"
def contar_hasta_tres():
    yield 1
    yield 2
    yield 3

gen = contar_hasta_tres()

print(next(gen))
print(next(gen))
print(next(gen))
```

Salida:

```text id="5vbvzk"
1
2
3
```

## `yield`

`yield` produce un valor y pausa la función, conservando su estado interno. La siguiente llamada a `next()` reanuda la ejecución desde ese punto.

Ejemplo:

```python id="b7f76b"
def secuencia():
    print("Antes del primer yield")
    yield 10

    print("Antes del segundo yield")
    yield 20

gen = secuencia()

print(next(gen))
print(next(gen))
```

Salida:

```text id="oxletc"
Antes del primer yield
10
Antes del segundo yield
20
```

## Diferencia entre `return` y `yield`

`return` finaliza la función y devuelve un resultado.

`yield` pausa la función y permite continuar más adelante.

Ejemplo con `return`:

```python id="fl4rbn"
def f():
    return 10

print(f())
```

Ejemplo con `yield`:

```python id="2kxw3n"
def g():
    yield 10

print(g())
```

Salida posible:

```text id="jlwmzq"
<generator object g at ...>
```

## Generador con bucle

Ejemplo típico:

```python id="z4f1vl"
def cuadrados(n):
    for x in range(n):
        yield x ** 2

gen = cuadrados(5)

print(list(gen))
```

Salida:

```text id="jpkh6s"
[0, 1, 4, 9, 16]
```

## Estado persistente del generador

Una función generadora conserva su punto de ejecución entre llamadas.

```python id="4s5mns"
def contador():
    n = 1
    while n <= 3:
        yield n
        n += 1

gen = contador()

print(next(gen))
print(next(gen))
print(next(gen))
```

Salida:

```text id="fjlwmm"
1
2
3
```

## Generadores infinitos

Un generador puede producir valores indefinidamente.

```python id="zwuehd"
def naturales():
    n = 0
    while True:
        yield n
        n += 1

gen = naturales()

print(next(gen))
print(next(gen))
print(next(gen))
```

Salida:

```text id="abupf6"
0
1
2
```

En estos casos debe controlarse cuidadosamente el consumo.

## `yield from`

Permite delegar la iteración a otro iterable o generador.

```python id="x2nydw"
def subgenerador():
    yield 1
    yield 2

def generador():
    yield 0
    yield from subgenerador()
    yield 3

print(list(generador()))
```

Salida:

```text id="dny7bx"
[0, 1, 2, 3]
```

## Comprensiones y generadores en diccionarios y conjuntos

También pueden combinarse ideas de construcción y consumo.

Ejemplo con diccionario:

```python id="io37b5"
cuadrados = {x: x ** 2 for x in range(5)}
print(cuadrados)
```

Ejemplo con conjunto:

```python id="d7qfvl"
residuos = {x % 2 for x in range(10)}
print(residuos)
```

## Materialización de generadores

Muchos generadores pueden convertirse en estructuras concretas usando:

* `list()`
* `tuple()`
* `set()`
* `dict()`, cuando los elementos tienen la forma adecuada

Ejemplo:

```python id="xxalnd"
gen = (x ** 2 for x in range(5))

print(list(gen))
```

Salida:

```text id="q2ngct"
[0, 1, 4, 9, 16]
```

Al materializarlo, el generador se consume.

## Diferencia entre secuencia reutilizable e iterador consumible

Una lista puede recorrerse varias veces:

```python id="cimq97"
valores = [1, 2, 3]

print(list(valores))
print(list(valores))
```

Salida:

```text id="gh1cpd"
[1, 2, 3]
[1, 2, 3]
```

Un generador no funciona así:

```python id="qjlwmh"
gen = (x for x in range(3))

print(list(gen))
print(list(gen))
```

Salida:

```text id="lh75qv"
[0, 1, 2]
[]
```

## Errores comunes

## Usar paréntesis esperando una lista

Problemático:

```python id="vqf7z1"
valores = (x ** 2 for x in range(5))
print(valores[0])
```

Esto falla porque el resultado es un generador, no una lista indexable.

## Consumir un generador y esperar reutilizarlo

```python id="5dlkm4"
gen = (x for x in range(3))

print(list(gen))
print(list(gen))
```

La segunda salida estará vacía.

## Escribir comprensiones demasiado complejas

Problemático:

```python id="2z73w9"
resultado = [(x, y, z) for x in range(3) for y in range(3) for z in range(3) if x + y > z if z % 2 == 0]
```

Aunque sea válida, puede ser poco legible. En casos así conviene usar bucles explícitos.

## Confundir iterable con iterador

Una lista es iterable, pero no es en sí un iterador consumible con estado avanzado.

```python id="xvv4bw"
valores = [1, 2, 3]

print(next(valores))
```

Esto genera error. Primero debe obtenerse un iterador:

```python id="skx2lh"
valores = [1, 2, 3]
it = iter(valores)

print(next(it))
```

## Olvidar que `next()` puede lanzar `StopIteration`

```python id="d0bckj"
it = iter([1])

print(next(it))
print(next(it))
```

La segunda llamada genera `StopIteration`.

## Buenas prácticas

## Usar comprensiones cuando la transformación sea clara y breve

```python id="lhd4zc"
cuadrados = [x ** 2 for x in range(10)]
```

## Preferir expresiones generadoras cuando no se necesite materializar todo

```python id="9jlvv3"
total = sum(x ** 2 for x in range(1000))
```

## Usar funciones generadoras para flujos de datos o secuencias grandes

```python id="a1fjlwm"
def leer_lineas(ruta):
    with open(ruta, "r", encoding="utf-8") as archivo:
        for linea in archivo:
            yield linea.strip()
```

## Evitar comprensiones excesivamente anidadas o difíciles de leer

Si la lógica deja de ser evidente en una sola mirada, conviene volver a bucles explícitos o extraer una función auxiliar.

## Tener presente que los generadores se consumen

Si se va a recorrer varias veces el resultado, puede ser preferible materializarlo.

## Ejemplo integrado

```python id="ex9qai"
def cuadrados_pares(limite):
    for numero in range(limite):
        if numero % 2 == 0:
            yield numero ** 2


def main():
    lista = [x ** 2 for x in range(6)]
    conjunto = {x % 3 for x in range(10)}
    diccionario = {x: x ** 2 for x in range(5)}
    generador = (x ** 2 for x in range(6))
    generador_filtrado = cuadrados_pares(10)

    print("Lista:", lista)
    print("Conjunto:", conjunto)
    print("Diccionario:", diccionario)
    print("Primeros valores del generador:", next(generador), next(generador))
    print("Generador filtrado:", list(generador_filtrado))


if __name__ == "__main__":
    main()
```

## Orden didáctico interno

```text id="ky5xy3"
1. Comprensión de lista
2. Comprensión con condición
3. Comprensión de conjunto
4. Comprensión de diccionario
5. Diferencia entre comprensión y bucle explícito
6. Iterable e iterador
7. iter() y next()
8. Consumo de iteradores
9. Expresiones generadoras
10. Diferencia entre lista y generador
11. Funciones generadoras con yield
12. Estado persistente del generador
13. yield from
14. Materialización y consumo
15. Errores comunes y buenas prácticas
```