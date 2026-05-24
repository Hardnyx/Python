# `python-docx`

## Propósito

`python-docx` es una librería externa para crear, leer y modificar documentos de Microsoft Word en formato `.docx`.

Se utiliza para automatizar reportes, generar documentos estructurados, completar plantillas, agregar títulos, párrafos, tablas, imágenes, saltos de página, estilos y contenido textual sin abrir manualmente Microsoft Word.

## Naturaleza de la librería

`python-docx` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install python-docx
```

La importación habitual es:

```python
from docx import Document
```

El nombre de instalación es `python-docx`, pero el paquete se importa como `docx`.

## Idea central

La idea principal de `python-docx` es manipular un documento Word mediante objetos.

```text
Document -> Paragraph -> Run
Document -> Table -> Row -> Cell
```

Es decir:

```text
documento -> párrafo -> fragmento de texto
documento -> tabla -> fila -> celda
```

Ejemplo básico:

```python
from docx import Document

document = Document()

document.add_heading("Reporte", level=1)
document.add_paragraph("Contenido del reporte.")

document.save("reporte.docx")
```

## Cuándo usar `python-docx`

Conviene usar `python-docx` cuando se necesita:

- crear documentos Word `.docx`
- generar reportes automáticos
- escribir párrafos y títulos
- crear tablas
- insertar imágenes
- agregar saltos de página
- aplicar estilos existentes
- completar documentos con datos generados por Python
- producir documentos a partir de resultados de análisis

## Cuándo no usar `python-docx`

No conviene usar `python-docx` cuando se necesita:

- editar archivos `.doc` antiguos
- controlar diseño avanzado de Word con precisión total
- convertir Word a PDF directamente
- ejecutar macros
- manipular contenido complejo no soportado por la API
- reemplazar texto preservando formato complejo sin lógica adicional

Para convertir a PDF suelen requerirse otras herramientas externas, como Microsoft Word vía automatización en Windows, LibreOffice por línea de comandos u otras librerías especializadas.

## Instalación

Instalación básica:

```bash
python -m pip install python-docx
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
python-docx==1.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
from docx import Document
```

Verificación:

```python
import docx

print(docx.__version__)
```

## Crear un documento nuevo

```python
from docx import Document

document = Document()

document.add_heading("Título del documento", level=1)
document.add_paragraph("Primer párrafo del documento.")

document.save("documento.docx")
```

## Abrir un documento existente

```python
from docx import Document

document = Document("documento.docx")

for paragraph in document.paragraphs:
    print(paragraph.text)
```

## Guardar un documento

```python
document.save("salida.docx")
```

Si se guarda con el mismo nombre del archivo original, se sobrescribe.

Para conservar el original, conviene guardar con otro nombre:

```python
document.save("salida_modificada.docx")
```

## Objeto `Document`

`Document` representa el documento Word completo.

Desde este objeto se puede acceder o agregar:

- párrafos
- títulos
- tablas
- imágenes
- saltos de página
- secciones
- estilos
- propiedades básicas del documento

Ejemplo:

```python
from docx import Document

document = Document()

print(type(document))
```

## Agregar títulos

## `add_heading()`

```python
document.add_heading("Reporte de ventas", level=1)
```

El parámetro `level` indica el nivel del título.

```python
document.add_heading("Título principal", level=0)
document.add_heading("Sección", level=1)
document.add_heading("Subsección", level=2)
```

## Agregar párrafos

## `add_paragraph()`

```python
document.add_paragraph("Este es un párrafo.")
```

También puede guardarse el objeto párrafo:

```python
paragraph = document.add_paragraph("Texto inicial.")
```

## Agregar fragmentos con `add_run()`

Un `Run` representa una parte de un párrafo con formato propio.

```python
paragraph = document.add_paragraph()

paragraph.add_run("Texto normal. ")
paragraph.add_run("Texto en negrita.").bold = True
paragraph.add_run("Texto en cursiva.").italic = True
```

## Formato de runs

```python
run = paragraph.add_run("Texto destacado")
run.bold = True
run.italic = True
run.underline = True
```

## Cambiar tamaño de fuente

```python
from docx.shared import Pt

run = paragraph.add_run("Texto con tamaño")
run.font.size = Pt(12)
```

## Cambiar nombre de fuente

```python
run.font.name = "Arial"
```

## Color de fuente

```python
from docx.shared import RGBColor

run = paragraph.add_run("Texto rojo")
run.font.color.rgb = RGBColor(255, 0, 0)
```

## Leer párrafos

```python
from docx import Document

document = Document("documento.docx")

for paragraph in document.paragraphs:
    print(paragraph.text)
```

## Modificar texto de un párrafo

```python
paragraph = document.paragraphs[0]
paragraph.text = "Nuevo texto del párrafo."
```

Debe tenerse cuidado: asignar `paragraph.text` puede simplificar o perder formato interno de los runs del párrafo.

## Alineación de párrafos

```python
from docx.enum.text import WD_ALIGN_PARAGRAPH

paragraph = document.add_paragraph("Texto centrado")
paragraph.alignment = WD_ALIGN_PARAGRAPH.CENTER
```

Opciones comunes:

```python
WD_ALIGN_PARAGRAPH.LEFT
WD_ALIGN_PARAGRAPH.CENTER
WD_ALIGN_PARAGRAPH.RIGHT
WD_ALIGN_PARAGRAPH.JUSTIFY
```

## Espaciado de párrafos

```python
from docx.shared import Pt

paragraph = document.add_paragraph("Texto con espaciado")
paragraph.paragraph_format.space_after = Pt(12)
paragraph.paragraph_format.space_before = Pt(6)
```

## Interlineado

```python
paragraph.paragraph_format.line_spacing = 1.5
```

## Sangría

```python
from docx.shared import Cm

paragraph.paragraph_format.left_indent = Cm(1)
paragraph.paragraph_format.first_line_indent = Cm(0.5)
```

## Listas

Word maneja listas mediante estilos.

## Lista con viñetas

```python
document.add_paragraph(
    "Primer punto",
    style="List Bullet"
)

document.add_paragraph(
    "Segundo punto",
    style="List Bullet"
)
```

## Lista numerada

```python
document.add_paragraph(
    "Primer paso",
    style="List Number"
)

document.add_paragraph(
    "Segundo paso",
    style="List Number"
)
```

## Saltos de página

```python
document.add_page_break()
```

Ejemplo:

```python
document.add_heading("Primera sección", level=1)
document.add_paragraph("Contenido de la primera sección.")

document.add_page_break()

document.add_heading("Segunda sección", level=1)
document.add_paragraph("Contenido de la segunda sección.")
```

## Tablas

## Crear una tabla

```python
table = document.add_table(rows=1, cols=3)
```

## Agregar encabezados

```python
header_cells = table.rows[0].cells

header_cells[0].text = "Producto"
header_cells[1].text = "Cantidad"
header_cells[2].text = "Precio"
```

## Agregar filas

```python
row_cells = table.add_row().cells

row_cells[0].text = "Laptop"
row_cells[1].text = "2"
row_cells[2].text = "3500"
```

## Ejemplo de tabla completa

```python
from docx import Document

document = Document()

document.add_heading("Reporte de ventas", level=1)

records = [
    ("Laptop", 2, 3500),
    ("Mouse", 10, 80),
    ("Teclado", 5, 150)
]

table = document.add_table(rows=1, cols=3)
table.style = "Table Grid"

header_cells = table.rows[0].cells
header_cells[0].text = "Producto"
header_cells[1].text = "Cantidad"
header_cells[2].text = "Precio"

for product, quantity, price in records:
    row_cells = table.add_row().cells
    row_cells[0].text = product
    row_cells[1].text = str(quantity)
    row_cells[2].text = str(price)

document.save("reporte_ventas.docx")
```

## Leer tablas

```python
from docx import Document

document = Document("reporte.docx")

for table in document.tables:
    for row in table.rows:
        values = [cell.text for cell in row.cells]
        print(values)
```

## Acceder a una celda

```python
cell = table.cell(0, 0)
print(cell.text)
```

En tablas, los índices empiezan en `0`.

## Estilos de tabla

```python
table.style = "Table Grid"
```

Otros estilos dependen de los estilos disponibles en el documento o plantilla.

## Imágenes

## Insertar imagen

```python
from docx import Document
from docx.shared import Inches

document = Document()

document.add_picture("logo.png", width=Inches(1.5))

document.save("imagen.docx")
```

## Usar centímetros

```python
from docx.shared import Cm

document.add_picture("logo.png", width=Cm(4))
```

## Secciones

Las secciones permiten controlar aspectos como márgenes, orientación y tamaño de página.

## Acceder a secciones

```python
section = document.sections[0]
```

## Márgenes

```python
from docx.shared import Cm

section = document.sections[0]

section.top_margin = Cm(2)
section.bottom_margin = Cm(2)
section.left_margin = Cm(2.5)
section.right_margin = Cm(2.5)
```

## Orientación de página

```python
from docx.enum.section import WD_ORIENT

section = document.sections[0]
section.orientation = WD_ORIENT.LANDSCAPE
```

Al cambiar orientación, puede ser necesario ajustar ancho y alto de página.

## Encabezado

```python
section = document.sections[0]
header = section.header

paragraph = header.paragraphs[0]
paragraph.text = "Encabezado del documento"
```

## Pie de página

```python
section = document.sections[0]
footer = section.footer

paragraph = footer.paragraphs[0]
paragraph.text = "Pie de página"
```

## Estilos

Word usa estilos para controlar apariencia de títulos, párrafos, listas y tablas.

## Aplicar estilo a párrafo

```python
document.add_paragraph(
    "Texto con estilo",
    style="Intense Quote"
)
```

## Aplicar estilo a tabla

```python
table.style = "Table Grid"
```

## Listar estilos disponibles

```python
for style in document.styles:
    print(style.name)
```

## Usar una plantilla

Una forma práctica de controlar diseño es partir de un archivo `.docx` ya preparado con estilos.

```python
from docx import Document

document = Document("plantilla.docx")

document.add_heading("Nuevo reporte", level=1)
document.add_paragraph("Contenido generado automáticamente.")

document.save("reporte_generado.docx")
```

Este patrón es útil cuando se necesita respetar una identidad visual o estructura corporativa.

## Reemplazo de texto simple

Para reemplazos simples en párrafos:

```python
from docx import Document

document = Document("plantilla.docx")

for paragraph in document.paragraphs:
    if "{{NOMBRE}}" in paragraph.text:
        paragraph.text = paragraph.text.replace("{{NOMBRE}}", "Ana")

document.save("salida.docx")
```

Este enfoque puede alterar formato interno si el texto original estaba dividido en varios runs.

## Reemplazo preservando runs simples

Cuando el marcador está dentro de un mismo run:

```python
for paragraph in document.paragraphs:
    for run in paragraph.runs:
        if "{{NOMBRE}}" in run.text:
            run.text = run.text.replace("{{NOMBRE}}", "Ana")
```

Este método conserva mejor el formato del run, pero no resuelve todos los casos si el marcador está dividido entre varios runs.

## Reemplazo en tablas

```python
for table in document.tables:
    for row in table.rows:
        for cell in row.cells:
            for paragraph in cell.paragraphs:
                for run in paragraph.runs:
                    if "{{MONTO}}" in run.text:
                        run.text = run.text.replace("{{MONTO}}", "1000")
```

## Generar documento desde datos

```python
from docx import Document

records = [
    {"product": "Laptop", "amount": 3500},
    {"product": "Mouse", "amount": 80}
]

document = Document()

document.add_heading("Reporte", level=1)

for record in records:
    document.add_paragraph(
        f"{record['product']}: {record['amount']}"
    )

document.save("reporte.docx")
```

## Uso con pandas

`python-docx` puede combinarse con `pandas` para generar reportes Word desde DataFrames.

```python
import pandas as pd
from docx import Document

df = pd.DataFrame({
    "Producto": ["Laptop", "Mouse"],
    "Monto": [3500, 80]
})

document = Document()

document.add_heading("Reporte desde pandas", level=1)

table = document.add_table(
    rows=1,
    cols=len(df.columns)
)

table.style = "Table Grid"

header_cells = table.rows[0].cells

for index, column_name in enumerate(df.columns):
    header_cells[index].text = column_name

for _, row in df.iterrows():
    row_cells = table.add_row().cells

    for index, value in enumerate(row):
        row_cells[index].text = str(value)

document.save("reporte_pandas.docx")
```

## Uso con gráficos

`python-docx` no crea gráficos estadísticos como `matplotlib`, pero puede insertar imágenes generadas por otras librerías.

```python
import matplotlib.pyplot as plt
from docx import Document
from docx.shared import Inches

plt.plot([1, 2, 3], [10, 20, 15])
plt.savefig("grafico.png")
plt.close()

document = Document()
document.add_heading("Reporte con gráfico", level=1)
document.add_picture("grafico.png", width=Inches(5))

document.save("reporte_grafico.docx")
```

## Propiedades básicas del documento

```python
document.core_properties.title = "Reporte"
document.core_properties.author = "Python"
document.core_properties.subject = "Documento generado automáticamente"
```

## Limitaciones importantes

`python-docx` cubre muchas tareas comunes, pero no todas las capacidades de Microsoft Word.

Limitaciones frecuentes:

- no convierte directamente a PDF
- no ejecuta macros
- no recalcula contenido dinámico complejo
- no maneja todos los elementos avanzados de Word desde API pública
- el reemplazo de texto puede complicarse si el texto está dividido en varios runs
- la manipulación de diseños complejos puede requerir plantillas bien preparadas

## Diferencia entre texto de párrafo y runs

Un párrafo puede contener varios runs.

Ejemplo conceptual:

```text
Párrafo: "Texto normal y texto destacado"
Run 1: "Texto normal y "
Run 2: "texto destacado"
```

Cada run puede tener formato propio.

Por eso, modificar `paragraph.text` puede simplificar la estructura interna y afectar formato.

## Errores comunes

## Instalar `docx` en lugar de `python-docx`

Problemático:

```bash
python -m pip install docx
```

Lo correcto es:

```bash
python -m pip install python-docx
```

La importación sí se hace desde `docx`:

```python
from docx import Document
```

## Olvidar guardar el documento

Problemático:

```python
document.add_paragraph("Texto")
```

sin:

```python
document.save("salida.docx")
```

## Sobrescribir el archivo original

Problemático:

```python
document.save("plantilla.docx")
```

si el archivo original debe conservarse.

Más seguro:

```python
document.save("plantilla_completada.docx")
```

## Asignar `paragraph.text` sin considerar formato

```python
paragraph.text = paragraph.text.replace("{{NOMBRE}}", "Ana")
```

Puede eliminar formato interno de runs.

Si el marcador está dentro de un mismo run, puede ser mejor reemplazar en `run.text`.

## Esperar que convierta Word a PDF

`python-docx` trabaja con `.docx`, pero no realiza conversión directa a PDF.

## Esperar compatibilidad total con elementos avanzados

Algunos elementos de Word pueden no estar disponibles mediante métodos directos de la librería.

## No validar rutas de imágenes

Problemático:

```python
document.add_picture("logo.png")
```

si el archivo no existe en la ruta esperada.

Conviene usar `pathlib` y validar existencia cuando corresponda.

## Buenas prácticas

## Usar plantillas cuando el diseño sea importante

```python
document = Document("plantilla.docx")
```

## Guardar con nombre de salida distinto

```python
document.save("reporte_generado.docx")
```

## Separar generación de contenido y guardado

```python
document = build_report(data)
document.save(output_path)
```

## Usar estilos de Word en lugar de formato manual excesivo

```python
document.add_heading("Sección", level=1)
document.add_paragraph("Texto", style="Normal")
```

## Usar tablas para datos estructurados

```python
table = document.add_table(rows=1, cols=3)
```

## Convertir valores a texto al escribir en celdas

```python
cell.text = str(value)
```

## Controlar unidades con `Inches`, `Cm` o `Pt`

```python
from docx.shared import Inches, Cm, Pt
```

## Mantener marcadores simples en plantillas

Ejemplos:

```text
{{NOMBRE}}
{{FECHA}}
{{MONTO}}
```

Marcadores simples reducen problemas de reemplazo.

## Ejemplo integrado

```python
from pathlib import Path

import pandas as pd
from docx import Document
from docx.shared import Inches


def add_dataframe_table(document, df):
    table = document.add_table(
        rows=1,
        cols=len(df.columns)
    )

    table.style = "Table Grid"

    header_cells = table.rows[0].cells

    for index, column_name in enumerate(df.columns):
        header_cells[index].text = str(column_name)

    for _, row in df.iterrows():
        row_cells = table.add_row().cells

        for index, value in enumerate(row):
            row_cells[index].text = "" if pd.isna(value) else str(value)

    return table


def create_report(data, output_path, chart_path=None):
    document = Document()

    document.core_properties.title = "Reporte generado"
    document.add_heading("Reporte de ventas", level=1)

    document.add_paragraph(
        "Resumen generado automáticamente a partir de datos estructurados."
    )

    df = pd.DataFrame(data)

    add_dataframe_table(document, df)

    if chart_path is not None and chart_path.exists():
        document.add_page_break()
        document.add_heading("Gráfico", level=1)
        document.add_picture(str(chart_path), width=Inches(5))

    document.save(output_path)


data = [
    {"Producto": "Laptop", "Cantidad": 2, "Monto": 3500},
    {"Producto": "Mouse", "Cantidad": 10, "Monto": 80},
    {"Producto": "Teclado", "Cantidad": 5, "Monto": 150}
]

create_report(
    data,
    Path("reporte_ventas.docx")
)

print("Documento generado correctamente")
```

## Relación con otras librerías

`python-docx` se relaciona especialmente con:

- `pandas`, para convertir tablas de datos en reportes Word
- `matplotlib`, para generar gráficos como imágenes e insertarlos
- `openpyxl`, cuando los datos provienen de Excel
- `python-pptx`, cuando también se generan presentaciones
- `pypdf`, cuando se trabaja además con PDFs
- `pathlib`, para manejar rutas de documentos e imágenes
- `datetime`, para insertar fechas en reportes
- `jinja2` o `docxtpl`, cuando se requiere un sistema de plantillas más avanzado

## Orden didáctico interno

```text
1. Propósito de python-docx
2. Instalación e importación
3. Document
4. Crear y abrir documentos
5. Párrafos, headings y runs
6. Formato básico de texto
7. Tablas
8. Imágenes
9. Secciones, encabezados y pies
10. Plantillas y reemplazos simples
11. Uso con pandas y gráficos
12. Limitaciones
13. Errores comunes
14. Buenas prácticas
```