# `pandas`

## Propósito

`pandas` es una librería externa para manipulación y análisis de datos en Python. Se utiliza para trabajar con datos tabulares, series temporales, archivos CSV, Excel, bases de datos, reportes, limpieza de datos, agregaciones, cruces, filtros, transformaciones y preparación de información para análisis o visualización.

Es una de las librerías más importantes del ecosistema de datos en Python.

## Naturaleza de la librería

`pandas` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install pandas
```

La importación habitual es:

```python
import pandas as pd
```

El alias `pd` es la convención estándar usada en documentación, ejemplos y proyectos reales.

## Idea central

La idea principal de pandas es trabajar con datos estructurados de forma tabular.

Una tabla puede representarse como un `DataFrame`:

```python
import pandas as pd

data = {
    "name": ["Ana", "Luis", "Marta"],
    "age": [20, 25, 22],
    "city": ["Lima", "Cusco", "Arequipa"]
}

df = pd.DataFrame(data)

print(df)
```

Salida:

```text
    name  age      city
0    Ana   20      Lima
1   Luis   25     Cusco
2  Marta   22  Arequipa
```

## Instalación

Instalación básica:

```bash
python -m pip install pandas
```

Para leer y escribir archivos Excel suele requerirse también `openpyxl`:

```bash
python -m pip install pandas openpyxl
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
pandas==3.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
import pandas as pd
```

Verificación:

```python
import pandas as pd

print(pd.__version__)
```

## Estructuras principales

Pandas tiene dos estructuras fundamentales:

```text
Series
DataFrame
```

## `Series`

Una `Series` representa una columna o secuencia unidimensional con índice.

```python
import pandas as pd

series = pd.Series([10, 20, 30])

print(series)
```

Salida:

```text
0    10
1    20
2    30
dtype: int64
```

## `DataFrame`

Un `DataFrame` representa una tabla bidimensional con filas y columnas.

```python
import pandas as pd

df = pd.DataFrame({
    "product": ["A", "B", "C"],
    "price": [100, 200, 300]
})

print(df)
```

Salida:

```text
  product  price
0       A    100
1       B    200
2       C    300
```

## Crear DataFrames

## Desde diccionario de listas

```python
import pandas as pd

df = pd.DataFrame({
    "name": ["Ana", "Luis"],
    "age": [20, 25]
})

print(df)
```

## Desde lista de diccionarios

```python
import pandas as pd

records = [
    {"name": "Ana", "age": 20},
    {"name": "Luis", "age": 25}
]

df = pd.DataFrame(records)

print(df)
```

## Desde lista de listas

```python
import pandas as pd

data = [
    ["Ana", 20],
    ["Luis", 25]
]

df = pd.DataFrame(data, columns=["name", "age"])

print(df)
```

## Inspección inicial

## `head()`

Muestra las primeras filas.

```python
print(df.head())
```

Con cantidad específica:

```python
print(df.head(10))
```

## `tail()`

Muestra las últimas filas.

```python
print(df.tail())
```

## `shape`

Devuelve cantidad de filas y columnas.

```python
print(df.shape)
```

Salida conceptual:

```text
(filas, columnas)
```

## `columns`

Devuelve los nombres de columnas.

```python
print(df.columns)
```

## `index`

Devuelve el índice del DataFrame.

```python
print(df.index)
```

## `dtypes`

Muestra los tipos de datos por columna.

```python
print(df.dtypes)
```

## `info()`

Muestra resumen estructural del DataFrame.

```python
df.info()
```

Incluye columnas, tipos, cantidad de valores no nulos y uso de memoria.

## `describe()`

Muestra estadísticas descriptivas para columnas numéricas.

```python
print(df.describe())
```

Para incluir columnas no numéricas:

```python
print(df.describe(include="all"))
```

## Lectura de archivos

## `read_csv()`

Lee archivos CSV.

```python
import pandas as pd

df = pd.read_csv("data.csv")

print(df.head())
```

## CSV con separador específico

```python
df = pd.read_csv("data.csv", sep=";")
```

## CSV con codificación específica

```python
df = pd.read_csv("data.csv", encoding="utf-8")
```

En algunos archivos puede requerirse otra codificación, por ejemplo:

```python
df = pd.read_csv("data.csv", encoding="latin1")
```

## `read_excel()`

Lee archivos Excel.

```python
import pandas as pd

df = pd.read_excel("data.xlsx")

print(df.head())
```

Leer una hoja específica:

```python
df = pd.read_excel("data.xlsx", sheet_name="Hoja1")
```

## `read_json()`

Lee datos JSON.

```python
df = pd.read_json("data.json")
```

## `read_sql()`

Lee datos desde una consulta SQL usando una conexión compatible.

```python
df = pd.read_sql("SELECT * FROM tabla", connection)
```

## Exportación de archivos

## `to_csv()`

```python
df.to_csv("output.csv", index=False)
```

Con separador específico:

```python
df.to_csv("output.csv", index=False, sep=";")
```

## `to_excel()`

```python
df.to_excel("output.xlsx", index=False)
```

Con hoja específica:

```python
df.to_excel("output.xlsx", sheet_name="Datos", index=False)
```

## `to_json()`

```python
df.to_json("output.json", orient="records", force_ascii=False)
```

## Selección de columnas

## Una columna

```python
names = df["name"]

print(names)
```

El resultado es una `Series`.

## Varias columnas

```python
subset = df[["name", "age"]]

print(subset)
```

El resultado es un `DataFrame`.

## Selección de filas por posición con `iloc`

`iloc` selecciona por posición entera.

```python
print(df.iloc[0])
```

Primera fila y columnas específicas:

```python
print(df.iloc[0, 1])
```

Rango de filas:

```python
print(df.iloc[0:5])
```

Filas y columnas por posición:

```python
print(df.iloc[0:5, 0:2])
```

## Selección por etiquetas con `loc`

`loc` selecciona por etiquetas de índice y nombres de columnas.

```python
print(df.loc[0, "name"])
```

Filas y columnas:

```python
print(df.loc[0:5, ["name", "age"]])
```

## Diferencia entre `loc` e `iloc`

```text
loc   -> selecciona por etiquetas
iloc  -> selecciona por posiciones enteras
```

Ejemplo:

```python
df.loc[0, "name"]
df.iloc[0, 0]
```

## Filtrado de filas

## Filtro simple

```python
adults = df[df["age"] >= 18]

print(adults)
```

## Filtro con varias condiciones

Para combinar condiciones se usan `&` y `|`, no `and` ni `or`.

```python
filtered = df[
    (df["age"] >= 18) & (df["city"] == "Lima")
]

print(filtered)
```

## Filtro con `isin()`

```python
filtered = df[df["city"].isin(["Lima", "Cusco"])]

print(filtered)
```

## Filtro con texto

```python
filtered = df[df["name"].str.contains("Ana", na=False)]

print(filtered)
```

## Crear columnas

## Asignación directa

```python
df["tax"] = df["price"] * 0.18
```

## Crear columna con condición

```python
df["category"] = df["price"].apply(
    lambda value: "high" if value >= 200 else "low"
)
```

## Crear columna con `assign()`

```python
df = df.assign(
    total=df["price"] * df["quantity"]
)
```

## Modificar columnas

## Renombrar columnas

```python
df = df.rename(columns={
    "old_name": "new_name"
})
```

## Eliminar columnas

```python
df = df.drop(columns=["column_to_remove"])
```

## Cambiar tipo de dato

```python
df["age"] = df["age"].astype(int)
```

Convertir a texto:

```python
df["code"] = df["code"].astype(str)
```

Convertir fechas:

```python
df["date"] = pd.to_datetime(df["date"])
```

## Ordenamiento

## `sort_values()`

```python
df = df.sort_values("price")
```

Orden descendente:

```python
df = df.sort_values("price", ascending=False)
```

Ordenar por varias columnas:

```python
df = df.sort_values(["city", "price"])
```

## `sort_index()`

```python
df = df.sort_index()
```

## Valores faltantes

Pandas representa valores faltantes con valores como `NaN`, `NaT` o `pd.NA`, según el tipo de dato.

## Detectar valores faltantes

```python
print(df.isna())
```

Cantidad por columna:

```python
print(df.isna().sum())
```

## Eliminar filas con faltantes

```python
df_clean = df.dropna()
```

Eliminar filas donde falta una columna específica:

```python
df_clean = df.dropna(subset=["price"])
```

## Rellenar faltantes

```python
df["price"] = df["price"].fillna(0)
```

Rellenar texto:

```python
df["city"] = df["city"].fillna("No informado")
```

## Reemplazo de valores

```python
df["status"] = df["status"].replace({
    "A": "Activo",
    "I": "Inactivo"
})
```

## Duplicados

## Detectar duplicados

```python
print(df.duplicated())
```

Duplicados por columnas específicas:

```python
print(df.duplicated(subset=["document_id"]))
```

## Eliminar duplicados

```python
df = df.drop_duplicates()
```

Por columnas específicas:

```python
df = df.drop_duplicates(subset=["document_id"])
```

## Operaciones agregadas

## Suma

```python
print(df["price"].sum())
```

## Promedio

```python
print(df["price"].mean())
```

## Mínimo y máximo

```python
print(df["price"].min())
print(df["price"].max())
```

## Conteo

```python
print(df["price"].count())
```

## Conteo de valores únicos

```python
print(df["city"].nunique())
```

## Frecuencia de valores

```python
print(df["city"].value_counts())
```

## Agrupación con `groupby()`

`groupby()` permite agrupar filas por una o más columnas y aplicar operaciones de agregación, transformación o filtrado. La documentación oficial describe esta lógica como una combinación de dividir el objeto, aplicar una función y combinar resultados.

## Agrupar por una columna

```python
summary = df.groupby("city")["price"].sum()

print(summary)
```

## Agrupar por varias columnas

```python
summary = df.groupby(["city", "category"])["price"].sum()

print(summary)
```

## Agregación múltiple

```python
summary = df.groupby("city").agg(
    total_price=("price", "sum"),
    average_price=("price", "mean"),
    count=("price", "count")
)

print(summary)
```

## Resetear índice después de agrupar

```python
summary = (
    df.groupby("city")
    .agg(total_price=("price", "sum"))
    .reset_index()
)

print(summary)
```

## Transformaciones por grupo

```python
df["city_average"] = df.groupby("city")["price"].transform("mean")
```

## Combinación de DataFrames

## `concat()`

Une DataFrames por filas o columnas.

Por filas:

```python
combined = pd.concat([df_1, df_2], ignore_index=True)
```

Por columnas:

```python
combined = pd.concat([df_1, df_2], axis=1)
```

## `merge()`

Combina DataFrames usando columnas clave, similar a un join de SQL.

```python
result = df_orders.merge(
    df_clients,
    on="client_id",
    how="left"
)
```

Tipos frecuentes de `how`:

```text
inner
left
right
outer
```

## `join()`

Permite unir DataFrames usando índices.

```python
result = df_1.join(df_2)
```

## Tablas dinámicas

## `pivot_table()`

```python
table = pd.pivot_table(
    df,
    values="sales",
    index="city",
    columns="category",
    aggfunc="sum",
    fill_value=0
)

print(table)
```

## `melt()`

Transforma columnas en filas.

```python
long_df = df.melt(
    id_vars=["date"],
    value_vars=["sales_a", "sales_b"],
    var_name="product",
    value_name="sales"
)
```

## Trabajo con texto

Pandas permite aplicar métodos de texto sobre columnas usando `.str`.

## Minúsculas

```python
df["name"] = df["name"].str.lower()
```

## Mayúsculas

```python
df["name"] = df["name"].str.upper()
```

## Quitar espacios

```python
df["name"] = df["name"].str.strip()
```

## Reemplazar texto

```python
df["name"] = df["name"].str.replace("á", "a", regex=False)
```

## Contiene texto

```python
filtered = df[df["name"].str.contains("ana", case=False, na=False)]
```

## Trabajo con fechas

## Convertir a fecha

```python
df["date"] = pd.to_datetime(df["date"])
```

## Extraer año

```python
df["year"] = df["date"].dt.year
```

## Extraer mes

```python
df["month"] = df["date"].dt.month
```

## Extraer día

```python
df["day"] = df["date"].dt.day
```

## Formatear fecha como texto

```python
df["date_text"] = df["date"].dt.strftime("%d/%m/%Y")
```

## Filtrar por fechas

```python
filtered = df[df["date"] >= "2026-01-01"]
```

## Índices

## Establecer índice

```python
df = df.set_index("id")
```

## Resetear índice

```python
df = df.reset_index()
```

## Acceso por índice

```python
row = df.loc["ID001"]
```

## Aplicar funciones

## `apply()` sobre columnas

```python
df["price_with_tax"] = df["price"].apply(lambda x: x * 1.18)
```

## `apply()` sobre filas

```python
df["description"] = df.apply(
    lambda row: f"{row['product']} - {row['city']}",
    axis=1
)
```

Debe usarse con criterio, porque muchas operaciones son más eficientes con métodos vectorizados.

## `map()`

Útil para transformar valores de una `Series`.

```python
df["status_text"] = df["status"].map({
    "A": "Activo",
    "I": "Inactivo"
})
```

## `where()`

```python
df["price_checked"] = df["price"].where(df["price"] >= 0, 0)
```

## `query()`

Permite filtrar con una expresión textual.

```python
filtered = df.query("age >= 18 and city == 'Lima'")
```

## `eval()`

Permite evaluar expresiones sobre columnas.

```python
df["total"] = df.eval("price * quantity")
```

## Relación con NumPy

Pandas se apoya en ideas de NumPy, especialmente en operaciones vectorizadas, arreglos y tipos numéricos.

```python
import numpy as np
import pandas as pd

df = pd.DataFrame({
    "value": [1, 2, 3]
})

df["sqrt"] = np.sqrt(df["value"])

print(df)
```

## Relación con Matplotlib

Pandas puede graficar de forma básica usando integración con Matplotlib.

```python
df["price"].plot(kind="hist")
```

En proyectos más controlados, suele preferirse usar `matplotlib` directamente.

## Relación con Excel

Pandas permite leer y escribir archivos Excel.

```python
df = pd.read_excel("input.xlsx")
df.to_excel("output.xlsx", index=False)
```

Para estilos avanzados, fórmulas, formatos, anchos o manipulación detallada del libro, suele usarse `openpyxl`.

## Relación con SQL

Pandas puede leer resultados SQL y convertirlos en DataFrames.

```python
df = pd.read_sql("SELECT * FROM ventas", connection)
```

También puede escribir datos en tablas SQL con una conexión compatible.

```python
df.to_sql("ventas", connection, if_exists="replace", index=False)
```

## Casos de uso frecuentes

## Leer un CSV, limpiar y exportar

```python
import pandas as pd

df = pd.read_csv("input.csv", sep=";")

df["name"] = df["name"].str.strip()
df = df.drop_duplicates()

df.to_excel("output.xlsx", index=False)
```

## Resumir ventas por categoría

```python
summary = (
    df.groupby("category")
    .agg(total_sales=("sales", "sum"))
    .reset_index()
)

print(summary)
```

## Cruzar dos tablas

```python
result = orders.merge(
    clients,
    on="client_id",
    how="left"
)
```

## Filtrar datos

```python
filtered = df[
    (df["year"] == 2026) & (df["amount"] > 1000)
]
```

## Errores comunes

## Confundir una columna con DataFrame

```python
df["name"]
```

devuelve una `Series`.

```python
df[["name"]]
```

devuelve un `DataFrame`.

## Usar `and` en filtros

Problemático:

```python
filtered = df[(df["age"] >= 18) and (df["city"] == "Lima")]
```

Correcto:

```python
filtered = df[(df["age"] >= 18) & (df["city"] == "Lima")]
```

## Olvidar paréntesis en filtros compuestos

Problemático:

```python
filtered = df[df["age"] >= 18 & df["city"] == "Lima"]
```

Correcto:

```python
filtered = df[(df["age"] >= 18) & (df["city"] == "Lima")]
```

## Modificar una copia sin darse cuenta

Pandas puede advertir sobre asignaciones ambiguas cuando se trabaja con subconjuntos.

Más claro:

```python
filtered = df[df["age"] >= 18].copy()
filtered["category"] = "adult"
```

## Leer códigos como números

Si una columna contiene códigos con ceros a la izquierda, conviene leerla como texto.

```python
df = pd.read_excel(
    "data.xlsx",
    dtype={"document_id": str}
)
```

## No revisar tipos después de leer archivos

```python
print(df.dtypes)
```

Es importante porque fechas, códigos y montos pueden leerse con tipos no esperados.

## Exportar con índice accidental

Problemático:

```python
df.to_excel("output.xlsx")
```

Puede exportar el índice como columna.

Más común:

```python
df.to_excel("output.xlsx", index=False)
```

## Usar `apply()` cuando existe operación vectorizada

Menos eficiente:

```python
df["total"] = df.apply(lambda row: row["price"] * row["quantity"], axis=1)
```

Más claro y eficiente:

```python
df["total"] = df["price"] * df["quantity"]
```

## Buenas prácticas

## Usar el alias `pd`

```python
import pandas as pd
```

## Revisar estructura al cargar datos

```python
print(df.head())
print(df.info())
print(df.dtypes)
```

## Definir tipos al leer datos sensibles

```python
df = pd.read_excel(
    "data.xlsx",
    dtype={"document_id": str}
)
```

## Usar `copy()` cuando se trabaja con subconjuntos

```python
filtered = df[df["status"] == "A"].copy()
```

## Separar limpieza en funciones

```python
def clean_text_column(series):
    return series.astype(str).str.strip()
```

## Encadenar operaciones con claridad

```python
summary = (
    df.dropna(subset=["amount"])
    .groupby("category")
    .agg(total_amount=("amount", "sum"))
    .reset_index()
)
```

## Usar `index=False` al exportar reportes

```python
df.to_excel("output.xlsx", index=False)
```

## Validar resultados intermedios

```python
print(df.shape)
print(df.isna().sum())
```

## Ejemplo integrado

```python
from pathlib import Path

import pandas as pd


def load_sales(input_path):
    return pd.read_excel(
        input_path,
        dtype={
            "client_id": str,
            "product_id": str
        }
    )


def clean_sales(df):
    result = df.copy()

    result["client_name"] = result["client_name"].str.strip()
    result["product"] = result["product"].str.strip()
    result["date"] = pd.to_datetime(result["date"])
    result["amount"] = result["amount"].fillna(0)

    result["year"] = result["date"].dt.year
    result["month"] = result["date"].dt.month

    return result


def summarize_sales(df):
    summary = (
        df.groupby(["year", "month", "product"])
        .agg(
            total_amount=("amount", "sum"),
            operations=("amount", "count")
        )
        .reset_index()
        .sort_values(["year", "month", "product"])
    )

    return summary


input_path = Path("sales.xlsx")
output_path = Path("sales_summary.xlsx")

sales = load_sales(input_path)
cleaned_sales = clean_sales(sales)
summary = summarize_sales(cleaned_sales)

summary.to_excel(output_path, index=False)

print("Resumen exportado correctamente")
```

## Relación con otras librerías

`pandas` se relaciona especialmente con:

- `numpy`, por operaciones numéricas y estructuras base
- `openpyxl`, para leer y escribir archivos Excel `.xlsx`
- `xlsxwriter`, para exportación Excel con formato
- `matplotlib`, para visualización
- `seaborn`, para visualización estadística
- `plotly`, para gráficos interactivos
- `sqlalchemy`, para conexión con bases de datos
- `requests` y `httpx`, para obtener datos desde APIs
- `pyarrow`, para formatos columnares como Parquet
- `scikit-learn`, para preparación de datos en machine learning

## Orden didáctico interno

```text
1. Propósito de pandas
2. Instalación e importación
3. Series y DataFrame
4. Creación e inspección de datos
5. Lectura y exportación de archivos
6. Selección con columnas, loc e iloc
7. Filtros y creación de columnas
8. Limpieza de faltantes y duplicados
9. Agrupaciones con groupby()
10. Cruces con merge() y concat()
11. Fechas y texto
12. Exportación de resultados
13. Errores comunes
14. Buenas prácticas
```