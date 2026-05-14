# `zip()`

## Propósito

`zip()` permite combinar varios iterables elemento a elemento. El resultado agrupa en tuplas los valores que ocupan la misma posición relativa en cada iterable.

Es una función integrada especialmente útil para:

- recorrer varias secuencias en paralelo
- asociar claves con valores
- reconstruir o reorganizar datos tabulares
- transformar filas en columnas o columnas en filas
- desempaquetar estructuras agrupadas

## Forma general

```python
zip(*iterables)
````

## Idea central

Si se tienen varios iterables:

```text
A = [a1, a2, a3]
B = [b1, b2, b3]
C = [c1, c2, c3]
```

entonces:

```python
zip(A, B, C)
```

produce conceptualmente:

```text
(a1, b1, c1)
(a2, b2, c2)
(a3, b3, c3)
```

## Argumentos

## `*iterables`

`zip()` recibe dos o más iterables, aunque también puede usarse con uno solo o incluso con ninguno.

Ejemplos válidos:

```python
zip([1, 2, 3], ["a", "b", "c"])
zip(range(3), "abc")
zip([10, 20, 30])
zip()
```

## Valor de retorno

`zip()` devuelve un objeto de tipo `zip`.

```python
pares = zip([1, 2, 3], ["a", "b", "c"])

print(pares)
print(type(pares))
```

Salida posible:

```text
<zip object at 0x...>
<class 'zip'>
```

No devuelve una lista, sino un iterable consumible.

## Comportamiento general

## Combinación básica de dos iterables

```python
numeros = [1, 2, 3]
letras = ["a", "b", "c"]

resultado = list(zip(numeros, letras))
print(resultado)
```

Salida:

```text
[(1, 'a'), (2, 'b'), (3, 'c')]
```

Cada tupla contiene un elemento de cada iterable, tomado de la misma posición.

## Combinación de tres iterables

```python
ids = [101, 102, 103]
nombres = ["Ana", "Luis", "Marta"]
edades = [20, 25, 22]

resultado = list(zip(ids, nombres, edades))
print(resultado)
```

Salida:

```text
[(101, 'Ana', 20), (102, 'Luis', 25), (103, 'Marta', 22)]
```

## Uso directo en `for`

La forma más común de usar `zip()` es dentro de un bucle `for`.

```python
nombres = ["Ana", "Luis", "Marta"]
edades = [20, 25, 22]

for nombre, edad in zip(nombres, edades):
    print(nombre, edad)
```

Salida:

```text
Ana 20
Luis 25
Marta 22
```

## Longitud desigual de los iterables

`zip()` se detiene cuando se agota el iterable más corto.

```python
nombres = ["Ana", "Luis", "Marta"]
edades = [20, 25]

resultado = list(zip(nombres, edades))
print(resultado)
```

Salida:

```text
[('Ana', 20), ('Luis', 25)]
```

El valor `"Marta"` no aparece porque el segundo iterable terminó antes.

Esta es una de las reglas más importantes de `zip()`.

## Uso con un solo iterable

Si se pasa un solo iterable, cada resultado será una tupla de un solo elemento.

```python
valores = [10, 20, 30]

resultado = list(zip(valores))
print(resultado)
```

Salida:

```text
[(10,), (20,), (30,)]
```

## Uso sin argumentos

Si se llama sin argumentos, produce un iterable vacío.

```python
resultado = list(zip())
print(resultado)
```

Salida:

```text
[]
```

## Consumo del objeto `zip`

El objeto devuelto por `zip()` es iterable y consumible.

```python
pares = zip([1, 2, 3], ["a", "b", "c"])

print(next(pares))
print(next(pares))
```

Salida:

```text
(1, 'a')
(2, 'b')
```

Si se sigue consumiendo hasta el final, ya no volverá a empezar automáticamente.

```python
pares = zip([1, 2, 3], ["a", "b", "c"])

print(list(pares))
print(list(pares))
```

Salida:

```text
[(1, 'a'), (2, 'b'), (3, 'c')]
[]
```

## Uso con `list()`

Cuando se desea ver o reutilizar el contenido completo, suele materializarse con `list()`.

```python
nombres = ["Ana", "Luis", "Marta"]
edades = [20, 25, 22]

pares = list(zip(nombres, edades))
print(pares)
```

## Uso con `dict()`

Un uso muy frecuente consiste en combinar claves y valores para construir un diccionario.

```python
claves = ["nombre", "edad", "ciudad"]
valores = ["Ana", 20, "Lima"]

datos = dict(zip(claves, valores))
print(datos)
```

Salida:

```text
{'nombre': 'Ana', 'edad': 20, 'ciudad': 'Lima'}
```

Esto funciona porque cada elemento generado por `zip()` es una tupla de dos posiciones:

```text
(clave, valor)
```

## Uso con `enumerate()`

`zip()` puede combinarse con `enumerate()` cuando se desea numerar pares o grupos de elementos.

```python
nombres = ["Ana", "Luis", "Marta"]
edades = [20, 25, 22]

for indice, (nombre, edad) in enumerate(zip(nombres, edades), start=1):
    print(indice, nombre, edad)
```

Salida:

```text
1 Ana 20
2 Luis 25
3 Marta 22
```

## Uso con cadenas

Las cadenas también son iterables, por lo que pueden combinarse carácter a carácter.

```python
texto_1 = "ABC"
texto_2 = "123"

resultado = list(zip(texto_1, texto_2))
print(resultado)
```

Salida:

```text
[('A', '1'), ('B', '2'), ('C', '3')]
```

## Uso con `range()`

```python
for indice, letra in zip(range(3), ["a", "b", "c"]):
    print(indice, letra)
```

Salida:

```text
0 a
1 b
2 c
```

Aunque esto funciona, cuando lo que se desea es índice y valor de una sola secuencia, suele ser más apropiado `enumerate()`.

## Desempaquetado con `zip(*iterable)`

Uno de los usos más importantes de `zip()` es el desempaquetado o transposición.

Supóngase una lista de pares:

```python
pares = [("Ana", 20), ("Luis", 25), ("Marta", 22)]
```

Puede separarse en dos grupos así:

```python
nombres, edades = zip(*pares)

print(nombres)
print(edades)
```

Salida:

```text
('Ana', 'Luis', 'Marta')
(20, 25, 22)
```

Aquí el operador `*` desempaqueta la lista de pares como argumentos separados para `zip()`.

## Transposición de filas y columnas

`zip()` también puede usarse para transformar filas en columnas.

Ejemplo:

```python
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

transpuesta = list(zip(*matriz))
print(transpuesta)
```

Salida:

```text
[(1, 4, 7), (2, 5, 8), (3, 6, 9)]
```

Cada fila original se convierte en parte de una nueva columna agrupada.

## Uso con comprensión de listas

Puede combinarse con comprensiones.

```python
nombres = ["Ana", "Luis", "Marta"]
edades = [20, 25, 22]

resultado = [f"{nombre} ({edad})" for nombre, edad in zip(nombres, edades)]
print(resultado)
```

Salida:

```text
['Ana (20)', 'Luis (25)', 'Marta (22)']
```

## Casos de uso frecuentes

## Recorrer dos listas en paralelo

```python
productos = ["Teclado", "Mouse", "Monitor"]
precios = [120, 50, 800]

for producto, precio in zip(productos, precios):
    print(producto, precio)
```

## Construir diccionarios

```python
claves = ["id", "nombre", "edad"]
valores = [101, "Ana", 20]

registro = dict(zip(claves, valores))
print(registro)
```

## Transponer una matriz

```python
matriz = [
    [1, 2],
    [3, 4],
    [5, 6]
]

print(list(zip(*matriz)))
```

Salida:

```text
[(1, 3, 5), (2, 4, 6)]
```

## Separar columnas previamente agrupadas

```python
datos = [("Ana", 20), ("Luis", 25), ("Marta", 22)]

nombres, edades = zip(*datos)

print(nombres)
print(edades)
```

## Errores comunes

## Suponer que `zip()` devuelve lista

Problemático:

```python
resultado = zip([1, 2, 3], ["a", "b", "c"])
print(resultado[0])
```

Esto falla porque el objeto `zip` no es indexable como una lista común.

Si se necesita indexación, primero debe materializarse:

```python
resultado = list(zip([1, 2, 3], ["a", "b", "c"]))
print(resultado[0])
```

## Olvidar que `zip()` se consume

```python
resultado = zip([1, 2, 3], ["a", "b", "c"])

print(list(resultado))
print(list(resultado))
```

La segunda salida estará vacía.

## Suponer que usa la longitud del iterable más largo

Problemático:

```python
nombres = ["Ana", "Luis", "Marta"]
edades = [20, 25]

print(list(zip(nombres, edades)))
```

Salida:

```text
[('Ana', 20), ('Luis', 25)]
```

`zip()` se detiene en el iterable más corto.

## Desempaquetar mal los valores en `for`

Problemático:

```python
nombres = ["Ana", "Luis"]
edades = [20, 25]

for par in zip(nombres, edades):
    print(nombre, edad)
```

Aquí `nombre` y `edad` no están definidos.

Correcto:

```python
for nombre, edad in zip(nombres, edades):
    print(nombre, edad)
```

## Usar `zip()` cuando solo se necesita una secuencia

Si solo se recorre una secuencia, `zip()` no aporta claridad.

Menos adecuado:

```python
for valor in zip(valores):
    print(valor)
```

Más claro:

```python
for valor in valores:
    print(valor)
```

## Buenas prácticas

## Usar `zip()` para recorridos paralelos

```python
for nombre, edad in zip(nombres, edades):
    print(nombre, edad)
```

## Tener presente que el recorrido termina en el iterable más corto

Esto debe considerarse especialmente cuando los datos deberían tener longitudes coincidentes.

## Materializar con `list()` o `dict()` solo cuando sea necesario

```python
pares = list(zip(a, b))
registro = dict(zip(claves, valores))
```

## Usar `zip(*datos)` cuando se necesite transponer o separar columnas

```python
columnas = list(zip(*filas))
```

## Combinar con `enumerate()` cuando se necesite índice y recorrido paralelo

```python
for i, (a, b) in enumerate(zip(lista_1, lista_2), start=1):
    print(i, a, b)
```

## Ejemplo integrado

```python
def show_sales_report(products, prices, quantities):
    print("Reporte de ventas")
    print("-" * 30)

    for indice, (producto, precio, cantidad) in enumerate(
        zip(products, prices, quantities),
        start=1
    ):
        total = precio * cantidad
        print(f"{indice}. {producto} -> precio: {precio}, cantidad: {cantidad}, total: {total}")


products = ["Teclado", "Mouse", "Monitor"]
prices = [120, 50, 800]
quantities = [2, 3, 1]

show_sales_report(products, prices, quantities)
```

Salida aproximada:

```text
Reporte de ventas
------------------------------
1. Teclado -> precio: 120, cantidad: 2, total: 240
2. Mouse -> precio: 50, cantidad: 3, total: 150
3. Monitor -> precio: 800, cantidad: 1, total: 800
```

## Relación con otros elementos integrados

`zip()` se relaciona especialmente con:

* `enumerate()` cuando se desea numerar recorridos paralelos
* `range()` en ciertos patrones de recorrido
* `list()` y `dict()` para materializar el resultado
* desempaquetado con `*`
* iteración general con `for`

## Orden didáctico interno

```text
1. Propósito de zip()
2. Forma general
3. Argumentos y valor de retorno
4. Combinación básica de iterables
5. Regla del iterable más corto
6. Uso con for
7. Uso con list() y dict()
8. Desempaquetado con zip(*iterable)
9. Transposición de datos
10. Errores comunes
11. Buenas prácticas
```