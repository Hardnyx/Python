# `sum()`

## Propósito

`sum()` permite calcular la suma de los elementos de un iterable. Es una función integrada orientada principalmente a valores numéricos y resulta muy útil para agregación, acumulación y cálculo de totales.

## Forma general

```python
sum(iterable, /, start=0)
````

## Idea central

`sum()` recorre un iterable y va acumulando sus elementos a partir de un valor inicial.

Conceptualmente:

```text id="x3f2ja"
start + elemento_1 + elemento_2 + ... + elemento_n
```

## Argumentos

## `iterable`

Es el objeto cuyos elementos se desean sumar.

Puede ser, por ejemplo:

* una lista
* una tupla
* un rango
* un generador
* cualquier iterable cuyos elementos sean sumables entre sí

Ejemplo:

```python id="f8g2dv"
print(sum([1, 2, 3]))
print(sum((10, 20, 30)))
print(sum(range(5)))
```

Salida:

```text id="q1s9km"
6
60
10
```

## `start`

Es el valor inicial de la suma.

Valor por defecto:

```python id="h5d7nr"
0
```

Ejemplo:

```python id="y2m4bc"
print(sum([1, 2, 3], 10))
```

Salida:

```text id="b7k1qt"
16
```

Aquí la operación es:

```text id="j8p3wv"
10 + 1 + 2 + 3
```

## Valor de retorno

`sum()` devuelve el resultado acumulado de los elementos del iterable, comenzando desde `start`.

El tipo del resultado depende de los valores sumados y del valor inicial.

```python id="u4c8xe"
print(sum([1, 2, 3]))
print(type(sum([1, 2, 3])))

print(sum([1.5, 2.5]))
print(type(sum([1.5, 2.5])))
```

Salida:

```text id="k9v2md"
6
<class 'int'>
4.0
<class 'float'>
```

## Comportamiento general

## Suma de enteros

```python id="d2x7pa"
print(sum([1, 2, 3, 4]))
```

Salida:

```text id="z5n1hu"
10
```

## Suma de flotantes

```python id="g3r8lw"
print(sum([1.5, 2.5, 3.0]))
```

Salida:

```text id="m7q4te"
7.0
```

## Suma de mezcla numérica compatible

```python id="p9v2ks"
print(sum([1, 2.5, 3]))
```

Salida:

```text id="c4w8jy"
6.5
```

## Suma de un rango

```python id="r6t1mf"
print(sum(range(1, 6)))
```

Salida:

```text id="n2p7xa"
15
```

## Suma de un generador

```python id="q8m4uc"
print(sum(x ** 2 for x in range(5)))
```

Salida:

```text id="v1k9zd"
30
```

Esto es muy común cuando se quiere evitar construir una lista intermedia.

## Uso de `start`

## Inicio distinto de cero

```python id="j3t6qy"
valores = [10, 20, 30]
print(sum(valores, 100))
```

Salida:

```text id="s8w2nk"
160
```

## Uso con tipos compatibles con el valor inicial

El valor inicial participa en la acumulación. Por eso debe ser compatible con los elementos del iterable.

Ejemplo válido:

```python id="w2m5cr"
print(sum([1, 2, 3], 0.5))
```

Salida:

```text id="b9q7jh"
6.5
```

## Iterables vacíos

Si el iterable está vacío, `sum()` devuelve el valor inicial.

```python id="t5x3ne"
print(sum([]))
print(sum([], 10))
```

Salida:

```text id="d7k1mw"
0
10
```

## Uso típico con expresiones generadoras

`sum()` se combina muy bien con generadores.

```python id="m8p4tv"
total = sum(x for x in range(1, 11) if x % 2 == 0)
print(total)
```

Salida:

```text id="y6n2qc"
30
```

Aquí se están sumando solo los números pares entre 1 y 10.

## Diferencia entre `sum()` y acumulación manual

Ejemplo manual:

```python id="n7v1sk"
valores = [1, 2, 3, 4]

total = 0
for valor in valores:
    total += valor

print(total)
```

Salida:

```text id="e4m8yd"
10
```

Forma equivalente con `sum()`:

```python id="q2x6pw"
valores = [1, 2, 3, 4]
print(sum(valores))
```

Cuando solo interesa sumar, `sum()` suele ser más claro y directo.

## Uso con booleanos

Los valores booleanos son sumables porque `True` equivale a `1` y `False` equivale a `0`.

```python id="v9q3tl"
print(sum([True, False, True, True]))
```

Salida:

```text id="p1m7dx"
3
```

Esto resulta útil para contar condiciones verdaderas.

```python id="c6w2ra"
valores = [10, 15, 8, 20, 3]
cantidad = sum(valor > 10 for valor in valores)

print(cantidad)
```

Salida:

```text id="z3k8nv"
2
```

## Uso con listas de listas

`sum()` no es una herramienta adecuada para concatenar listas.

Problemático:

```python id="x1p6jb"
print(sum([[1, 2], [3, 4]], []))
```

Aunque puede funcionar en algunos casos, no es recomendable como patrón general.

La propia documentación de Python desaconseja usar `sum()` para concatenar secuencias como cadenas, bytes o listas. ([docs.python.org](https://docs.python.org/3/library/functions.html#sum))

## Uso con cadenas

`sum()` no debe usarse con cadenas.

```python id="h4m9qe"
print(sum(["a", "b", "c"]))
```

Esto genera error porque la suma de cadenas no está soportada por `sum()` en este contexto.

Para cadenas, corresponde usar:

```python id="r8n2kw"
print("".join(["a", "b", "c"]))
```

## Casos de uso frecuentes

## Total de valores numéricos

```python id="m1v4cp"
ventas = [120, 150, 90, 200]
print(sum(ventas))
```

## Suma de cuadrados

```python id="b2q7tn"
print(sum(x ** 2 for x in range(5)))
```

## Conteo de condiciones verdaderas

```python id="w9k3sx"
notas = [8, 12, 15, 9, 18]
aprobados = sum(nota >= 11 for nota in notas)

print(aprobados)
```

## Suma con valor inicial

```python id="t4p8ja"
valores = [5, 10, 15]
print(sum(valores, 100))
```

## Errores comunes

## Usar `sum()` con cadenas

Problemático:

```python id="g7m2vd"
sum(["a", "b", "c"])
```

Esto genera `TypeError`.

Para texto, corresponde usar `str.join()`.

## Usar `sum()` para concatenar listas

Problemático:

```python id="d3w8ny"
sum([[1, 2], [3, 4]], [])
```

Aunque pueda producir resultado, no es la herramienta adecuada.

## Pasar un valor inicial incompatible

Problemático:

```python id="q6n1tk"
sum([1, 2, 3], "0")
```

Esto genera `TypeError` porque no se puede sumar una cadena con enteros.

## Suponer que `sum()` modifica el iterable original

```python id="j9p4xc"
valores = [1, 2, 3]
resultado = sum(valores)

print(valores)
print(resultado)
```

Salida:

```text id="k2v7me"
[1, 2, 3]
6
```

El iterable original no cambia.

## Buenas prácticas

## Usar `sum()` para agregación numérica clara

```python id="n5w3qa"
total = sum(valores)
```

## Combinar `sum()` con generadores cuando no se necesite materializar una lista

```python id="z8m1rh"
total = sum(x ** 2 for x in range(100))
```

## Usar `sum()` con booleanos para contar condiciones

```python id="p4k6tw"
cantidad = sum(valor > 0 for valor in datos)
```

## No usar `sum()` para concatenar texto o listas

Para cadenas:

```python id="f7q2ny"
"".join(partes)
```

Para listas, suele convenir otro enfoque más explícito.

## Ejemplo integrado

```python id="m2t8qv"
def show_sales_summary(sales):
    total_sales = sum(sales)
    average_sales = total_sales / len(sales) if sales else 0
    high_sales_count = sum(sale >= 150 for sale in sales)

    print("Resumen de ventas")
    print("-" * 30)
    print("Ventas:", sales)
    print("Total:", total_sales)
    print("Promedio:", average_sales)
    print("Ventas altas:", high_sales_count)


show_sales_summary([120, 150, 90, 200, 175])
```

Salida aproximada:

```text id="c8m4wr"
Resumen de ventas
------------------------------
Ventas: [120, 150, 90, 200, 175]
Total: 735
Promedio: 147.0
Ventas altas: 3
```

## Relación con otros elementos integrados

`sum()` se relaciona especialmente con:

* `len()` cuando se calcula promedio
* `min()` y `max()` para resumir colecciones
* `sorted()` cuando se analizan datos ordenados
* expresiones generadoras para agregación eficiente
* valores booleanos en conteos de condiciones

## Orden didáctico interno

```text id="u1p9mx"
1. Propósito de sum()
2. Forma general
3. Argumentos iterable y start
4. Valor de retorno
5. Suma de enteros, flotantes y generadores
6. Uso con booleanos
7. Uso con iterables vacíos
8. Diferencia con acumulación manual
9. Errores comunes
10. Buenas prácticas
```