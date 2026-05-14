# `range()`

## Propósito

`range()` construye objetos de tipo `range`, usados para representar progresiones de enteros definidas por inicio, fin y paso sin materializar todos sus valores en una lista. Se usa sobre todo para iteración controlada, conteos, recorridos por índices y secuencias enteras de longitud predecible. ([Python documentation][2])

## Forma general

```python
range(stop)
range(start, stop)
range(start, stop, step)
```

Estas son las tres firmas principales del built-in. El valor devuelto no es una lista, sino un objeto `range`. ([Python documentation][2])

## Argumentos

**`stop`**

Límite superior exclusivo de la progresión. Si solo se pasa un argumento, la secuencia comienza en `0` y termina antes de `stop`. ([Python documentation][2])

**`start`**

Valor inicial de la progresión. Solo aparece cuando se usan dos o tres argumentos. ([Python documentation][2])

**`step`**

Incremento entre valores consecutivos. Puede ser positivo o negativo, pero no puede ser `0`. Si es `0`, Python lanza `ValueError`. ([Python documentation][1])

## Valor de retorno

`range()` devuelve un objeto del tipo integrado `range`. Ese objeto representa la progresión de enteros y puede recorrerse, medirse con `len()`, consultarse con `in` y convertirse a otras colecciones como `list` o `tuple`. ([Python documentation][1])

## Casos básicos

### `range(stop)`

```python
print(list(range(5)))
```

Resultado:

```python
[0, 1, 2, 3, 4]
```

### `range(start, stop)`

```python
print(list(range(2, 7)))
```

Resultado:

```python
[2, 3, 4, 5, 6]
```

### `range(start, stop, step)`

```python
print(list(range(2, 11, 2)))
```

Resultado:

```python
[2, 4, 6, 8, 10]
```

La regla central es que el límite superior es **exclusivo**. ([Python documentation][1])

## Uso típico

El uso más frecuente de `range()` es dentro de `for`, porque permite generar una progresión entera sin crear una lista explícita. También es común en cuentas regresivas, saltos regulares y recorridos por índices. ([Python documentation][1])

```python
for i in range(5):
    print(i)
```

```python
for i in range(10, 0, -1):
    print(i)
```

## Cuándo conviene usarlo

`range()` conviene cuando se necesita:

* una secuencia de enteros
* una cantidad fija de iteraciones
* recorrer posiciones numéricas
* una progresión con paso regular

No conviene usar `list(range(...))` salvo que realmente se necesite una lista materializada. El valor principal de `range()` es precisamente evitar esa materialización. ([Python documentation][1])

## Errores comunes

### Pensar que devuelve una lista

```python
r = range(5)
print(type(r))
```

`range()` no devuelve una lista, sino un objeto `range`. ([Python documentation][1])

### Suponer que el límite superior se incluye

```python
list(range(1, 5))
```

produce `1, 2, 3, 4`, no incluye el `5`. ([Python documentation][1])

### Usar `step = 0`

```python
range(1, 10, 0)
```

genera `ValueError`. ([Python documentation][1])

## Relación con el tipo `range`

La función `range()` debe entenderse como el constructor práctico de objetos `range`. La documentación detallada de cómo se comporta el objeto resultante como secuencia se desarrolla en la ficha del tipo `range`. ([Python documentation][1])