# `seaborn`

## Propósito

`seaborn` es una librería externa para visualización estadística en Python. Se utiliza para crear gráficos exploratorios, comparar distribuciones, analizar relaciones entre variables, visualizar categorías, construir mapas de calor y generar gráficos estadísticos con una sintaxis más declarativa que `matplotlib`.

Está construida sobre `matplotlib` y se integra muy bien con `pandas`.

## Naturaleza de la librería

`seaborn` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install seaborn
```

La importación habitual es:

```python
import seaborn as sns
```

También suele combinarse con:

```python
import matplotlib.pyplot as plt
import pandas as pd
```

## Idea central

La idea principal de `seaborn` es crear gráficos estadísticos a partir de datasets completos, especialmente `DataFrame` de `pandas`.

Ejemplo básico:

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "product": ["A", "B", "C"],
    "amount": [100, 200, 150]
})

sns.barplot(
    data=df,
    x="product",
    y="amount"
)

plt.show()
```

A diferencia de `matplotlib`, donde normalmente se pasan listas o arreglos directamente, en `seaborn` se suele pasar el `DataFrame` completo y luego indicar los nombres de las columnas.

## Instalación

Instalación básica:

```bash
python -m pip install seaborn
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
seaborn==0.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
import seaborn as sns
```

Con `matplotlib`:

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

Verificación:

```python
import seaborn as sns

print(sns.__version__)
```

## Relación con matplotlib

`seaborn` no reemplaza completamente a `matplotlib`.

En la práctica:

```text
seaborn    -> construye gráficos estadísticos de alto nivel
matplotlib -> controla la figura, los ejes, el guardado y ajustes finos
```

Ejemplo:

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.scatterplot(
    data=df,
    x="price",
    y="sales"
)

plt.title("Precio vs ventas")
plt.xlabel("Precio")
plt.ylabel("Ventas")

plt.show()
```

También puede usarse el enfoque orientado a objetos de `matplotlib`:

```python
fig, ax = plt.subplots()

sns.scatterplot(
    data=df,
    x="price",
    y="sales",
    ax=ax
)

ax.set_title("Precio vs ventas")
ax.set_xlabel("Precio")
ax.set_ylabel("Ventas")

plt.show()
```

## Datos en formato largo

`seaborn` funciona especialmente bien con datos en formato largo.

Formato largo:

```text
fecha       producto   monto
2026-01-01  A          100
2026-01-01  B          150
2026-02-01  A          120
2026-02-01  B          160
```

Esto permite indicar columnas semánticas:

```python
sns.lineplot(
    data=df,
    x="fecha",
    y="monto",
    hue="producto"
)
```

## Datos en formato ancho

Formato ancho:

```text
fecha       producto_a   producto_b
2026-01-01  100          150
2026-02-01  120          160
```

Se puede graficar, pero para muchos gráficos de `seaborn` suele ser más flexible convertir a formato largo con `pandas.melt()`.

```python
long_df = df.melt(
    id_vars="fecha",
    var_name="producto",
    value_name="monto"
)
```

## Tema visual

## `set_theme()`

`set_theme()` aplica una configuración visual general.

```python
import seaborn as sns

sns.set_theme()
```

Con estilo específico:

```python
sns.set_theme(style="whitegrid")
```

Estilos frecuentes:

```text
darkgrid
whitegrid
dark
white
ticks
```

## Gráfico de dispersión

## `scatterplot()`

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "price": [10, 20, 30, 40, 50],
    "sales": [100, 90, 75, 60, 45],
    "category": ["A", "A", "B", "B", "B"]
})

fig, ax = plt.subplots()

sns.scatterplot(
    data=df,
    x="price",
    y="sales",
    hue="category",
    ax=ax
)

ax.set_title("Precio vs ventas")

plt.show()
```

## Uso de `hue`

`hue` separa los datos por color según una variable.

```python
sns.scatterplot(
    data=df,
    x="price",
    y="sales",
    hue="category"
)
```

## Uso de `style`

`style` separa los datos por forma de marcador.

```python
sns.scatterplot(
    data=df,
    x="price",
    y="sales",
    hue="category",
    style="category"
)
```

## Uso de `size`

`size` representa una variable mediante el tamaño del marcador.

```python
sns.scatterplot(
    data=df,
    x="price",
    y="sales",
    size="sales"
)
```

## Gráfico de líneas

## `lineplot()`

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "month": ["Ene", "Feb", "Mar", "Abr", "Ene", "Feb", "Mar", "Abr"],
    "sales": [100, 120, 115, 140, 90, 100, 105, 130],
    "product": ["A", "A", "A", "A", "B", "B", "B", "B"]
})

fig, ax = plt.subplots()

sns.lineplot(
    data=df,
    x="month",
    y="sales",
    hue="product",
    marker="o",
    ax=ax
)

ax.set_title("Ventas por mes")

plt.show()
```

## Gráfico relacional de alto nivel

## `relplot()`

`relplot()` es una función de nivel de figura para visualizar relaciones entre variables.

Puede usar:

```text
kind="scatter"
kind="line"
```

Ejemplo:

```python
sns.relplot(
    data=df,
    x="price",
    y="sales",
    hue="category",
    kind="scatter"
)

plt.show()
```

Con facetas:

```python
sns.relplot(
    data=df,
    x="price",
    y="sales",
    hue="category",
    col="category",
    kind="scatter"
)

plt.show()
```

## Distribuciones

## `histplot()`

```python
import seaborn as sns
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

sns.histplot(
    data=df,
    x="amount",
    bins=20,
    ax=ax
)

ax.set_title("Distribución de montos")

plt.show()
```

## Histograma con densidad

```python
sns.histplot(
    data=df,
    x="amount",
    kde=True
)
```

## `kdeplot()`

```python
sns.kdeplot(
    data=df,
    x="amount"
)

plt.show()
```

El KDE muestra una estimación suavizada de la distribución.

## `ecdfplot()`

```python
sns.ecdfplot(
    data=df,
    x="amount"
)

plt.show()
```

La ECDF muestra la proporción acumulada de observaciones.

## Gráfico de distribución de alto nivel

## `displot()`

`displot()` es una función de nivel de figura para visualizar distribuciones.

```python
sns.displot(
    data=df,
    x="amount",
    kind="hist"
)

plt.show()
```

Con KDE:

```python
sns.displot(
    data=df,
    x="amount",
    kind="kde"
)

plt.show()
```

Con facetas:

```python
sns.displot(
    data=df,
    x="amount",
    col="category",
    kind="hist"
)

plt.show()
```

## Gráficos categóricos

## `barplot()`

`barplot()` muestra una estimación agregada para una variable numérica por categoría.

```python
import seaborn as sns
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

sns.barplot(
    data=df,
    x="category",
    y="amount",
    ax=ax
)

ax.set_title("Monto promedio por categoría")

plt.show()
```

Por defecto, `barplot()` resume los datos. No debe confundirse con una barra de valores ya agregados si el dataset tiene múltiples filas por categoría.

## `countplot()`

`countplot()` muestra conteos por categoría.

```python
sns.countplot(
    data=df,
    x="category"
)

plt.show()
```

## `boxplot()`

`boxplot()` muestra la distribución de una variable numérica por categoría.

```python
sns.boxplot(
    data=df,
    x="category",
    y="amount"
)

plt.show()
```

## `violinplot()`

`violinplot()` muestra la forma de la distribución por categoría.

```python
sns.violinplot(
    data=df,
    x="category",
    y="amount"
)

plt.show()
```

## `stripplot()`

`stripplot()` muestra puntos individuales por categoría.

```python
sns.stripplot(
    data=df,
    x="category",
    y="amount"
)

plt.show()
```

## `swarmplot()`

`swarmplot()` muestra puntos individuales intentando reducir solapamientos.

```python
sns.swarmplot(
    data=df,
    x="category",
    y="amount"
)

plt.show()
```

Puede ser pesado si hay demasiados puntos.

## Gráfico categórico de alto nivel

## `catplot()`

`catplot()` permite crear gráficos categóricos de nivel de figura.

```python
sns.catplot(
    data=df,
    x="category",
    y="amount",
    kind="box"
)

plt.show()
```

Tipos frecuentes:

```text
strip
swarm
box
violin
boxen
point
bar
count
```

Con facetas:

```python
sns.catplot(
    data=df,
    x="category",
    y="amount",
    col="region",
    kind="box"
)

plt.show()
```

## Mapas de calor

## `heatmap()`

`heatmap()` visualiza una matriz mediante colores.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

matrix = pd.DataFrame({
    "A": [1.0, 0.5, 0.2],
    "B": [0.5, 1.0, 0.7],
    "C": [0.2, 0.7, 1.0]
}, index=["A", "B", "C"])

fig, ax = plt.subplots()

sns.heatmap(
    matrix,
    annot=True,
    ax=ax
)

ax.set_title("Mapa de calor")

plt.show()
```

## Matriz de correlación

```python
correlation = df[["price", "sales", "amount"]].corr()

sns.heatmap(
    correlation,
    annot=True
)

plt.show()
```

## Pairplot

## `pairplot()`

`pairplot()` permite visualizar relaciones por pares entre variables numéricas.

```python
sns.pairplot(
    data=df,
    vars=["price", "sales", "amount"],
    hue="category"
)

plt.show()
```

Es útil para exploración inicial, pero puede volverse pesado cuando hay muchas variables o muchas filas.

## Regresión visual

## `regplot()`

```python
sns.regplot(
    data=df,
    x="price",
    y="sales"
)

plt.show()
```

`regplot()` muestra una nube de puntos y una línea de ajuste.

## `lmplot()`

`lmplot()` es una función de nivel de figura para regresiones visuales con facetas.

```python
sns.lmplot(
    data=df,
    x="price",
    y="sales",
    hue="category"
)

plt.show()
```

## Facetas

Las facetas permiten dividir un gráfico según categorías.

Ejemplo con `relplot()`:

```python
sns.relplot(
    data=df,
    x="price",
    y="sales",
    col="category",
    kind="scatter"
)

plt.show()
```

Ejemplo con filas y columnas:

```python
sns.relplot(
    data=df,
    x="price",
    y="sales",
    row="region",
    col="category",
    kind="scatter"
)

plt.show()
```

## `FacetGrid`

`FacetGrid` permite mayor control sobre gráficos facetados.

```python
grid = sns.FacetGrid(
    data=df,
    col="category"
)

grid.map_dataframe(
    sns.scatterplot,
    x="price",
    y="sales"
)

plt.show()
```

En muchos casos, primero conviene usar `relplot()`, `displot()` o `catplot()`, que internamente ya trabajan con grids.

## Paletas de color

## Paleta en un gráfico

```python
sns.scatterplot(
    data=df,
    x="price",
    y="sales",
    hue="category",
    palette="deep"
)
```

Paletas frecuentes:

```text
deep
muted
pastel
bright
dark
colorblind
viridis
magma
coolwarm
```

## Definir paleta global

```python
sns.set_theme(
    style="whitegrid",
    palette="muted"
)
```

## Elegir colores según el tipo de dato

Regla práctica:

```text
categorías sin orden   -> paleta cualitativa
valores ordenados      -> paleta secuencial
valores con centro     -> paleta divergente
```

## Control de estilo

## `set_style()`

```python
sns.set_style("whitegrid")
```

## `set_context()`

Controla escala de textos y elementos.

```python
sns.set_context("notebook")
```

Opciones frecuentes:

```text
paper
notebook
talk
poster
```

## Uso con `matplotlib` orientado a objetos

Patrón recomendado:

```python
fig, ax = plt.subplots(figsize=(8, 5))

sns.boxplot(
    data=df,
    x="category",
    y="amount",
    ax=ax
)

ax.set_title("Distribución de montos")
ax.set_xlabel("Categoría")
ax.set_ylabel("Monto")

fig.tight_layout()

plt.show()
```

## Guardar gráficos

Para funciones de nivel de ejes:

```python
fig, ax = plt.subplots()

sns.scatterplot(
    data=df,
    x="price",
    y="sales",
    ax=ax
)

fig.savefig(
    "grafico.png",
    dpi=300,
    bbox_inches="tight"
)

plt.close(fig)
```

Para funciones de nivel de figura, como `relplot()`, `displot()` o `catplot()`:

```python
grid = sns.relplot(
    data=df,
    x="price",
    y="sales",
    hue="category"
)

grid.figure.savefig(
    "grafico.png",
    dpi=300,
    bbox_inches="tight"
)

plt.close(grid.figure)
```

## Uso con pandas

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.read_excel("ventas.xlsx")

summary = (
    df.groupby("Producto")
    .agg(Monto=("Monto", "sum"))
    .reset_index()
)

fig, ax = plt.subplots()

sns.barplot(
    data=summary,
    x="Producto",
    y="Monto",
    ax=ax
)

ax.set_title("Monto por producto")
ax.tick_params(axis="x", rotation=45)

fig.tight_layout()

plt.show()
```

## Uso con NumPy

Aunque `seaborn` suele usarse con `DataFrame`, también puede trabajar con arreglos.

```python
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

values = np.random.default_rng(seed=42).normal(size=100)

sns.histplot(values)

plt.show()
```

## Uso para análisis exploratorio

`seaborn` es especialmente útil para exploración inicial de datos.

Ejemplos:

```python
sns.histplot(data=df, x="amount")
sns.boxplot(data=df, x="category", y="amount")
sns.scatterplot(data=df, x="price", y="sales", hue="category")
sns.heatmap(df.corr(numeric_only=True), annot=True)
```

## Casos de uso frecuentes

## Comparar distribuciones

```python
sns.boxplot(
    data=df,
    x="category",
    y="amount"
)
```

## Ver relación entre variables

```python
sns.scatterplot(
    data=df,
    x="price",
    y="sales",
    hue="category"
)
```

## Graficar evolución temporal

```python
sns.lineplot(
    data=df,
    x="date",
    y="value",
    hue="series"
)
```

## Analizar correlaciones

```python
sns.heatmap(
    df.corr(numeric_only=True),
    annot=True
)
```

## Ver múltiples relaciones

```python
sns.pairplot(
    data=df,
    hue="category"
)
```

## Errores comunes

## Usar `seaborn` sin entender `matplotlib`

Aunque `seaborn` simplifica la creación de gráficos, muchos ajustes finales se hacen con `matplotlib`.

Conviene entender:

```text
Figure
Axes
set_title()
set_xlabel()
set_ylabel()
savefig()
```

## Confundir `barplot()` con suma total

`barplot()` suele mostrar una estimación agregada, como la media, si hay varias observaciones por categoría.

Si se necesita suma total, primero debe agregarse el DataFrame:

```python
summary = (
    df.groupby("category")
    .agg(total_amount=("amount", "sum"))
    .reset_index()
)

sns.barplot(
    data=summary,
    x="category",
    y="total_amount"
)
```

## No usar formato largo

Muchos gráficos de `seaborn` son más claros si los datos están en formato largo.

Puede usarse:

```python
df.melt()
```

## No cerrar figuras al generar muchas imágenes

```python
plt.close(fig)
```

o:

```python
plt.close(grid.figure)
```

## Usar demasiadas categorías en `hue`

Demasiados niveles de color vuelven el gráfico difícil de interpretar.

## Usar `pairplot()` con demasiadas columnas

`pairplot()` puede generar demasiados gráficos y volverse lento.

Conviene seleccionar variables relevantes:

```python
sns.pairplot(
    data=df,
    vars=["x1", "x2", "x3"]
)
```

## No revisar datos antes de graficar

Conviene revisar:

```python
df.head()
df.dtypes
df.isna().sum()
```

antes de construir gráficos.

## Buenas prácticas

## Usar `sns.set_theme()`

```python
sns.set_theme(style="whitegrid")
```

## Usar `DataFrame` y nombres de columnas

```python
sns.scatterplot(
    data=df,
    x="price",
    y="sales"
)
```

## Usar `fig, ax` cuando se necesita control

```python
fig, ax = plt.subplots()
sns.scatterplot(data=df, x="price", y="sales", ax=ax)
```

## Agregar títulos y etiquetas

```python
ax.set_title("Título")
ax.set_xlabel("Eje X")
ax.set_ylabel("Eje Y")
```

## Agregar datos antes de graficar si se necesita total

```python
summary = df.groupby("category").sum(numeric_only=True).reset_index()
```

## Guardar gráficos con buena resolución

```python
fig.savefig(
    "grafico.png",
    dpi=300,
    bbox_inches="tight"
)
```

## Usar el tipo de gráfico según el objetivo

```text
scatterplot -> relación entre dos variables numéricas
lineplot    -> evolución o secuencia
histplot    -> distribución
boxplot     -> distribución por categoría
barplot     -> comparación agregada
heatmap     -> matriz o correlaciones
pairplot    -> exploración multivariable
```

## Ejemplo integrado

```python
from pathlib import Path

import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt


def load_sales(input_path):
    return pd.read_excel(input_path)


def prepare_summary(df):
    summary = (
        df.groupby("Producto")
        .agg(Monto=("Monto", "sum"))
        .reset_index()
        .sort_values("Monto", ascending=False)
    )

    return summary


def create_bar_chart(summary, output_path):
    sns.set_theme(style="whitegrid")

    fig, ax = plt.subplots(figsize=(8, 5))

    sns.barplot(
        data=summary,
        x="Producto",
        y="Monto",
        ax=ax
    )

    ax.set_title("Monto total por producto")
    ax.set_xlabel("Producto")
    ax.set_ylabel("Monto")
    ax.tick_params(axis="x", rotation=45)

    fig.tight_layout()

    fig.savefig(
        output_path,
        dpi=300,
        bbox_inches="tight"
    )

    plt.close(fig)


input_path = Path("ventas.xlsx")
output_path = Path("grafico_ventas_seaborn.png")

sales = load_sales(input_path)
summary = prepare_summary(sales)

create_bar_chart(summary, output_path)

print("Gráfico generado correctamente")
```

## Relación con otras librerías

`seaborn` se relaciona especialmente con:

- `matplotlib`, porque se construye sobre su sistema de figuras y ejes
- `pandas`, porque trabaja muy bien con DataFrames
- `numpy`, porque puede graficar arreglos y datos numéricos
- `scipy`, por análisis estadístico complementario
- `statsmodels`, cuando se analizan modelos y relaciones estadísticas
- `plotly`, como alternativa para gráficos interactivos
- `python-docx`, para insertar gráficos exportados en Word
- `python-pptx`, para insertar gráficos exportados en PowerPoint
- `openpyxl` y `xlsxwriter`, para reportes Excel con imágenes de gráficos

## Orden didáctico interno

```text
1. Propósito de seaborn
2. Instalación e importación
3. Relación con matplotlib
4. Datos en formato largo y ancho
5. Tema visual
6. Gráficos relacionales: scatterplot, lineplot y relplot
7. Distribuciones: histplot, kdeplot, ecdfplot y displot
8. Gráficos categóricos: barplot, countplot, boxplot, violinplot y catplot
9. Heatmap y correlaciones
10. Pairplot, regplot y lmplot
11. Facetas
12. Paletas y estilos
13. Guardado de gráficos
14. Errores comunes
15. Buenas prácticas
```