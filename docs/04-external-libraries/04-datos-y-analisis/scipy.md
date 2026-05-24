# `scipy`

## Propósito

`scipy` es una librería externa para computación científica y técnica en Python. Se utiliza para resolver problemas matemáticos, estadísticos, numéricos y científicos que van más allá de las operaciones básicas de `numpy`.

Permite trabajar con optimización, integración numérica, álgebra lineal avanzada, estadística, interpolación, señales, matrices dispersas, distancias, funciones especiales y procesamiento científico en general.

## Naturaleza de la librería

`scipy` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install scipy
```

La importación general puede hacerse así:

```python
import scipy
```

Sin embargo, en la práctica suele importarse el submódulo necesario:

```python
from scipy import stats
from scipy import optimize
from scipy import integrate
from scipy import linalg
```

No existe un alias universal tan dominante como `np` para `numpy` o `pd` para `pandas`.

## Idea central

`scipy` se apoya en `numpy` y amplía sus capacidades.

Ejemplo simple:

```python
import numpy as np
from scipy import stats

values = np.array([10, 20, 30, 40, 50])

print(stats.zscore(values))
```

Salida aproximada:

```text
[-1.41421356 -0.70710678  0.          0.70710678  1.41421356]
```

## Instalación

Instalación básica:

```bash
python -m pip install scipy
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
scipy==1.x.x
```

La versión exacta puede variar según el entorno.

## Importación

Importación general:

```python
import scipy
```

Verificación:

```python
import scipy

print(scipy.__version__)
```

Importación por submódulo:

```python
from scipy import stats
from scipy import optimize
from scipy import integrate
from scipy import linalg
from scipy import interpolate
from scipy import sparse
from scipy import signal
from scipy import spatial
```

## Relación con NumPy

`scipy` está construido sobre `numpy`.

En general:

- `numpy` aporta arreglos, operaciones vectorizadas y álgebra básica
- `scipy` aporta algoritmos científicos y matemáticos más especializados

Ejemplo:

```python
import numpy as np
from scipy import linalg

matrix = np.array([
    [3, 1],
    [1, 2]
])

values = np.array([9, 8])

solution = linalg.solve(matrix, values)

print(solution)
```

## Submódulos principales

Los submódulos más importantes de `scipy` para uso general son:

```text
scipy.stats
scipy.optimize
scipy.integrate
scipy.interpolate
scipy.linalg
scipy.sparse
scipy.signal
scipy.spatial
scipy.io
scipy.special
```

## `scipy.stats`

## Propósito

`scipy.stats` contiene herramientas estadísticas y distribuciones de probabilidad.

Permite trabajar con:

- distribuciones continuas y discretas
- pruebas estadísticas
- estadísticos descriptivos
- correlaciones
- z-scores
- intervalos y probabilidades
- generación de muestras aleatorias desde distribuciones

## Z-score

```python
import numpy as np
from scipy import stats

values = np.array([10, 20, 30, 40, 50])

z_scores = stats.zscore(values)

print(z_scores)
```

## Distribución normal

```python
from scipy import stats

probability = stats.norm.cdf(1.96)

print(probability)
```

Esto calcula la probabilidad acumulada hasta `1.96` en una distribución normal estándar.

## Valor crítico

```python
from scipy import stats

critical_value = stats.norm.ppf(0.975)

print(critical_value)
```

`ppf()` devuelve el cuantil asociado a una probabilidad acumulada.

## Densidad de probabilidad

```python
from scipy import stats

density = stats.norm.pdf(0)

print(density)
```

## Prueba t de una muestra

```python
import numpy as np
from scipy import stats

sample = np.array([10, 12, 9, 11, 13, 10])

result = stats.ttest_1samp(sample, popmean=10)

print(result.statistic)
print(result.pvalue)
```

## Prueba t de dos muestras

```python
import numpy as np
from scipy import stats

sample_a = np.array([10, 12, 11, 13, 12])
sample_b = np.array([8, 9, 10, 9, 11])

result = stats.ttest_ind(sample_a, sample_b)

print(result.statistic)
print(result.pvalue)
```

## Correlación de Pearson

```python
import numpy as np
from scipy import stats

x = np.array([1, 2, 3, 4, 5])
y = np.array([2, 4, 5, 4, 6])

result = stats.pearsonr(x, y)

print(result.statistic)
print(result.pvalue)
```

## Correlación de Spearman

```python
from scipy import stats

result = stats.spearmanr(x, y)

print(result.statistic)
print(result.pvalue)
```

## `scipy.optimize`

## Propósito

`scipy.optimize` contiene herramientas para optimización numérica y búsqueda de raíces.

Permite resolver problemas como:

- minimizar funciones
- maximizar funciones mediante transformación
- encontrar raíces
- ajustar parámetros
- resolver sistemas no lineales
- optimizar con restricciones

## Minimización simple

```python
from scipy import optimize


def objective(x):
    return (x - 3) ** 2


result = optimize.minimize(objective, x0=0)

print(result.x)
print(result.fun)
print(result.success)
```

## Minimización con varias variables

```python
import numpy as np
from scipy import optimize


def objective(values):
    x, y = values
    return (x - 2) ** 2 + (y + 1) ** 2


result = optimize.minimize(
    objective,
    x0=np.array([0, 0])
)

print(result.x)
print(result.fun)
```

## Búsqueda de raíz

```python
from scipy import optimize


def function(x):
    return x ** 2 - 4


root = optimize.root_scalar(
    function,
    bracket=[0, 5]
)

print(root.root)
```

## Resolver sistema no lineal

```python
import numpy as np
from scipy import optimize


def system(values):
    x, y = values

    return [
        x + y - 3,
        x ** 2 + y ** 2 - 5
    ]


result = optimize.root(
    system,
    x0=np.array([1, 1])
)

print(result.x)
print(result.success)
```

## Ajuste de curva

```python
import numpy as np
from scipy import optimize


def model(x, a, b):
    return a * x + b


x_data = np.array([1, 2, 3, 4, 5])
y_data = np.array([2.1, 4.1, 6.0, 8.2, 10.1])

params, covariance = optimize.curve_fit(
    model,
    x_data,
    y_data
)

print(params)
```

## `scipy.integrate`

## Propósito

`scipy.integrate` contiene herramientas para integración numérica y resolución de ecuaciones diferenciales.

Permite trabajar con:

- integrales definidas
- integración múltiple
- ecuaciones diferenciales ordinarias
- sistemas dinámicos

## Integral definida con `quad()`

```python
from scipy import integrate


def function(x):
    return x ** 2


result, error = integrate.quad(function, 0, 1)

print(result)
print(error)
```

La integral de `x ** 2` entre `0` y `1` es aproximadamente `1/3`.

## Integral de una función trigonométrica

```python
import numpy as np
from scipy import integrate


def function(x):
    return np.sin(x)


result, error = integrate.quad(function, 0, np.pi)

print(result)
```

Salida aproximada:

```text
2.0
```

## Resolver ecuación diferencial

```python
import numpy as np
from scipy import integrate


def model(t, y):
    return -0.5 * y


time = np.linspace(0, 10, 100)

solution = integrate.solve_ivp(
    model,
    t_span=(0, 10),
    y0=[10],
    t_eval=time
)

print(solution.y)
```

## `scipy.interpolate`

## Propósito

`scipy.interpolate` permite construir funciones interpoladas a partir de datos conocidos.

Se utiliza cuando se tienen puntos observados y se necesita estimar valores intermedios.

## Interpolación lineal

```python
import numpy as np
from scipy import interpolate

x = np.array([0, 1, 2, 3])
y = np.array([0, 2, 4, 6])

function = interpolate.interp1d(x, y)

print(function(1.5))
```

Salida:

```text
3.0
```

## Interpolación cúbica

```python
function = interpolate.interp1d(
    x,
    y,
    kind="cubic"
)
```

Este tipo de interpolación requiere suficientes puntos y debe usarse con cuidado para evitar resultados artificiales.

## `scipy.linalg`

## Propósito

`scipy.linalg` contiene herramientas de álgebra lineal.

Aunque `numpy.linalg` cubre operaciones básicas, `scipy.linalg` suele ofrecer más funciones y algoritmos especializados.

## Resolver sistema lineal

```python
import numpy as np
from scipy import linalg

a = np.array([
    [3, 1],
    [1, 2]
])

b = np.array([9, 8])

x = linalg.solve(a, b)

print(x)
```

## Determinante

```python
import numpy as np
from scipy import linalg

a = np.array([
    [1, 2],
    [3, 4]
])

print(linalg.det(a))
```

## Inversa

```python
print(linalg.inv(a))
```

## Valores propios

```python
eigenvalues, eigenvectors = linalg.eig(a)

print(eigenvalues)
print(eigenvectors)
```

## Descomposición LU

```python
import numpy as np
from scipy import linalg

a = np.array([
    [4, 3],
    [6, 3]
])

p, l, u = linalg.lu(a)

print(p)
print(l)
print(u)
```

## `scipy.sparse`

## Propósito

`scipy.sparse` permite trabajar con matrices dispersas.

Una matriz dispersa es una matriz con muchos ceros. Guardarla como matriz completa puede desperdiciar memoria.

## Crear matriz dispersa

```python
import numpy as np
from scipy import sparse

matrix = np.array([
    [1, 0, 0],
    [0, 0, 2],
    [0, 3, 0]
])

sparse_matrix = sparse.csr_matrix(matrix)

print(sparse_matrix)
```

## Convertir a matriz densa

```python
dense = sparse_matrix.toarray()

print(dense)
```

## Uso típico

Las matrices dispersas son útiles en:

- machine learning
- sistemas lineales grandes
- grafos
- matrices de texto
- modelos con muchas variables y muchos ceros

## `scipy.signal`

## Propósito

`scipy.signal` contiene herramientas para procesamiento de señales.

Permite trabajar con:

- filtros
- convolución
- correlación
- detección de picos
- ventanas
- sistemas discretos y continuos

## Detección de picos

```python
import numpy as np
from scipy import signal

values = np.array([0, 1, 0, 2, 0, 3, 0])

peaks, properties = signal.find_peaks(values)

print(peaks)
```

Salida:

```text
[1 3 5]
```

## Convolución

```python
import numpy as np
from scipy import signal

a = np.array([1, 2, 3])
b = np.array([0, 1, 0.5])

result = signal.convolve(a, b)

print(result)
```

## `scipy.spatial`

## Propósito

`scipy.spatial` contiene herramientas para trabajar con distancias, geometría espacial y estructuras como árboles KD.

Se usa en:

- cálculo de distancias
- vecinos más cercanos
- agrupamiento
- geometría computacional
- análisis espacial

## Distancia euclidiana

```python
import numpy as np
from scipy.spatial import distance

a = np.array([0, 0])
b = np.array([3, 4])

result = distance.euclidean(a, b)

print(result)
```

Salida:

```text
5.0
```

## Matriz de distancias

```python
import numpy as np
from scipy.spatial import distance

points = np.array([
    [0, 0],
    [3, 4],
    [6, 8]
])

distances = distance.cdist(points, points)

print(distances)
```

## `scipy.io`

## Propósito

`scipy.io` permite leer y escribir algunos formatos científicos.

Uno de sus usos frecuentes es trabajar con archivos `.mat` de MATLAB.

## Leer archivo `.mat`

```python
from scipy import io

data = io.loadmat("data.mat")

print(data.keys())
```

## Guardar archivo `.mat`

```python
from scipy import io

data = {
    "values": [1, 2, 3]
}

io.savemat("output.mat", data)
```

## `scipy.special`

## Propósito

`scipy.special` contiene funciones matemáticas especiales.

Incluye funciones como:

- gamma
- beta
- error function
- funciones de Bessel
- combinatorias especiales
- funciones usadas en estadística, física e ingeniería

## Función gamma

```python
from scipy import special

print(special.gamma(5))
```

Salida:

```text
24.0
```

## Combinaciones

```python
from scipy import special

print(special.comb(5, 2))
```

Salida:

```text
10.0
```

## Relación con pandas

`scipy` puede complementar análisis hechos en `pandas`.

Ejemplo:

```python
import pandas as pd
from scipy import stats

df = pd.DataFrame({
    "returns": [0.01, 0.02, -0.01, 0.03, 0.00]
})

z_scores = stats.zscore(df["returns"])

df["z_score"] = z_scores

print(df)
```

## Relación con matplotlib

`scipy` suele generar resultados que luego se visualizan con `matplotlib`.

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

x = np.linspace(-4, 4, 100)
y = stats.norm.pdf(x)

plt.plot(x, y)
plt.show()
```

## Relación con scikit-learn

`scipy` es importante en machine learning porque muchas herramientas trabajan con matrices, distancias, optimización y matrices dispersas.

Ejemplo conceptual:

```text
scipy.sparse -> matrices dispersas para modelos de texto
scipy.optimize -> optimización numérica
scipy.spatial -> distancias y vecinos
```

## Casos de uso frecuentes

## Estadística inferencial

```python
from scipy import stats

result = stats.ttest_1samp(sample, popmean=0)

print(result.pvalue)
```

## Optimización de funciones

```python
from scipy import optimize

result = optimize.minimize(objective, x0=0)

print(result.x)
```

## Integración numérica

```python
from scipy import integrate

result, error = integrate.quad(function, 0, 1)
```

## Interpolación

```python
from scipy import interpolate

function = interpolate.interp1d(x, y)
```

## Álgebra lineal avanzada

```python
from scipy import linalg

solution = linalg.solve(a, b)
```

## Errores comunes

## Usar SciPy antes de entender NumPy

`scipy` trabaja principalmente con arreglos y estructuras numéricas. Conviene entender primero:

```text
np.array
shape
dtype
axis
broadcasting
máscaras booleanas
```

## Importar todo SciPy sin necesidad

Menos claro:

```python
import scipy
```

Más específico:

```python
from scipy import stats
from scipy import optimize
```

## Confundir `numpy.linalg` y `scipy.linalg`

Ambos pueden resolver problemas de álgebra lineal, pero `scipy.linalg` suele ofrecer más funciones especializadas.

## No revisar el resultado de optimización

Problemático:

```python
result = optimize.minimize(objective, x0=0)
print(result.x)
```

Más seguro:

```python
if result.success:
    print(result.x)
else:
    print(result.message)
```

## No validar supuestos estadísticos

Una prueba estadística no debe usarse mecánicamente. Antes de interpretar resultados conviene revisar:

- tipo de variable
- tamaño de muestra
- independencia
- distribución aproximada
- supuestos de la prueba
- contexto del problema

## Ignorar errores numéricos

Métodos como integración, optimización o ajuste pueden devolver resultados aproximados. Conviene revisar errores, convergencia y mensajes.

## Usar interpolación para extrapolar sin control

La interpolación estima valores dentro del rango conocido. Usarla fuera del rango puede generar resultados poco confiables.

## Buenas prácticas

## Importar solo el submódulo necesario

```python
from scipy import stats
from scipy import optimize
```

## Trabajar con arreglos NumPy

```python
values = np.array(values, dtype=float)
```

## Revisar resultados y mensajes

```python
print(result.success)
print(result.message)
```

## Validar dimensiones

```python
print(matrix.shape)
```

## Separar funciones matemáticas del flujo principal

```python
def objective(x):
    return (x - 3) ** 2
```

## Documentar supuestos estadísticos

Los resultados estadísticos deben interpretarse según el contexto y los supuestos de la prueba.

## Usar visualización para revisar resultados

```python
plt.plot(x, y)
```

## Ejemplo integrado

```python
import numpy as np
from scipy import optimize, stats


def calculate_z_scores(values):
    array = np.array(values, dtype=float)

    return stats.zscore(array)


def objective(x):
    return (x - 5) ** 2 + 10


def find_minimum():
    result = optimize.minimize(objective, x0=0)

    if not result.success:
        raise RuntimeError(result.message)

    return result.x[0], result.fun


values = [10, 20, 30, 40, 50]

z_scores = calculate_z_scores(values)
minimum_x, minimum_value = find_minimum()

print("Resultados SciPy")
print("." * 30)
print("Z-scores:", z_scores)
print("Punto mínimo:", minimum_x)
print("Valor mínimo:", minimum_value)
```

Salida aproximada:

```text
Resultados SciPy
..............................
Z-scores: [-1.41421356 -0.70710678  0.          0.70710678  1.41421356]
Punto mínimo: 4.99999999
Valor mínimo: 10.0
```

## Relación con otras librerías

`scipy` se relaciona especialmente con:

- `numpy`, porque se apoya en arreglos y cálculo vectorizado
- `pandas`, para análisis de datos estructurados
- `matplotlib`, para visualizar funciones, distribuciones y resultados
- `statsmodels`, para estadística y econometría más especializada
- `scikit-learn`, para machine learning clásico
- `networkx`, cuando se combinan distancias, grafos y matrices
- `sympy`, como alternativa simbólica frente al enfoque numérico de SciPy

## Orden didáctico interno

```text
1. Propósito de scipy
2. Instalación e importación
3. Relación con numpy
4. Submódulos principales
5. scipy.stats
6. scipy.optimize
7. scipy.integrate
8. scipy.interpolate
9. scipy.linalg
10. scipy.sparse
11. scipy.signal
12. scipy.spatial
13. scipy.io y scipy.special
14. Errores comunes
15. Buenas prácticas
```