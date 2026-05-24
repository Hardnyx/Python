El siguiente archivo debería ser:

```text
docs/04-external-libraries/06-visualizacion/matplotlib.md
```

Contenido propuesto:

````markdown
# `matplotlib`

## Propósito

`matplotlib` es una librería externa para crear gráficos en Python. Se utiliza para visualizar datos mediante gráficos de líneas, barras, dispersión, histogramas, áreas, cajas, mapas de calor simples, gráficos de torta, figuras compuestas y visualizaciones personalizadas.

Es una de las librerías más importantes del ecosistema de visualización en Python y sirve como base para otras herramientas de visualización, incluyendo integraciones con `pandas` y `seaborn`.

## Naturaleza de la librería

`matplotlib` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install matplotlib
```

La importación más habitual es:

```python
import matplotlib.pyplot as plt
```

El alias `plt` es la convención estándar usada en documentación, ejemplos y proyectos reales.

## Idea central

La idea principal de `matplotlib` es crear figuras y ejes sobre los cuales se dibujan gráficos.

Ejemplo básico:

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [10, 20, 15, 25]

plt.plot(x, y)
plt.show()
```

Este código crea un gráfico de líneas simple.

## Instalación

Instalación básica:

```bash
python -m pip install matplotlib
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
matplotlib==3.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
import matplotlib.pyplot as plt
```

Verificación:

```python
import matplotlib

print(matplotlib.__version__)
```

## `pyplot`

`matplotlib.pyplot` es el módulo más usado para construir gráficos de manera práctica.

```python
import matplotlib.pyplot as plt
```

Permite usar funciones como:

```python
plt.plot()
plt.bar()
plt.scatter()
plt.hist()
plt.xlabel()
plt.ylabel()
plt.title()
plt.legend()
plt.grid()
plt.show()
plt.savefig()
```

## Figure y Axes

Matplotlib trabaja con dos conceptos centrales:

```text
Figure -> figura completa
Axes   -> área específica donde se dibuja un gráfico
```

## `Figure`

Representa la figura completa.

Puede contener uno o varios gráficos.

## `Axes`

Representa el área donde se dibujan los datos.

Un `Axes` puede tener:

- eje X
- eje Y
- título
- etiquetas
- leyenda
- grilla
- líneas
- barras
- puntos
- anotaciones

## Forma recomendada con `subplots()`

Aunque `pyplot` permite escribir gráficos de forma rápida, en proyectos más ordenados suele preferirse usar `fig, ax`.

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [10, 20, 15, 25]

fig, ax = plt.subplots()

ax.plot(x, y)
ax.set_title("Ventas")
ax.set_xlabel("Periodo")
ax.set_ylabel("Monto")

plt.show()
```

Este estilo es más claro cuando se crean gráficos complejos o reutilizables.

## Diferencia entre `plt` y `ax`

## Estilo con `plt`

```python
plt.plot(x, y)
plt.title("Ventas")
plt.xlabel("Periodo")
plt.ylabel("Monto")
plt.show()
```

Es simple y útil para exploración rápida.

## Estilo con `ax`

```python
fig, ax = plt.subplots()

ax.plot(x, y)
ax.set_title("Ventas")
ax.set_xlabel("Periodo")
ax.set_ylabel("Monto")

plt.show()
```

Es más estructurado y recomendable para código organizado.

## Regla práctica

Para pruebas rápidas, `plt` puede ser suficiente.

Para scripts, reportes, funciones y proyectos mantenibles, conviene usar `fig, ax`.

## Gráfico de líneas

## `plot()`

```python
import matplotlib.pyplot as plt

months = ["Ene", "Feb", "Mar", "Abr"]
sales = [100, 120, 90, 150]

fig, ax = plt.subplots()

ax.plot(months, sales)

ax.set_title("Ventas mensuales")
ax.set_xlabel("Mes")
ax.set_ylabel("Ventas")

plt.show()
```

## Varias líneas

```python
import matplotlib.pyplot as plt

months = ["Ene", "Feb", "Mar", "Abr"]
sales_2025 = [100, 120, 90, 150]
sales_2026 = [110, 130, 115, 170]

fig, ax = plt.subplots()

ax.plot(months, sales_2025, label="2025")
ax.plot(months, sales_2026, label="2026")

ax.set_title("Ventas mensuales")
ax.set_xlabel("Mes")
ax.set_ylabel("Ventas")
ax.legend()

plt.show()
```

## Gráfico de barras

## `bar()`

```python
import matplotlib.pyplot as plt

products = ["Laptop", "Mouse", "Teclado"]
amounts = [3500, 800, 750]

fig, ax = plt.subplots()

ax.bar(products, amounts)

ax.set_title("Monto por producto")
ax.set_xlabel("Producto")
ax.set_ylabel("Monto")

plt.show()
```

## Barras horizontales

## `barh()`

```python
import matplotlib.pyplot as plt

products = ["Laptop", "Mouse", "Teclado"]
amounts = [3500, 800, 750]

fig, ax = plt.subplots()

ax.barh(products, amounts)

ax.set_title("Monto por producto")
ax.set_xlabel("Monto")
ax.set_ylabel("Producto")

plt.show()
```

## Gráfico de dispersión

## `scatter()`

```python
import matplotlib.pyplot as plt

prices = [10, 20, 30, 40, 50]
sales = [100, 90, 70, 60, 40]

fig, ax = plt.subplots()

ax.scatter(prices, sales)

ax.set_title("Relación entre precio y ventas")
ax.set_xlabel("Precio")
ax.set_ylabel("Ventas")

plt.show()
```

Este tipo de gráfico es útil para observar relaciones entre dos variables numéricas.

## Histograma

## `hist()`

```python
import matplotlib.pyplot as plt

values = [10, 12, 15, 18, 18, 20, 22, 25, 30, 35]

fig, ax = plt.subplots()

ax.hist(values, bins=5)

ax.set_title("Distribución de valores")
ax.set_xlabel("Valor")
ax.set_ylabel("Frecuencia")

plt.show()
```

## Caja y bigotes

## `boxplot()`

```python
import matplotlib.pyplot as plt

values = [10, 12, 15, 18, 20, 22, 25, 30, 35]

fig, ax = plt.subplots()

ax.boxplot(values)

ax.set_title("Distribución de valores")
ax.set_ylabel("Valor")

plt.show()
```

Este gráfico ayuda a visualizar mediana, dispersión y valores atípicos.

## Gráfico de torta

## `pie()`

```python
import matplotlib.pyplot as plt

labels = ["A", "B", "C"]
values = [40, 35, 25]

fig, ax = plt.subplots()

ax.pie(
    values,
    labels=labels,
    autopct="%1.1f%%"
)

ax.set_title("Participación")

plt.show()
```

Los gráficos de torta deben usarse con moderación. Para muchas categorías, un gráfico de barras suele ser más claro.

## Gráfico de área

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [10, 20, 15, 25]

fig, ax = plt.subplots()

ax.fill_between(x, y)

ax.set_title("Área bajo la serie")
ax.set_xlabel("Periodo")
ax.set_ylabel("Valor")

plt.show()
```

## Etiquetas y título

## Título

```python
ax.set_title("Título del gráfico")
```

## Etiqueta del eje X

```python
ax.set_xlabel("Eje X")
```

## Etiqueta del eje Y

```python
ax.set_ylabel("Eje Y")
```

## Límites de ejes

```python
ax.set_xlim(0, 10)
ax.set_ylim(0, 100)
```

## Leyenda

Para mostrar leyenda, cada serie debe tener `label`.

```python
ax.plot(x, y, label="Serie A")
ax.legend()
```

Ubicación de leyenda:

```python
ax.legend(loc="upper left")
```

## Grilla

```python
ax.grid(True)
```

También puede aplicarse solo a un eje:

```python
ax.grid(True, axis="y")
```

## Tamaño de figura

El tamaño se define con `figsize`.

```python
fig, ax = plt.subplots(figsize=(8, 5))
```

La unidad es pulgadas.

```text
figsize=(ancho, alto)
```

## Resolución

Al guardar imágenes, puede indicarse `dpi`.

```python
fig.savefig("grafico.png", dpi=300)
```

Un `dpi` mayor genera una imagen con más resolución, pero también más peso.

## Guardar gráficos

## `savefig()`

```python
fig.savefig("grafico.png")
```

También puede guardarse como PDF:

```python
fig.savefig("grafico.pdf")
```

O como SVG:

```python
fig.savefig("grafico.svg")
```

## Guardar con márgenes ajustados

```python
fig.savefig(
    "grafico.png",
    dpi=300,
    bbox_inches="tight"
)
```

`bbox_inches="tight"` ayuda a evitar que etiquetas o títulos queden recortados.

## Mostrar gráficos

## `plt.show()`

```python
plt.show()
```

Muestra la figura en pantalla o en el entorno interactivo.

En scripts que solo generan archivos, puede no ser necesario mostrar el gráfico.

## Cerrar figuras

Cuando se generan muchos gráficos, conviene cerrar figuras para liberar memoria.

```python
plt.close(fig)
```

Ejemplo:

```python
fig, ax = plt.subplots()
ax.plot([1, 2, 3], [10, 20, 15])
fig.savefig("grafico.png")
plt.close(fig)
```

## Uso con NumPy

Matplotlib funciona muy bien con arreglos de NumPy.

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 10, 100)
y = np.sin(x)

fig, ax = plt.subplots()

ax.plot(x, y)

ax.set_title("Función seno")
ax.set_xlabel("x")
ax.set_ylabel("sin(x)")

plt.show()
```

## Uso con pandas

Pandas puede graficar usando Matplotlib internamente.

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "month": ["Ene", "Feb", "Mar", "Abr"],
    "sales": [100, 120, 90, 150]
})

fig, ax = plt.subplots()

ax.plot(df["month"], df["sales"])

ax.set_title("Ventas mensuales")
ax.set_xlabel("Mes")
ax.set_ylabel("Ventas")

plt.show()
```

## Gráfico desde DataFrame

```python
df.plot(
    x="month",
    y="sales",
    kind="line"
)

plt.show()
```

Aunque pandas tiene métodos de graficación propios, para mayor control visual conviene usar Matplotlib directamente.

## Fechas en el eje X

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "date": pd.to_datetime([
        "2026-01-01",
        "2026-02-01",
        "2026-03-01"
    ]),
    "value": [100, 120, 115]
})

fig, ax = plt.subplots()

ax.plot(df["date"], df["value"])

ax.set_title("Serie temporal")
ax.set_xlabel("Fecha")
ax.set_ylabel("Valor")

fig.autofmt_xdate()

plt.show()
```

`fig.autofmt_xdate()` ayuda a rotar y acomodar etiquetas de fechas.

## Rotar etiquetas

```python
ax.tick_params(axis="x", rotation=45)
```

Ejemplo:

```python
fig, ax = plt.subplots()

ax.bar(products, amounts)
ax.tick_params(axis="x", rotation=45)

plt.show()
```

## Anotaciones

## `annotate()`

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [10, 20, 15, 25]

fig, ax = plt.subplots()

ax.plot(x, y)

ax.annotate(
    "Máximo",
    xy=(4, 25),
    xytext=(3, 27),
    arrowprops={"arrowstyle": "->"}
)

plt.show()
```

## Texto libre

```python
ax.text(
    2,
    20,
    "Punto relevante"
)
```

## Líneas de referencia

## Línea horizontal

```python
ax.axhline(100)
```

## Línea vertical

```python
ax.axvline(2026)
```

Ejemplo:

```python
fig, ax = plt.subplots()

ax.plot(x, y)
ax.axhline(15)

plt.show()
```

## Múltiples gráficos en una figura

## `subplots()`

```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(2, 1, figsize=(8, 6))

axes[0].plot([1, 2, 3], [10, 20, 15])
axes[0].set_title("Serie A")

axes[1].bar(["A", "B", "C"], [5, 7, 3])
axes[1].set_title("Serie B")

plt.tight_layout()
plt.show()
```

## `tight_layout()`

```python
plt.tight_layout()
```

Ajusta espacios para reducir superposición entre títulos, ejes y etiquetas.

Con el estilo orientado a objetos:

```python
fig.tight_layout()
```

## Compartir ejes

```python
fig, axes = plt.subplots(2, 1, sharex=True)
```

Esto es útil cuando varios gráficos comparten el mismo eje X.

## Colores y estilos

Matplotlib permite personalizar colores, líneas, marcadores y estilos.

## Color

```python
ax.plot(x, y, color="blue")
```

## Tipo de línea

```python
ax.plot(x, y, linestyle="--")
```

## Marcadores

```python
ax.plot(x, y, marker="o")
```

## Grosor de línea

```python
ax.plot(x, y, linewidth=2)
```

## Transparencia

```python
ax.scatter(x, y, alpha=0.7)
```

## Personalización combinada

```python
ax.plot(
    x,
    y,
    color="blue",
    linestyle="--",
    marker="o",
    linewidth=2
)
```

## Mapas de calor simples

```python
import numpy as np
import matplotlib.pyplot as plt

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

fig, ax = plt.subplots()

image = ax.imshow(matrix)

fig.colorbar(image, ax=ax)

ax.set_title("Mapa de calor")

plt.show()
```

## Barras de error

```python
import matplotlib.pyplot as plt

x = [1, 2, 3]
y = [10, 15, 13]
errors = [1, 2, 1]

fig, ax = plt.subplots()

ax.errorbar(x, y, yerr=errors, marker="o")

ax.set_title("Valores con error")
ax.set_xlabel("x")
ax.set_ylabel("y")

plt.show()
```

## Escalas logarítmicas

## Eje Y logarítmico

```python
ax.set_yscale("log")
```

## Eje X logarítmico

```python
ax.set_xscale("log")
```

Ejemplo:

```python
fig, ax = plt.subplots()

ax.plot([1, 10, 100], [1, 100, 10000])
ax.set_xscale("log")
ax.set_yscale("log")

plt.show()
```

## Exportación para reportes

Matplotlib se usa frecuentemente para generar imágenes que luego se insertan en Word, PowerPoint o Excel.

```python
fig.savefig(
    "grafico_reporte.png",
    dpi=300,
    bbox_inches="tight"
)
```

Luego la imagen puede insertarse con:

```text
python-docx
python-pptx
openpyxl
xlsxwriter
```

## Uso con `python-docx`

```python
import matplotlib.pyplot as plt
from docx import Document
from docx.shared import Inches

fig, ax = plt.subplots()
ax.plot([1, 2, 3], [10, 20, 15])
fig.savefig("grafico.png", dpi=300, bbox_inches="tight")
plt.close(fig)

document = Document()
document.add_picture("grafico.png", width=Inches(5))
document.save("reporte.docx")
```

## Uso con `python-pptx`

```python
import matplotlib.pyplot as plt
from pptx import Presentation
from pptx.util import Inches

fig, ax = plt.subplots()
ax.bar(["A", "B", "C"], [10, 20, 15])
fig.savefig("grafico.png", dpi=300, bbox_inches="tight")
plt.close(fig)

presentation = Presentation()
slide = presentation.slides.add_slide(presentation.slide_layouts[6])
slide.shapes.add_picture("grafico.png", Inches(1), Inches(1), width=Inches(8))
presentation.save("reporte.pptx")
```

## Uso con `openpyxl`

```python
import matplotlib.pyplot as plt
from openpyxl import Workbook
from openpyxl.drawing.image import Image

fig, ax = plt.subplots()
ax.plot([1, 2, 3], [10, 20, 15])
fig.savefig("grafico.png", dpi=300, bbox_inches="tight")
plt.close(fig)

workbook = Workbook()
worksheet = workbook.active

image = Image("grafico.png")
worksheet.add_image(image, "A1")

workbook.save("reporte.xlsx")
```

## Casos de uso frecuentes

## Graficar una serie temporal

```python
fig, ax = plt.subplots()

ax.plot(df["date"], df["value"])
ax.set_title("Serie temporal")
ax.set_xlabel("Fecha")
ax.set_ylabel("Valor")

fig.autofmt_xdate()

plt.show()
```

## Comparar categorías

```python
fig, ax = plt.subplots()

ax.bar(df["category"], df["amount"])
ax.set_title("Monto por categoría")

plt.show()
```

## Explorar una distribución

```python
fig, ax = plt.subplots()

ax.hist(df["amount"], bins=20)
ax.set_title("Distribución de montos")

plt.show()
```

## Analizar relación entre variables

```python
fig, ax = plt.subplots()

ax.scatter(df["price"], df["sales"])
ax.set_title("Precio vs ventas")
ax.set_xlabel("Precio")
ax.set_ylabel("Ventas")

plt.show()
```

## Errores comunes

## Olvidar `plt.show()`

En algunos entornos, si no se usa:

```python
plt.show()
```

el gráfico puede no mostrarse.

## No cerrar figuras al generar muchas imágenes

Problemático:

```python
for item in items:
    fig, ax = plt.subplots()
    ax.plot(item.x, item.y)
    fig.savefig(f"{item.name}.png")
```

Mejor:

```python
for item in items:
    fig, ax = plt.subplots()
    ax.plot(item.x, item.y)
    fig.savefig(f"{item.name}.png")
    plt.close(fig)
```

## Mezclar excesivamente `plt` y `ax`

Menos claro:

```python
fig, ax = plt.subplots()
plt.plot(x, y)
ax.set_title("Título")
```

Más consistente:

```python
fig, ax = plt.subplots()
ax.plot(x, y)
ax.set_title("Título")
```

## No ajustar etiquetas largas

Las etiquetas pueden superponerse.

Soluciones frecuentes:

```python
ax.tick_params(axis="x", rotation=45)
fig.tight_layout()
```

## Guardar gráficos con elementos recortados

Problemático:

```python
fig.savefig("grafico.png")
```

si etiquetas o títulos quedan fuera.

Más seguro:

```python
fig.savefig("grafico.png", bbox_inches="tight")
```

## Graficar datos sin limpiar

Datos faltantes, textos donde deberían haber números o fechas mal interpretadas pueden generar gráficos engañosos.

Conviene revisar:

```python
df.dtypes
df.isna().sum()
df.head()
```

## Usar gráfico inadecuado

Ejemplos frecuentes:

```text
muchas categorías -> evitar torta
serie temporal -> preferir línea
distribución -> histograma o boxplot
relación entre dos variables numéricas -> dispersión
comparación categórica -> barras
```

## No etiquetar ejes

Un gráfico sin títulos ni etiquetas puede ser difícil de interpretar.

```python
ax.set_title(...)
ax.set_xlabel(...)
ax.set_ylabel(...)
```

## Buenas prácticas

## Usar `fig, ax`

```python
fig, ax = plt.subplots()
```

## Etiquetar siempre el gráfico

```python
ax.set_title("Título")
ax.set_xlabel("Eje X")
ax.set_ylabel("Eje Y")
```

## Usar leyenda cuando haya varias series

```python
ax.legend()
```

## Ajustar el layout antes de mostrar o guardar

```python
fig.tight_layout()
```

## Guardar con resolución suficiente

```python
fig.savefig("grafico.png", dpi=300, bbox_inches="tight")
```

## Cerrar figuras en procesos por lotes

```python
plt.close(fig)
```

## Separar preparación de datos y graficación

```python
summary = prepare_summary(df)
create_chart(summary, output_path)
```

## Usar el tipo de gráfico según el objetivo

```text
línea       -> evolución
barras      -> comparación
histograma  -> distribución
dispersión  -> relación
boxplot     -> dispersión y atípicos
```

## Ejemplo integrado

```python
from pathlib import Path

import pandas as pd
import matplotlib.pyplot as plt


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


def create_sales_chart(summary, output_path):
    fig, ax = plt.subplots(figsize=(8, 5))

    ax.bar(summary["Producto"], summary["Monto"])

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
output_path = Path("grafico_ventas.png")

sales = load_sales(input_path)
summary = summarize_sales(sales)

create_sales_chart(summary, output_path)

print("Gráfico generado correctamente")
```

## Relación con otras librerías

`matplotlib` se relaciona especialmente con:

- `numpy`, para graficar arreglos numéricos y funciones
- `pandas`, para visualizar DataFrames y Series
- `scipy`, para graficar resultados científicos y estadísticos
- `statsmodels`, para visualizar modelos, residuos y series
- `seaborn`, como capa de visualización estadística sobre Matplotlib
- `plotly`, como alternativa para gráficos interactivos
- `pillow`, para procesar imágenes generadas
- `python-docx`, para insertar gráficos en Word
- `python-pptx`, para insertar gráficos en PowerPoint
- `openpyxl` y `xlsxwriter`, para insertar gráficos como imágenes en Excel

## Orden didáctico interno

```text
1. Propósito de matplotlib
2. Instalación e importación
3. Figure y Axes
4. Uso con plt y uso con fig, ax
5. Gráficos principales: línea, barras, dispersión, histograma y boxplot
6. Títulos, etiquetas, leyendas y grilla
7. Tamaño, resolución y guardado
8. Fechas, rotación de etiquetas y anotaciones
9. Múltiples gráficos con subplots()
10. Uso con numpy y pandas
11. Exportación para reportes
12. Errores comunes
13. Buenas prácticas
```