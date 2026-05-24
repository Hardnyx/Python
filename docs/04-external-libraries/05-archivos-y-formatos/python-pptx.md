# `python-pptx`

## Propósito

`python-pptx` es una librería externa para crear, leer y modificar presentaciones de Microsoft PowerPoint en formato `.pptx`.

Se utiliza para generar presentaciones automáticamente, crear reportes ejecutivos, insertar texto, imágenes, tablas, gráficos, formas, títulos, diapositivas y contenido basado en datos provenientes de archivos, APIs, bases de datos o análisis realizados en Python.

## Naturaleza de la librería

`python-pptx` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install python-pptx
```

La importación habitual es:

```python
from pptx import Presentation
```

El nombre de instalación es `python-pptx`, pero el paquete se importa como `pptx`.

## Idea central

La idea principal de `python-pptx` es manipular una presentación como un conjunto de objetos.

```text
Presentation -> Slide -> Shape
Presentation -> Slide -> Placeholder
Presentation -> Slide -> TextFrame -> Paragraph -> Run
```

Es decir:

```text
presentación -> diapositiva -> forma
presentación -> diapositiva -> marcador de posición
presentación -> diapositiva -> caja de texto -> párrafo -> fragmento
```

Ejemplo básico:

```python
from pptx import Presentation

presentation = Presentation()

slide_layout = presentation.slide_layouts[0]
slide = presentation.slides.add_slide(slide_layout)

slide.shapes.title.text = "Reporte"
slide.placeholders[1].text = "Presentación generada con Python"

presentation.save("reporte.pptx")
```

## Cuándo usar `python-pptx`

Conviene usar `python-pptx` cuando se necesita:

- crear presentaciones `.pptx`
- generar reportes automatizados
- completar plantillas de PowerPoint
- insertar tablas con resultados
- insertar imágenes y gráficos exportados
- crear presentaciones desde datos de Excel, CSV, APIs o bases de datos
- automatizar diapositivas repetitivas
- producir entregables visuales desde Python

## Cuándo no usar `python-pptx`

No conviene usar `python-pptx` cuando se necesita:

- trabajar con archivos `.ppt` antiguos
- convertir PowerPoint a PDF directamente
- ejecutar macros
- controlar animaciones complejas
- editar todos los aspectos visuales avanzados de PowerPoint
- reemplazar completamente el trabajo de diseño manual en presentaciones muy gráficas

Para conversión a PDF suelen requerirse herramientas externas, como PowerPoint, LibreOffice u otros motores de conversión.

## Instalación

Instalación básica:

```bash
python -m pip install python-pptx
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
python-pptx==1.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
from pptx import Presentation
```

Importaciones frecuentes adicionales:

```python
from pptx.util import Inches, Pt
from pptx.dml.color import RGBColor
from pptx.enum.text import PP_ALIGN
from pptx.enum.shapes import MSO_SHAPE
from pptx.enum.chart import XL_CHART_TYPE
```

Verificación:

```python
import pptx

print(pptx.__version__)
```

## Crear una presentación nueva

```python
from pptx import Presentation

presentation = Presentation()

presentation.save("presentacion.pptx")
```

## Abrir una presentación existente

```python
from pptx import Presentation

presentation = Presentation("plantilla.pptx")

print(len(presentation.slides))
```

## Guardar una presentación

```python
presentation.save("salida.pptx")
```

Si se guarda con el mismo nombre del archivo original, se sobrescribe.

Para conservar el original, conviene guardar con otro nombre:

```python
presentation.save("salida_generada.pptx")
```

## Objeto `Presentation`

`Presentation` representa el archivo PowerPoint completo.

Desde este objeto se puede acceder a:

- diapositivas
- diseños de diapositiva
- tamaños de página
- propiedades del documento
- temas y elementos existentes de una plantilla
- contenido editable de las diapositivas

Ejemplo:

```python
from pptx import Presentation

presentation = Presentation()

print(type(presentation))
```

## Tamaño de diapositiva

La presentación tiene ancho y alto.

```python
from pptx import Presentation
from pptx.util import Inches

presentation = Presentation()

presentation.slide_width = Inches(13.333)
presentation.slide_height = Inches(7.5)

presentation.save("widescreen.pptx")
```

Formato panorámico habitual:

```text
13.333 x 7.5 pulgadas
```

## Diseños de diapositiva

PowerPoint trabaja con diseños predefinidos llamados `slide_layouts`.

```python
slide_layout = presentation.slide_layouts[0]
```

Algunos índices frecuentes son:

```text
0 -> título
1 -> título y contenido
5 -> solo título
6 -> diapositiva en blanco
```

El significado exacto puede variar según la plantilla usada.

## Agregar una diapositiva

```python
slide_layout = presentation.slide_layouts[0]
slide = presentation.slides.add_slide(slide_layout)
```

## Diapositiva de título

```python
from pptx import Presentation

presentation = Presentation()

slide_layout = presentation.slide_layouts[0]
slide = presentation.slides.add_slide(slide_layout)

slide.shapes.title.text = "Reporte mensual"
slide.placeholders[1].text = "Generado automáticamente"

presentation.save("titulo.pptx")
```

## Diapositiva en blanco

```python
slide_layout = presentation.slide_layouts[6]
slide = presentation.slides.add_slide(slide_layout)
```

Una diapositiva en blanco es útil cuando se desea controlar manualmente la posición de todos los elementos.

## Acceder a diapositivas

```python
for slide in presentation.slides:
    print(slide)
```

Cantidad de diapositivas:

```python
print(len(presentation.slides))
```

Primera diapositiva:

```python
slide = presentation.slides[0]
```

## Placeholders

Los placeholders son marcadores definidos por el diseño de la diapositiva.

Ejemplo:

```python
slide.shapes.title.text = "Título"
slide.placeholders[1].text = "Subtítulo"
```

No todos los diseños tienen los mismos placeholders.

## Inspeccionar placeholders

```python
for placeholder in slide.placeholders:
    print(placeholder.placeholder_format.idx, placeholder.name)
```

Esto ayuda a identificar qué marcador corresponde a cada parte del diseño.

## Agregar texto

## Usar placeholder de título

```python
slide.shapes.title.text = "Título de la diapositiva"
```

## Agregar caja de texto

```python
from pptx.util import Inches

left = Inches(1)
top = Inches(1.5)
width = Inches(6)
height = Inches(1)

text_box = slide.shapes.add_textbox(left, top, width, height)
text_frame = text_box.text_frame

text_frame.text = "Texto agregado con Python"
```

## TextFrame

Una caja de texto contiene un `text_frame`.

```python
text_frame = text_box.text_frame
```

El `text_frame` contiene párrafos.

```python
paragraph = text_frame.paragraphs[0]
```

## Párrafos

```python
paragraph = text_frame.paragraphs[0]
paragraph.text = "Primer párrafo"
```

Agregar otro párrafo:

```python
paragraph = text_frame.add_paragraph()
paragraph.text = "Segundo párrafo"
```

## Runs

Un run es un fragmento de texto dentro de un párrafo con formato propio.

```python
paragraph = text_frame.paragraphs[0]

run = paragraph.add_run()
run.text = "Texto destacado"
run.font.bold = True
```

## Formato de texto

## Tamaño de fuente

```python
from pptx.util import Pt

run.font.size = Pt(18)
```

## Negrita

```python
run.font.bold = True
```

## Cursiva

```python
run.font.italic = True
```

## Color de fuente

```python
from pptx.dml.color import RGBColor

run.font.color.rgb = RGBColor(31, 78, 121)
```

## Nombre de fuente

```python
run.font.name = "Arial"
```

## Alineación de párrafo

```python
from pptx.enum.text import PP_ALIGN

paragraph.alignment = PP_ALIGN.CENTER
```

Opciones frecuentes:

```python
PP_ALIGN.LEFT
PP_ALIGN.CENTER
PP_ALIGN.RIGHT
PP_ALIGN.JUSTIFY
```

## Viñetas

```python
text_frame = text_box.text_frame
text_frame.clear()

items = ["Primer punto", "Segundo punto", "Tercer punto"]

for index, item in enumerate(items):
    if index == 0:
        paragraph = text_frame.paragraphs[0]
    else:
        paragraph = text_frame.add_paragraph()

    paragraph.text = item
    paragraph.level = 0
```

## Niveles de viñeta

```python
paragraph.level = 0
```

Subnivel:

```python
paragraph.level = 1
```

## Agregar imágenes

```python
from pptx.util import Inches

slide.shapes.add_picture(
    "imagen.png",
    Inches(1),
    Inches(1),
    width=Inches(4)
)
```

Con ancho y alto:

```python
slide.shapes.add_picture(
    "imagen.png",
    Inches(1),
    Inches(1),
    width=Inches(4),
    height=Inches(3)
)
```

Si solo se indica ancho o alto, se conserva la proporción.

## Agregar formas

```python
from pptx.enum.shapes import MSO_SHAPE
from pptx.util import Inches

shape = slide.shapes.add_shape(
    MSO_SHAPE.ROUNDED_RECTANGLE,
    Inches(1),
    Inches(1),
    Inches(3),
    Inches(1)
)

shape.text = "Proceso"
```

## Formato de formas

## Relleno

```python
from pptx.dml.color import RGBColor

shape.fill.solid()
shape.fill.fore_color.rgb = RGBColor(31, 78, 121)
```

## Línea

```python
shape.line.color.rgb = RGBColor(255, 255, 255)
```

## Texto dentro de forma

```python
shape.text = "Texto"
```

También puede trabajarse con `shape.text_frame` para mayor control.

## Agregar líneas

```python
from pptx.enum.shapes import MSO_CONNECTOR
from pptx.util import Inches

line = slide.shapes.add_connector(
    MSO_CONNECTOR.STRAIGHT,
    Inches(1),
    Inches(1),
    Inches(4),
    Inches(1)
)
```

## Agregar tablas

```python
from pptx.util import Inches

rows = 3
cols = 3

table_shape = slide.shapes.add_table(
    rows,
    cols,
    Inches(1),
    Inches(1.5),
    Inches(8),
    Inches(2)
)

table = table_shape.table
```

## Escribir en celdas de tabla

```python
table.cell(0, 0).text = "Producto"
table.cell(0, 1).text = "Cantidad"
table.cell(0, 2).text = "Monto"

table.cell(1, 0).text = "Laptop"
table.cell(1, 1).text = "2"
table.cell(1, 2).text = "3500"
```

## Crear tabla desde datos

```python
from pptx.util import Inches

records = [
    ["Producto", "Cantidad", "Monto"],
    ["Laptop", "2", "3500"],
    ["Mouse", "10", "80"]
]

rows = len(records)
cols = len(records[0])

table_shape = slide.shapes.add_table(
    rows,
    cols,
    Inches(1),
    Inches(1.5),
    Inches(8),
    Inches(2)
)

table = table_shape.table

for row_index, row in enumerate(records):
    for col_index, value in enumerate(row):
        table.cell(row_index, col_index).text = str(value)
```

## Formato básico de tabla

```python
from pptx.util import Pt
from pptx.dml.color import RGBColor

cell = table.cell(0, 0)
paragraph = cell.text_frame.paragraphs[0]
run = paragraph.runs[0]

run.font.bold = True
run.font.size = Pt(12)
run.font.color.rgb = RGBColor(255, 255, 255)

cell.fill.solid()
cell.fill.fore_color.rgb = RGBColor(31, 78, 121)
```

## Agregar gráficos

`python-pptx` permite agregar gráficos de PowerPoint.

Ejemplo de gráfico de columnas:

```python
from pptx.chart.data import CategoryChartData
from pptx.enum.chart import XL_CHART_TYPE
from pptx.util import Inches

chart_data = CategoryChartData()
chart_data.categories = ["Laptop", "Mouse", "Teclado"]
chart_data.add_series("Ventas", (10, 25, 15))

slide.shapes.add_chart(
    XL_CHART_TYPE.COLUMN_CLUSTERED,
    Inches(1),
    Inches(1.5),
    Inches(8),
    Inches(4.5),
    chart_data
)
```

## Tipos de gráfico frecuentes

```python
XL_CHART_TYPE.COLUMN_CLUSTERED
XL_CHART_TYPE.BAR_CLUSTERED
XL_CHART_TYPE.LINE
XL_CHART_TYPE.PIE
```

## Agregar gráfico desde datos calculados

```python
from pptx.chart.data import CategoryChartData
from pptx.enum.chart import XL_CHART_TYPE
from pptx.util import Inches

summary = {
    "Laptop": 3500,
    "Mouse": 800,
    "Teclado": 750
}

chart_data = CategoryChartData()
chart_data.categories = list(summary.keys())
chart_data.add_series("Monto", list(summary.values()))

slide.shapes.add_chart(
    XL_CHART_TYPE.BAR_CLUSTERED,
    Inches(1),
    Inches(1.5),
    Inches(8),
    Inches(4.5),
    chart_data
)
```

## Insertar gráficos como imágenes

Cuando el gráfico se genera con `matplotlib`, `seaborn` o `plotly`, puede guardarse como imagen e insertarse.

```python
import matplotlib.pyplot as plt
from pptx import Presentation
from pptx.util import Inches

plt.plot([1, 2, 3], [10, 20, 15])
plt.savefig("grafico.png")
plt.close()

presentation = Presentation()
slide = presentation.slides.add_slide(presentation.slide_layouts[6])

slide.shapes.add_picture(
    "grafico.png",
    Inches(1),
    Inches(1),
    width=Inches(8)
)

presentation.save("grafico.pptx")
```

## Unidades de medida

`python-pptx` usa unidades internas llamadas EMU. Para trabajar de forma más legible, se usan utilidades como:

```python
from pptx.util import Inches, Cm, Pt
```

## Pulgadas

```python
left = Inches(1)
```

## Centímetros

```python
from pptx.util import Cm

left = Cm(2.5)
```

## Puntos tipográficos

```python
from pptx.util import Pt

font_size = Pt(18)
```

## Posición y tamaño

La mayoría de elementos se agregan con:

```text
left
top
width
height
```

Ejemplo:

```python
shape = slide.shapes.add_textbox(
    Inches(1),
    Inches(1),
    Inches(6),
    Inches(1)
)
```

Interpretación:

```text
left   -> distancia desde el borde izquierdo
top    -> distancia desde el borde superior
width  -> ancho del elemento
height -> alto del elemento
```

## Leer texto de una presentación

```python
from pptx import Presentation

presentation = Presentation("presentacion.pptx")

for slide in presentation.slides:
    for shape in slide.shapes:
        if hasattr(shape, "text"):
            print(shape.text)
```

## Recorrer formas

```python
for slide in presentation.slides:
    for shape in slide.shapes:
        print(shape.shape_type)
```

## Detectar si una forma tiene texto

```python
if shape.has_text_frame:
    print(shape.text)
```

## Leer tablas existentes

```python
for slide in presentation.slides:
    for shape in slide.shapes:
        if shape.has_table:
            table = shape.table

            for row in table.rows:
                values = [cell.text for cell in row.cells]
                print(values)
```

## Usar plantillas

Una práctica común consiste en crear una plantilla `.pptx` con diseño, colores, tipografías y layouts definidos, y luego completarla con Python.

```python
from pptx import Presentation

presentation = Presentation("plantilla.pptx")

slide_layout = presentation.slide_layouts[1]
slide = presentation.slides.add_slide(slide_layout)

slide.shapes.title.text = "Resumen ejecutivo"
slide.placeholders[1].text = "Contenido generado automáticamente"

presentation.save("presentacion_generada.pptx")
```

## Reemplazo de texto simple

```python
from pptx import Presentation

presentation = Presentation("plantilla.pptx")

replacements = {
    "{{TITULO}}": "Reporte mensual",
    "{{FECHA}}": "31/12/2026"
}

for slide in presentation.slides:
    for shape in slide.shapes:
        if not shape.has_text_frame:
            continue

        for paragraph in shape.text_frame.paragraphs:
            for run in paragraph.runs:
                for key, value in replacements.items():
                    if key in run.text:
                        run.text = run.text.replace(key, value)

presentation.save("salida.pptx")
```

Este enfoque funciona mejor cuando el marcador está dentro de un mismo run. Si el marcador está dividido entre varios runs, se requiere una lógica más elaborada.

## Propiedades del documento

```python
presentation.core_properties.title = "Reporte mensual"
presentation.core_properties.subject = "Presentación generada con Python"
presentation.core_properties.author = "Python"
```

## Uso con pandas

`python-pptx` puede combinarse con `pandas` para crear diapositivas a partir de DataFrames.

```python
import pandas as pd
from pptx import Presentation
from pptx.util import Inches

df = pd.DataFrame({
    "Producto": ["Laptop", "Mouse", "Teclado"],
    "Monto": [3500, 800, 750]
})

presentation = Presentation()
slide = presentation.slides.add_slide(presentation.slide_layouts[6])

rows = len(df) + 1
cols = len(df.columns)

table_shape = slide.shapes.add_table(
    rows,
    cols,
    Inches(1),
    Inches(1),
    Inches(8),
    Inches(2)
)

table = table_shape.table

for col_index, column_name in enumerate(df.columns):
    table.cell(0, col_index).text = str(column_name)

for row_index, row in enumerate(df.itertuples(index=False), start=1):
    for col_index, value in enumerate(row):
        table.cell(row_index, col_index).text = str(value)

presentation.save("reporte_tabla.pptx")
```

## Uso con matplotlib

```python
import matplotlib.pyplot as plt
from pptx import Presentation
from pptx.util import Inches

values = [10, 25, 15]
labels = ["Laptop", "Mouse", "Teclado"]

plt.bar(labels, values)
plt.tight_layout()
plt.savefig("ventas.png")
plt.close()

presentation = Presentation()
slide = presentation.slides.add_slide(presentation.slide_layouts[6])

slide.shapes.add_picture(
    "ventas.png",
    Inches(1),
    Inches(1),
    width=Inches(8)
)

presentation.save("reporte_grafico.pptx")
```

## Uso con openpyxl

`openpyxl` puede preparar o leer datos desde Excel, y `python-pptx` puede usarlos para generar diapositivas.

Flujo típico:

```text
openpyxl / pandas -> datos -> python-pptx -> presentación
```

## Limitaciones importantes

`python-pptx` cubre muchas tareas comunes, pero no todas las capacidades de PowerPoint.

Limitaciones frecuentes:

- no convierte directamente a PDF
- no ejecuta macros
- no controla animaciones complejas
- no replica completamente el motor visual de PowerPoint
- puede requerir plantillas para diseños profesionales complejos
- el reemplazo de texto puede complicarse si el contenido está dividido en varios runs
- algunas manipulaciones avanzadas pueden requerir trabajar con XML interno

## Errores comunes

## Instalar `pptx` en lugar de `python-pptx`

Problemático:

```bash
python -m pip install pptx
```

Lo correcto es:

```bash
python -m pip install python-pptx
```

La importación sí se hace desde `pptx`:

```python
from pptx import Presentation
```

## Olvidar guardar la presentación

Problemático:

```python
presentation = Presentation()
slide = presentation.slides.add_slide(presentation.slide_layouts[0])
```

Falta:

```python
presentation.save("salida.pptx")
```

## Sobrescribir la plantilla original

Problemático:

```python
presentation.save("plantilla.pptx")
```

si el archivo original debe conservarse.

Más seguro:

```python
presentation.save("plantilla_completada.pptx")
```

## Usar un layout sin revisar placeholders

Problemático:

```python
slide.placeholders[1].text = "Texto"
```

si ese placeholder no existe en el layout.

Más seguro:

```python
for placeholder in slide.placeholders:
    print(placeholder.placeholder_format.idx, placeholder.name)
```

## No controlar unidades

Problemático:

```python
slide.shapes.add_textbox(1, 1, 5, 1)
```

Esos valores no representan pulgadas directamente.

Más claro:

```python
slide.shapes.add_textbox(
    Inches(1),
    Inches(1),
    Inches(5),
    Inches(1)
)
```

## Esperar conversión automática a PDF

`python-pptx` genera o modifica `.pptx`, pero no convierte directamente a PDF.

## No cerrar archivos de imagen generados

Cuando se generan gráficos con `matplotlib`, conviene cerrar la figura.

```python
plt.close()
```

## Buenas prácticas

## Usar plantillas para diseños profesionales

```python
presentation = Presentation("plantilla.pptx")
```

## Guardar con nombre de salida distinto

```python
presentation.save("presentacion_generada.pptx")
```

## Usar `Inches`, `Cm` y `Pt`

```python
from pptx.util import Inches, Cm, Pt
```

## Separar datos, diseño y generación

```python
data = load_data()
presentation = build_presentation(data)
presentation.save(output_path)
```

## Inspeccionar layouts y placeholders

```python
for layout in presentation.slide_layouts:
    print(layout.name)
```

## Usar nombres claros para funciones

```python
def add_title_slide(presentation, title, subtitle):
    ...
```

## Usar pandas para preparar datos tabulares

```python
df = prepare_summary()
```

y `python-pptx` para construir las diapositivas.

## Insertar gráficos como imágenes cuando se necesita control visual

```python
slide.shapes.add_picture("grafico.png", Inches(1), Inches(1))
```

## Ejemplo integrado

```python
from pathlib import Path

import pandas as pd
import matplotlib.pyplot as plt
from pptx import Presentation
from pptx.util import Inches, Pt
from pptx.dml.color import RGBColor


def create_sales_chart(df, output_path):
    plt.figure()
    plt.bar(df["Producto"], df["Monto"])
    plt.title("Monto por producto")
    plt.tight_layout()
    plt.savefig(output_path)
    plt.close()


def add_title_slide(presentation, title, subtitle):
    slide = presentation.slides.add_slide(
        presentation.slide_layouts[0]
    )

    slide.shapes.title.text = title
    slide.placeholders[1].text = subtitle


def add_table_slide(presentation, df):
    slide = presentation.slides.add_slide(
        presentation.slide_layouts[6]
    )

    title_box = slide.shapes.add_textbox(
        Inches(0.7),
        Inches(0.3),
        Inches(12),
        Inches(0.5)
    )

    title_frame = title_box.text_frame
    title_frame.text = "Resumen de ventas"

    title_run = title_frame.paragraphs[0].runs[0]
    title_run.font.size = Pt(24)
    title_run.font.bold = True
    title_run.font.color.rgb = RGBColor(31, 78, 121)

    rows = len(df) + 1
    cols = len(df.columns)

    table_shape = slide.shapes.add_table(
        rows,
        cols,
        Inches(0.7),
        Inches(1.2),
        Inches(8),
        Inches(2.5)
    )

    table = table_shape.table

    for col_index, column_name in enumerate(df.columns):
        table.cell(0, col_index).text = str(column_name)

    for row_index, row in enumerate(df.itertuples(index=False), start=1):
        for col_index, value in enumerate(row):
            table.cell(row_index, col_index).text = str(value)


def add_chart_slide(presentation, chart_path):
    slide = presentation.slides.add_slide(
        presentation.slide_layouts[6]
    )

    slide.shapes.add_textbox(
        Inches(0.7),
        Inches(0.3),
        Inches(12),
        Inches(0.5)
    ).text_frame.text = "Gráfico de ventas"

    slide.shapes.add_picture(
        str(chart_path),
        Inches(1),
        Inches(1.2),
        width=Inches(8.5)
    )


def create_presentation(df, output_path):
    chart_path = Path("sales_chart.png")

    create_sales_chart(df, chart_path)

    presentation = Presentation()
    presentation.slide_width = Inches(13.333)
    presentation.slide_height = Inches(7.5)

    add_title_slide(
        presentation,
        "Reporte de ventas",
        "Presentación generada automáticamente"
    )

    add_table_slide(presentation, df)
    add_chart_slide(presentation, chart_path)

    presentation.save(output_path)


data = pd.DataFrame({
    "Producto": ["Laptop", "Mouse", "Teclado"],
    "Cantidad": [2, 10, 5],
    "Monto": [3500, 800, 750]
})

create_presentation(
    data,
    Path("reporte_ventas.pptx")
)

print("Presentación generada correctamente")
```

## Relación con otras librerías

`python-pptx` se relaciona especialmente con:

- `pandas`, para convertir tablas y resúmenes en diapositivas
- `matplotlib`, para generar gráficos como imágenes
- `plotly`, cuando se exportan gráficos a imagen antes de insertarlos
- `openpyxl`, cuando los datos provienen de Excel
- `python-docx`, cuando también se generan reportes Word
- `pypdf`, cuando se trabaja con documentos finales en PDF
- `pathlib`, para manejar rutas
- `datetime`, para insertar fechas de reporte

## Orden didáctico interno

```text
1. Propósito de python-pptx
2. Instalación e importación
3. Presentation
4. Crear y abrir presentaciones
5. Slide layouts y placeholders
6. Agregar texto
7. TextFrame, Paragraph y Run
8. Imágenes, formas y líneas
9. Tablas
10. Gráficos
11. Plantillas y reemplazos simples
12. Uso con pandas y matplotlib
13. Limitaciones
14. Errores comunes
15. Buenas prácticas
```