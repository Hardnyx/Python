# `range`

## Propósito

`range` es un tipo integrado de secuencia inmutable que representa progresiones de enteros. Está pensado para describir secuencias numéricas de forma compacta, sin almacenar cada elemento como haría una lista. ([Python documentation][1])

## Naturaleza del tipo

`range` pertenece a los **sequence types** junto con `list` y `tuple`. Es una secuencia ordenada, inmutable y especializada en enteros. Python considera falsos los objetos `range(0)` y verdaderos los `range` no vacíos. ([Python documentation][1])

## Creación

Los objetos de este tipo se obtienen normalmente mediante la función integrada `range()`. La ficha de la función se centra en firmas y argumentos; aquí interesa el comportamiento del objeto ya creado. ([Python documentation][1])

```python
r = range(2, 10, 2)
print(type(r))
```

## Representación

```python
r = range(2, 10, 2)
print(r)
```

La salida muestra una representación como `range(2, 10, 2)`, no una lista. ([Python documentation][1])

## Longitud

Los objetos `range` tienen longitud definida y funcionan con `len()`.

```python
r = range(2, 10, 2)
print(len(r))
```

Esto refleja cuántos enteros contiene la progresión, no el valor de su límite superior. ([Python documentation][1])

## Indexación

`range` admite indexación igual que otras secuencias.

```python
r = range(10)
print(r[0])
print(r[3])
print(r[-1])
```

También puede lanzar `IndexError` si se intenta acceder fuera de rango, como ocurre con otras secuencias. ([Python documentation][1])

## Slicing

Los objetos `range` admiten slicing y el resultado también es un objeto `range`, no una lista.

```python
r = range(10)
subr = r[1:8:2]

print(subr)
print(list(subr))
```

Esto permite seguir trabajando con una representación compacta de la secuencia. ([Python documentation][1])

## Iteración

`range` es iterable.

```python
for x in range(3):
    print(x)
```

También puede usarse con `iter()` y `next()` porque participa del protocolo de iteración como una secuencia integrada. ([Python documentation][1])

## Pertenencia

El operador `in` funciona con `range`.

```python
r = range(1, 10)
print(5 in r)
print(10 in r)
```

Esto permite comprobar si un entero forma parte de la progresión representada. ([Python documentation][1])

## Igualdad

Los objetos `range` admiten comparación por igualdad.

```python
print(range(5) == range(5))
print(range(0, 3, 2) == range(0, 4, 2))
```

La igualdad depende de la secuencia que representan, no solo de que los parámetros visibles sean idénticos. ([Python documentation][1])

## Inmutabilidad

`range` es inmutable. No admite asignación por índice ni operaciones de modificación in place.

```python
r = range(5)
r[0] = 10
```

Eso produce `TypeError`, igual que al intentar reasignar elementos en otras secuencias inmutables. ([Python documentation][1])

## Truthiness

Un `range` vacío se evalúa como `False`. Un `range` no vacío se evalúa como `True`.

```python
print(bool(range(0)))
print(bool(range(5)))
```

La documentación oficial incluye explícitamente `range(0)` entre los objetos built-in considerados falsos. ([Python documentation][1])

## Materialización

Aunque `range` no es una lista, puede convertirse a otras colecciones si se necesita.

```python
r = range(5)

print(list(r))
print(tuple(r))
print(set(r))
```

Eso resulta útil cuando ya no basta con la representación compacta y se necesita una colección materializada. ([Python documentation][1])

## Diferencia frente a `list`

Un `range` y una lista pueden representar la misma secuencia de enteros, pero no son el mismo tipo ni tienen la misma finalidad.

```python
r = range(5)
valores = [0, 1, 2, 3, 4]

print(type(r))
print(type(valores))
```

`range` describe la progresión de forma compacta; `list` almacena explícitamente sus elementos. ([Python documentation][1])

## Cuándo pensar en `range` como tipo

Conviene pensar en `range` como tipo cuando interese explicar que el resultado de `range()`:

* es una secuencia
* tiene longitud
* admite indexación y slicing
* es inmutable
* participa en truthiness y pertenencia

Ese enfoque es útil para entender el modelo de objetos de Python, no solo la llamada de la función. ([Python documentation][1])

## Errores comunes

### Tratarlo como lista mutable

```python
r = range(5)
r.append(5)
```

Eso falla porque `range` no es una lista y no tiene métodos mutables como `append()`. ([Python documentation][1])

### Olvidar que el slicing devuelve `range`

```python
r = range(10)
print(type(r[1:5]))
```

El resultado sigue siendo un objeto `range`. ([Python documentation][1])

### Asumir que siempre es verdadero

```python
if range(0):
    print("sí")
```

`range(0)` es falso en contexto booleano porque es una secuencia vacía. ([Python documentation][1])