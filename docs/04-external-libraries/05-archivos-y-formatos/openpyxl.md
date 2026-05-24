# `openpyxl`

## Propósito

`openpyxl` es una librería externa para leer, crear, modificar y guardar archivos de Excel en formatos modernos como `.xlsx`, `.xlsm`, `.xltx` y `.xltm`.

Se utiliza para automatizar reportes, completar plantillas, leer datos desde hojas de cálculo, escribir resultados, aplicar formatos, crear fórmulas, manejar hojas, ajustar dimensiones, insertar tablas, gráficos e imágenes, y trabajar con libros de Excel sin abrir manualmente la aplicación.

## Naturaleza de la librería

`openpyxl` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install openpyxl
```

Importaciones frecuentes:

```python
from openpyxl import Workbook
from openpyxl import load_workbook
```

También se importan clases auxiliares para estilos, tablas, gráficos, validaciones o imágenes cuando se necesitan.

## Idea central

La idea principal de `openpyxl` es manipular un archivo de Excel como un conjunto de objetos.

```text
Workbook -> Worksheet -> Cell
```

Es decir:

```text
libro -> hoja -> celda
```

Ejemplo básico:

```python
from openpyxl import Workbook

workbook = Workbook()
worksheet = workbook.active

worksheet["A1"] = "Producto"
worksheet["B1"] = "Monto"

worksheet.append(["Laptop", 3500])
worksheet.append(["Mouse", 80])

workbook.save("reporte.xlsx")
```

## Cuándo usar `openpyxl`

Conviene usar `openpyxl` cuando se necesita:

- crear archivos `.xlsx`
- leer archivos Excel existentes
- modificar libros de Excel ya creados
- conservar hojas y formatos básicos
- completar plantillas
- escribir fórmulas
- aplicar estilos
- ajustar anchos de columnas
- crear reportes automatizados
- trabajar con celdas, hojas, tablas y rangos

## Cuándo no usar `openpyxl`

No es la mejor opción cuando solo se necesita análisis tabular simple.

Para limpiar, cruzar, filtrar o transformar datos, suele ser más conveniente usar:

```text
pandas
polars
```

Luego, si se necesita exportar a Excel con formato detallado, puede combinarse con `openpyxl`.

Tampoco es ideal para escribir archivos `.xls` antiguos. `openpyxl` está orientado a formatos modernos de Excel basados en Office Open XML.

## Instalación

Instalación básica:

```bash
python -m pip install openpyxl
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
openpyxl==3.x.x
```

La versión exacta puede variar según el entorno.

## Importaciones principales

## Crear libros nuevos

```python
from openpyxl import Workbook
```

## Abrir libros existentes

```python
from openpyxl import load_workbook
```

## Verificación

```python
import openpyxl

print(openpyxl.__version__)
```

## Crear un libro nuevo

```python
from openpyxl import Workbook

workbook = Workbook()

worksheet = workbook.active
worksheet.title = "Datos"

worksheet["A1"] = "Nombre"
worksheet["B1"] = "Edad"

worksheet.append(["Ana", 20])
worksheet.append(["Luis", 25])

workbook.save("personas.xlsx")
```

## `Workbook`

`Workbook` representa un libro de Excel.

```python
from openpyxl import Workbook

workbook = Workbook()
```

Cuando se crea un libro nuevo, incluye al menos una hoja activa.

```python
worksheet = workbook.active
```

## Hoja activa

```python
worksheet = workbook.active
```

La hoja activa es la hoja seleccionada por defecto dentro del libro.

## Cambiar nombre de hoja

```python
worksheet.title = "Datos"
```

## Crear hojas

```python
worksheet = workbook.create_sheet("Resumen")
```

Crear hoja en una posición específica:

```python
worksheet = workbook.create_sheet("Inicio", 0)
```

## Listar hojas

```python
print(workbook.sheetnames)
```

## Acceder a una hoja por nombre

```python
worksheet = workbook["Datos"]
```

## Eliminar hojas

```python
worksheet = workbook["HojaTemporal"]
workbook.remove(worksheet)
```

## Guardar libro

```python
workbook.save("archivo.xlsx")
```

El archivo no se escribe en disco hasta llamar a `save()`.

## Abrir un libro existente

```python
from openpyxl import load_workbook

workbook = load_workbook("archivo.xlsx")

worksheet = workbook.active

print(workbook.sheetnames)
print(worksheet.title)
```

## Abrir una hoja específica

```python
worksheet = workbook["Datos"]
```

## Guardar cambios

```python
workbook.save("archivo_modificado.xlsx")
```

Si se usa el mismo nombre de archivo, se sobrescribe el libro original.

## Acceso a celdas

## Por coordenada tipo Excel

```python
worksheet["A1"] = "Producto"
worksheet["B1"] = "Monto"

print(worksheet["A1"].value)
```

## Con `cell()`

```python
cell = worksheet.cell(row=1, column=1)
cell.value = "Producto"
```

En `openpyxl`, las filas y columnas empiezan en `1`, no en `0`.

```text
row=1, column=1 -> A1
row=1, column=2 -> B1
row=2, column=1 -> A2
```

## Leer valor de celda

```python
value = worksheet["A1"].value

print(value)
```

## Escribir valor en celda

```python
worksheet["A1"] = "Total"
worksheet["B1"] = 1000
```

## Tipos de datos

`openpyxl` puede escribir valores como:

```text
str
int
float
bool
datetime
None
```

Ejemplo:

```python
from datetime import datetime
from openpyxl import Workbook

workbook = Workbook()
worksheet = workbook.active

worksheet["A1"] = "Fecha"
worksheet["B1"] = datetime.now()

workbook.save("fechas.xlsx")
```

## Agregar filas con `append()`

`append()` agrega una fila al final de los datos existentes.

```python
from openpyxl import Workbook

workbook = Workbook()
worksheet = workbook.active

worksheet.append(["Producto", "Monto"])
worksheet.append(["Laptop", 3500])
worksheet.append(["Mouse", 80])

workbook.save("productos.xlsx")
```

## Agregar filas desde una lista de registros

```python
records = [
    ["Laptop", 3500],
    ["Mouse", 80],
    ["Teclado", 150]
]

for record in records:
    worksheet.append(record)
```

## Agregar filas desde diccionarios

```python
records = [
    {"product": "Laptop", "amount": 3500},
    {"product": "Mouse", "amount": 80}
]

worksheet.append(["Producto", "Monto"])

for record in records:
    worksheet.append([
        record["product"],
        record["amount"]
    ])
```

## Leer filas

## `iter_rows()`

```python
for row in worksheet.iter_rows():
    values = [cell.value for cell in row]
    print(values)
```

## Leer valores directamente

```python
for row in worksheet.iter_rows(values_only=True):
    print(row)
```

`values_only=True` devuelve valores, no objetos celda.

## Leer un rango de filas y columnas

```python
for row in worksheet.iter_rows(
    min_row=2,
    max_row=10,
    min_col=1,
    max_col=3,
    values_only=True
):
    print(row)
```

## Leer columnas

## `iter_cols()`

```python
for column in worksheet.iter_cols(
    min_col=1,
    max_col=2,
    values_only=True
):
    print(column)
```

## Dimensiones usadas

## Fila máxima

```python
print(worksheet.max_row)
```

## Columna máxima

```python
print(worksheet.max_column)
```

Estas propiedades ayudan a recorrer la zona ocupada de la hoja, aunque pueden incluir celdas que alguna vez tuvieron formato o contenido.

## Recorrer datos como tabla simple

```python
from openpyxl import load_workbook

workbook = load_workbook("productos.xlsx")
worksheet = workbook["Datos"]

for row in worksheet.iter_rows(min_row=2, values_only=True):
    product, amount = row
    print(product, amount)
```

## Convertir hoja a lista de diccionarios

```python
from openpyxl import load_workbook

workbook = load_workbook("productos.xlsx")
worksheet = workbook["Datos"]

rows = list(
    worksheet.iter_rows(values_only=True)
)

headers = rows[0]
data_rows = rows[1:]

records = []

for row in data_rows:
    record = dict(zip(headers, row))
    records.append(record)

print(records)
```

## Fórmulas

Las fórmulas se escriben como texto empezando con `=`.

```python
from openpyxl import Workbook

workbook = Workbook()
worksheet = workbook.active

worksheet["A1"] = 10
worksheet["A2"] = 20
worksheet["A3"] = "=SUM(A1:A2)"

workbook.save("formulas.xlsx")
```

## Lectura de fórmulas vs resultados calculados

Al abrir un libro, puede usarse `data_only`.

```python
workbook = load_workbook(
    "formulas.xlsx",
    data_only=True
)
```

Con `data_only=True`, `openpyxl` intenta leer el último valor calculado guardado por Excel.

Importante: `openpyxl` no recalcula fórmulas como Excel. Si el archivo nunca fue calculado por Excel u otro motor compatible, el valor calculado puede no estar disponible.

## Estilos

`openpyxl` permite aplicar estilos a celdas.

Importaciones frecuentes:

```python
from openpyxl.styles import Font, PatternFill, Border, Side, Alignment
```

## Fuente

```python
from openpyxl.styles import Font

worksheet["A1"].font = Font(bold=True)
```

## Color de relleno

```python
from openpyxl.styles import PatternFill

worksheet["A1"].fill = PatternFill(
    fill_type="solid",
    fgColor="FFC000"
)
```

## Alineación

```python
from openpyxl.styles import Alignment

worksheet["A1"].alignment = Alignment(
    horizontal="center",
    vertical="center"
)
```

## Bordes

```python
from openpyxl.styles import Border, Side

thin = Side(style="thin")

worksheet["A1"].border = Border(
    left=thin,
    right=thin,
    top=thin,
    bottom=thin
)
```

## Formato numérico

```python
worksheet["B2"] = 1234.56
worksheet["B2"].number_format = '#,##0.00'
```

Formato de porcentaje:

```python
worksheet["C2"] = 0.18
worksheet["C2"].number_format = '0.00%'
```

Formato de fecha:

```python
worksheet["D2"].number_format = 'DD/MM/YYYY'
```

## Aplicar estilo a encabezados

```python
from openpyxl.styles import Font, PatternFill, Alignment

header_fill = PatternFill(
    fill_type="solid",
    fgColor="1F4E78"
)

header_font = Font(
    color="FFFFFF",
    bold=True
)

for cell in worksheet[1]:
    cell.fill = header_fill
    cell.font = header_font
    cell.alignment = Alignment(horizontal="center")
```

## Ancho de columnas

```python
worksheet.column_dimensions["A"].width = 20
worksheet.column_dimensions["B"].width = 15
```

## Alto de filas

```python
worksheet.row_dimensions[1].height = 25
```

## Ajustar ancho de columnas automáticamente

Excel calcula anchos visuales con criterios propios. `openpyxl` no replica perfectamente el AutoFit de Excel, pero puede aplicarse una aproximación.

```python
from openpyxl.utils import get_column_letter

for column_cells in worksheet.columns:
    max_length = 0
    column_letter = get_column_letter(column_cells[0].column)

    for cell in column_cells:
        if cell.value is None:
            continue

        value_length = len(str(cell.value))

        if value_length > max_length:
            max_length = value_length

    worksheet.column_dimensions[column_letter].width = max_length + 2
```

## Congelar paneles

```python
worksheet.freeze_panes = "A2"
```

Esto congela la fila superior.

Para congelar primera fila y primera columna:

```python
worksheet.freeze_panes = "B2"
```

## Filtros

```python
worksheet.auto_filter.ref = worksheet.dimensions
```

También se puede especificar un rango:

```python
worksheet.auto_filter.ref = "A1:D100"
```

## Combinar celdas

```python
worksheet.merge_cells("A1:D1")
worksheet["A1"] = "Reporte"
```

Descombinar:

```python
worksheet.unmerge_cells("A1:D1")
```

## Insertar filas y columnas

## Insertar filas

```python
worksheet.insert_rows(2)
```

Insertar varias filas:

```python
worksheet.insert_rows(2, amount=3)
```

## Insertar columnas

```python
worksheet.insert_cols(2)
```

## Eliminar filas y columnas

## Eliminar filas

```python
worksheet.delete_rows(2)
```

## Eliminar columnas

```python
worksheet.delete_cols(2)
```

## Mover rangos

```python
worksheet.move_range("A1:B3", rows=2, cols=1)
```

Este tipo de operación debe usarse con cuidado porque puede afectar referencias y estructura de la hoja.

## Tablas

`openpyxl` permite crear tablas de Excel.

```python
from openpyxl import Workbook
from openpyxl.worksheet.table import Table, TableStyleInfo

workbook = Workbook()
worksheet = workbook.active
worksheet.title = "Datos"

worksheet.append(["Producto", "Monto"])
worksheet.append(["Laptop", 3500])
worksheet.append(["Mouse", 80])

table = Table(
    displayName="TablaDatos",
    ref="A1:B3"
)

style = TableStyleInfo(
    name="TableStyleMedium9",
    showFirstColumn=False,
    showLastColumn=False,
    showRowStripes=True,
    showColumnStripes=False
)

table.tableStyleInfo = style

worksheet.add_table(table)

workbook.save("tabla.xlsx")
```

## Gráficos

`openpyxl` puede crear gráficos básicos.

Ejemplo de gráfico de barras:

```python
from openpyxl import Workbook
from openpyxl.chart import BarChart, Reference

workbook = Workbook()
worksheet = workbook.active

rows = [
    ["Producto", "Monto"],
    ["Laptop", 3500],
    ["Mouse", 80],
    ["Teclado", 150]
]

for row in rows:
    worksheet.append(row)

chart = BarChart()
chart.title = "Montos por producto"
chart.y_axis.title = "Monto"
chart.x_axis.title = "Producto"

data = Reference(
    worksheet,
    min_col=2,
    min_row=1,
    max_row=4
)

categories = Reference(
    worksheet,
    min_col=1,
    min_row=2,
    max_row=4
)

chart.add_data(data, titles_from_data=True)
chart.set_categories(categories)

worksheet.add_chart(chart, "D2")

workbook.save("grafico.xlsx")
```

## Imágenes

```python
from openpyxl import Workbook
from openpyxl.drawing.image import Image

workbook = Workbook()
worksheet = workbook.active

image = Image("logo.png")
worksheet.add_image(image, "A1")

workbook.save("imagen.xlsx")
```

## Validación de datos

```python
from openpyxl import Workbook
from openpyxl.worksheet.datavalidation import DataValidation

workbook = Workbook()
worksheet = workbook.active

validation = DataValidation(
    type="list",
    formula1='"Pendiente,Aprobado,Rechazado"',
    allow_blank=True
)

worksheet.add_data_validation(validation)
validation.add("A2:A100")

workbook.save("validacion.xlsx")
```

## Comentarios

```python
from openpyxl.comments import Comment

worksheet["A1"] = "Dato"
worksheet["A1"].comment = Comment(
    "Comentario de ejemplo",
    "Autor"
)
```

## Hipervínculos

```python
worksheet["A1"] = "Sitio"
worksheet["A1"].hyperlink = "https://example.com"
worksheet["A1"].style = "Hyperlink"
```

## Protección de hoja

```python
worksheet.protection.sheet = True
worksheet.protection.password = "clave"
```

Esta protección no debe considerarse seguridad fuerte. Es una protección propia de Excel para edición accidental o control básico de hoja.

## Modo solo lectura

Para archivos grandes puede abrirse en modo solo lectura.

```python
workbook = load_workbook(
    "archivo.xlsx",
    read_only=True
)

worksheet = workbook.active

for row in worksheet.iter_rows(values_only=True):
    print(row)

workbook.close()
```

## Modo escritura optimizada

Para generar archivos grandes puede usarse modo write-only.

```python
from openpyxl import Workbook

workbook = Workbook(write_only=True)
worksheet = workbook.create_sheet("Datos")

for number in range(1000):
    worksheet.append([number, number * 2])

workbook.save("grande.xlsx")
```

## Mantener macros

`openpyxl` puede abrir archivos `.xlsm`, pero para conservar macros al guardar debe usarse `keep_vba=True`.

```python
workbook = load_workbook(
    "archivo.xlsm",
    keep_vba=True
)

workbook.save("archivo_modificado.xlsm")
```

Importante: `openpyxl` no crea ni edita código VBA. Solo puede conservarlo en ciertos flujos si se abre el archivo adecuadamente.

## Relación con pandas

Pandas usa motores de lectura y escritura para trabajar con Excel. En muchos casos, `openpyxl` se usa como motor para archivos `.xlsx`.

Ejemplo con pandas:

```python
import pandas as pd

df = pd.read_excel("datos.xlsx", engine="openpyxl")
```

Exportación:

```python
df.to_excel("salida.xlsx", index=False, engine="openpyxl")
```

## Cuándo usar pandas y cuándo openpyxl

## Pandas

Conviene para:

- limpiar datos
- transformar tablas
- filtrar registros
- cruzar datos
- agrupar y resumir
- trabajar con DataFrames

## openpyxl

Conviene para:

- controlar celdas específicas
- aplicar estilos
- modificar plantillas
- escribir fórmulas
- crear hojas
- ajustar formatos
- trabajar con tablas, gráficos e imágenes
- conservar estructura de un libro existente

## Patrón combinado

```python
import pandas as pd
from openpyxl import load_workbook

df = pd.read_excel("input.xlsx")

summary = (
    df.groupby("Producto")
    .agg(Total=("Monto", "sum"))
    .reset_index()
)

summary.to_excel("output.xlsx", index=False)

workbook = load_workbook("output.xlsx")
worksheet = workbook.active

worksheet.freeze_panes = "A2"

workbook.save("output.xlsx")
```

## Errores comunes

## Olvidar guardar el libro

Problemático:

```python
worksheet["A1"] = "Dato"
```

sin:

```python
workbook.save("archivo.xlsx")
```

## Usar índices desde cero en `cell()`

Problemático:

```python
worksheet.cell(row=0, column=0)
```

En `openpyxl`, las filas y columnas empiezan en `1`.

Correcto:

```python
worksheet.cell(row=1, column=1)
```

## Esperar que `openpyxl` recalcule fórmulas

`openpyxl` escribe fórmulas, pero no las calcula como Excel.

Si se lee con `data_only=True`, se lee el último resultado calculado guardado en el archivo, si existe.

## Sobrescribir el archivo original sin copia

Problemático:

```python
workbook.save("reporte.xlsx")
```

si el archivo original debe conservarse.

Más seguro:

```python
workbook.save("reporte_modificado.xlsx")
```

## Usar `openpyxl` para transformar datos tabulares masivos sin necesidad

Para transformaciones de datos, suele ser más claro usar `pandas` o `polars`.

## No cerrar libros en modo solo lectura

Cuando se usa `read_only=True`, conviene cerrar el libro al finalizar.

```python
workbook.close()
```

## Modificar archivos `.xlsm` sin `keep_vba=True`

Si se quiere conservar macros, debe abrirse con:

```python
load_workbook("archivo.xlsm", keep_vba=True)
```

## Usar `append()` esperando escribir en una posición específica

`append()` agrega al final. Para escribir en una celda específica, debe usarse:

```python
worksheet["A10"] = "Dato"
```

o:

```python
worksheet.cell(row=10, column=1, value="Dato")
```

## Buenas prácticas

## Usar nombres descriptivos

```python
workbook = Workbook()
worksheet = workbook.active
```

## Guardar con nombre nuevo al modificar plantillas

```python
workbook.save("reporte_generado.xlsx")
```

## Usar `values_only=True` cuando solo se necesitan valores

```python
for row in worksheet.iter_rows(values_only=True):
    ...
```

## Separar lectura, transformación y escritura

```python
records = read_workbook(path)
summary = build_summary(records)
write_report(summary, output_path)
```

## Usar pandas para transformación tabular

```python
df = pd.read_excel("input.xlsx")
```

y `openpyxl` para acabado final del archivo.

## Definir estilos una vez y reutilizarlos

```python
header_font = Font(bold=True, color="FFFFFF")
header_fill = PatternFill(fill_type="solid", fgColor="1F4E78")
```

## Controlar tipos sensibles

Códigos, DNI, identificadores y campos con ceros a la izquierda deben tratarse como texto.

## Evitar escribir credenciales o rutas rígidas

Usar `pathlib` y configuración externa cuando corresponda.

## Ejemplo integrado

```python
from pathlib import Path

from openpyxl import Workbook
from openpyxl.styles import Alignment, Font, PatternFill
from openpyxl.utils import get_column_letter
from openpyxl.worksheet.table import Table, TableStyleInfo


def create_sales_report(records, output_path):
    workbook = Workbook()
    worksheet = workbook.active
    worksheet.title = "Ventas"

    headers = ["Producto", "Cantidad", "Precio", "Total"]
    worksheet.append(headers)

    for record in records:
        quantity = record["quantity"]
        price = record["price"]
        total = quantity * price

        worksheet.append([
            record["product"],
            quantity,
            price,
            total
        ])

    header_fill = PatternFill(
        fill_type="solid",
        fgColor="1F4E78"
    )

    header_font = Font(
        color="FFFFFF",
        bold=True
    )

    for cell in worksheet[1]:
        cell.fill = header_fill
        cell.font = header_font
        cell.alignment = Alignment(horizontal="center")

    for row in worksheet.iter_rows(min_row=2, min_col=3, max_col=4):
        for cell in row:
            cell.number_format = '#,##0.00'

    for column_cells in worksheet.columns:
        max_length = 0
        column_letter = get_column_letter(column_cells[0].column)

        for cell in column_cells:
            if cell.value is None:
                continue

            max_length = max(max_length, len(str(cell.value)))

        worksheet.column_dimensions[column_letter].width = max_length + 2

    worksheet.freeze_panes = "A2"
    worksheet.auto_filter.ref = worksheet.dimensions

    table = Table(
        displayName="TablaVentas",
        ref=worksheet.dimensions
    )

    style = TableStyleInfo(
        name="TableStyleMedium9",
        showFirstColumn=False,
        showLastColumn=False,
        showRowStripes=True,
        showColumnStripes=False
    )

    table.tableStyleInfo = style
    worksheet.add_table(table)

    workbook.save(output_path)


records = [
    {"product": "Laptop", "quantity": 2, "price": 3500},
    {"product": "Mouse", "quantity": 10, "price": 80},
    {"product": "Teclado", "quantity": 5, "price": 150}
]

create_sales_report(
    records,
    Path("reporte_ventas.xlsx")
)

print("Reporte generado correctamente")
```

## Relación con otras librerías

`openpyxl` se relaciona especialmente con:

- `pandas`, para leer, transformar y exportar datos tabulares
- `xlsxwriter`, como alternativa para crear archivos Excel nuevos con formato
- `pyxlsb`, para leer ciertos archivos binarios `.xlsb`
- `python-docx`, cuando se automatizan reportes Word además de Excel
- `python-pptx`, cuando se generan presentaciones a partir de datos
- `pathlib`, para manejar rutas de archivos
- `datetime`, para escribir fechas
- `os` y `shutil`, para flujos de archivos y copias

## Orden didáctico interno

```text
1. Propósito de openpyxl
2. Instalación e importación
3. Workbook, Worksheet y Cell
4. Crear y abrir libros
5. Leer y escribir celdas
6. Recorrer filas y columnas
7. Guardar archivos
8. Fórmulas
9. Estilos y formatos
10. Dimensiones, filtros y paneles
11. Tablas, gráficos e imágenes
12. Modos read_only y write_only
13. Relación con pandas
14. Errores comunes
15. Buenas prácticas
```