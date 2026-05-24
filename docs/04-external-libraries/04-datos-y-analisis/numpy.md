# `numpy`

## Propósito

`numpy` es una librería externa para computación numérica en Python. Se utiliza para trabajar con arreglos multidimensionales, operaciones vectorizadas, álgebra lineal, funciones matemáticas, números aleatorios, transformaciones de datos y procesamiento eficiente de información numérica.

Es una de las bases más importantes del ecosistema científico y de datos en Python. Muchas librerías como `pandas`, `scipy`, `matplotlib`, `scikit-learn` y otras herramientas de análisis se apoyan directa o indirectamente en estructuras o conceptos de NumPy.

## Naturaleza de la librería

`numpy` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install numpy
```

La importación habitual es:

```python
import numpy as np
```

El alias `np` es la convención estándar y aparece de forma generalizada en documentación, ejemplos y proyectos reales.

## Idea central

La idea principal de NumPy es trabajar con arreglos numéricos de forma eficiente.

En Python puro, una lista puede almacenar números:

```python
values = [1, 2, 3, 4]
```

En NumPy, esos datos pueden representarse como un arreglo:

```python
import numpy as np

values = np.array([1, 2, 3, 4])

print(values)
```

Salida:

```text
[1 2 3 4]
```

La diferencia importante es que NumPy permite operar sobre todos los elementos de manera vectorizada:

```python
import numpy as np

values = np.array([1, 2, 3, 4])

print(values * 2)
```

Salida:

```text
[2 4 6 8]
```

## Instalación

Instalación básica:

```bash
python -m pip install numpy
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
numpy==2.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
import numpy as np
```

Verificación:

```python
import numpy as np

print(np.__version__)
```

## Objeto principal: `ndarray`

El objeto principal de NumPy es `ndarray`.

Un `ndarray` representa un arreglo N-dimensional.

Ejemplos:

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([[1, 2, 3], [4, 5, 6]])

print(type(a))
print(type(b))
```

Salida:

```text
<class 'numpy.ndarray'>
<class 'numpy.ndarray'>
```

## Arreglo de una dimensión

```python
import numpy as np

values = np.array([10, 20, 30])

print(values)
print(values.ndim)
print(values.shape)
```

Salida:

```text
[10 20 30]
1
(3,)
```

## Arreglo de dos dimensiones

```python
import numpy as np

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(matrix)
print(matrix.ndim)
print(matrix.shape)
```

Salida:

```text
[[1 2 3]
 [4 5 6]]
2
(2, 3)
```

## Atributos principales

## `ndim`

Indica el número de dimensiones del arreglo.

```python
import numpy as np

array = np.array([[1, 2], [3, 4]])

print(array.ndim)
```

## `shape`

Indica el tamaño del arreglo por dimensión.

```python
import numpy as np

array = np.array([[1, 2, 3], [4, 5, 6]])

print(array.shape)
```

Salida:

```text
(2, 3)
```

## `size`

Indica la cantidad total de elementos.

```python
import numpy as np

array = np.array([[1, 2, 3], [4, 5, 6]])

print(array.size)
```

Salida:

```text
6
```

## `dtype`

Indica el tipo de dato interno de los elementos.

```python
import numpy as np

array = np.array([1, 2, 3])

print(array.dtype)
```

Salida posible:

```text
int64
```

## `itemsize`

Indica el tamaño en bytes de cada elemento.

```python
import numpy as np

array = np.array([1, 2, 3], dtype=np.int64)

print(array.itemsize)
```

Salida:

```text
8
```

## Creación de arreglos

## `np.array()`

Crea un arreglo a partir de una lista, tupla u otra estructura compatible.

```python
import numpy as np

array = np.array([1, 2, 3])

print(array)
```

## `np.zeros()`

Crea un arreglo lleno de ceros.

```python
import numpy as np

array = np.zeros(5)

print(array)
```

Salida:

```text
[0. 0. 0. 0. 0.]
```

Arreglo bidimensional:

```python
import numpy as np

matrix = np.zeros((2, 3))

print(matrix)
```

## `np.ones()`

Crea un arreglo lleno de unos.

```python
import numpy as np

array = np.ones(4)

print(array)
```

Salida:

```text
[1. 1. 1. 1.]
```

## `np.full()`

Crea un arreglo lleno de un valor específico.

```python
import numpy as np

array = np.full((2, 3), 7)

print(array)
```

Salida:

```text
[[7 7 7]
 [7 7 7]]
```

## `np.arange()`

Crea una secuencia numérica con inicio, fin y paso.

```python
import numpy as np

array = np.arange(0, 10, 2)

print(array)
```

Salida:

```text
[0 2 4 6 8]
```

El límite superior no se incluye, igual que con `range()`.

## `np.linspace()`

Crea valores igualmente espaciados entre un inicio y un fin.

```python
import numpy as np

array = np.linspace(0, 1, 5)

print(array)
```

Salida:

```text
[0.   0.25 0.5  0.75 1.  ]
```

A diferencia de `np.arange()`, aquí se indica la cantidad de puntos deseada.

## `np.eye()`

Crea una matriz identidad.

```python
import numpy as np

identity = np.eye(3)

print(identity)
```

Salida:

```text
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
```

## Tipos de datos con `dtype`

NumPy permite controlar el tipo interno de los datos.

```python
import numpy as np

array = np.array([1, 2, 3], dtype=np.float64)

print(array)
print(array.dtype)
```

Salida:

```text
[1. 2. 3.]
float64
```

Tipos frecuentes:

```text
int32
int64
float32
float64
bool
str
object
```

## Conversión de tipo con `astype()`

```python
import numpy as np

array = np.array([1.2, 2.8, 3.5])

converted = array.astype(int)

print(converted)
```

Salida:

```text
[1 2 3]
```

`astype()` devuelve un nuevo arreglo convertido.

## Indexación

## Indexación en una dimensión

```python
import numpy as np

array = np.array([10, 20, 30, 40])

print(array[0])
print(array[2])
print(array[-1])
```

Salida:

```text
10
30
40
```

## Indexación en dos dimensiones

```python
import numpy as np

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(matrix[0, 0])
print(matrix[1, 2])
```

Salida:

```text
1
6
```

La forma:

```python
matrix[1, 2]
```

es preferible a:

```python
matrix[1][2]
```

porque expresa directamente la posición multidimensional.

## Slicing

## Slicing en una dimensión

```python
import numpy as np

array = np.array([10, 20, 30, 40, 50])

print(array[1:4])
print(array[:3])
print(array[::2])
```

Salida:

```text
[20 30 40]
[10 20 30]
[10 30 50]
```

## Slicing en dos dimensiones

```python
import numpy as np

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

print(matrix[0:2, 1:3])
```

Salida:

```text
[[2 3]
 [5 6]]
```

## Filas y columnas

Extraer una fila:

```python
import numpy as np

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

print(matrix[1, :])
```

Salida:

```text
[4 5 6]
```

Extraer una columna:

```python
print(matrix[:, 1])
```

Salida:

```text
[2 5 8]
```

## Copias y vistas

En NumPy, algunas operaciones devuelven vistas sobre el mismo arreglo y otras devuelven copias.

Ejemplo de vista con slicing:

```python
import numpy as np

array = np.array([1, 2, 3, 4])
view = array[1:3]

view[0] = 99

print(array)
print(view)
```

Salida:

```text
[ 1 99  3  4]
[99  3]
```

La modificación de `view` afectó al arreglo original.

## Copia explícita

Para evitar compartir datos:

```python
import numpy as np

array = np.array([1, 2, 3, 4])
copy = array[1:3].copy()

copy[0] = 99

print(array)
print(copy)
```

Salida:

```text
[1 2 3 4]
[99  3]
```

## Operaciones vectorizadas

Las operaciones aritméticas se aplican elemento por elemento.

```python
import numpy as np

array = np.array([1, 2, 3])

print(array + 10)
print(array * 2)
print(array ** 2)
```

Salida:

```text
[11 12 13]
[2 4 6]
[1 4 9]
```

## Operaciones entre arreglos

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([10, 20, 30])

print(a + b)
print(a * b)
```

Salida:

```text
[11 22 33]
[10 40 90]
```

## Broadcasting

Broadcasting permite operar arreglos con formas distintas cuando son compatibles.

Ejemplo simple:

```python
import numpy as np

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

values = np.array([10, 20, 30])

print(matrix + values)
```

Salida:

```text
[[11 22 33]
 [14 25 36]]
```

El arreglo `values` se aplica a cada fila de la matriz.

## Error por formas incompatibles

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([10, 20])

print(a + b)
```

Esto genera un error porque las formas no son compatibles para broadcasting.

## Funciones universales

Las funciones universales, conocidas como `ufuncs`, aplican operaciones elemento por elemento.

Ejemplos:

```python
import numpy as np

array = np.array([1, 4, 9, 16])

print(np.sqrt(array))
print(np.exp(array))
print(np.log(array))
```

También existen funciones trigonométricas:

```python
import numpy as np

angles = np.array([0, np.pi / 2, np.pi])

print(np.sin(angles))
print(np.cos(angles))
```

## Agregaciones

## `sum()`

```python
import numpy as np

array = np.array([1, 2, 3, 4])

print(array.sum())
print(np.sum(array))
```

Salida:

```text
10
10
```

## `mean()`

```python
import numpy as np

array = np.array([10, 20, 30])

print(array.mean())
print(np.mean(array))
```

Salida:

```text
20.0
20.0
```

## `min()` y `max()`

```python
import numpy as np

array = np.array([10, 5, 30])

print(array.min())
print(array.max())
```

Salida:

```text
5
30
```

## `std()` y `var()`

```python
import numpy as np

array = np.array([10, 20, 30])

print(array.std())
print(array.var())
```

## Eje con `axis`

`axis` indica la dimensión sobre la que se aplica una operación.

```python
import numpy as np

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(matrix.sum(axis=0))
print(matrix.sum(axis=1))
```

Salida:

```text
[5 7 9]
[ 6 15]
```

Interpretación:

```text
axis=0 -> operación por columnas
axis=1 -> operación por filas
```

## Cambiar forma con `reshape()`

```python
import numpy as np

array = np.arange(6)

matrix = array.reshape(2, 3)

print(matrix)
```

Salida:

```text
[[0 1 2]
 [3 4 5]]
```

La cantidad total de elementos debe ser compatible.

## Aplanar arreglos

## `ravel()`

```python
import numpy as np

matrix = np.array([
    [1, 2],
    [3, 4]
])

print(matrix.ravel())
```

## `flatten()`

```python
print(matrix.flatten())
```

Diferencia práctica:

- `ravel()` intenta devolver una vista cuando puede
- `flatten()` devuelve una copia

## Transposición

```python
import numpy as np

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(matrix.T)
```

Salida:

```text
[[1 4]
 [2 5]
 [3 6]]
```

## Concatenación

## `np.concatenate()`

```python
import numpy as np

a = np.array([1, 2])
b = np.array([3, 4])

result = np.concatenate([a, b])

print(result)
```

Salida:

```text
[1 2 3 4]
```

## `np.vstack()`

Une arreglos verticalmente.

```python
import numpy as np

a = np.array([1, 2])
b = np.array([3, 4])

print(np.vstack([a, b]))
```

Salida:

```text
[[1 2]
 [3 4]]
```

## `np.hstack()`

Une arreglos horizontalmente.

```python
import numpy as np

a = np.array([1, 2])
b = np.array([3, 4])

print(np.hstack([a, b]))
```

Salida:

```text
[1 2 3 4]
```

## Comparaciones

Las comparaciones también son vectorizadas.

```python
import numpy as np

array = np.array([10, 20, 30])

print(array > 15)
```

Salida:

```text
[False  True  True]
```

## Máscaras booleanas

Una máscara booleana permite filtrar elementos.

```python
import numpy as np

array = np.array([10, 20, 30, 40])

mask = array > 20

print(mask)
print(array[mask])
```

Salida:

```text
[False False  True  True]
[30 40]
```

Forma directa:

```python
print(array[array > 20])
```

## Condiciones con `np.where()`

```python
import numpy as np

array = np.array([10, 20, 30, 40])

result = np.where(array >= 30, "alto", "bajo")

print(result)
```

Salida:

```text
['bajo' 'bajo' 'alto' 'alto']
```

## Valores faltantes con `np.nan`

`np.nan` representa un valor numérico faltante o no definido dentro del dominio de punto flotante.

```python
import numpy as np

array = np.array([1.0, np.nan, 3.0])

print(array)
```

## Detección de `nan`

```python
import numpy as np

array = np.array([1.0, np.nan, 3.0])

print(np.isnan(array))
```

Salida:

```text
[False  True False]
```

## Funciones que ignoran `nan`

```python
import numpy as np

array = np.array([1.0, np.nan, 3.0])

print(np.nanmean(array))
print(np.nansum(array))
```

## Números aleatorios

La forma moderna recomendada es usar un generador:

```python
import numpy as np

rng = np.random.default_rng()

values = rng.integers(1, 10, size=5)

print(values)
```

## Distribución normal

```python
import numpy as np

rng = np.random.default_rng()

samples = rng.normal(loc=0, scale=1, size=5)

print(samples)
```

## Semilla reproducible

```python
import numpy as np

rng = np.random.default_rng(seed=42)

values = rng.integers(1, 10, size=5)

print(values)
```

Usar una semilla permite reproducir resultados aleatorios.

## Álgebra lineal

NumPy incluye herramientas básicas de álgebra lineal mediante `np.linalg`.

## Producto matricial

```python
import numpy as np

a = np.array([
    [1, 2],
    [3, 4]
])

b = np.array([
    [10, 20],
    [30, 40]
])

print(a @ b)
```

También puede usarse:

```python
print(np.matmul(a, b))
```

## Determinante

```python
import numpy as np

a = np.array([
    [1, 2],
    [3, 4]
])

print(np.linalg.det(a))
```

## Inversa

```python
import numpy as np

a = np.array([
    [1, 2],
    [3, 4]
])

print(np.linalg.inv(a))
```

## Resolver sistemas lineales

```python
import numpy as np

a = np.array([
    [3, 1],
    [1, 2]
])

b = np.array([9, 8])

x = np.linalg.solve(a, b)

print(x)
```

## Guardar y cargar arreglos

## `np.save()`

Guarda un arreglo en formato binario de NumPy.

```python
import numpy as np

array = np.array([1, 2, 3])

np.save("array.npy", array)
```

## `np.load()`

Carga un arreglo guardado.

```python
import numpy as np

array = np.load("array.npy")

print(array)
```

## `np.savetxt()`

Guarda un arreglo en texto.

```python
import numpy as np

array = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

np.savetxt("array.csv", array, delimiter=",", fmt="%d")
```

## `np.loadtxt()`

Carga datos desde texto simple.

```python
import numpy as np

array = np.loadtxt("array.csv", delimiter=",")

print(array)
```

## Relación con listas de Python

Una lista de Python es flexible, pero no está especializada en cálculo numérico vectorizado.

```python
values = [1, 2, 3]
```

Un arreglo de NumPy está diseñado para operaciones numéricas eficientes y homogéneas.

```python
import numpy as np

array = np.array([1, 2, 3])
```

Diferencia importante:

```python
values = [1, 2, 3]
array = np.array([1, 2, 3])

print(values * 2)
print(array * 2)
```

Salida:

```text
[1, 2, 3, 1, 2, 3]
[2 4 6]
```

## Relación con `pandas`

`pandas` se apoya fuertemente en la idea de datos tabulares, pero NumPy aporta estructuras y operaciones numéricas fundamentales.

Ejemplo:

```python
import numpy as np
import pandas as pd

array = np.array([10, 20, 30])

series = pd.Series(array)

print(series)
```

## Relación con `matplotlib`

`matplotlib` puede graficar arreglos de NumPy directamente.

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.show()
```

## Casos de uso frecuentes

## Cálculo vectorizado

```python
import numpy as np

prices = np.array([100, 200, 300])
tax_rate = 0.18

taxes = prices * tax_rate

print(taxes)
```

## Normalización simple

```python
import numpy as np

values = np.array([10, 20, 30, 40])

normalized = (values - values.mean()) / values.std()

print(normalized)
```

## Filtrado numérico

```python
import numpy as np

values = np.array([5, 10, 15, 20])

filtered = values[values >= 10]

print(filtered)
```

## Simulación simple

```python
import numpy as np

rng = np.random.default_rng(seed=42)

returns = rng.normal(loc=0.01, scale=0.05, size=10)

print(returns)
```

## Errores comunes

## Confundir listas con arreglos

Problemático:

```python
values = [1, 2, 3]

print(values * 2)
```

Esto repite la lista, no multiplica cada elemento.

Para operación vectorizada:

```python
import numpy as np

values = np.array([1, 2, 3])

print(values * 2)
```

## No revisar `shape`

Muchos errores vienen de formas incompatibles.

```python
print(array.shape)
```

Antes de operar matrices, conviene revisar dimensiones.

## Usar `np.arange()` con decimales esperando exactitud perfecta

```python
import numpy as np

print(np.arange(0, 1, 0.1))
```

Para secuencias decimales con cantidad fija de puntos, suele ser más claro usar `np.linspace()`.

## Comparar arreglos con `==` esperando un único booleano

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([1, 0, 3])

print(a == b)
```

Salida:

```text
[ True False  True]
```

Para comprobar si todos los elementos son iguales:

```python
print(np.array_equal(a, b))
```

## Usar `and` u `or` con arreglos booleanos

Problemático:

```python
mask = (array > 10) and (array < 30)
```

Debe usarse `&` y paréntesis:

```python
mask = (array > 10) & (array < 30)
```

Para `or` elemento por elemento:

```python
mask = (array < 10) | (array > 30)
```

## Modificar una vista pensando que es copia

```python
view = array[1:3]
view[0] = 99
```

Puede modificar el arreglo original.

Cuando se necesite independencia:

```python
copy = array[1:3].copy()
```

## Ignorar `nan` en agregaciones

```python
import numpy as np

array = np.array([1.0, np.nan, 3.0])

print(array.mean())
```

El resultado será `nan`.

Si se quiere ignorar valores faltantes:

```python
print(np.nanmean(array))
```

## Buenas prácticas

## Usar el alias `np`

```python
import numpy as np
```

## Revisar `shape`, `ndim` y `dtype`

```python
print(array.shape)
print(array.ndim)
print(array.dtype)
```

## Preferir operaciones vectorizadas

Más conveniente:

```python
result = array * 2
```

Menos conveniente para arreglos grandes:

```python
result = [x * 2 for x in array]
```

## Usar `np.linspace()` para intervalos decimales controlados

```python
x = np.linspace(0, 1, 11)
```

## Usar `copy()` cuando se necesite independencia

```python
new_array = array.copy()
```

## Usar `np.random.default_rng()`

```python
rng = np.random.default_rng(seed=42)
```

## Validar dimensiones antes de álgebra lineal

```python
print(a.shape)
print(b.shape)
```

## Ejemplo integrado

```python
import numpy as np


def summarize_values(values):
    array = np.array(values, dtype=float)

    valid_values = array[~np.isnan(array)]

    summary = {
        "count": valid_values.size,
        "mean": valid_values.mean(),
        "minimum": valid_values.min(),
        "maximum": valid_values.max(),
        "std": valid_values.std()
    }

    return summary


values = [10, 20, np.nan, 30, 40]

summary = summarize_values(values)

print("Resumen numérico")
print("." * 30)

for key, value in summary.items():
    print(f"{key}: {value}")
```

Salida aproximada:

```text
Resumen numérico
..............................
count: 4
mean: 25.0
minimum: 10.0
maximum: 40.0
std: 11.180339887498949
```

## Relación con otras librerías

`numpy` se relaciona especialmente con:

- `pandas`, por análisis tabular y series de datos
- `matplotlib`, por visualización numérica
- `scipy`, por métodos científicos y matemáticos avanzados
- `scikit-learn`, por machine learning clásico
- `statsmodels`, por estadística y econometría
- `numba`, por aceleración de funciones numéricas
- `polars`, aunque su modelo principal no se basa en `ndarray`
- `torch` y `tensorflow`, por la relación conceptual con tensores y arreglos multidimensionales

## Orden didáctico interno

```text
1. Propósito de numpy
2. Instalación e importación
3. ndarray
4. shape, ndim, size y dtype
5. Creación de arreglos
6. Indexación y slicing
7. Copias y vistas
8. Operaciones vectorizadas
9. Broadcasting
10. Agregaciones y axis
11. Máscaras booleanas
12. Reshape, transposición y concatenación
13. Random y álgebra lineal
14. Guardado y carga de arreglos
15. Errores comunes
16. Buenas prácticas
```