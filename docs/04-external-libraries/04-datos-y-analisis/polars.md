# `polars`

## Propósito

`polars` es una librería externa para manipulación y análisis de datos tabulares. Se utiliza para leer, transformar, filtrar, agregar, combinar y exportar datos de forma eficiente, especialmente cuando se trabaja con archivos grandes, consultas encadenadas o transformaciones que pueden beneficiarse de optimización interna.

Es una alternativa moderna a `pandas` en muchos flujos de análisis tabular, con énfasis en rendimiento, expresiones, evaluación lazy y procesamiento paralelo.

## Naturaleza de la librería

`polars` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install polars
```

La importación habitual es:

```python
import polars as pl
```

El alias `pl` es la convención más frecuente.

## Idea central

La idea principal de Polars es trabajar con tablas usando expresiones.

Ejemplo mínimo:

```python
import polars as pl

df = pl.DataFrame({
    "product": ["A", "B", "C"],
    "price": [100, 200, 300]
})

result = df.with_columns(
    (pl.col("price") * 1.18).alias("price_with_tax")
)

print(result)
```

Salida aproximada:

```text
shape: (3, 3)
┌─────────┬───────┬────────────────┐
│ product ┆ price ┆ price_with_tax │
│ ---     ┆ ---   ┆ ---            │
│ str     ┆ i64   ┆ f64            │
╞═════════╪═══════╪════════════════╡
│ A       ┆ 100   ┆ 118.0          │
│ B       ┆ 200   ┆ 236.0          │
│ C       ┆ 300   ┆ 354.0          │
└─────────┴───────┴────────────────┘
```

## Instalación

Instalación básica:

```bash
python -m pip install polars
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
polars==1.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
import polars as pl
```

Verificación:

```python
import polars as pl

print(pl.__version__)
```

## Estructuras principales

Polars trabaja principalmente con:

```text
DataFrame
Series
LazyFrame
Expression
```

## `DataFrame`

Un `DataFrame` representa una tabla con filas y columnas.

```python
import polars as pl

df = pl.DataFrame({
    "name": ["Ana", "Luis", "Marta"],
    "age": [20, 25, 22],
    "city": ["Lima", "Cusco", "Arequipa"]
})

print(df)
```

## `Series`

Una `Series` representa una columna individual.

```python
import polars as pl

series = pl.Series("price", [100, 200, 300])

print(series)
```

## `LazyFrame`

Un `LazyFrame` representa una consulta diferida. No ejecuta inmediatamente todas las operaciones, sino que construye un plan de consulta que luego se materializa con `.collect()`. La documentación oficial describe `LazyFrame` como una representación de un grafo de cómputo o consulta que permite optimización completa de la consulta y paralelismo. :contentReference[oaicite:1]{index=1}

```python
import polars as pl

lf = pl.LazyFrame({
    "product": ["A", "B", "C"],
    "price": [100, 200, 300]
})

result = (
    lf
    .filter(pl.col("price") >= 200)
    .select(["product", "price"])
    .collect()
)

print(result)
```

## `Expression`

Una expresión describe una operación sobre una o más columnas.

```python
pl.col("price") * 1.18
```

Las expresiones se ejecutan dentro de contextos como `select`, `with_columns`, `filter` y `group_by`, que son señalados por la documentación oficial como contextos comunes de Polars. :contentReference[oaicite:2]{index=2}

## Crear DataFrames

## Desde diccionario de listas

```python
import polars as pl

df = pl.DataFrame({
    "name": ["Ana", "Luis"],
    "age": [20, 25]
})

print(df)
```

## Desde lista de diccionarios

```python
import polars as pl

records = [
    {"name": "Ana", "age": 20},
    {"name": "Luis", "age": 25}
]

df = pl.DataFrame(records)

print(df)
```

## Desde columnas con tipos definidos

```python
import polars as pl

df = pl.DataFrame(
    {
        "code": ["001", "002"],
        "amount": [100.5, 200.0]
    },
    schema={
        "code": pl.String,
        "amount": pl.Float64
    }
)

print(df)
```

## Inspección inicial

## `head()`

```python
print(df.head())
```

Con cantidad específica:

```python
print(df.head(10))
```

## `tail()`

```python
print(df.tail())
```

## `shape`

```python
print(df.shape)
```

Devuelve:

```text
(filas, columnas)
```

## `columns`

```python
print(df.columns)
```

## `schema`

```python
print(df.schema)
```

Muestra nombres de columnas y tipos de datos.

## `dtypes`

```python
print(df.dtypes)
```

## `describe()`

```python
print(df.describe())
```

Genera estadísticas descriptivas básicas.

## Lectura de archivos

## `read_csv()`

`pl.read_csv()` lee un CSV y devuelve un `DataFrame`. La documentación oficial indica que esta función lee un archivo CSV en memoria como `DataFrame`; también advierte que usar `read_csv().lazy()` es un antipatrón porque primero materializa todo el archivo y no permite empujar optimizaciones hacia el lector. :contentReference[oaicite:3]{index=3}

```python
import polars as pl

df = pl.read_csv("data.csv")

print(df.head())
```

## CSV con separador específico

```python
df = pl.read_csv("data.csv", separator=";")
```

## CSV con codificación

```python
df = pl.read_csv("data.csv", encoding="utf8")
```

## CSV con esquema

```python
df = pl.read_csv(
    "data.csv",
    schema_overrides={
        "code": pl.String,
        "amount": pl.Float64
    }
)
```

## `scan_csv()`

`pl.scan_csv()` lee un CSV de forma lazy. La documentación oficial indica que esto permite al optimizador empujar filtros y selección de columnas hacia la lectura, reduciendo potencialmente el uso de memoria. :contentReference[oaicite:4]{index=4}

```python
import polars as pl

lf = pl.scan_csv("data.csv")

result = (
    lf
    .filter(pl.col("amount") > 1000)
    .select(["client_id", "amount"])
    .collect()
)

print(result)
```

## `read_parquet()`

```python
df = pl.read_parquet("data.parquet")
```

## `scan_parquet()`

`pl.scan_parquet()` permite leer archivos Parquet de forma lazy y aplicar optimizaciones como predicate pushdown y projection pushdown hacia el nivel de lectura. :contentReference[oaicite:5]{index=5}

```python
lf = pl.scan_parquet("data.parquet")

result = (
    lf
    .filter(pl.col("year") == 2026)
    .collect()
)
```

## Exportación de archivos

## `write_csv()`

```python
df.write_csv("output.csv")
```

Con separador:

```python
df.write_csv("output.csv", separator=";")
```

## `write_parquet()`

```python
df.write_parquet("output.parquet")
```

## `write_excel()`

```python
df.write_excel("output.xlsx")
```

Para reportes Excel con formato más detallado, puede ser necesario combinar con otras herramientas especializadas según el caso.

## Selección de columnas

## `select()`

`select()` selecciona columnas o calcula expresiones.

```python
import polars as pl

df = pl.DataFrame({
    "product": ["A", "B", "C"],
    "price": [100, 200, 300],
    "quantity": [2, 1, 5]
})

result = df.select([
    "product",
    "price"
])

print(result)
```

## Selección con expresiones

```python
result = df.select([
    pl.col("product"),
    (pl.col("price") * pl.col("quantity")).alias("total")
])

print(result)
```

## Seleccionar todas las columnas

```python
result = df.select(pl.all())

print(result)
```

## Filtrar filas

## `filter()`

`filter()` conserva las filas donde la expresión booleana evalúa como verdadera. La documentación oficial indica que las filas donde el filtro no evalúa como `True` se descartan, incluyendo valores nulos. :contentReference[oaicite:6]{index=6}

```python
result = df.filter(pl.col("price") >= 200)

print(result)
```

## Filtro con varias condiciones

```python
result = df.filter(
    (pl.col("price") >= 100) & (pl.col("quantity") > 1)
)

print(result)
```

## Filtro con `is_in()`

```python
result = df.filter(
    pl.col("product").is_in(["A", "C"])
)

print(result)
```

## Filtro de texto

```python
result = df.filter(
    pl.col("product").str.contains("A")
)

print(result)
```

## Crear o modificar columnas

## `with_columns()`

`with_columns()` agrega columnas nuevas o reemplaza columnas existentes si el nombre coincide. La documentación oficial indica que acepta expresiones y que las columnas agregadas reemplazan columnas existentes con el mismo nombre. :contentReference[oaicite:7]{index=7}

```python
result = df.with_columns(
    (pl.col("price") * pl.col("quantity")).alias("total")
)

print(result)
```

## Varias columnas nuevas

```python
result = df.with_columns([
    (pl.col("price") * 1.18).alias("price_with_tax"),
    (pl.col("price") * pl.col("quantity")).alias("total")
])

print(result)
```

## Reemplazar una columna existente

```python
result = df.with_columns(
    (pl.col("price") * 1.18).alias("price")
)

print(result)
```

## Renombrar columnas

```python
result = df.rename({
    "product": "product_name",
    "price": "unit_price"
})

print(result)
```

## Eliminar columnas

```python
result = df.drop("quantity")

print(result)
```

Varias columnas:

```python
result = df.drop(["price", "quantity"])
```

## Ordenamiento

## `sort()`

```python
result = df.sort("price")

print(result)
```

Orden descendente:

```python
result = df.sort("price", descending=True)
```

Orden por varias columnas:

```python
result = df.sort(["product", "price"])
```

## Agregaciones básicas

## `sum()`

```python
result = df.select(
    pl.col("price").sum().alias("total_price")
)

print(result)
```

## `mean()`

```python
result = df.select(
    pl.col("price").mean().alias("average_price")
)
```

## `min()` y `max()`

```python
result = df.select([
    pl.col("price").min().alias("minimum_price"),
    pl.col("price").max().alias("maximum_price")
])
```

## `count()` y `len()`

```python
result = df.select(
    pl.len().alias("rows")
)

print(result)
```

## Agrupación con `group_by()`

`group_by()` inicia una operación de agrupación. La documentación oficial muestra su uso con `.agg()` para calcular agregados por grupo. :contentReference[oaicite:8]{index=8}

```python
import polars as pl

df = pl.DataFrame({
    "city": ["Lima", "Lima", "Cusco"],
    "amount": [100, 200, 300]
})

summary = (
    df
    .group_by("city")
    .agg(
        pl.col("amount").sum().alias("total_amount")
    )
)

print(summary)
```

## Agrupar por varias columnas

```python
summary = (
    df
    .group_by(["city", "category"])
    .agg(
        pl.col("amount").sum().alias("total_amount")
    )
)
```

## Agregación múltiple

```python
summary = (
    df
    .group_by("city")
    .agg([
        pl.col("amount").sum().alias("total_amount"),
        pl.col("amount").mean().alias("average_amount"),
        pl.len().alias("operations")
    ])
)

print(summary)
```

## Expresiones condicionales

## `when()`, `then()`, `otherwise()`

```python
result = df.with_columns(
    pl.when(pl.col("price") >= 200)
    .then(pl.lit("high"))
    .otherwise(pl.lit("low"))
    .alias("price_level")
)

print(result)
```

La documentación oficial advierte que las expresiones en cadenas `when-then-otherwise` se calculan en paralelo y luego se filtran, por lo que cada expresión debe ser válida por sí misma independientemente de la condición. :contentReference[oaicite:9]{index=9}

## Trabajo con texto

Polars usa el espacio `.str` para métodos de texto.

## Minúsculas

```python
result = df.with_columns(
    pl.col("product").str.to_lowercase().alias("product_lower")
)
```

## Mayúsculas

```python
result = df.with_columns(
    pl.col("product").str.to_uppercase().alias("product_upper")
)
```

## Contiene texto

```python
result = df.filter(
    pl.col("product").str.contains("A")
)
```

## Reemplazo de texto

```python
result = df.with_columns(
    pl.col("product").str.replace("A", "Producto A").alias("product")
)
```

## Trabajo con fechas

## Convertir texto a fecha

```python
result = df.with_columns(
    pl.col("date").str.to_date("%Y-%m-%d").alias("date")
)
```

## Extraer año, mes y día

```python
result = df.with_columns([
    pl.col("date").dt.year().alias("year"),
    pl.col("date").dt.month().alias("month"),
    pl.col("date").dt.day().alias("day")
])
```

## Valores nulos

## Detectar nulos

```python
result = df.select(
    pl.col("price").is_null().sum().alias("missing_prices")
)

print(result)
```

## Rellenar nulos

```python
result = df.with_columns(
    pl.col("price").fill_null(0).alias("price")
)
```

## Eliminar filas con nulos

```python
result = df.drop_nulls()
```

En columnas específicas:

```python
result = df.drop_nulls(["price"])
```

## Duplicados

## Eliminar duplicados

```python
result = df.unique()
```

Por columnas específicas:

```python
result = df.unique(subset=["client_id"])
```

## Joins

## `join()`

```python
orders = pl.DataFrame({
    "client_id": ["1", "2", "3"],
    "amount": [100, 200, 300]
})

clients = pl.DataFrame({
    "client_id": ["1", "2"],
    "name": ["Ana", "Luis"]
})

result = orders.join(
    clients,
    on="client_id",
    how="left"
)

print(result)
```

Tipos frecuentes:

```text
inner
left
right
full
semi
anti
cross
```

## Concatenación

## `pl.concat()`

Por filas:

```python
result = pl.concat([df_1, df_2])
```

## Concatenación vertical

```python
result = pl.concat([df_1, df_2], how="vertical")
```

## Concatenación horizontal

```python
result = pl.concat([df_1, df_2], how="horizontal")
```

## Lazy API

## Propósito

La Lazy API permite construir consultas que se optimizan antes de ejecutarse. Es especialmente útil para archivos grandes, operaciones encadenadas y consultas donde conviene que Polars reduzca lecturas, columnas o filas necesarias antes de materializar el resultado. La documentación oficial indica que `LazyFrame.collect()` materializa un `LazyFrame` en un `DataFrame` y que, por defecto, las optimizaciones están activadas. :contentReference[oaicite:10]{index=10}

## Crear LazyFrame desde DataFrame

```python
lf = df.lazy()
```

## Crear LazyFrame desde archivo

```python
lf = pl.scan_csv("data.csv")
```

## Ejecutar consulta con `collect()`

```python
result = (
    lf
    .filter(pl.col("amount") > 1000)
    .select(["client_id", "amount"])
    .collect()
)

print(result)
```

## `read_csv()` vs `scan_csv()`

## `read_csv()`

```python
df = pl.read_csv("data.csv")
```

Lee y materializa el archivo inmediatamente como `DataFrame`.

## `scan_csv()`

```python
lf = pl.scan_csv("data.csv")
```

Crea una consulta lazy sobre el archivo y permite optimizaciones antes de materializar.

## Regla práctica

Para archivos pequeños y exploración simple, `read_csv()` puede ser suficiente.

Para archivos grandes o pipelines encadenados, `scan_csv()` suele ser más adecuado.

## Eager vs Lazy

## Eager

El resultado se ejecuta inmediatamente.

```python
result = (
    df
    .filter(pl.col("amount") > 1000)
    .select(["client_id", "amount"])
)
```

## Lazy

Primero se construye un plan y luego se ejecuta con `.collect()`.

```python
result = (
    lf
    .filter(pl.col("amount") > 1000)
    .select(["client_id", "amount"])
    .collect()
)
```

## Ventajas de Lazy

La evaluación lazy permite:

- optimización de consultas
- reducción de columnas leídas
- reducción de filas procesadas
- mejor rendimiento potencial
- menor presión de memoria en ciertos flujos

## Relación con `pandas`

Polars y pandas pueden resolver problemas similares de análisis tabular, pero tienen modelos distintos.

## `pandas`

- API muy extendida
- ecosistema maduro
- manipulación tabular flexible
- ejecución principalmente eager
- integración amplia con librerías existentes

## `polars`

- motor de consulta rápido
- ejecución eager y lazy
- expresiones fuertemente usadas
- optimización de consultas
- buen rendimiento con datos grandes

## Conversión desde pandas

```python
import pandas as pd
import polars as pl

pdf = pd.DataFrame({
    "a": [1, 2, 3]
})

df = pl.from_pandas(pdf)

print(df)
```

## Conversión a pandas

```python
pdf = df.to_pandas()

print(pdf)
```

## Relación con NumPy

Polars puede interactuar con NumPy, aunque su modelo principal no es un `ndarray`.

```python
import numpy as np
import polars as pl

array = np.array([1, 2, 3])

df = pl.DataFrame({
    "value": array
})

print(df)
```

## Casos de uso frecuentes

## Leer, filtrar y exportar CSV

```python
import polars as pl

df = pl.read_csv("input.csv", separator=";")

result = (
    df
    .filter(pl.col("amount") > 1000)
    .select(["client_id", "amount"])
)

result.write_csv("output.csv", separator=";")
```

## Pipeline lazy sobre archivo grande

```python
import polars as pl

result = (
    pl.scan_csv("input.csv", separator=";")
    .filter(pl.col("amount") > 1000)
    .group_by("client_id")
    .agg(
        pl.col("amount").sum().alias("total_amount")
    )
    .collect()
)

print(result)
```

## Crear columnas calculadas

```python
result = df.with_columns(
    (pl.col("price") * pl.col("quantity")).alias("total")
)
```

## Agrupar y resumir

```python
summary = (
    df
    .group_by("category")
    .agg([
        pl.col("amount").sum().alias("total_amount"),
        pl.len().alias("rows")
    ])
)
```

## Errores comunes

## Escribir código estilo pandas en Polars

Problemático:

```python
df["total"] = df["price"] * df["quantity"]
```

En Polars suele usarse:

```python
df = df.with_columns(
    (pl.col("price") * pl.col("quantity")).alias("total")
)
```

## Usar `read_csv().lazy()` para archivos grandes

Menos recomendable:

```python
lf = pl.read_csv("data.csv").lazy()
```

La documentación oficial señala que esto fuerza a materializar el CSV completo antes de pasar a lazy, por lo que no permite empujar optimizaciones hacia la lectura. :contentReference[oaicite:11]{index=11}

Más adecuado:

```python
lf = pl.scan_csv("data.csv")
```

## Olvidar `.collect()` en LazyFrame

Problemático:

```python
result = (
    pl.scan_csv("data.csv")
    .filter(pl.col("amount") > 1000)
)

print(result)
```

Esto imprime el plan lazy, no el resultado materializado.

Correcto:

```python
result = (
    pl.scan_csv("data.csv")
    .filter(pl.col("amount") > 1000)
    .collect()
)
```

## Usar `and` u `or` en filtros

Problemático:

```python
df.filter(
    (pl.col("amount") > 1000) and (pl.col("city") == "Lima")
)
```

Correcto:

```python
df.filter(
    (pl.col("amount") > 1000) & (pl.col("city") == "Lima")
)
```

Para `or`:

```python
df.filter(
    (pl.col("city") == "Lima") | (pl.col("city") == "Cusco")
)
```

## Olvidar paréntesis en condiciones compuestas

Problemático:

```python
df.filter(pl.col("amount") > 1000 & pl.col("city") == "Lima")
```

Correcto:

```python
df.filter(
    (pl.col("amount") > 1000) & (pl.col("city") == "Lima")
)
```

## Confundir `null` con `NaN`

En datos numéricos, `null` y `NaN` no siempre se tratan igual. Conviene distinguir:

```python
pl.col("value").is_null()
pl.col("value").is_nan()
```

## No revisar el esquema

Antes de transformar columnas, conviene revisar tipos.

```python
print(df.schema)
```

## Suponer que todas las operaciones mantienen orden

Algunas operaciones, como ciertas agregaciones, no necesariamente deben interpretarse como preservadoras de orden salvo que se configure explícitamente cuando corresponda.

## Buenas prácticas

## Usar el alias `pl`

```python
import polars as pl
```

## Usar expresiones con `pl.col()`

```python
pl.col("amount") * 1.18
```

## Preferir `scan_csv()` o `scan_parquet()` para pipelines grandes

```python
lf = pl.scan_csv("data.csv")
```

## Usar `.collect()` solo al final de una consulta lazy

```python
result = (
    lf
    .filter(...)
    .select(...)
    .collect()
)
```

## Revisar `schema`

```python
print(df.schema)
```

## Encadenar transformaciones de forma clara

```python
result = (
    df
    .filter(pl.col("amount") > 0)
    .with_columns(
        (pl.col("amount") * 1.18).alias("amount_with_tax")
    )
    .select(["client_id", "amount_with_tax"])
)
```

## Usar nombres explícitos con `alias()`

```python
(pl.col("amount") * 1.18).alias("amount_with_tax")
```

## Definir tipos al leer datos sensibles

```python
df = pl.read_csv(
    "data.csv",
    schema_overrides={
        "document_id": pl.String
    }
)
```

## Ejemplo integrado

```python
from pathlib import Path

import polars as pl


def summarize_sales(input_path):
    result = (
        pl.scan_csv(
            input_path,
            separator=";",
            schema_overrides={
                "client_id": pl.String,
                "product_id": pl.String
            }
        )
        .with_columns([
            pl.col("amount").cast(pl.Float64),
            pl.col("date").str.to_date("%Y-%m-%d").alias("date")
        ])
        .filter(pl.col("amount") > 0)
        .with_columns([
            pl.col("date").dt.year().alias("year"),
            pl.col("date").dt.month().alias("month")
        ])
        .group_by(["year", "month", "product_id"])
        .agg([
            pl.col("amount").sum().alias("total_amount"),
            pl.len().alias("operations")
        ])
        .sort(["year", "month", "product_id"])
        .collect()
    )

    return result


input_path = Path("sales.csv")
output_path = Path("sales_summary.csv")

summary = summarize_sales(input_path)

summary.write_csv(output_path, separator=";")

print("Resumen exportado correctamente")
```

## Relación con otras librerías

`polars` se relaciona especialmente con:

- `pandas`, como alternativa o complemento para datos tabulares
- `numpy`, por intercambio de datos numéricos
- `pyarrow`, por formatos columnares y ecosistema Arrow
- `matplotlib` y `plotly`, para visualizar resultados
- `scikit-learn`, cuando los datos preparados se usan en modelos
- `sqlalchemy` o conectores de base de datos, cuando los datos provienen de consultas
- `requests` y `httpx`, cuando los datos se obtienen desde APIs

## Orden didáctico interno

```text
1. Propósito de polars
2. Instalación e importación
3. DataFrame, Series, LazyFrame y Expression
4. Creación e inspección de datos
5. Lectura y exportación de archivos
6. select(), filter() y with_columns()
7. Expresiones con pl.col()
8. group_by() y agregaciones
9. Texto, fechas, nulos y duplicados
10. Joins y concatenación
11. Lazy API con scan_csv(), scan_parquet() y collect()
12. Diferencia frente a pandas
13. Errores comunes
14. Buenas prácticas
```