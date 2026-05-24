# `plotly`

## Propósito

`plotly` es una librería externa para crear gráficos interactivos en Python. Se utiliza para visualizar datos mediante gráficos de líneas, barras, dispersión, áreas, histogramas, cajas, mapas de calor, gráficos 3D, gráficos financieros, mapas, subplots y visualizaciones exportables a HTML.

Su principal diferencia frente a `matplotlib` y `seaborn` es que los gráficos de Plotly son interactivos por defecto. Permiten acciones como acercar, alejar, mover, ocultar series desde la leyenda, inspeccionar valores al pasar el cursor y exportar visualmente desde el navegador.

## Naturaleza de la librería

`plotly` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install plotly
```

La importación habitual depende del nivel de control que se necesite.

Para gráficos rápidos:

```python
import plotly.express as px
```

Para gráficos más personalizados:

```python
import plotly.graph_objects as go
```

También se usa con frecuencia:

```python
import plotly.io as pio
```

## Idea central

La idea principal de Plotly es crear objetos de figura interactivos.

Flujo típico:

```text
datos -> figura Plotly -> visualización interactiva -> exportación opcional
```

Ejemplo básico con `plotly.express`:

```python
import plotly.express as px

data = {
    "product": ["Laptop", "Mouse", "Teclado"],
    "amount": [3500, 800, 750]
}

fig = px.bar(
    data,
    x="product",
    y="amount",
    title="Monto por producto"
)

fig.show()
```

## Cuándo usar Plotly

Conviene usar Plotly cuando se necesita:

- gráficos interactivos
- exploración visual con hover
- zoom y navegación
- leyendas interactivas
- exportación a HTML
- dashboards o aplicaciones web
- visualizaciones para compartir en navegador
- gráficos con muchas opciones de interacción
- mapas, gráficos 3D o visualizaciones dinámicas
- integración con Dash, Streamlit o notebooks

## Cuándo no usar Plotly

No siempre conviene usar Plotly cuando se necesita:

- una imagen estática simple
- máxima compatibilidad en documentos impresos
- gráficos muy livianos para scripts básicos
- control absoluto de estilo editorial
- reportes donde solo se insertarán imágenes fijas

Para imágenes estáticas simples, `matplotlib` suele ser suficiente.

Para visualización estadística exploratoria rápida, `seaborn` puede ser más directo.

## Instalación

Instalación básica:

```bash
python -m pip install plotly
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
plotly==6.x.x
```

La versión exacta puede variar según el entorno.

## Instalación para exportar imágenes estáticas

Para exportar figuras como PNG, JPEG, WebP, SVG o PDF, se suele instalar Kaleido.

```bash
python -m pip install kaleido
```

En entornos recientes, Kaleido puede requerir Chrome o Chromium compatible.

Plotly permite instalar Chrome desde la línea de comandos:

```bash
plotly_get_chrome
```

También puede hacerse desde Python:

```python
import plotly.io as pio

pio.get_chrome()
```

## Importaciones principales

## Plotly Express

```python
import plotly.express as px
```

`plotly.express` permite crear gráficos comunes con pocas líneas de código.

## Graph Objects

```python
import plotly.graph_objects as go
```

`plotly.graph_objects` permite construir figuras con más control sobre trazas, ejes, layout y detalles.

## Plotly IO

```python
import plotly.io as pio
```

`plotly.io` permite controlar renderizadores, exportación y configuración de salida.

## Verificación

```python
import plotly

print(plotly.__version__)
```

## Plotly Express

## Propósito

`plotly.express` es la interfaz de alto nivel de Plotly.

Se utiliza para crear gráficos rápidamente a partir de datos tabulares.

Ejemplo:

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "month": ["Ene", "Feb", "Mar", "Abr"],
    "sales": [100, 120, 90, 150]
})

fig = px.line(
    df,
    x="month",
    y="sales",
    title="Ventas mensuales"
)

fig.show()
```

## Ventaja de Plotly Express

Permite pasar un `DataFrame` completo e indicar columnas por nombre.

```python
fig = px.scatter(
    df,
    x="price",
    y="sales",
    color="category"
)
```

Esto hace que el código sea expresivo y similar a librerías como `seaborn`.

## Graph Objects

## Propósito

`plotly.graph_objects` es la interfaz de bajo nivel.

Permite construir figuras agregando trazas manualmente.

Ejemplo:

```python
import plotly.graph_objects as go

fig = go.Figure()

fig.add_trace(
    go.Scatter(
        x=["Ene", "Feb", "Mar", "Abr"],
        y=[100, 120, 90, 150],
        mode="lines+markers",
        name="Ventas"
    )
)

fig.update_layout(
    title="Ventas mensuales",
    xaxis_title="Mes",
    yaxis_title="Ventas"
)

fig.show()
```

## Diferencia entre Plotly Express y Graph Objects

## Plotly Express

Se usa para gráficos rápidos y declarativos.

```python
fig = px.bar(
    df,
    x="product",
    y="amount"
)
```

## Graph Objects

Se usa cuando se necesita mayor control.

```python
fig = go.Figure()

fig.add_trace(
    go.Bar(
        x=df["product"],
        y=df["amount"]
    )
)
```

## Regla práctica

Para empezar, conviene usar `plotly.express`.

Cuando el gráfico requiere personalización avanzada, varias trazas específicas o control fino, conviene pasar a `plotly.graph_objects`.

## Objeto `Figure`

El objeto central de Plotly es `Figure`.

Una figura contiene:

- datos
- trazas
- layout
- ejes
- títulos
- leyendas
- configuración de interacción
- opciones de exportación

Ejemplo:

```python
import plotly.express as px

fig = px.bar(
    x=["A", "B", "C"],
    y=[10, 20, 15]
)

print(type(fig))
```

## Mostrar figuras

## `.show()`

```python
fig.show()
```

Esto muestra la figura usando el renderizador configurado para el entorno.

En notebooks, puede mostrarse dentro de la celda.

En scripts, puede abrirse en navegador según configuración.

## Renderizadores

Plotly permite configurar renderizadores.

```python
import plotly.io as pio

print(pio.renderers.default)
```

Configurar renderizador:

```python
pio.renderers.default = "browser"
```

Otros renderizadores dependen del entorno:

```text
notebook
jupyterlab
browser
vscode
png
svg
```

## Gráfico de líneas

## `px.line()`

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "month": ["Ene", "Feb", "Mar", "Abr"],
    "sales": [100, 120, 90, 150]
})

fig = px.line(
    df,
    x="month",
    y="sales",
    title="Ventas mensuales",
    markers=True
)

fig.show()
```

## Varias líneas

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "month": ["Ene", "Feb", "Mar", "Abr", "Ene", "Feb", "Mar", "Abr"],
    "sales": [100, 120, 90, 150, 80, 95, 110, 130],
    "product": ["A", "A", "A", "A", "B", "B", "B", "B"]
})

fig = px.line(
    df,
    x="month",
    y="sales",
    color="product",
    markers=True,
    title="Ventas mensuales por producto"
)

fig.show()
```

## Gráfico de barras

## `px.bar()`

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "product": ["Laptop", "Mouse", "Teclado"],
    "amount": [3500, 800, 750]
})

fig = px.bar(
    df,
    x="product",
    y="amount",
    title="Monto por producto"
)

fig.show()
```

## Barras horizontales

```python
fig = px.bar(
    df,
    x="amount",
    y="product",
    orientation="h",
    title="Monto por producto"
)

fig.show()
```

## Barras agrupadas

```python
fig = px.bar(
    df,
    x="month",
    y="sales",
    color="product",
    barmode="group",
    title="Ventas por producto"
)

fig.show()
```

## Barras apiladas

```python
fig = px.bar(
    df,
    x="month",
    y="sales",
    color="product",
    barmode="stack",
    title="Ventas apiladas"
)

fig.show()
```

## Gráfico de dispersión

## `px.scatter()`

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "price": [10, 20, 30, 40, 50],
    "sales": [100, 90, 75, 60, 45],
    "category": ["A", "A", "B", "B", "B"]
})

fig = px.scatter(
    df,
    x="price",
    y="sales",
    color="category",
    title="Precio vs ventas"
)

fig.show()
```

## Tamaño de puntos

```python
fig = px.scatter(
    df,
    x="price",
    y="sales",
    size="sales",
    color="category",
    title="Precio vs ventas"
)
```

## Hover personalizado

```python
fig = px.scatter(
    df,
    x="price",
    y="sales",
    color="category",
    hover_data=["category"]
)

fig.show()
```

## Histograma

## `px.histogram()`

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "amount": [100, 120, 150, 180, 200, 220, 300, 350]
})

fig = px.histogram(
    df,
    x="amount",
    nbins=5,
    title="Distribución de montos"
)

fig.show()
```

## Histograma por categoría

```python
fig = px.histogram(
    df,
    x="amount",
    color="category",
    title="Distribución por categoría"
)

fig.show()
```

## Boxplot

## `px.box()`

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "category": ["A", "A", "A", "B", "B", "B"],
    "amount": [100, 120, 150, 200, 220, 300]
})

fig = px.box(
    df,
    x="category",
    y="amount",
    title="Distribución por categoría"
)

fig.show()
```

## Violin plot

```python
fig = px.violin(
    df,
    x="category",
    y="amount",
    box=True,
    points="all",
    title="Distribución por categoría"
)

fig.show()
```

## Gráfico de torta

## `px.pie()`

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "category": ["A", "B", "C"],
    "amount": [40, 35, 25]
})

fig = px.pie(
    df,
    names="category",
    values="amount",
    title="Participación por categoría"
)

fig.show()
```

Los gráficos de torta deben usarse con moderación. Para muchas categorías, suele ser más claro un gráfico de barras.

## Treemap

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "category": ["Tecnología", "Tecnología", "Oficina"],
    "product": ["Laptop", "Mouse", "Silla"],
    "amount": [3500, 800, 1200]
})

fig = px.treemap(
    df,
    path=["category", "product"],
    values="amount",
    title="Monto por categoría y producto"
)

fig.show()
```

## Sunburst

```python
fig = px.sunburst(
    df,
    path=["category", "product"],
    values="amount",
    title="Distribución jerárquica"
)

fig.show()
```

## Mapa de calor

## `px.imshow()`

```python
import pandas as pd
import plotly.express as px

matrix = pd.DataFrame({
    "A": [1.0, 0.5, 0.2],
    "B": [0.5, 1.0, 0.7],
    "C": [0.2, 0.7, 1.0]
}, index=["A", "B", "C"])

fig = px.imshow(
    matrix,
    text_auto=True,
    title="Mapa de calor"
)

fig.show()
```

## Correlaciones

```python
correlation = df[["price", "sales", "amount"]].corr()

fig = px.imshow(
    correlation,
    text_auto=True,
    title="Matriz de correlación"
)

fig.show()
```

## Gráficos 3D

## `px.scatter_3d()`

```python
import pandas as pd
import plotly.express as px

df = pd.DataFrame({
    "x": [1, 2, 3, 4],
    "y": [10, 15, 13, 17],
    "z": [100, 120, 110, 150],
    "category": ["A", "A", "B", "B"]
})

fig = px.scatter_3d(
    df,
    x="x",
    y="y",
    z="z",
    color="category",
    title="Dispersión 3D"
)

fig.show()
```

## Gráfico de área

```python
fig = px.area(
    df,
    x="month",
    y="sales",
    color="product",
    title="Ventas acumuladas por producto"
)

fig.show()
```

## Gráficos financieros

Plotly tiene trazas para gráficos financieros, como velas japonesas.

```python
import pandas as pd
import plotly.graph_objects as go

df = pd.DataFrame({
    "date": pd.to_datetime(["2026-01-01", "2026-01-02", "2026-01-03"]),
    "open": [100, 105, 102],
    "high": [110, 108, 107],
    "low": [95, 100, 98],
    "close": [105, 102, 106]
})

fig = go.Figure(
    data=[
        go.Candlestick(
            x=df["date"],
            open=df["open"],
            high=df["high"],
            low=df["low"],
            close=df["close"]
        )
    ]
)

fig.update_layout(
    title="Gráfico de velas",
    xaxis_title="Fecha",
    yaxis_title="Precio"
)

fig.show()
```

## Personalización de títulos y ejes

## Con Plotly Express

```python
fig = px.bar(
    df,
    x="product",
    y="amount",
    title="Monto por producto",
    labels={
        "product": "Producto",
        "amount": "Monto"
    }
)

fig.show()
```

## Con `update_layout()`

```python
fig.update_layout(
    title="Monto por producto",
    xaxis_title="Producto",
    yaxis_title="Monto"
)
```

## Leyenda

```python
fig.update_layout(
    legend_title_text="Categoría"
)
```

## Plantillas visuales

Plotly incluye plantillas visuales.

```python
fig.update_layout(
    template="plotly_white"
)
```

Plantillas frecuentes:

```text
plotly
plotly_white
plotly_dark
ggplot2
seaborn
simple_white
presentation
```

## Tamaño del gráfico

```python
fig.update_layout(
    width=900,
    height=500
)
```

## Formato de hover

```python
fig.update_traces(
    hovertemplate="Producto: %{x}<br>Monto: %{y}<extra></extra>"
)
```

`<extra></extra>` permite ocultar información adicional del recuadro de hover.

## Texto sobre barras

```python
fig = px.bar(
    df,
    x="product",
    y="amount",
    text="amount",
    title="Monto por producto"
)

fig.update_traces(
    textposition="outside"
)

fig.show()
```

## Formato de ejes

```python
fig.update_yaxes(
    tickformat=",.2f"
)
```

Formato porcentual:

```python
fig.update_yaxes(
    tickformat=".2%"
)
```

## Fechas en el eje X

```python
fig.update_xaxes(
    tickformat="%d/%m/%Y"
)
```

## Rango de ejes

```python
fig.update_xaxes(
    range=["2026-01-01", "2026-12-31"]
)

fig.update_yaxes(
    range=[0, 1000]
)
```

## Subplots

Para crear subplots se usa `make_subplots`.

```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go

fig = make_subplots(
    rows=1,
    cols=2,
    subplot_titles=("Ventas", "Montos")
)

fig.add_trace(
    go.Bar(
        x=["A", "B", "C"],
        y=[10, 20, 15],
        name="Ventas"
    ),
    row=1,
    col=1
)

fig.add_trace(
    go.Scatter(
        x=["A", "B", "C"],
        y=[100, 200, 150],
        mode="lines+markers",
        name="Montos"
    ),
    row=1,
    col=2
)

fig.update_layout(
    title="Resumen por categoría"
)

fig.show()
```

## Facetas con Plotly Express

Plotly Express permite crear facetas con `facet_col` y `facet_row`.

```python
fig = px.scatter(
    df,
    x="price",
    y="sales",
    color="category",
    facet_col="category",
    title="Precio vs ventas por categoría"
)

fig.show()
```

## Animaciones

Plotly Express puede crear animaciones cuando existe una variable temporal o secuencial.

```python
import plotly.express as px

df = px.data.gapminder()

fig = px.scatter(
    df,
    x="gdpPercap",
    y="lifeExp",
    animation_frame="year",
    animation_group="country",
    size="pop",
    color="continent",
    hover_name="country",
    log_x=True,
    size_max=60
)

fig.show()
```

## Exportación a HTML

## `write_html()`

Una ventaja importante de Plotly es exportar gráficos interactivos como HTML.

```python
fig.write_html("grafico.html")
```

Esto permite abrir el gráfico en un navegador.

## HTML completo

```python
fig.write_html(
    "grafico.html",
    full_html=True
)
```

## HTML sin incluir todo el documento

```python
html = fig.to_html(
    full_html=False
)
```

Esto puede servir para insertar el gráfico dentro de otra página HTML.

## Exportación a imagen estática

## `write_image()`

Para exportar a PNG, JPEG, WebP, SVG o PDF, se usa `write_image()`.

```python
fig.write_image("grafico.png")
```

También puede exportarse a PDF:

```python
fig.write_image("grafico.pdf")
```

Para esto suele requerirse Kaleido instalado y configuración compatible.

## Exportación como bytes

```python
image_bytes = fig.to_image(format="png")
```

Esto es útil cuando la imagen se necesita en memoria.

## Uso con pandas

Plotly funciona muy bien con `DataFrame` de pandas.

```python
import pandas as pd
import plotly.express as px

df = pd.read_excel("ventas.xlsx")

summary = (
    df.groupby("Producto")
    .agg(Monto=("Monto", "sum"))
    .reset_index()
)

fig = px.bar(
    summary,
    x="Producto",
    y="Monto",
    title="Monto por producto"
)

fig.show()
```

## Uso con polars

Con Polars, puede convertirse el resultado a pandas cuando sea necesario.

```python
import polars as pl
import plotly.express as px

df = pl.read_csv("ventas.csv", separator=";")

summary = (
    df
    .group_by("Producto")
    .agg(
        pl.col("Monto").sum().alias("Monto")
    )
)

fig = px.bar(
    summary.to_pandas(),
    x="Producto",
    y="Monto",
    title="Monto por producto"
)

fig.show()
```

## Uso con NumPy

```python
import numpy as np
import plotly.express as px

x = np.linspace(0, 10, 100)
y = np.sin(x)

fig = px.line(
    x=x,
    y=y,
    labels={
        "x": "x",
        "y": "sin(x)"
    },
    title="Función seno"
)

fig.show()
```

## Uso en notebooks

En Jupyter Notebook o JupyterLab, `fig.show()` puede mostrar la figura directamente en la salida de la celda.

```python
fig.show()
```

Si el gráfico no aparece, puede revisarse el renderizador:

```python
import plotly.io as pio

print(pio.renderers.default)
```

## Uso en VS Code

En VS Code, Plotly puede mostrarse en notebooks o mediante el navegador, según el renderizador configurado.

```python
import plotly.io as pio

pio.renderers.default = "browser"
```

## Uso con Dash

Plotly se relaciona directamente con Dash, que permite construir aplicaciones web interactivas en Python.

Flujo conceptual:

```text
Plotly -> figura interactiva
Dash   -> aplicación web que muestra y actualiza figuras
```

Ejemplo conceptual:

```python
import plotly.express as px

fig = px.bar(
    x=["A", "B", "C"],
    y=[10, 20, 15]
)
```

Ese objeto `fig` puede mostrarse en una aplicación Dash.

## Uso con Streamlit

Streamlit puede mostrar gráficos Plotly.

```python
import streamlit as st
import plotly.express as px

fig = px.bar(
    x=["A", "B", "C"],
    y=[10, 20, 15]
)

st.plotly_chart(fig)
```

## Uso con documentos Word o PowerPoint

Para insertar gráficos Plotly en Word o PowerPoint, normalmente se exportan como imagen estática.

```python
fig.write_image("grafico.png")
```

Luego se insertan con:

```text
python-docx
python-pptx
```

## Uso con HTML

Para conservar interactividad, conviene exportar como HTML.

```python
fig.write_html("grafico_interactivo.html")
```

Esto es más adecuado que exportar a PNG cuando la interacción es parte importante del análisis.

## Casos de uso frecuentes

## Dashboard exploratorio simple

```python
fig = px.scatter(
    df,
    x="price",
    y="sales",
    color="category",
    hover_data=["product"]
)

fig.show()
```

## Reporte HTML interactivo

```python
fig.write_html("reporte_interactivo.html")
```

## Gráfico para PowerPoint

```python
fig.write_image("grafico.png")
```

## Comparación por categorías

```python
fig = px.bar(
    summary,
    x="category",
    y="amount",
    color="category"
)
```

## Análisis temporal

```python
fig = px.line(
    df,
    x="date",
    y="value",
    color="series"
)
```

## Errores comunes

## Usar Plotly esperando una imagen estática automáticamente

Plotly produce gráficos interactivos. Para imagen estática se necesita exportar explícitamente.

```python
fig.write_image("grafico.png")
```

Esto requiere dependencias adicionales como Kaleido.

## No instalar Kaleido para exportar imágenes

Problemático:

```python
fig.write_image("grafico.png")
```

sin tener instalado Kaleido.

Instalación:

```bash
python -m pip install kaleido
```

## No tener Chrome o Chromium cuando Kaleido lo requiere

En configuraciones recientes, Kaleido puede necesitar Chrome o Chromium compatible.

Puede instalarse con:

```bash
plotly_get_chrome
```

o desde Python:

```python
import plotly.io as pio

pio.get_chrome()
```

## Confundir HTML con imagen

```python
fig.write_html("grafico.html")
```

genera un archivo interactivo para navegador.

```python
fig.write_image("grafico.png")
```

genera una imagen estática.

## Usar demasiados puntos sin optimización

Los gráficos interactivos pueden volverse pesados si se grafican demasiados puntos.

En esos casos conviene:

- agregar datos
- filtrar datos
- muestrear datos
- usar WebGL cuando corresponda
- evaluar herramientas especializadas para series muy grandes

## Usar colores excesivos

Demasiadas categorías en `color` pueden hacer que el gráfico sea difícil de interpretar.

## No controlar el hover

El hover puede mostrar demasiada información si no se define con cuidado.

```python
hover_data=["columna_relevante"]
```

## No revisar datos antes de graficar

Conviene revisar:

```python
df.head()
df.dtypes
df.isna().sum()
```

antes de construir gráficos.

## Usar gráfico incorrecto

Regla práctica:

```text
línea       -> evolución temporal
barras      -> comparación entre categorías
scatter     -> relación entre variables numéricas
histograma  -> distribución
boxplot     -> dispersión por categoría
heatmap     -> matrices o correlaciones
treemap     -> jerarquías y composición
```

## Buenas prácticas

## Usar Plotly Express para empezar

```python
import plotly.express as px
```

## Pasar DataFrames con nombres de columnas claros

```python
fig = px.bar(
    df,
    x="Producto",
    y="Monto"
)
```

## Usar títulos y etiquetas explícitas

```python
labels={
    "Producto": "Producto",
    "Monto": "Monto total"
}
```

## Usar `update_layout()` para ajustes generales

```python
fig.update_layout(
    title="Reporte de ventas",
    template="plotly_white"
)
```

## Usar `update_traces()` para ajustar trazas

```python
fig.update_traces(
    textposition="outside"
)
```

## Exportar a HTML cuando se quiera conservar interactividad

```python
fig.write_html("grafico.html")
```

## Exportar a imagen solo cuando el destino sea estático

```python
fig.write_image("grafico.png")
```

## Separar preparación de datos y visualización

```python
summary = prepare_summary(df)
fig = create_chart(summary)
```

## Mantener gráficos simples

La interactividad no reemplaza la claridad del diseño.

Conviene evitar demasiadas series, colores, textos o elementos visuales en un solo gráfico.

## Ejemplo integrado

```python
from pathlib import Path

import pandas as pd
import plotly.express as px


def load_sales(input_path):
    return pd.read_excel(input_path)


def summarize_sales(df):
    summary = (
        df.groupby("Producto")
        .agg(Monto=("Monto", "sum"))
        .reset_index()
        .sort_values("Monto", ascending=False)
    )

    return summary


def create_sales_chart(summary):
    fig = px.bar(
        summary,
        x="Producto",
        y="Monto",
        text="Monto",
        title="Monto total por producto",
        labels={
            "Producto": "Producto",
            "Monto": "Monto total"
        },
        template="plotly_white"
    )

    fig.update_traces(
        textposition="outside",
        hovertemplate="Producto: %{x}<br>Monto: %{y:,.2f}<extra></extra>"
    )

    fig.update_layout(
        xaxis_title="Producto",
        yaxis_title="Monto total",
        showlegend=False
    )

    return fig


input_path = Path("ventas.xlsx")
html_output_path = Path("grafico_ventas.html")
image_output_path = Path("grafico_ventas.png")

sales = load_sales(input_path)
summary = summarize_sales(sales)

fig = create_sales_chart(summary)

fig.write_html(html_output_path)

try:
    fig.write_image(image_output_path)
except ValueError:
    print("No se pudo exportar la imagen estática. Revisar instalación de Kaleido y Chrome.")

print("Gráfico interactivo generado correctamente")
```

## Ejemplo con Graph Objects

```python
import plotly.graph_objects as go

months = ["Ene", "Feb", "Mar", "Abr"]
sales_2025 = [100, 120, 90, 150]
sales_2026 = [110, 130, 115, 170]

fig = go.Figure()

fig.add_trace(
    go.Scatter(
        x=months,
        y=sales_2025,
        mode="lines+markers",
        name="2025"
    )
)

fig.add_trace(
    go.Scatter(
        x=months,
        y=sales_2026,
        mode="lines+markers",
        name="2026"
    )
)

fig.update_layout(
    title="Ventas mensuales",
    xaxis_title="Mes",
    yaxis_title="Ventas",
    template="plotly_white"
)

fig.show()
```

## Relación con otras librerías

`plotly` se relaciona especialmente con:

- `pandas`, para trabajar con datos tabulares
- `polars`, cuando los datos se preparan en Polars y luego se convierten para graficar
- `numpy`, para graficar arreglos y funciones
- `matplotlib`, como alternativa estática más tradicional
- `seaborn`, como alternativa estadística basada en Matplotlib
- `dash`, para construir aplicaciones web interactivas con gráficos Plotly
- `streamlit`, para mostrar gráficos interactivos en aplicaciones rápidas
- `kaleido`, para exportar figuras como imágenes estáticas
- `python-docx`, para insertar imágenes exportadas en Word
- `python-pptx`, para insertar imágenes exportadas en PowerPoint

## Orden didáctico interno

```text
1. Propósito de plotly
2. Instalación e importación
3. Plotly Express y Graph Objects
4. Objeto Figure
5. Mostrar figuras con show()
6. Gráficos principales: línea, barras, scatter, histograma, boxplot y heatmap
7. Personalización con update_layout() y update_traces()
8. Hover, leyendas, plantillas y ejes
9. Subplots, facetas y animaciones
10. Exportación a HTML
11. Exportación a imágenes con Kaleido
12. Uso con pandas, polars y numpy
13. Relación con Dash, Streamlit y reportes
14. Errores comunes
15. Buenas prácticas
```